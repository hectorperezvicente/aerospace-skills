# State Machine Patterns Specification

## Purpose

This specification defines the requirements for flight state machine templates (pre-flight → boost → coast → apogee → descent → recovery), entry/exit actions, and guarded transitions for amateur rocketry flight software.

## Requirements

### Requirement: Flight State Machine Definition

The system MUST implement a flight state machine with the standard phases: pre-flight, boost, coast, apogee, descent, and recovery.

#### Scenario: Pre-Flight to Boost Transition
- GIVEN the system is in pre-flight state
- WHEN launch detection occurs (e.g., acceleration exceeds threshold)
- THEN the state machine SHALL transition to boost state

#### Scenario: Boost to Coast Transition
- GIVEN the system is in boost state
- WHEN burnout detection occurs (e.g., acceleration drops below threshold)
- THEN the state machine SHALL transition to coast state

#### Scenario: Coast to Apogee Transition
- GIVEN the system is in coast state
- WHEN apogee detection occurs (e.g., vertical velocity crosses zero)
- THEN the state machine SHALL transition to apogee state

#### Scenario: Apogee to Descent Transition
- GIVEN the system is in apogee state
- WHEN descent detection occurs (e.g., vertical velocity becomes negative and sustained)
- THEN the state machine SHALL transition to descent state

#### Scenario: Descent to Recovery Transition
- GIVEN the system is in descent state
- WHEN recovery activation conditions are met (e.g., altitude below threshold)
- THEN the state machine SHALL transition to recovery state

### Requirement: State Entry and Exit Actions

The system MUST define and execute specific entry and exit actions for each flight state.

#### Scenario: Boost State Entry Actions
- GIVEN the state machine enters boost state
- WHEN transition occurs
- THEN the system SHALL activate engine monitoring and start guidance algorithms

#### Scenario: Boost State Exit Actions
- GIVEN the state machine exits boost state
- WHEN transition occurs
- THEN the system SHALL deactivate engine monitoring and log boost phase data

#### Scenario: Apogee State Entry Actions
- GIVEN the state machine enters apogee state
- WHEN transition occurs
- THEN the system SHALL log apogee altitude and time, and prepare for descent events

### Requirement: Guarded Transitions and Safety Checks

The system MUST implement guarded transitions with safety checks to prevent invalid state changes.

#### Scenario: Invalid Boost-to-Apogee Transition Prevention
- GIVEN the system is in boost state
- WHEN an apogee-trigger condition is detected prematurely
- THEN the state machine SHALL NOT transition to apogee state and SHALL log an error

#### Scenario: Descent State Safety Timeout
- GIVEN the system is in descent state
- WHEN no recovery activation occurs within a maximum time limit
- THEN the state SHALL trigger autonomous recovery activation as a safety measure

#### Scenario: Recovery State Completion
- GIVEN the system is in recovery state
- WHEN recovery events are completed (e.g., parachute deployment confirmed)
- THEN the state machine SHALL transition to a completed state and log mission completion