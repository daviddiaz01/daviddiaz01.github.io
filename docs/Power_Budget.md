# Power Budget

## **Power Budget Overview**

This table provides a power budget analysis for the key components in the schematic, ensuring the system meets its power requirements efficiently.


### **Component Breakdown**
| **Component**        | **Image** | **Voltage (V)** | **Current (mA)** | **Power (mW)** |
|----------------------|----------|---------------|----------------|-------------|
| **ESP32-S3-WROOM**  | ![ESP32](images/ESP32.png) | 3.3V | 240mA | **792mW** |
| **LM2575D2T-3.3 Regulator** | ![Regulator](images/Regulator.png) | 9V Input | 500mA | **4500mW** (Input side) |
| **OPT4060 Color Sensor** | ![Color Sensor](images/Color_Sensornew.png) | 3.3V | 3mA | **9.9mW** |
| **USB Connector (5V, 500mA max)** | ![USB](images/USB.png) | 5V | 500mA | **2500mW (2.5W)** |
| **Barrel Jack Adapter** | ![Barrel Jack](images/Barrel_Jack.png) | 9V | 1A | **9000mW** |
| **Power Switch** | ![Switch](images/Switch.png) | 9V | - | **-** |
| **Power Adapter** | ![Adapter](images/Adapter.png) | 9V | - | **-** |
---

### **Total Estimated Power Consumption**
- **Total power draw at 3.3V:** **~850mW**
- **Total power draw at 9V input:** **~1W** (considering regulator efficiency)
- **Peak power consumption via USB (5V):** **~2.5W (500mA max)**

#### **Regulator Efficiency Considerations**
The **LM2575D2T-3.3** is a **switching regulator**, meaning it has higher efficiency (~80-90%).
- **Estimated output power:** **850mW**
- **Estimated input power (@ 9V):**  
