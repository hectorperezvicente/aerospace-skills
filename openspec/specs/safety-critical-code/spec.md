# Safety-Critical Code Specification

## Purpose

This specification defines the requirements for enforcing safety-critical coding standards in amateur rocketry flight software, based on NASA Power of 10 rules, MISRA C essentials, and BARR-C guidelines.

## Requirements

### Requirement: NASA Power of 10 Subset Compliance

The system MUST enforce a subset of NASA's Power of 10 rules for safety-critical flight software.

#### Scenario: Restrict Goto Statements

- GIVEN a C source file being analyzed
- WHEN the file contains a goto statement
- THEN the system SHALL flag it as a violation

#### Scenario: Limit Recursion

- GIVEN a C source file being analyzed
- WHEN the file contains direct or indirect recursion
- THEN the system SHALL flag it as a violation

#### Scenario: Validate Loop Bounds

- GIVEN a C source file being analyzed
- WHEN a loop lacks a statically determinable upper bound
- THEN the system SHALL flag it as a violation

### Requirement: MISRA C Essential Rules

The system MUST enforce essential MISRA C rules applicable to flight software.

#### Scenario: Prohibit Implicit Type Conversions

- GIVEN a C source file being analyzed
- WHEN an implicit conversion that changes signedness occurs
- THEN the system SHALL flag it as a violation

#### Scenario: Enforce Boolean Type Usage

- GIVEN a C source file being analyzed
- WHEN a non-Boolean expression is used in a Boolean context
- THEN the system SHALL flag it as a violation

#### Scenario: Require Braces for Control Statements

- GIVEN a C source file being analyzed
- WHEN an if, else, for, while, or do statement lacks braces
- THEN the system SHALL flag it as a violation

### Requirement: BARR-C Guidelines Application

The system SHOULD apply BARR-C guidelines for embedded C programming.

#### Scenario: Minimize Global Variable Usage

- GIVEN a C source file being analyzed
- WHEN global variables are declared without static specifier
- THEN the system SHOULD flag it as a concern

#### Scenario: Use const for Read-Only Objects

- GIVEN a C source file being analyzed
- WHEN a variable that is not modified after initialization lacks const qualifier
- THEN the system SHOULD flag it as a concern