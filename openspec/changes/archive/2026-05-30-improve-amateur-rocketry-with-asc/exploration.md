## Exploration: Improve Amateur Rocketry Skill Using Advanced Skill Creator

### Current State

The **amateur-rocketry** skill lives at `skills/amateur-rocketry/` and was created on 2026-05-25 as a standalone v1.0. It provides methodology-focused guidance for safety-critical embedded software development in student rocketry projects.

**Structure:**
- `SKILL.md` (99 lines) — Core activation contract, hard rules (NASA Power of 10 subset + MISRA Essentials + BARR-C), decision gates table, and 5-step execution workflow mapped to SDD phases.
- `references/` — 7 reference files (safety-standards, code-review-checklist, testing-guide, telemetry-patterns, state-machine-patterns, competition-checklist, integration-guide).

**Current condition:**
- The skill was created through the SDD pipeline and has archived specs, design, and tasks.
- It has **zero test cases** — there are no `evals/evals.json`, no benchmark data, no way to measure whether the skill actually improves agent behavior.
- The description / triggering mechanism has never been optimized — no trigger eval set, no `run_loop` optimization.
- The skill exists in the openspec workflow but was never iterated after creation (no feedback loop, no quality metrics).
- The skill references `embedded-rocketry` in its integration guide, but that skill **does not exist** as a file on disk (it's only a system-prompt trigger mention).

The **advanced-skill-creator** (ASC) skill lives at `~/.config/opencode/skills/advanced-skill-creator/` and provides a complete test-and-iterate pipeline:

**ASC Capabilities:**
| Capability | What It Enables |
|---|---|
| **Test case authoring** | Write structured eval prompts with expectations/assertions |
| **A/B comparison** | Run with-skill vs. without-skill (baseline) in parallel subagents |
| **Quantitative grading** | Auto-grade assertions, produce pass rates, aggregate metrics |
| **Eval viewer** | `generate_review.py` → browser-based qualitative review with feedback capture |
| **Iteration loop** | Revise skill → rerun tests → review → repeat until quality plateaus |
| **Description optimization** | Generate trigger eval set → `run_loop.py` optimizes frontmatter description for higher triggering accuracy |
| **Blind comparison** | Independent agent judges outputs without knowing which version produced them |
| **Analyst pass** | Identifies non-discriminating assertions, flaky evals, time/token tradeoffs |

### What's Missing From Amateur Rocketry Today

1. **No test cases** — We can't objectively measure whether the skill improves outcomes.
2. **No baseline** — We don't know what agents do *without* the skill.
3. **No iteration history** — v1.0 was shipped and never revisited.
4. **No trigger optimization** — The description may not reliably activate when needed.
5. **No integration tests** — We don't know if co-loading with embedded-rocketry works or makes sense.
6. **No quality metrics** — Pass rates, timing, token consumption are all unknown.

### What the Advanced Skill Creator Enables for Improvement

The ASC pipeline maps directly onto what amateur-rocketry needs:

| ASC Phase | What It Would Do | Expected Outcome |
|---|---|---|
| **Write eval prompts** | 3-5 realistic test prompts (e.g., "design a flight state machine for a dual-deploy rocket") | Concrete, repeatable test scenarios |
| **Run with-skill + baseline** | Parallel subagents: one with amateur-rocketry, one without | Measurable diff in output quality |
| **Grade assertions** | Auto-grade: "output follows BARR-C static globals rule", "state machine uses enum" | Pass rate per scenario |
| **Iterate** | Revise SKILL.md or reference files based on failures | Better pass rate next iteration |
| **Optimize description** | Generate 20 trigger/non-trigger queries, run `run_loop.py` | Higher activation rate in real use |

### Potential Approaches

1. **Full ASC pipeline (recommended)**
   - Write 3-5 eval prompts covering the skill's core competencies (safety standards compliance, state machine design, telemetry patterns, testing strategy, competition prep)
   - Run with-skill vs. baseline in parallel
   - Grade and review via eval viewer
   - Iterate until quality plateaus
   - Then run description optimization
   - **Pros**: Complete, measurable improvement; produces data to prove the skill works
   - **Cons**: Takes more time upfront; requires setting up workspace directories in the repo
   - **Effort**: Medium-High

2. **Trigger-only optimization**
   - Skip eval/test iteration entirely
   - Jump straight to description optimization: generate trigger eval set, run `run_loop.py`
   - **Pros**: Quickest path; addresses a real pain point (skill not triggering when needed)
   - **Cons**: Doesn't improve the skill content at all; if content has issues, trigger optimization just makes bad advice more accessible
   - **Effort**: Low

3. **Lightweight iteration**
   - Write 2-3 eval prompts manually
   - Run them manually (no parallel subagents, no formal grading)
   - Tweak the skill based on observed behavior
   - Skip the viewer, skip bencharmarking
   - **Pros**: Quick feedback without full ASC infrastructure
   - **Cons**: Lacks rigor; no baseline comparison; hard to know if you're actually improving
   - **Effort**: Low-Medium

4. **Minimal: add evals.json only**
   - Create `evals/evals.json` with test prompts and expectations so future improvement cycles have a starting point
   - Don't run anything yet
   - **Pros**: Low effort, documents the testing contract for future work
   - **Cons**: No actual improvement to the skill
   - **Effort**: Low

### Recommendation

**Approach 1 (Full ASC pipeline)** — porque la skill de amateur-rocketry está en v1.0 desde su creación y nunca fue testeada ni iterada. No sabemos si realmente ayuda o no. El advanced-skill-creator está diseñado exactamente para este caso de uso: tomar una skill existente, crear evaluaciones, correr baseline, y mejorar en base a data real.

La secuencia sería:
1. Copiar la skill a un workspace de trabajo (`improve-amateur-rocketry-with-asc/`)
2. Escribir 3-5 eval prompts representativos del dominio
3. Spawnear runs paralelos (with-skill vs. without-skill)
4. Gradear assertions y generar benchmark
5. Lanzar el eval viewer (feedback humano)
6. Iterar sobre SKILL.md y/o referencias basándose en los resultados
7. Una vez estable, optimizar la descripción con `run_loop.py`
8. Aplicar los cambios finales a `skills/amateur-rocketry/`

Vale la pena porque:
- La skill cubre safety-critical code — errores tienen consecuencias reales
- Estudiantes la van a usar para competencias (IREC, ESRA) — necesita ser confiable
- Estás invirtiendo en tener una skill de calidad, no algo tirado ahí

### Risks

- **No hay embedded-rocketry skill en disco**: La guía de integración referencia un skill que no existe físicamente. Esto puede causar confusión. Decidir si se crea, se remove la referencia, o se deja como documentación futura.
- **Sin CI ni test runner**: El proyecto no tiene test runner detectado (no CI pipeline). Las evaluaciones serán manuales/subagent-based, no parte de un pipeline automatizado.
- **Contenido muy específico a estándares**: Las aserciones sobre NASA Power of 10 / MISRA / BARR-C necesitan validación externa para confirmar que son correctas. Un error en los estándares podría propagarse.
- **Overfitting a los eval prompts**: Riesgo de que la skill mejore solo para los prompts de prueba. Usar variedad de prompts en cada iteración para mitigarlo.

### Ready for Proposal

Yes. The exploration is complete — proceed to sdd-propose with the change name `improve-amateur-rocketry-with-asc`.
