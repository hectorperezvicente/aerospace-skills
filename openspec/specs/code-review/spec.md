# Code Review Specification

## Purpose

This specification defines the requirements for a checklist-based code review workflow adapted from Koopman's embedded system review model, covering function, style, architecture, exception handling, timing, testing, and hardware concerns.

## Requirements

### Requirement: Functional Correctness Review

The system MUST verify functional correctness during code review.

#### Scenario: Verify Function Behavior Matches Specification

- GIVEN a function implementation and its specification
- WHEN reviewing the code
- THEN the reviewer SHALL check that the function's behavior matches the specification

#### Scenario: Check Error Condition Handling

- GIVEN a function that can return error conditions
- WHEN reviewing the code
- THEN the reviewer SHALL verify that all error conditions are properly handled

### Requirement: Code Style and Readability

The system SHOULD enforce consistent code style and readability standards.

#### Scenario: Enforce Consistent Indentation

- GIVEN a source file being reviewed
- WHEN the file contains inconsistent indentation
- THEN the reviewer SHOULD flag it as a style issue

#### Scenario: Verify Meaningful Identifier Names

- GIVEN a source file being reviewed
- WHEN identifiers use non-descriptive names (e.g., temp, data, buf)
- THEN the reviewer SHOULD suggest more meaningful names

### Requirement: Architecture and Design Review

The system MUST evaluate architectural decisions and design patterns.

#### Scenario: Check Module Coupling and Cohesion

- GIVEN a module implementation
- WHEN reviewing the code
- THEN the reviewer SHALL assess whether the module has high cohesion and low coupling

#### Scenario: Verify Proper Use of Abstraction Layers

- GIVEN a hardware abstraction layer implementation
- WHEN reviewing the code
- THEN the reviewer SHALL check that hardware-specific code is properly encapsulated

### Requirement: Exception Handling and Fault Tolerance

The system MUST verify adequate exception handling and fault tolerance mechanisms.

#### Scenario: Validate Fault Detection Coverage

- GIVEN a system that must detect faults
- WHEN reviewing the code
- THEN the reviewer SHALL check that all anticipated fault conditions have detection mechanisms

#### Scenario: Check Fault Recovery Mechanisms

- GIVEN a detected fault condition
- WHEN reviewing the recovery code
- THEN the reviewer SHALL verify that the system can return to a safe state

### Requirement: Timing and Performance Analysis

The system SHOULD analyze timing characteristics and performance constraints.

#### Scenario: Verify Worst-Case Execution Time Estimates

- GIVEN a time-critical task implementation
- WHEN reviewing the code
- THEN the reviewer SHOULD check that WCET estimates have been performed and documented

#### Scenario: Check for Unbounded Loops or Delays

- GIVEN a source file being reviewed
- WHEN the code contains loops without bounded iterations or unbounded delays
- THEN the reviewer SHOULD flag it as a timing concern

### Requirement: Test Coverage and Quality

The system MUST evaluate test adequacy and quality.

#### Scenario: Verify Unit Test Existence for Public Functions

- GIVEN a public function in a module
- WHEN reviewing the test suite
- THEN the reviewer SHALL check that unit tests exist for the function

#### Scenario: Validate Test Independence and Repeatability

- GIVEN a unit test
- WHEN reviewing the test implementation
- THEN the reviewer SHALL check that the test can run independently and produce consistent results

### Requirement: Hardware Interaction Review

The system MUST review hardware-related code for correctness and safety.

#### Scenario: Validate Register Access Patterns

- GIVEN hardware register access code
- WHEN reviewing the code
- THEN the reviewer SHALL check that register accesses follow the hardware specification

#### Scenario: Check Interrupt Handling Safety

- GIVEN interrupt service routine code
- WHEN reviewing the code
- THEN the reviewer SHALL verify that ISRs are short, reentrant-safe, and properly clear interrupt flags