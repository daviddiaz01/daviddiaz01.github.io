# Sensor Subsystem API Documentation

## Introduction
The Sensor Subsystem is responsible for detecting line contrast using the OPT4060 RGBW Color Sensor and transmitting this data to the ESP32-S3-WROOM-1-N4 via I²C. The ESP32 processes the sensor data and forwards it over UART for further use in the system.

This document outlines:
- The message structure for sending and receiving sensor data.
- The data format ensuring compliance with the UART daisy chain protocol.
- The handling of malformed messages and acknowledgment processes.

---

## **Message Structure**

### **ESP32 Requests Sensor Data**
| **Byte** | **Variable Name** | **Data Type** | **Example Value** |
|---------|-----------------|-------------|----------------|
| Byte 1  | Message Type    | `uint8_t`   | `0x10` |
| Byte 2  | Source ID       | `uint8_t`   | `0x01` (ESP32) |
| Byte 3  | End Byte        | `uint8_t`   | `0x42` |

**Purpose:**  
- The ESP32 sends this request to obtain real-time color sensor data from the OPT4060.

---

### **Sensor Subsystem Sends Data to ESP32**
| **Byte** | **Variable Name** | **Data Type** | **Example Value** |
|---------|-----------------|-------------|----------------|
| Byte 1  | Message Type    | `uint8_t`   | `0x20` |
| Byte 2  | Source ID       | `uint8_t`   | `0x02` (Sensor) |
| Byte 3  | Color Value (LSB) | `uint8_t` | `0x7A` |
| Byte 4  | Color Value (MSB) | `uint8_t` | `0x00` |
| Byte 5  | Checksum        | `uint8_t`   | `0xA1` |
| Byte 6  | End Byte        | `uint8_t`   | `0x42` |

**Purpose:**  
- The OPT4060 Color Sensor** transmits processed RGBW values to the ESP32.
- The checksum ensures data integrity.

---

## **Sensor Subsystem Communication Flow**

1. **ESP32 Requests Data**
   - Sends an I²C request to the OPT4060 sensor.

2. **Sensor Responds to ESP32**
   - The OPT4060 transmits processed color values.

3. **ESP32 Processes Sensor Data**
   - The ESP32 interprets the sensor values for line contrast detection.

4. **ESP32 Forwards Data**
   - The ESP32 sends the processed sensor values to the next stage in the system via UART.

---

## **Conclusion**
This API documentation ensures that the Sensor Subsystem follows the I²C and UART communication protocols and integrates properly with the ESP32. The structured message format** and error handling mechanisms guarantee accurate and reliable data transmission.
