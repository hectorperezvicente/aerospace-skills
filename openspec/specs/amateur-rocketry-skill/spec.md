# Amateur Rocketry Skill Specification

## Purpose

This specification defines the amateur/student rocketry software skill that provides methodology-focused guidance for safety-critical embedded software development in rocketry projects. The skill enforces coding standards, code review checklists, testing strategies, and competition deliverable templates, teaching the HOW (process) rather than the WHAT (domain knowledge).

## Requirements

### Requirement: Safety-Critical Code Enforcement

The system MUST enforce a subset of NASA Power of 10 rules, MISRA C essentials, and BARR-C guidelines for embedded flight software development.

#### Scenario: NASA Power of 10 Compliance Check
- GIVEN a C source file for flight software
- WHEN the code is analyzed
- THEN the system SHALL flag violations of restricted goto statements, unbounded recursion, and loops without static upper bounds

#### Scenario: MISRA C Essential Rules Application
- GIVEN a C source file being analyzed
- WHEN implicit type conversions that change signedness occur
- THEN the system SHALL flag them as violations

#### Scenario: BARR-C Guidelines Application
- GIVEN a C source file being analyzed
- WHEN global variables lack static specifier
- THEN the system SHOULD flag them as concerns

### Requirement: Checklist-Based Code Review Workflow

The system MUST provide a checklist-based code review workflow adapted from Koopman's embedded system review model covering function, style, architecture, exception handling, timing, testing, and hardware concerns.

#### Scenario: Functional Correctness Verification
- GIVEN a function implementation and specification
- WHEN reviewing the code
- THEN the reviewer SHALL check that behavior matches specification and error conditions are handled

#### Scenario: Architecture and Design Evaluation
- GIVEN a module implementation
- WHEN reviewing the code
- THEN the reviewer SHALL assess cohesion, coupling, and proper use of abstraction layers

#### Scenario: Hardware Interaction Review
- GIVEN hardware-related code
- WHEN reviewing the code
- THEN the reviewer SHALL check register access patterns and interrupt handling safety

### Requirement: Comprehensive Testing Strategy

The system MUST define test levels (unit/integration/HIL/flight simulation), provide Unity/CMock/Ceedling guidance, and specify hardware abstraction mocking patterns.

#### Scenario: Multi-Level Test Implementation
- GIVEN flight software modules
- WHEN implementing tests
- THEN the system SHALL require unit tests with mocks, integration tests for interfaces, HIL tests with simulated inputs, and flight simulation end-to-end tests

#### Scenario: Testing Framework Usage
- GIVEN a C function to test
- WHEN using Unity framework
- THEN the test SHALL follow Arrange-Act-Assert pattern with descriptive names

#### Scenario: Hardware Abstraction Mocking
- GIVEN code using hardware abstraction layer
- WHEN unit testing
- THEN the test SHALL use mocks for sensor interfaces, actuator controls, and timing services

### Requirement: Telemetry Patterns Definition

The system MUST specify LoRa packet structure, frequency planning per IREC/ESRA band plan, FEC implementation, and SD card data logging for telemetry systems.

#### Scenario: LoRa Packet Structure Compliance
- GIVEN a LoRa telemetry packet
- WHEN the packet is constructed
- THEN it SHALL include preamble, sync word, header, payload, and CRC with standardized format

#### Scenario: Frequency Band Selection
- GIVEN launch location regulatory region
- WHEN configuring LoRa radio
- THEN the system SHALL select frequencies within permitted ISM band (e.g., 902-928 MHz US, 863-870 MHz EU)

#### Scenario: SD Card Data Logging
- GIVEN flight computer with SD card
- WHEN system powers on
- THEN it SHALL create timestamp-named log file and use buffered writes with periodic syncs

### Requirement: State Machine Patterns for Flight Software

The system MUST define flight state machine templates (pre-flight → boost → coast → apogee → descent → recovery), entry/exit actions, and guarded transitions.

#### Scenario: Standard Flight State Transitions
- GIVEN system in pre-flight state
- WHEN launch detection occurs
- THEN state machine SHALL transition to boost state
- GIVEN system in boost state
- WHEN burnout detection occurs
- THEN state machine SHALL transition to coast state
- GIVEN system in coast state
- WHEN apogee detection occurs
- THEN state machine SHALL transition to apogee state

#### Scenario: State Entry/Exit Actions
- GIVEN state machine enters boost state
- WHEN transition occurs
- THEN system SHALL activate engine monitoring and start guidance algorithms
- GIVEN state machine exits boost state
- WHEN transition occurs
- THEN system SHALL deactivate engine monitoring and log boost phase data

#### Scenario: Guarded Transitions with Safety Checks
- GIVEN system in boost state
- WHEN apogee-trigger condition detected prematurely
- THEN state machine SHALL NOT transition to apogee state and SHALL log error

### Requirement: Competition Readiness Framework

The system MUST provide IREC DTEG-aligned documentation checklist, test evidence requirements, and flight readiness review gates.

#### Scenario: Documentation Checklist Alignment
- GIVEN team preparing for IREC competition
- WHEN reviewing documentation requirements
- THEN checklist SHALL include technical data package, test plans, and hazard analysis per DTEG Sections 4, 5.2, and 6

#### Scenario: Test Evidence Requirements
- GIVEN completed subsystem test
- WHEN preparing for flight readiness review
- THEN system SHALL require signed test procedures, data sheets, and anomaly reports as evidence

#### Scenario: Flight Readiness Review Gates
- GIVEN all subsystems completed individual testing
- WHEN entering Pre-FRT gate
- THEN system SHALL verify all subsystem test evidence is complete and discrepancies resolved
- GIVEN vehicle on launch pad
- WHEN conducting LFRFGO review
- THEN system SHALL verify range safety approvals, final mass properties, and GO/NO-GO criteria