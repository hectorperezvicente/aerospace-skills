# Tasks: Improve Amateur Rocketry Skill with Advanced Skill Creator

## Review Workload Forecast

| Field | Value |
|-------|-------|
| Estimated changed lines | 150-200 |
| 400-line budget risk | Low |
| Chained PRs recommended | No |
| Suggested split | Single PR |
| Delivery strategy | ask-on-risk |

Decision needed before apply: No
Chained PRs recommended: No
Chain strategy: single-pr
400-line budget risk: Low

## Phase 1: Workspace & Eval Authoring

- [ ] 1.1 Create workspace dir `improve-amateur-rocketry-with-asc/` with `original-snapshot/` and `iteration-1/` subdirs
- [ ] 1.2 Snapshot current skill — copy `skills/amateur-rocketry/SKILL.md` and `references/` to `original-snapshot/`
- [ ] 1.3 Write eval prompt 0 (safety standards): "Review this C flight computer code for NASA Power of 10, MISRA, and BARR-C compliance..."
- [ ] 1.4 Write eval prompt 1 (state machines): "Design a flight state machine for a dual-deploy rocket with redundant apogee detection..."
- [ ] 1.5 Write eval prompt 2 (telemetry): "Define a LoRa telemetry packet structure for a 4-channel sensor suite at 915 MHz..."
- [ ] 1.6 Write eval prompt 3 (testing): "Create a test plan for a flight computer's barometric altitude sensor..."
- [ ] 1.7 Create `evals/evals.json` with all 4 prompts, expected outputs, and expectations
- [ ] 1.8 Draft 3-5 assertions per eval matching spec scenarios (enum check, static globals, return codes, LoRa fields)

## Phase 2: Parallel Runs & Grading

- [ ] 2.1 For each eval, spawn with-skill subagent pointing at `original-snapshot/` — save outputs to `iteration-1/eval-N/with_skill/`
- [ ] 2.2 For each eval, spawn baseline subagent (no skill) — save outputs to `iteration-1/eval-N/without_skill/`
- [ ] 2.3 Capture timing/token data from each subagent completion into `timing.json`
- [ ] 2.4 Run grader agent against each output using assertions from evals.json
- [ ] 2.5 Save grading results to `grading.json` per eval run
- [ ] 2.6 Run `python -m scripts.aggregate_benchmark` to produce `benchmark.json` and `benchmark.md`

## Phase 3: Review & Iterate

- [ ] 3.1 Launch `generate_review.py` eval viewer with iteration-1 outputs and benchmark
- [ ] 3.2 Collect human feedback from eval viewer reviews
- [ ] 3.3 Analyze failures: identify which domains and assertions had low pass rates
- [ ] 3.4 Modify `SKILL.md` based on eval feedback (fix unclear guidance, add missing rules, improve examples)
- [ ] 3.5 Modify individual reference files based on domain-specific failures
- [ ] 3.6 Repeat Phase 2 with modified skill into `iteration-2/` using previous iteration as baseline
- [ ] 3.7 Repeat Phase 3.1-3.5 until pass rate ≥80% or feedback plateaus

## Phase 4: Description Optimization

- [ ] 4.1 Generate 20 trigger eval queries (mix of should-trigger and should-not-trigger) for description optimization
- [ ] 4.2 Present trigger eval set to user for review via HTML template
- [ ] 4.3 Run `python -m scripts.run_loop --eval-set trigger-eval.json --skill-path skills/amateur-rocketry/ --max-iterations 5`
- [ ] 4.4 Apply `best_description` from run_loop output to SKILL.md frontmatter

## Phase 5: Apply & Archive

- [ ] 5.1 Copy final SKILL.md and references from last iteration to `skills/amateur-rocketry/`
- [ ] 5.2 Copy `evals/evals.json` and benchmark artifacts to `skills/amateur-rocketry/evals/`
- [ ] 5.3 Commit all changes: skill files, evals, frontmatter description, workspace artifacts
- [ ] 5.4 Verify final skill meets spec scenarios (testability, iterability, measurability)
