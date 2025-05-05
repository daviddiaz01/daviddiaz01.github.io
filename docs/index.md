# Welcome to My Page  
David Diaz – EGR 314

## About the Project  
This project centers on the development of an autonomous line-following robot that uses sensor feedback to detect and follow a designated path. The robot integrates multiple subsystems working together to handle sensing, motor control, power regulation, and communication.

As part of this team project, I am responsible for designing and implementing the Sensor Subsystem, which plays a critical role in detecting the line and guiding the robot's movement. This subsystem uses a color sensor to detect contrast on the ground and sends real-time data to the ESP32-S3 microcontroller, which processes the information and communicates with the motor controller.

This site documents my individual contributions to the team project and includes my final block diagram, component selection, schematic, PCB design, communication API, and code resources.

## Key Objectives  
- Autonomous navigation – enable real-time path-following based on sensor input  
- Accurate sensor processing – utilize the ESP32-S3 to reliably read and process contrast data  
- Stable power management – ensure proper voltage regulation for the sensor and microcontroller  

## Project Subsystems  
| Subsystem           | Function                                             |
|---------------------|------------------------------------------------------|
| Sensor Subsystem    | Detects line contrast and sends data to MCU         |
| Motor Control       | Drives wheels based on processed sensor input       |
| Power Management    | Regulates voltages for all hardware components      |
| Communication       | Allows serial/Bluetooth interaction for debugging   |

## Development Process  
- Component selection – chose components based on accuracy, speed, and compatibility  
- Circuit design & PCB layout – integrated all parts into a compact and functional PCB design  
- Firmware development – programmed the ESP32-S3 to handle sensor communication over I²C and send UART messages  
- Testing & optimization – validated sensor performance in various lighting and surface conditions

