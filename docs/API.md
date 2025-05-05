# API

## Introduction  
The Sensor Subsystem uses the OPT4060 RGBW Color Sensor to detect line contrast for a line-following robot. This sensor communicates with the ESP32-S3-WROOM-1-N4 over I²C. The ESP32 processes the RGBW data and calculates a signed `line_position` value, which it transmits via UART to other subsystems, including:

- Motor Controller (0x03)  
- OLED Display (0x04)  
- WiFi Module (0x05)  

This API outlines the internal I²C communication protocol, UART message structure, and protocol compliance.

This subsystem was fully tested and confirmed to work exactly as intended. All messages are reliably transmitted and received, and the calculated `line_position` values enable accurate path tracking during operation.

---

## Message Overview  
| Message         | Variable Name | Type   | Min   | Max   | Example |
|------------------|----------------|--------|--------|--------|---------|
| Line Position    | line_position  | int8_t | -100  | 100   | 25      |

---

## Internal I²C Communication  

### ESP32 Requests Sensor Data from OPT4060  
| Byte | Description     | Type     | Value  |
|------|------------------|----------|--------|
| 1    | I²C Command      | uint8_t  | 0x10   |
| 2    | Source ID (ESP32) | uint8_t | 0x01   |
| 3    | Destination (Sensor) | uint8_t | 0x02 |
| 4    | End Byte         | uint8_t  | 0x42   |

### OPT4060 Sends RGBW Data to ESP32  
| Byte | Description     | Type     | Value  |
|------|------------------|----------|--------|
| 1    | Message Type     | uint8_t  | 0x20   |
| 2    | Source ID (Sensor) | uint8_t | 0x02  |
| 3    | Color LSB        | uint8_t  | 0x7A   |
| 4    | Color MSB        | uint8_t  | 0x00   |
| 5    | Checksum         | uint8_t  | 0xA1   |
| 6    | End Byte         | uint8_t  | 0x42   |

---

## ESP32 Internal Processing  
The ESP32 extracts and processes the RGBW data, mapping it to an `int8_t` `line_position` value between -100 and 100.

- Negative values: Indicates drift to the left  
- Positive values: Indicates drift to the right  

This processed value is forwarded to the appropriate subsystems.

---

## UART Message Format – ESP32 Sends Processed Data  

### To Motor Controller (0x03)  
| Byte | Description   | Type    | Example |
|------|----------------|---------|---------|
| 1    | Start Byte     | uint8_t | 0x41    |
| 2    | Sender ID      | uint8_t | 0x01    |
| 3    | Receiver ID    | uint8_t | 0x03    |
| 4    | Line Position  | int8_t  | 25      |
| 5    | End Byte       | uint8_t | 0x42    |

### To OLED Display (0x04)  
| Byte | Description   | Type    | Example |
|------|----------------|---------|---------|
| 1    | Start Byte     | uint8_t | 0x41    |
| 2    | Sender ID      | uint8_t | 0x01    |
| 3    | Receiver ID    | uint8_t | 0x04    |
| 4    | Line Position  | int8_t  | 25      |
| 5    | End Byte       | uint8_t | 0x42    |

### To WiFi Module (0x05)  
| Byte | Description   | Type    | Example |
|------|----------------|---------|---------|
| 1    | Start Byte     | uint8_t | 0x41    |
| 2    | Sender ID      | uint8_t | 0x01    |
| 3    | Receiver ID    | uint8_t | 0x05    |
| 4    | Line Position  | int8_t  | 25      |
| 5    | End Byte       | uint8_t | 0x42    |

---

## Communication Flow  
| Stage                         | Description                                 |
|-------------------------------|---------------------------------------------|
| ESP32 initiates I²C request   | Sends command to OPT4060 for RGBW data      |
| OPT4060 responds              | Returns RGBW data over I²C                  |
| ESP32 calculates line position | Converts RGBW into a signed value (±100)   |
| ESP32 sends UART messages     | Sends result to Motor, Display, and WiFi subsystems |

---

## Data Type Summary  
- `line_position` is a signed 8-bit integer (`int8_t`)  
- Message framing bytes (`start`, `end`, `sender`, `receiver`) use `uint8_t`  
- RGBW sensor data is transferred as raw `uint8_t` or `uint16_t` values  

---
## Code Zip File
[Download Zip File](images/API-SENSOR-DD-main.zip)

---

## Source Code Repository

You can view my full Sensor Subsystem API implementation code:

[https://github.com/daviddiaz01/API-SENSOR-DD.git](https://github.com/daviddiaz01/API-SENSOR-DD.git)




## Conclusion  
This API defines how the Sensor Subsystem communicates with other modules using both I²C and UART. It ensures reliable signed `line_position` transmission, correct subsystem addressing, and compliance with the course messaging protocol.  

All communications and functionality were tested thoroughly. The subsystem performs consistently and as intended, with all messages reaching their destinations accurately and on time.
