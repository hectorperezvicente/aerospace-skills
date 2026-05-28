---
name: amateur-rocketry
description: "Specialized skill for amateur/student rocketry software development with focus on safety standards, telemetry, and flight software patterns. Use when developing embedded systems for rockets, payloads, or ground support equipment in educational or amateur contexts."
triggers:
  - embedded rocketry
  - STM32 telemetry
  - rocket programming
  - flight software
  - payload development
  - amateur rocketry
  - student rocketry
  - high-power rocketry
  - model rocketry
  - IREC
  - ESRA
  - spaceport america cup
license: MIT
metadata:
  author: hectorperezvicente
  version: "1.0"
  license: MIT
---

## Activation Triggers

This skill activates when working on amateur or student rocketry projects involving:
- Embedded systems for flight computers or payload controllers
- Telemetry systems (LoRa, RF, GPS)
- State machines for flight phases (boost, coast, apogee, descent, recovery)
- Safety-critical code requiring adherence to aerospace standards
- Competition documentation (IREC, ESRA, Spaceport America Cup)
- Ground support equipment and launch procedures
- Hardware-in-the-loop testing and flight simulation

## Hard Rules (NASA Power of 10 Subset + MISRA Essentials + BARR-C)

### NASA Power of 10 Subset (Critical 3)
1. **No unrestricted goto statements** - Use structured control flow only
2. **No unbounded recursion** - All recursion must have provable bounds
3. **All loops must have fixed bounds** - Loop iterations must be determinable at compile time

### MISRA C:2012 Essentials (Adapted for Safety)
- **Rule 10.1**: Avoid implicit conversions that change signedness
- **Rule 11.4**: Do not perform pointer arithmetic on void pointers
- **Rule 13.5**: Avoid assignment in Boolean context

### BARR-C Guidelines (Embedded Systems)
- **Static globals prohibited** - All data must be passed explicitly
- **Enumerated types for state machines** - Use enum for flight states
- **Constants for magic numbers** - No unexplained literals
- **Return status from all functions** - Every function indicates success/failure
- **Compiler warnings treated as errors** - Build fails on any warning

## Decision Gates Table

| Decision Point | Options | Guidance |
|----------------|---------|----------|
| Microcontroller Platform | STM32, ESP32, Teensy, Custom | Consider team experience, toolchain availability, community support |
| Telemetry Protocol | LoRa, RFM69, WiFi, Cellular | Evaluate range requirements, power constraints, regulatory limits |
| Testing Approach | Unity only, Unity+CMock, Ceedling, Custom | Match to team size and CI/CD capabilities |
| State Machine Implementation | Switch-case, Function pointers, State pattern | Balance readability with extensibility needs |
| Build System | Makefile, CMake, PlatformIO, Arduino IDE | Consider team familiarity and debugging capabilities |

## 5-Step Execution Workflow

### 1. Explore
- Understand mission requirements and constraints
- Identify applicable safety standards (NASA Power of 10, MISRA, BARR-C)
- Define flight software architecture boundaries
- Determine hardware interfaces and communication protocols
- Document assumptions and risk areas

### 2. Specify
- Write detailed requirements with acceptance criteria
- Define telemetry packet structures and protocols
- Specify flight state machine with entry/exit conditions
- Create test plans covering unit, integration, HIL, and flight simulation
- Document safety-critical sections requiring extra verification

### 3. Implement
- Follow hard rules strictly (NASA Power of 10 subset + MISRA essentials + BARR-C)
- Use defensive programming with comprehensive error checking
- Implement hardware abstraction layers for portability
- Apply static analysis tools and treat warnings as errors
- Ensure all functions have clear contracts and return status codes

### 4. Verify
- Execute unit tests with mock hardware abstractions
- Conduct integration testing with actual hardware
- Perform hardware-in-the-loop (HIL) simulation
- Validate flight software in simulated flight conditions
- Review code for compliance with all hard rules

### 5. Archive
- Document lessons learned and flight data analysis
- Update reference materials with new findings
- Preserve test evidence and validation records
- Prepare documentation for competition review (IREC/ESRA)
- Archive configuration and build scripts for reproducibility
