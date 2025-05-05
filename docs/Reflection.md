# Reflection

## Lessons Learned

1. **Early integration testing is critical.** We learned that testing subsystems together early helps catch pin conflicts, timing issues, and logic errors before they become harder to fix.

2. **Protocol configuration must be precise.** Setting up I²C and UART on the ESP32 taught us that proper configuration (pin mapping, baud rate, pull-ups) is essential for communication to work.

3. **Clear documentation streamlines collaboration.** Creating block diagrams, state machines, and message flowcharts helped each subsystem team align and avoid miscommunication.

4. **Feedback leads to better design.** After receiving critique during our design review, we improved our UART message structure and added clear framing bytes to reduce transmission errors.

5. **PCB layout impacts performance.** We discovered that trace routing, component placement, and proper ground planes matter for both signal reliability and power stability.

6. **Power design is more complex than expected.** Linear regulators were simple to use, but we realized they generate excess heat and are less efficient than switching regulators for higher current loads.

7. **Time management is key.** Hardware takes longer than expected, especially fabrication, soldering, and debugging. Planning buffer time helped us meet deadlines.

8. **Version control improves teamwork.** GitHub helped our team manage parallel development, track changes, and avoid overwriting each other's code.

9. **Subsystem testing is essential.** Testing each module individually allowed us to isolate bugs and ensure each part worked before full integration.

10. **Debugging tools save time.** Using logic analyzers, serial monitors, and test LEDs made it easier to verify communication and hardware behavior during development.

---

## Recommendations for Future Students

1. Start your PCB design and schematic review early to leave time for iteration and manufacturing delays.  
2. Clearly define your UART and I²C message structures as early as possible to avoid major changes later in the semester.  
3. Use GitHub effectively — create issues, commit regularly, and keep your documentation in sync with your implementation.  
4. Don’t underestimate testing — test each subsystem individually and then as a whole system early and often.  
5. Meet as a team regularly, even if progress feels slow, to maintain accountability and catch design problems before they grow.

---

## Version 2.0: Communication Architecture Improvements

If we were to redesign the communication system for Version 2.0 of this project, several improvements could be made to enhance robustness, scalability, and debuggability.

First, we would modularize each communication protocol into separate software tasks or threads. For example, the UART message handling could be isolated from sensor data processing, reducing timing conflicts and increasing reliability. Implementing a queue-based UART buffer would allow us to decouple message sending and receiving from processing logic, making the system more fault-tolerant.

We would also improve the message framing structure by adding checksum or CRC verification for better error detection. This would help prevent corrupted messages from being misinterpreted. Additionally, a universal message parser module could be shared across all subsystems, ensuring consistent decoding and reducing duplicate code.

On the hardware side, we would add labeled test points for UART TX/RX and I²C lines, simplifying debugging and logic analyzer probing during development. Adding visual feedback LEDs to indicate communication status (e.g., TX activity, error states) would also improve field testing.

To increase flexibility, we would switch to a fully addressable protocol that allows dynamic sender/receiver assignment, enabling future support for expanding the system with additional sensors or controllers. Lastly, abstracting message types into an enum structure and separating message payloads from headers would allow easier upgrades in future firmware releases.

Together, these changes would make the communication architecture more maintainable, modular, and scalable for future iterations or more complex systems.
