# Sensor Subsystem API Documentation

## Introduction
The sensor subsystem uses the OPT4060 RGBW Color Sensor to detect line contrast and transmit line position data to the ESP32-S3-WROOM-1-N4 via I2C. The ESP32 processes this data and sends the result to the Motor Controller, OLED Display, and WiFi Module using the UART daisy chain protocol.

---

## Message Overview

| **Message**       | **Variable Name** | **Variable Type** | **Min** | **Max** | **Example** |
|-------------------|-------------------|--------------------|--------|--------|-------------|
| Line Position     | `line_position`   | `int8_t`           | -100   | 100    | 25          |

---

## Line Position - Transmit Format

This message is sent from the ESP32 to each destination subsystem.

### To Motor Controller (0x03)
| Byte 1 | Byte 2 | Byte 3 | Byte 4     | Byte 5 |
|--------|--------|--------|------------|--------|
| Start  | Sender | Receiver | Line Position | End    |
| 0x41   | 0x01   | 0x03     | `int8_t`      | 0x42   |

### To OLED Display (0x04)
| Byte 1 | Byte 2 | Byte 3 | Byte 4     | Byte 5 |
|--------|--------|--------|------------|--------|
| Start  | Sender | Receiver | Line Position | End    |
| 0x41   | 0x01   | 0x04     | `int8_t`      | 0x42   |

### To WiFi Module (0x05)
| Byte 1 | Byte 2 | Byte 3 | Byte 4     | Byte 5 |
|--------|--------|--------|------------|--------|
| Start  | Sender | Receiver | Line Position | End    |
| 0x41   | 0x01   | 0x05     | `int8_t`      | 0x42   |

---

## Sensor Subsystem Communication Flow

| **Process Stage**                       | **Description** |
|----------------------------------------|----------------|
| ESP32 requests data from OPT4060       | Sends an I²C request to the color sensor. |
| OPT4060 responds                        | Reads RGBW values and sends processed data. |
| ESP32 extracts line position           | Processes RGBW values and calculates the signed line position. |
| ESP32 sends processed data             | Sends line position via UART to: |
| Motor Controller (0x03)              | Adjusts motor speed if the line position shifts. |
| OLED Display (0x04)                  | Displays real-time feedback ("Line Centered", "Drifting Left"). |
| WiFi Module (0x05)                   | Logs the sensor readings for remote tracking. |

---


## Conclusion
This API ensures the sensor subsystem communicates accurately using the OPT4060 color sensor via I²C and distributes the signed line position data via UART to the rest of the system. The message structures and formatting match team-wide expectations, allowing the WiFi, HMI, and motor subsystems to receive, log, or act on real-time sensor data reliably.
