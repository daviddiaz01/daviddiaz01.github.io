# Component Selection for Sensor Subsystem

## Introduction
This document outlines the selection of major components for the sensor subsystem in the line-following robot. Each component is chosen based on its performance, compatibility, and integration with the ESP32-S3 microcontroller.

## Major Components

| **Component Type** | **Component Name** | **Function in the System** |
|--------------------|--------------------|---------------------------|
| **Color Sensor** | OPT4060 RGBW | Detects line contrast and sends data via I2C |
| **Microcontroller** | ESP32-S3-WROOM-1-N4 | Processes sensor data, controls robot functions |
| **Voltage Regulator** | ADPL44002-3.3 | Steps down 12V to **3.3V** to power ESP32 and sensor |
| **Power Supply** | 12V AC-DC Adapter | Provides power to the voltage regulator |
| **USB-to-Serial Adapter** | HiLetgo CP2102 | Required for programming ESP32-S3 via USB |
| **Push Button (RESET)** | Omron B3U Series (SMT) | Connects **EN (RESET) to GND** to restart ESP32 |
| **Push Button (BOOT Mode)** | Omron B3U Series (SMT) | Connects **GPIO0 to GND** to enter boot mode |
| **Switch (Enable ON/OFF)** | Omron B3U Series (SMT) | Controls the **EN pin** to turn the regulator ON/OFF |
| **Input Capacitor (CIN)** | 2.2µF (Ceramic, SMD 0805) | Filters noise and stabilizes **VIN** |
| **Output Capacitor (COUT)** | 2.2µF (Ceramic, SMD 0805) | Stabilizes **VOUT (3.3V output)** |
| **Soft-Start Capacitor (CSS)** | 1nF (Ceramic, SMD 0805) | Controls **soft-start delay** to prevent inrush current |

## Justification of Component Choices
Each of the above components was selected based on key factors such as electrical compatibility, ease of PCB integration, and availability.

### **1. Color Sensor - OPT4060 RGBW**
- Provides accurate RGBW color detection.
- Uses I2C communication, simplifying data transfer.
- Compact and reliable for high-speed line tracking.

### **2. Microcontroller - ESP32-S3-WROOM-1-N4**
- High-performance microcontroller with WiFi and Bluetooth capabilities.
- Supports multiple I2C and UART peripherals for sensor communication.
- Surface-mount package ensures compact PCB design.

### **3. Voltage Regulator - ADPL44002-3.3**
- Steps down **12V to 3.3V** efficiently.
- Low noise and high power stability.
- Requires minimal external components for integration.

### **4. Power Supply - 12V AC-DC Adapter**
- Provides a stable **12V power source**.
- Compatible with voltage regulator and ESP32 power requirements.

### **5. USB-to-Serial Adapter - HiLetgo CP2102**
- Required for flashing firmware to the ESP32-S3.
- Provides stable USB-to-UART communication.

### **6. Surface-Mount Push Buttons - Omron B3U Series**
- Used for **RESET** and **BOOT mode selection**.
- Compact and easy to integrate into the PCB.
- Provides long operational life.

### **7. Capacitors (CIN, COUT, CSS)**
- **CIN (2.2µF, Ceramic, SMD 0805)** - Stabilizes input voltage.
- **COUT (2.2µF, Ceramic, SMD 0805)** - Ensures stable 3.3V output.
- **CSS (1nF, Ceramic, SMD 0805)** - Soft-start functionality.

---

## Conclusion
This component selection ensures that the **sensor subsystem** operates efficiently with the ESP32-S3, providing **accurate color detection** and stable power regulation.

---
### **Next Steps**
1. Research and compare alternative components (Step 2).  
2. Verify PCB footprints and finalize the circuit design.  
3. Begin PCB layout in **KiCad** and prepare for manufacturing.

---

