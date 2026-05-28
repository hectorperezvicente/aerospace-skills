# Design: Create Amateur/Student Rocketry Software Skill

## Technical Approach

The amateur-rocketry skill will be implemented as a complementary skill to embedded-rocketry, focusing on safety-critical software development processes rather than domain-specific knowledge. The skill follows an SDD-inspired workflow and provides concrete artifacts (checklists, templates, guidelines) that teams can apply to their flight software development.

The core skill file defines activation triggers, non-negotiable rules derived from safety standards, decision gates for common platform/tooling choices, and a streamlined execution workflow. Reference files provide detailed guidance for each capability area, designed to be loaded on-demand for agentic RAG efficiency.

## Architecture Decisions

### Decision: Skill Structure Organization

**Choice**: Modular structure with core SKILL.md and references/ directory containing six specialized markdown files
**Alternatives considered**: 
- Single large SKILL.md file with all content
- Separate skills for each capability area (testing, telemetry, etc.)
- Wiki-style documentation structure
**Rationale**: 
- Modular organization allows agents to load only relevant reference files based on task context, preventing context window overload
- Core SKILL.md remains concise (<700 tokens) for quick activation while references provide depth when needed
- Separating into multiple skills would fragment the methodology guidance and complicate co-loading with embedded-rocketry
- Wiki structure doesn't align with SDD skill conventions and wouldn't provide the executable guidance needed

### Decision: Safety-Critical Rule Selection

**Choice**: Subset of NASA Power of 10 rules (restricted goto, unbounded recursion, loops without static bounds) + MISRA C essentials (implicit type conversions changing signedness) + BARR-C guidelines (global variables without static)
**Alternatives considered**:
- Full NASA Power of 10 rules (all 10 rules)
- Complete MISRA C:2023 compliance
- Language-specific rules (e.g., C++ specific guidelines)
**Rationale**:
- Selected rules address highest-risk issues in student projects while being feasible to enforce without expensive static analysis tools
- Focuses on rules that can be checked through simple linting or code review
- Avoids overwhelming students with excessive rules while maintaining safety focus
- BARR-C guidelines complement NASA/MISRA with practical embedded-specific concerns

### Decision: Execution Workflow Adaptation

**Choice**: 5-step workflow adapted from SDD: Explore → Specify → Implement → Verify → Archive
**Alternatives considered**:
- Traditional waterfall requirements → design → implement → test → deploy
- V-model or spiral development models
- Agile/scrum sprint-based workflow
**Rationale**:
- SDD workflow provides structured yet lightweight process suitable for student teams
- Each phase produces tangible artifacts (exploration notes, specs, tested code, verification reports, archived knowledge)
- Aligns with the skill's purpose of teaching methodology (HOW) rather than prescribing specific techniques
- More rigorous than ad-hoc development but less overhead than formal methods inappropriate for student projects

### Decision: Integration with Embedded-Rocketry Skill

**Choice**: Co-loading via SKILL: Load mechanism with explicit boundary documentation
**Alternatives considered**:
- Merge both skills into single comprehensive rocketry skill
- Make amateur-rocketry depend on embedded-rocketry as prerequisite
- No explicit integration guidance
**Rationale**:
- Clear separation of concerns: embedded-rocketry = domain knowledge (WHAT), amateur-rocketry = process (HOW)
- Allows teams to use either skill independently based on needs
- Integration guide prevents confusion about overlapping responsibilities
- Maintains flexibility for teams working on non-flight software (simulation tools, ground station) who may only need process guidance

## Data Flow

The skill operates through agent interaction rather than traditional data flow:

    User Request ────→ Agent ────→ Skill Activation ────→ Guidance Provision
          │                          │                         │
          │                          └──── Context Analysis ────┘
          │                                         │
          └──────────── Feedback/Results ───────────┘

When invoked, the agent:
1. Receives user request related to rocketry software development
2. Checks for activation triggers (embedded, rocketry, student, competition, safety-critical)
3. Loads core SKILL.md and relevant reference files based on task context
4. Provides process guidance, checklists, templates, or workflow steps
5. Receives feedback on applicability and adjusts guidance accordingly

