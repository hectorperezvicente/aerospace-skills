# Tasks: Create Amateur/Student Rocketry Software Skill

## Review Workload Forecast

| Field | Value |
|-------|-------|
| Estimated changed lines | ~830 (all new) |
| 400-line budget risk | High |
| Chained PRs recommended | Yes |
| Suggested split | PR 1: Core + Integration (~230 lines) → PR 2: Safety & Process (~300 lines) → PR 3: Domain Refs (~300 lines) |
| Delivery strategy | ask-on-risk |
| Chain strategy | stacked-to-main |

Decision needed before apply: Yes
Chained PRs recommended: Yes
Chain strategy: stacked-to-main
400-line budget risk: High

### Suggested Work Units

| Unit | Goal | Likely PR | Notes |
|------|------|-----------|-------|
| 1 | Core SKILL.md + integration-guide.md | PR 1 | Foundation for all references |
| 2 | Safety standards + code review + testing guides | PR 2 | Process references, depends on Unit 1 |
| 3 | Telemetry + state machine + competition refs | PR 3 | Domain references, depends on Unit 1 |

## Phase 1: Foundation

- [x] 1.1 Create `skills/amateur-rocketry/` and `skills/amateur-rocketry/references/` directories
- [x] 1.2 Create `skills/amateur-rocketry/SKILL.md` with frontmatter, activation triggers, 5 hard rules (NASA Power of 10 subset + MISRA essentials + BARR-C), decision gates table, and 5-step execution workflow (Explore → Specify → Implement → Verify → Archive)

## Phase 2: Process Reference Files

- [x] 2.1 Create `references/safety-standards.md` with NASA Power of 10 subset (restricted goto, unbounded recursion, static loop bounds), MISRA C essentials (signedness conversions), BARR-C guidelines (static globals) + practical examples per spec scenarios
- [x] 2.2 Create `references/code-review-checklist.md` with Koopman-adapted checklist covering function correctness, style, architecture/cohesion, exception handling, timing, testing coverage, and hardware interactions per spec scenarios
- [x] 2.3 Create `references/testing-guide.md` with test levels (unit/integration/HIL/flight simulation), Unity/CMock/Ceedling guidance, and hardware abstraction mocking patterns per spec scenarios

## Phase 3: Domain Reference Files

- [x] 3.1 Create `references/telemetry-patterns.md` with LoRa packet structure (preamble/sync/header/payload/CRC), IREC/ESRA frequency planning (902-928 MHz US, 863-870 MHz EU), FEC implementation, and SD card data logging per spec scenarios
- [x] 3.2 Create `references/state-machine-patterns.md` with flight state machine templates (pre-flight → boost → coast → apogee → descent → recovery), entry/exit actions, and guarded transitions per spec scenarios
- [x] 3.3 Create `references/competition-checklist.md` with IREC DTEG documentation checklist (Sections 4, 5.2, 6), test evidence requirements (signed procedures, data sheets, anomaly reports), and FRR gates (Pre-FRT, LFRFGO) per spec scenarios

## Phase 4: Integration & Verification

- [x] 4.1 Create `references/integration-guide.md` explaining co-loading with embedded-rocketry skill, process vs. domain boundary definitions, and complementary usage patterns for common flight software tasks
- [x] 4.2 Verify each reference file provides concrete, executable guidance without domain assumptions and stays under 200 lines for agentic RAG efficiency
- [x] 4.3 Verify SKILL.md stays under 700 tokens and frontmatter passes gentle-ai YAML validation (name, description, license, metadata.author, metadata.version)
