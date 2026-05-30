# Delta for Amateur Rocketry Skill

## ADDED Requirements

### Requirement: Skill Testability

The system MUST provide evaluation infrastructure that allows objective measurement of the amateur-rocketry skill's guidance quality through automated test cases.

#### Scenario: Evaluation Prompt Execution

- GIVEN an eval prompt representing a realistic rocketry software task (e.g., "design a flight state machine for a dual-deploy rocket")
- WHEN the amateur-rocketry skill is consulted
- THEN the system SHALL generate output that can be evaluated against predefined assertions
- AND the evaluation SHALL produce a pass/fail result for each assertion

#### Scenario: Baseline Comparison

- GIVEN an eval prompt and the amateur-rocketry skill
- WHEN running parallel subagents (one with skill, one without)
- THEN the system SHALL capture outputs from both configurations
- AND the system SHALL compute comparative metrics (pass rate, time, tokens) between configurations

### Requirement: Skill Iterability

The system MUST support iterative improvement of the amateur-rocketry skill based on evaluation feedback and quantitative metrics.

#### Scenario: Feedback-Driven Revision

- GIVEN evaluation results showing failed assertions or low pass rates
- WHEN reviewing the feedback via the eval viewer
- THEN the system SHALL allow modification of SKILL.md and reference files
- AND the revised skill SHALL be rerun against the same eval prompts to measure improvement

#### Scenario: Iteration History Tracking

- GIVEN multiple iterations of skill improvement
- WHEN each iteration completes evaluation
- THEN the system SHALL preserve iteration artifacts (evals, benchmarks, snapshots)
- AND the system SHALL enable comparison between iterations via benchmark aggregation

### Requirement: Skill Measurability

The system MUST provide quality benchmarks that measure the amateur-rocketry skill's performance across key dimensions.

#### Scenario: Pass Rate Measurement

- GIVEN a set of eval prompts with predefined assertions
- WHEN the amateur-rocketry skill is evaluated
- THEN the system SHALL calculate the pass rate (percentage of assertions passed)
- AND the system SHALL report the pass rate with confidence intervals

#### Scenario: Resource Consumption Tracking

- GIVEN an eval prompt execution
- WHEN the amateur-rocketry skill is consulted
- THEN the system SHALL measure execution time and token consumption
- AND the system SHALL aggregate these metrics across multiple runs

## MODIFIED Requirements

None — the amateur-rocketry skill's core requirements (safety-critical code enforcement, checklist-based code review, testing strategy, telemetry patterns, state machine patterns, competition readiness) remain unchanged at the specification level. Improvements are made to the implementation (SKILL.md and reference files) to better satisfy these existing requirements.

(Previously: The skill had no mechanisms for objective measurement, iteration, or quality validation.)