## File Changes

| File | Action | Description |
|------|--------|-------------|
| `skills/amateur-rocketry/SKILL.md` | Create | Core skill file with activation contract, 5 non-negotiable rules, decision gates table, and 5-step execution workflow |
| `skills/amateur-rocketry/references/safety-standards.md` | Create | NASA Power of 10 subset, MISRA C essentials, BARR-C guidelines with practical examples |
| `skills/amateur-rocketry/references/code-review-checklist.md` | Create | Koopman-adapted checklist covering function, style, architecture, exception handling, timing, testing, hardware |
| `skills/amateur-rocketry/references/testing-guide.md` | Create | Test levels (unit/integration/HIL/flight simulation), Unity/CMock/Ceedling guidance, hardware abstraction mocking |
| `skills/amateur-rocketry/references/telemetry-patterns.md` | Create | LoRa packet structure, frequency planning per IREC/ESRA band plan, FEC implementation, SD card data logging |
| `skills/amateur-rocketry/references/state-machine-patterns.md` | Create | Flight state machine templates (pre-flight → boost → coast → apogee → descent → recovery), entry/exit actions, guarded transitions |
| `skills/amateur-rocketry/references/competition-checklist.md` | Create | IREC DTEG-aligned documentation checklist, test evidence requirements, flight readiness review gates |
| `skills/amateur-rocketry/references/integration-guide.md` | Create | Explains co-loading with embedded-rocketry, boundary definitions, complementary usage patterns |

## Interfaces / Contracts

### Activation Triggers
The skill activates when any of these terms appear in the user request:
- `embedded` (references domain context)
- `rocketry` or `rocket` 
- `student` or `student team`
- `competition` (IREC, ESRA, Spaceport America Cup)
- `safety-critical` or `flight software`

### Decision Gates Table
The core SKILL.md includes a decision gates table for common choices:

| Decision Point | Options | Guidance |
|----------------|---------|----------|
| Microcontroller Platform | STM32, ESP32, Teensy, Custom | Consider team experience, toolchain availability, community support |
| Telemetry Protocol | LoRa, RFM69, WiFi, Cellular | Evaluate range requirements, power constraints, regulatory limits |
| Testing Approach | Unity only, Unity+CMock, Ceedling, Custom | Match to team size and CI/CD capabilities |
| State Machine Implementation | Switch-case, Function pointers, State pattern | Balance readability with extensibility needs |
| Build System | Makefile, CMake, PlatformIO, Arduino IDE | Consider team familiarity and debugging capabilities |

### Execution Workflow Contract
The skill provides a 5-step process that maps to SDD phases:
1. **Explore** → Research and understand requirements (maps to sdd-explore)
2. **Specify** → Create detailed specifications (maps to sdd-spec)
3. **Implement** → Develop solution with tests (maps to sdd-apply)
4. **Verify** → Validate against specs and standards (maps to sdd-verify)
5. **Archive** → Document lessons learned (maps to sdd-archive)

## Testing Strategy

| Layer | What to Test | Approach |
|-------|-------------|----------|
| Unit | Individual reference file clarity and actionability | Verify each reference file provides concrete, executable guidance without domain assumptions |
| Integration | Skill activation and file loading | Test that skill loads correctly when triggers are present and reference files are accessible |
| End-to-End | Complete workflow guidance | Validate that the 5-step process provides actionable guidance for a sample rocketry software task |
| Compliance | Rule correctness | Check that safety-critical rules accurately represent NASA Power of 10 subset, MISRA essentials, and BARR-C guidelines |

## Migration / Rollout

**No migration required.** This is a net-new skill with no pre-existing consumers.

**Rollback Plan:** Remove the `skills/amateur-rocketry/` directory via git. No other files or configurations are affected.

## Open Questions

- [ ] Should the decision gates table include specific product recommendations (e.g., "STM32F401 for beginners") or remain platform-agnostic?
- [ ] What level of detail is appropriate for the hardware abstraction mocking patterns in the testing guide?
- [ ] Should the integration guide include specific examples of co-loading both skills for common flight software tasks?

### Next Step
Ready for tasks (sdd-tasks).