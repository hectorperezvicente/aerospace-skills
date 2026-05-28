# Telemetry Patterns Specification

## Purpose

This specification defines the requirements for LoRa packet structure, frequency planning per IREC/ESRA band plan, forward error correction (FEC), and data logging to SD card for amateur rocketry telemetry systems.

## Requirements

### Requirement: LoRa Packet Structure Compliance

The system MUST define and use a standardized LoRa packet structure for telemetry transmission.

#### Scenario: Packet Header Fields

- GIVEN a LoRa telemetry packet
- WHEN the packet is constructed
- THEN it SHALL include a preamble, sync word, header (with payload length), payload, and CRC

#### Scenario: Payload Telemetry Data Encoding

- GIVEN sensor data to transmit
- WHEN preparing the payload
- THEN the system SHALL encode the data in a compact binary format with defined field order and scaling

### Requirement: Frequency Planning per IREC/ESRA Band Plan

The system MUST operate within authorized frequency bands as defined by IREC/ESRA for amateur rocketry.

#### Scenario: Band Selection for Region

- GIVEN the launch location's regulatory region (e.g., US, EU)
- WHEN configuring the LoRa radio
- THEN the system SHALL select frequencies within the permitted ISM band for that region (e.g., 902-928 MHz US, 863-870 MHz EU)

#### Scenario: Channel Separation

- GIVEN multiple telemetry transmitters in proximity
- WHEN assigning frequencies
- THEN the system SHALL ensure sufficient channel separation to avoid interference per band plan guidelines

### Requirement: Forward Error Correction (FEC) Implementation

The system SHOULD implement forward error correction to improve telemetry link robustness.

#### Scenario: Enable LoRa FEC Mode

- GIVEN a LoRa radio link with marginal signal-to-noise ratio
- WHEN configuring the radio
- THEN the system SHOULD enable FEC to reduce packet loss rate

#### Scenario: FEC Overhead Trade-off
- GIVEN a telemetry link requiring high data rate
- WHEN FEC is enabled
- THEN the system SHALL account for increased airtime due to FEC overhead

### Requirement: SD Card Data Logging

The system MUST log telemetry data to SD card as a backup to radio transmission.

#### Scenario: File Creation and Naming

- GIVEN a flight computer with SD card inserted
- WHEN the system powers on
- THEN it SHALL create a new log file with a timestamp-based name (e.g., YYYYMMDD_HHMMSS.bin)

#### Scenario: Data Writing Reliability
- GIVEN continuous telemetry data generation
- WHEN writing to the SD card
- THEN the system SHALL use buffered writes and periodic syncs to prevent data loss on power failure

#### Scenario: Log Format Consistency
- GIVEN telemetry data from multiple sensors
- WHEN logging to SD card
- THEN the system SHALL use the same binary format as transmitted over LoRa for consistency