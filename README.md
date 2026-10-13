
# Secure MAVLink — Drone Command Authentication & Intrusion Detection

A Python-based cybersecurity project exploring how to secure drone communications against unauthorized commands using MAVLink 2 message signing and intrusion detection.

This project uses a fully simulated drone powered by ArduPilot SITL (Software In The Loop). No physical drone or hardware is required.

## Project Overview

MAVLink is a communication protocol commonly used by drones and autonomous vehicles. Without message authentication, an attacker with access to an unsecured MAVLink communication channel may be able to send unauthorized commands.

The goal of this project is to demonstrate this vulnerability in a controlled simulation, implement authenticated communication, and develop a monitoring system that detects suspicious activity.

## Objectives

- Simulate a drone using ArduPilot SITL.
- Control the simulated drone using Python and `pymavlink`.
- Demonstrate unauthorized command injection in an isolated environment.
- Implement MAVLink 2 message signing to authenticate commands.
- Detect unsigned, replayed, and unexpected command traffic.
- Log suspicious activity for cybersecurity analysis.

## Tech Stack

| Technology | Purpose |
|------------|---------|
| Python | Drone control and security monitoring |
| pymavlink | MAVLink communication |
| MAVLink 2 | Communication protocol and message signing |
| ArduPilot SITL | Drone flight simulation |
| MAVProxy | Telemetry routing and monitoring |
| Linux / Ubuntu | Development environment |
| Git & GitHub | Version control and documentation |

## Getting Started

### 1. Clone ArduPilot

```bash
git clone --recurse-submodules https://github.com/ArduPilot/ardupilot.git
cd ardupilot
```

### 2. Install Dependencies

On Ubuntu:

```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
pip install pymavlink
```

### 3. Launch the Simulator

```bash
cd ~/ardupilot/ArduCopter
sim_vehicle.py -v ArduCopter --console --map
```

Wait for the simulator to initialize before sending flight commands.

### 4. Connect Using Python

Connect a Python script to the simulator's MAVLink telemetry endpoint.

Example:

```python
from pymavlink import mavutil

master = mavutil.mavlink_connection("udpin:0.0.0.0:14550")

master.wait_heartbeat()

print("Connected to simulated drone")
print(f"System ID: {master.target_system}")
print(f"Component ID: {master.target_component}")
```

Once connected, the drone can receive commands and transmit telemetry such as GPS position, altitude, and battery status.

## Development Roadmap

### Phase 1 — Establish Communication

- [ ] Install and configure ArduPilot SITL.
- [ ] Launch the simulated drone.
- [ ] Establish communication using pymavlink.
- [ ] Retrieve GPS, altitude, and battery telemetry.
- [ ] Arm, take off, and land through Python.

### Phase 2 — Simulate Unauthorized Commands

- [ ] Create a second Python client representing an unauthorized operator.
- [ ] Demonstrate command injection on an isolated test link.
- [ ] Observe and record the simulator's response.
- [ ] Document the security vulnerability.

### Phase 3 — Implement Authentication

- [ ] Configure MAVLink 2 message signing.
- [ ] Establish a shared secret signing key.
- [ ] Authenticate authorized commands.
- [ ] Test rejection of unsigned or invalidly signed commands.
- [ ] Compare behavior before and after enabling signing.

### Phase 4 — Intrusion Detection

- [ ] Develop a Python-based security monitor.
- [ ] Flag unsigned MAVLink traffic.
- [ ] Detect replay attempts where observable.
- [ ] Identify unexpected command sources.
- [ ] Generate timestamped security logs.
- [ ] Create alerts for suspicious activity.

### Phase 5 — Testing & Documentation

- [ ] Run authorized and unauthorized command scenarios.
- [ ] Evaluate authentication and detection effectiveness.
- [ ] Document test results and limitations.
- [ ] Publish findings and architecture diagrams.

## Security Considerations

MAVLink 2 message signing provides message authentication and integrity verification, but it does not encrypt telemetry or commands.

Signing also does not, by itself, provide fine-grained user authorization. Additional controls may be needed to distinguish trusted operators and protect against compromised signing keys.

This project focuses on message authentication, unauthorized command rejection, and intrusion detection rather than full end-to-end communication security.

## Initial Security Observation

The first experiment investigates whether a Python client connected to an unsigned MAVLink telemetry link can arm and control the simulated drone without authenticating.

**Status:** Pending experimental verification.

This baseline will be compared with the behavior observed after MAVLink 2 signing is enabled.

## Project Scope

All experiments are conducted in a controlled simulation environment using ArduPilot SITL.

No real drones, third-party vehicles, or external communication networks are targeted.

## Future Improvements

- Real-time security dashboard.
- Automated suspicious-command alerts.
- Structured security event reporting.
- Encrypted communication transport.
- Integration with ArduPilot Rover for autonomous ground vehicle security testing.

## Project Status

**In Development — Phase 1: Environment Setup & Basic Drone Communication**

## License

License to be determined.
