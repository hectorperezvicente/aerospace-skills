# Design: Improve Amateur Rocketry Skill with Advanced Skill Creator

## Technical Approach

Apply the advanced-skill-creator (ASC) pipeline to the amateur-rocketry skill: create eval prompts that test each domain, run with-skill vs. baseline A/B comparisons, grade assertions, iterate based on feedback, then optimize description. The skill's structure (SKILL.md + 7 references) stays intact — we enhance quality without changing architecture.

## Architecture Decisions

### Decision: Eval Prompt Coverage Strategy

| Option | Tradeoff | Decision |
|--------|----------|----------|
| One eval per reference file (7 total) | Too many for first iteration | ❌ |
| 3-5 evals covering core domains | Good coverage, manageable runs | ✅ |
| Single eval for whole skill | Misses per-domain issues | ❌ |

**Choice**: 4 evals — safety standards, state machine design, telemetry patterns, testing strategy. Competition readiness covered implicitly by the safety eval.

### Decision: Workspace Layout

**Choice**: Workspace at project root `improve-amateur-rocketry-with-asc/` with subdirectories per iteration
**Rationale**: Standard ASC convention; keeps eval artifacts inside the repo for traceability
**Structure**:
```
improve-amateur-rocketry-with-asc/
├── original-snapshot/    # SKILL.md + references before edits
├── iteration-1/
│   ├── eval-0-safety/
│   ├── eval-1-state-machine/
│   ├── eval-2-telemetry/
│   └── eval-3-testing/
├── iteration-2/
└── iteration-N/
```

### Decision: Grading Approach

**Choice**: Hybrid — programmatic assertions (check output for enum types, static globals absence, LoRa packet fields) + human review via eval viewer
**Rationale**: Assertions catch objective failures; human review catches subjective quality issues (clarity, actionability)
**Each eval gets 3-5 assertions** matching spec scenarios

### Decision: Snapshot Before Edit

**Choice**: Copy original SKILL.md + references to `original-snapshot/` before any modifications
**Rationale**: Baseline runs use the original; can diff between versions after each iteration

## Data Flow

```
User (eval prompts) ──→ Orquestador
                            │
              ┌─────────────┼─────────────┐
              │             │             │
         with-skill      baseline      grader
         subagent        subagent      subagent
         (skill loaded)  (no skill)    (reads assertions)
              │             │             │
              └─────────┬───┘             │
                        │                 │
                    outputs/           grading.json
                        │                 │
                        └────────┬────────┘
                                 │
                          generate_review.py
                                 │
                            eval viewer ←── human feedback
                                 │
                            iterate skill
                                 │
                         run_loop.py (trigger opt)
                                 │
                        apply to skills/amateur-rocketry/
```

## File Changes

| File | Action | Description |
|------|--------|-------------|
| `skills/amateur-rocketry/evals/evals.json` | Create | Eval prompts, assertions, metadata |
| `skills/amateur-rocketry/SKILL.md` | Modify | Refined guidance based on eval feedback |
| `skills/amateur-rocketry/references/*.md` | Modify | Individual refs updated per eval gaps |
| `skills/amateur-rocketry/frontmatter` | Modify | Description optimized via run_loop.py |
| `improve-amateur-rocketry-with-asc/` | Create | Workspace with snapshots, iteration artifacts |

## Interfaces / Contracts

### Eval Set Schema
```json
{
  "skill_name": "amateur-rocketry",
  "evals": [
    {
      "id": 0,
      "prompt": "Design a flight state machine for a dual-deploy rocket...",
      "expected_output": "State machine with enum types, guarded transitions...",
      "expectations": [
        "Uses enum for flight states",
        "Functions return status codes",
        "No static globals"
      ]
    }
  ]
}
```

### Assertion Grading
Pass/fail per assertion + evidence string. Aggregated into pass_rate per eval configuration.

## Testing Strategy

| Layer | What to Test | Approach |
|-------|-------------|----------|
| Eval prompts | Prompts trigger correct domain guidance | Manual review of outputs |
| Assertions | Assertions match spec scenarios | Grader subagent runs programmatic checks |
| Baseline comparison | With-skill outperforms without-skill | Pass rate delta ≥ threshold |
| Description opt | Trigger rate improves | run_loop.py metrics |

## Migration / Rollout

No migration required. The `evals/` directory and workspace are additive. Rollback: revert the commit that updates `skills/amateur-rocketry/`.

## Open Questions

- [ ] Should we validate NASA/MISRA/BARR-C rules against authoritative sources during iteration?
- [ ] How many iterations before plateau is expected? (Estimate 2-3)
