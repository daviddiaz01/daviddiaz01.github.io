# Sensor Subsystem API Documentation

## Introduction
The Sensor Subsystem detects line contrast using the OPT4060 RGBW Color Sensor and communicates data via I²C to the ESP32-S3-WROOM-1-N4 microcontroller. The ESP32 processes the sensor data and transmits it over UART for integration into the system.

---

## **Message Structure**

### **General Message Format**
| **Byte** | **Description**       | **Example Value** |
|---------|----------------------|----------------|
| Byte 1  | Message Type          | `0x10` |
| Byte 2  | Source ID             | `0x01` (ESP32) |
| Byte 3  | Destination ID        | `0x02` (Sensor) |
| Byte 4  | Sensor Data (LSB)     | `0x7A` |
| Byte 5  | Sensor Data (MSB)     | `0x00` |
| Byte 6  | End Byte              | `0x42` |

---

### **ESP32 Requests Sensor Data**
| **Byte** | **Variable Name** | **Data Type** | **Value** |
|---------|-----------------|-------------|----------|
| Byte 1  | Message Type    | `uint8_t`   | `0x10`  |
| Byte 2  | Source ID       | `uint8_t`   | `0x01`  |
| Byte 3  | End Byte        | `uint8_t`   | `0x42`  |

**Purpose:**  
The ESP32 sends this request to obtain real-time color sensor data over I²C from the OPT4060 sensor.

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
The OPT4060 Color Sensor transmits processed RGBW values to the ESP32.

---

### **ESP32 Sends Processed Data to Motor, OLED, and WiFi**
| **Byte** | **Variable Name** | **Data Type** | **Example Value** |
|---------|-----------------|-------------|----------------|
| Byte 1  | Message Type    | `uint8_t`   | `0x30` |
| Byte 2  | Source ID       | `uint8_t`   | `0x01` (ESP32) |
| Byte 3  | Destination ID (Motor Controller) | `uint8_t` | `0x03` |
| Byte 4  | Destination ID (OLED Display) | `uint8_t` | `0x04` |
| Byte 5  | Destination ID (WiFi Module) | `uint8_t` | `0x05` |
| Byte 6  | Sensor Data (LSB) | `uint8_t` | `0x7A` |
| Byte 7  | Sensor Data (MSB) | `uint8_t` | `0x00` |
| Byte 8  | End Byte        | `uint8_t`   | `0x42` |

**Purpose:**  
The ESP32 processes the sensor data and distributes it to multiple components for system integration. It sends the processed data to the motor controller to adjust speed based on the detected line position, ensuring precise movement and real-time corrections. The OLED display receives the same data to provide visual feedback, allowing users to monitor sensor readings and system status. Additionally, the WiFi module logs the transmitted data for remote monitoring, enabling real-time tracking and analysis of the sensor's performance.

---

## **Sensor API Communication Flow**

| **Process Stage**                          | **Description** |
|------------------------------------|----------------|
| **ESP32 Requests Data from the Sensor** | Sends an I²C request to the OPT4060. |
| **OPT4060 Sensor Processes the Request** | Reads current color contrast values and encodes the RGBW sensor data into the message format. |
| **OPT4060 Sends Data to ESP32** | Responds with processed color values and ensures checksum integrity before sending. |
| **ESP32 Receives and Interprets the Data** | Extracts color sensor readings, checks message validity (checksum, size), and ignores malformed messages or requests retransmission if needed. |
| **ESP32 Sends Processed Data via UART** | Sends processed data to the following components: |
| **Motor Controller (0x03)** | Adjusts motor speed if the line position shifts. |
| **OLED Display (0x04)** | Displays real-time feedback ("Line Centered", "Drifting Left"). |
| **WiFi Module (0x05)** | Logs the sensor readings for remote tracking. |


---

## **Conclusion**
This documentation ensures the sensor subsystem follows I²C and UART daisy chain protocols, enabling seamless communication. The structured message format allows accurate retrieval of color data from the OPT4060, which the ESP32 processes and transmits to the motor controller, OLED display, and WiFi module. Error-handling mechanisms, such as checksum verification and retransmission, ensure reliable data integrity. This API provides a clear and efficient method for integrating the sensor subsystem into the overall system.
