# Schematic 

## Schematic Diagram
![Schematic](images/SCEMATIC_FINAL_PCB.jpg)


## **PDF Link**  
[View Schematic (PDF)](images/SCEMATIC_FINAL_PCB.pdf)

## **PCB Project Zip File**
[Download Zip File](images/SensorSubsystemDD.zip)

## **Project Code Repository**    
[View Project Repository](https://github.com/daviddiaz01/API-SENSOR-DD.git)

### Functional Overview

This schematic and PCB layout provide power regulation, sensor interfacing, and communication between the ESP32-S3 and other subsystems. The 12V input is stepped down to 5V and 3.3V using linear regulators. The OPT4060 sensor connects to the ESP32 via I²C (IO18/IO19), and the ESP32 sends processed line position data via UART to the Motor, Display, and WiFi subsystems. GPIO 15 drives a debug LED for visual feedback during testing.

The circuit was fabricated, assembled, and tested. All components functioned as intended, and the PCB operated correctly under all expected conditions.

---

### Design Considerations

The schematic was organized to minimize routing complexity and isolate analog/digital sections for signal integrity. Decoupling capacitors were placed close to power pins, and ground planes were used to reduce noise. The PCB was laid out compactly to fit within project constraints while maintaining clear silkscreen labeling for ease of assembly and debugging.

---

### Version 2.0 Hardware Improvements

If I were to design a Version 2.0 of the Sensor Subsystem, I would consider the following improvements:

1. **Add onboard I²C pull-up resistors** to improve signal integrity and reduce dependence on external components.
2. **Include test points for I²C and UART lines** to allow easier in-circuit debugging and probing.
3. **Shrink board size further** by using more compact packages or rearranging component placement.
4. **Integrate power indicator LEDs** to visually confirm when 3.3V and 5V rails are active.

These changes would enhance power stability, simplify debugging, and improve overall robustness and maintainability of the hardware.
