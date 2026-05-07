# Arduino Bluetooth Robotic Car

**Arduino/C++ firmware for a Bluetooth-controlled robotic car with independent motor speed control, serial command parsing, ultrasonic obstacle detection, and servo-based distance scanning.**

This project demonstrates embedded firmware development for a small robotic vehicle platform. The firmware controls a dual-motor drive system, receives Bluetooth commands over serial, parses lightweight JSON-style control messages, applies PWM speed control, and includes an autonomous obstacle-avoidance mode using an ultrasonic sensor mounted on a servo.

The original build was created as a practical embedded systems project: it combines real hardware, motor control, sensor input, serial communication, timing logic, and safety behavior into one working firmware application.

![PXL_20250519_012057705](https://github.com/user-attachments/assets/a7bd9953-cd6c-4ee4-ac14-45c4a5843ea4)

---

## Why This Project Matters

This repository is more than a simple Arduino car. It shows hands-on embedded development skills that are directly relevant to firmware, robotics, IoT, and hardware/software integration roles:

- Low-level GPIO control
- PWM-based motor speed control
- Serial communication over Bluetooth
- Command parsing on a memory-constrained microcontroller
- Servo control
- Ultrasonic distance sensing
- Autonomous obstacle-avoidance logic
- Event-driven firmware behavior
- Timing-based command timeout protection
- Hardware debugging and integration

The project is a strong example of building firmware that interacts with physical systems, responds to real-time input, and controls electromechanical hardware.

---

## Project Overview

The robotic car supports two major operating concepts:

1. **Bluetooth remote control**
   - Receives commands over serial at 9600 baud.
   - Parses simple JSON-style command strings from a Bluetooth controller.
   - Maps received commands to movement actions.
   - Supports forward, reverse, left, and right motion.
   - Uses PWM to control motor speed.

2. **Autonomous obstacle avoidance**
   - Uses an ultrasonic distance sensor to detect obstacles.
   - Uses a servo motor to rotate the sensor and scan left, center, and right.
   - Compares measured distances to choose a safer direction.
   - Stops, reverses, or turns when an obstacle is detected.
   - Continues forward when the path is clear.

---

## Hardware Features

The project is designed around a small Arduino-based robotic car platform with:

- Arduino-compatible microcontroller
- Bluetooth serial module
- Dual motor driver
- Four motor control outputs
- PWM speed enable lines
- Ultrasonic distance sensor
- Servo motor for sensor rotation
- On-board LED feedback

---

## Firmware Features

### Motor Control

The firmware controls the drive system using digital direction pins and PWM enable pins. It includes dedicated movement functions for:

- `forward()`
- `back()`
- `left()`
- `right()`
- `stop()`

Each movement function updates the appropriate motor-driver pins and applies speed through PWM.

### Independent Speed Control

The firmware defines different speed profiles for different modes:

- Bluetooth-controlled driving speed
- Autonomous obstacle-avoidance speed
- Turning speed

This makes the car easier to tune for manual control versus autonomous movement.

### Bluetooth Serial Input

Bluetooth commands are received through the Arduino serial port at `9600` baud. The firmware listens for structured command input and builds a command string character by character.

This demonstrates:

- Serial communication
- Command framing
- Lightweight parsing
- Input validation
- Embedded command dispatching

### JSON-Style Command Parsing

The firmware parses simple command messages using lightweight string handling rather than depending on a heavy JSON library. This is appropriate for small embedded systems where RAM and flash usage matter.

The parser extracts command/value pairs and maps them to movement behavior.

### Timed Command Safety

The firmware includes a movement timeout behavior so a received movement command does not continue forever. After a command has been active for a defined time window, the car automatically stops.

This is an important embedded safety pattern because wireless control links can drop, packets can be missed, and a robot should not continue moving indefinitely after losing command input.

### Ultrasonic Distance Measurement

The project includes ultrasonic distance measurement using trigger and echo pins. The firmware sends a trigger pulse, measures echo duration, and converts that reading into distance.

### Servo-Based Scanning

A servo motor rotates the ultrasonic sensor to scan multiple directions:

- Center
- Right
- Left

The firmware compares distances and chooses a movement direction based on the clearest path.

### Obstacle Avoidance

When an obstacle is detected within the configured threshold, the car stops and scans nearby directions. Based on measured distance, it decides whether to turn left, turn right, reverse, or continue forward.

This gives the project a simple autonomous robotics behavior loop.

---

## Technical Skills Demonstrated

This repository highlights several embedded/firmware engineering skills:

- Arduino C++ firmware development
- GPIO configuration and control
- PWM output for motor speed
- Motor driver integration
- Servo control using Arduino libraries
- Ultrasonic sensor timing
- Bluetooth serial communication
- Lightweight protocol parsing
- Real-world hardware integration
- State-based movement control
- Safety timeout implementation
- Robotics behavior logic
- Debugging physical hardware through firmware

---

## Example Use Cases

This project can be used as a foundation for:

- Bluetooth-controlled robotics
- Introductory autonomous vehicles
- STEM robotics demonstrations
- Motor-control firmware experiments
- Obstacle-avoidance prototypes
- Embedded serial communication demos
- Robotics portfolio projects

---

## Repository Structure

```text
arduino_bluetooth_car/
├── arduino_bluetooth_car/
│   └── arduino_bluetooth_car.ino
├── README.md
└── LICENSE
```

---

## Main Firmware File

The main firmware is located at:

```text
arduino_bluetooth_car/arduino_bluetooth_car.ino
```

This file contains the motor control logic, Bluetooth command parser, ultrasonic distance function, servo scanning behavior, and autonomous obstacle-avoidance loop.

---

## Pin Overview

The firmware defines pins for:

| Function | Pin |
|---|---:|
| Servo | 3 |
| Motor Enable A | 5 |
| Motor Enable B | 6 |
| Motor Input 1 | 7 |
| Motor Input 2 | 8 |
| Motor Input 3 | 9 |
| Motor Input 4 | 11 |
| LED | 13 |
| Ultrasonic Echo | A4 |
| Ultrasonic Trigger | A5 |

---

## Movement Commands

The firmware maps parsed Bluetooth command values to movement actions:

| Action | Behavior |
|---|---|
| Forward | Drives both motors forward |
| Back | Reverses both motors |
| Left | Turns left using opposing motor directions |
| Right | Turns right using opposing motor directions |
| Stop | Disables motor enable outputs |

---

## Embedded Design Notes

Several design choices make this project useful as a firmware portfolio example:

### Lightweight Parsing

Instead of using a large parsing dependency, the firmware manually parses the small command format expected from the Bluetooth controller. This shows awareness of embedded resource limits.

### Hardware Abstraction Through Functions

Movement behavior is organized into dedicated functions. This keeps motor-driver logic easier to test, tune, and extend.

### Mode-Based Speed Selection

The firmware separates Bluetooth speed from autonomous avoidance speed. This allows different tuning for manual and automatic operation.

### Built-In Safety Timeout

The command timeout prevents the robot from continuing to move forever if the Bluetooth controller stops sending commands.

### Sensor-Driven Autonomy

The ultrasonic and servo logic adds real-world feedback, allowing the robot to make movement decisions based on physical surroundings.

---

## How to Run

1. Open the project in the Arduino IDE.
2. Connect the Arduino-compatible board.
3. Confirm the motor driver, Bluetooth module, servo, and ultrasonic sensor are wired to the expected pins.
4. Select the correct board and port.
5. Upload `arduino_bluetooth_car.ino`.
6. Pair with the Bluetooth module.
7. Send compatible movement commands from a Bluetooth controller app.

---

## Future Improvements

Potential improvements for this project include:

- Replace manual string parsing with a small command protocol state machine.
- Add checksum or message validation for Bluetooth commands.
- Add configurable command timeout duration.
- Add acceleration ramping for smoother motor startup.
- Add battery voltage monitoring.
- Add telemetry reporting over Bluetooth.
- Add calibration constants for motor balance.
- Add a manual/autonomous mode switch command.
- Add structured logging for debug builds.
- Add a wiring diagram and schematic.

---

![PXL_20250519_012120126](https://github.com/user-attachments/assets/413c32bd-ec5d-49b7-880d-9b533699f507)
![PXL_20250519_012132951](https://github.com/user-attachments/assets/ef0ddb45-f065-4a41-b164-08b7ec630c0f)
![PXL_20250519_012153578](https://github.com/user-attachments/assets/cd424184-bfe8-4095-bf92-b64982fe9fe3)

---

## License

This project is licensed under the GPL-3.0 license.
