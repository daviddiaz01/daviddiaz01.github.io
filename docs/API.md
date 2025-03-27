# Sensor Subsystem API Documentation

## Introduction

The Sensor Subsystem uses the OPT4060 RGBW Color Sensor to detect line contrast for a line-following robot. This sensor communicates with the ESP32-S3-WROOM-1-N4 over I2C, and the ESP32 then processes the data and transmits a signed line_position value via UART to other subsystems, including the Motor Controller (0x03), OLED Display (0x04), and WiFi Module (0x05). This document outlines internal communication, message formatting, and protocol compliance.

---

## Message Overview

| Message       | Variable Name   | Type     | Min   | Max   | Example |
|---------------|------------------|----------|--------|--------|---------|
| Line Position | `line_position`  | `int8_t` | -100  | 100   | 25      |

---

## Internal I2C Communication

### ESP32 Requests Sensor Data from OPT4060

| Byte | Description         | Type      | Value  |
|------|----------------------|-----------|--------|
| 1    | I2C Command          | `uint8_t` | 0x10   |
| 2    | Source ID (ESP32)    | `uint8_t` | 0x01   |
| 3    | Destination (Sensor) | `uint8_t` | 0x02   |
| 4    | End Byte             | `uint8_t` | 0x42   |

---

### OPT4060 Sends RGBW Data to ESP32

| Byte | Description         | Type      | Value  |
|------|----------------------|-----------|--------|
| 1    | Message Type         | `uint8_t` | 0x20   |
| 2    | Source ID (Sensor)   | `uint8_t` | 0x02   |
| 3    | Color LSB            | `uint8_t` | 0x7A   |
| 4    | Color MSB            | `uint8_t` | 0x00   |
| 5    | Checksum             | `uint8_t` | 0xA1   |
| 6    | End Byte             | `uint8_t` | 0x42   |

---

### ESP32 Extracts Line Position

The ESP32 calculates the line_position value based on the RGBW sensor reading and maps it into an int8_t range (-100 to 100).  
- Negative values: Drift to the left  
- Positive values: Drift to the right

---

## UART Message Format – ESP32 Sends Processed Data

### To Motor Controller (0x03)

| Byte | Description      | Type      | Example |
|------|------------------|-----------|---------|
| 1    | Start Byte       | `uint8_t` | 0x41    |
| 2    | Sender ID        | `uint8_t` | 0x01    |
| 3    | Receiver ID      | `uint8_t` | 0x03    |
| 4    | Line Position    | `int8_t`  | 25      |
| 5    | End Byte         | `uint8_t` | 0x42    |

### To OLED Display (0x04)

| Byte | Description      | Type      | Example |
|------|------------------|-----------|---------|
| 1    | Start Byte       | `uint8_t` | 0x41    |
| 2    | Sender ID        | `uint8_t` | 0x01    |
| 3    | Receiver ID      | `uint8_t` | 0x04    |
| 4    | Line Position    | `int8_t`  | 25      |
| 5    | End Byte         | `uint8_t` | 0x42    |

### To WiFi Module (0x05)

| Byte | Description      | Type      | Example |
|------|------------------|-----------|---------|
| 1    | Start Byte       | `uint8_t` | 0x41    |
| 2    | Sender ID        | `uint8_t` | 0x01    |
| 3    | Receiver ID      | `uint8_t` | 0x05    |
| 4    | Line Position    | `int8_t`  | 25      |
| 5    | End Byte         | `uint8_t` | 0x42    |

---

## Communication Flow

| Stage                          | Description                                           |
|--------------------------------|-------------------------------------------------------|
| ESP32 initiates I2C request    | Sends request to OPT4060 for RGBW data               |
| OPT4060 responds               | Sends RGBW data back to ESP32                         |
| ESP32 calculates line position| Converts RGBW data to a signed line_position value  |
| ESP32 sends UART messages      | Sends processed data to:                              |
|                                | → Motor Controller (0x03)                           |
|                                | → OLED Display (0x04)                               |
|                                | → WiFi Module (0x05)                                |

---

## Data Type 

- line position uses int8_t to represent both left and right drift (negative and positive).
- Message framing (start, end, sender, receiver) uses uint8_t for byte consistency.
- Sensor RGBW data is transmitted as raw uint8_t or uint16_t values.

---
## **Code Zip File**
[Download Zip File](images/API_DD_SUBSYSTEM.zip)




## Conclusion

The API defines how the Sensor Subsystem communicates with other modules using I2C and UART. It supports signed line_position values, subsystem-specific addressing, and robust message formatting
