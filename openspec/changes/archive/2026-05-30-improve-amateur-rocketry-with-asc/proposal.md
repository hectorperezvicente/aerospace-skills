# Proposal: Improve Amateur Rocketry Skill with Advanced Skill Creator

## Intent

The amateur-rocketry skill (v1.0) was shipped with zero test cases, no baseline, no iteration history, and no trigger optimization. We don't know if it actually improves agent output or just adds noise. The advanced-skill-creator (ASC) provides the eval/iterate/optimize pipeline needed to measure, improve, and validate the skill.

## Scope

### In Scope
- Write 3-5 eval prompts covering the skill's core domains (safety standards, state machines, telemetry, testing, competition prep)
- Run with-skill vs. without-skill baseline in parallel subagents
- Grade assertions, aggregate into benchmark, launch eval viewer for human feedback
- Iterate SKILL.md and reference files based on eval failures
- Run description optimization via `run_loop.py` for better triggering
- Apply final changes to `skills/amateur-rocketry/`
- Create `evals/` directory in the skill folder with eval artifacts

### Out of Scope
- Creating the embedded-rocketry skill (referenced in integration-guide.md)
- Setting up CI/CD integration for automated eval runs
- Adding new reference files or domain coverage
- Changing the skill's architecture or 5-step workflow

## Capabilities

### New Capabilities
None — no new user-facing capabilities are introduced. The skill's domain coverage stays the same.

### Modified Capabilities
- `amateur-rocketry-skill`: Quality improvement only. Spec requirements remain unchanged; implementation (SKILL.md + references) will be refined based on eval data.

## Approach

Execute the full ASC pipeline as recommended in exploration:

1. **Workspace setup** — Copy skill to `improve-amateur-rocketry-with-asc/` workspace with snapshot of original
2. **Eval authoring** — Write 3-5 representative prompts covering safety rules, state machines, telemetry, testing, competition docs
3. **Parallel runs** — Spawn with-skill + baseline subagents for each eval
4. **Grading** — Auto-grade assertions via grader agent, capture timing/token metrics
5. **Review** — Launch `generate_review.py` eval viewer for human qualitative feedback
6. **Iterate** — Revise SKILL.md and/or reference files based on feedback; rerun until plateau
7. **Optimize triggers** — Generate trigger eval set, run `run_loop.py` for description optimization
8. **Apply** — Copy final skill files back to `skills/amateur-rocketry/`

## Affected Areas

| Area | Impact | Description |
|------|--------|-------------|
| `skills/amateur-rocketry/SKILL.md` | Modified | Refined based on eval feedback |
| `skills/amateur-rocketry/references/*.md` | Modified | Individual refs may be updated |
| `skills/amateur-rocketry/evals/` | New | Eval prompts, metadata, benchmark results |
| `skills/amateur-rocketry/frontmatter description` | Modified | Optimized for triggering |

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Overfitting to eval prompts | Med | Rotate eval prompts between iterations; use diverse scenarios |
| embedded-rocketry skill doesn't exist on disk | High | Note in integration-guide that it's a planned skill; don't block this change |
| No CI for automated regression | High | Eval artifacts in repo enable manual re-runs; automated CI is deferred |
| Safety standard rules may be incorrect | Low | Validate NASA/MISRA/BARR-C rules against authoritative sources during review |

## Rollback Plan

1. `git revert` the commit that updates `skills/amateur-rocketry/`
2. The workspace directory (`improve-amateur-rocketry-with-asc/`) preserves the original snapshot and all iteration history for reference
3. The `evals/` directory can be removed if unwanted — no production dependency

## Dependencies

- advanced-skill-creator must be installed at `~/.config/opencode/skills/advanced-skill-creator/`
- Access to subagents for parallel eval runs
- Python 3 for `generate_review.py` and `run_loop.py`

## Success Criteria

- [ ] Pass rate ≥80% on eval assertions for the final skill version
- [ ] With-skill runs outperform baselines on ≥3 of 5 evals
- [ ] Description optimization improves trigger rate vs. original
- [ ] All iteration artifacts (evals, benchmarks, snapshots) are committed to the repo
- [ ] Human review confirms the skill produces measurably better output than v1.0
