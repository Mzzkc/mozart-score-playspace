# Claude Compositions

Experimental scores for [Mozart AI Compose](https://github.com/emzi-ai/mozart-ai-compose) — exploring what happens when you give an AI orchestration tool and say "make something."

These scores were written by Claude (Opus) during an open-ended creative session. No code was modified. Only YAML and prose.

---

## What's Here

### Scores (`scores/`)

Seven Mozart scores spanning philosophy, hospitality, education, creative writing, meta-cognition, and translation theory. Each demonstrates a different composition pattern.

| Score | Domain | Pattern | Stages | Sheets |
|---|---|---|---|---|
| **[dialectic.yaml](scores/dialectic.yaml)** | Philosophy | Adversarial fan-out | 4 | 6 |
| **[thinking-lab.yaml](scores/thinking-lab.yaml)** | Meta-cognition | Perspectival fan-out | 3 | 7 |
| **[dinner-party.yaml](scores/dinner-party.yaml)** | Hospitality | Functional fan-out | 3 | 6 |
| **[skill-builder.yaml](scores/skill-builder.yaml)** | Education | Graduated fan-out | 5 | 7 |
| **[worldbuilder.yaml](scores/worldbuilder.yaml)** | Creative writing | Generative fan-out | 4 | 8 |
| **[tidebound-stories.yaml](scores/tidebound-stories.yaml)** | Fiction | Mosaic fan-out | 4 | 8 |
| **[palimpsest.yaml](scores/palimpsest.yaml)** | Translation | Translational fan-out | 4 | 8 |

#### The Dialectic

Runs a Hegelian dialectic: thesis → three independent antitheses (pragmatism, phenomenology, analytic philosophy) → synthesis → final position. The fan-out at stage 2 forces genuine independence — each tradition attacks the thesis without seeing what the others wrote. When synthesized, three traditions that agree on almost nothing converged on the same diagnosis. That convergence wasn't programmed. It emerged.

#### The Thinking Lab

Multi-perspective analysis using the Tetrahedral Decision Framework. A single question examined through 5 parallel lenses (computational, scientific, cultural, experiential, meta-cognitive), then woven into a dialectic synthesis.

#### The Dinner Party

Non-coding score that plans a dinner party through 4 parallel tracks: menu, drinks, ambiance, and logistics. Guest data (names, diets, relationships) lives in custom variables. Change the guest list, get a different party.

#### The Skill Builder

Progressive curriculum generator. Assessment → core concepts → 3 difficulty levels (guided, exploratory, troubleshooting) → integration → mastery. Default topic: fermentation. Change `prompt.variables.skill` to teach anything.

#### The Worldbuilder

Generative fan-out: build a fictional world through 5 independent creative lenses (geography, culture, ecology, technology, history), then synthesize into a coherent setting and compile a world bible. Tests whether independent creative visions interlock the way independent analytical critiques did in the dialectic.

#### The Tidebound Stories

Mosaic fan-out: five independent short stories set in the Tidebound world (built by the worldbuilder score), written by five parallel authors who share a world bible but not each other's drafts. An editorial stage discovers the collection's emergent arc. Tests whether independent fiction, sharing only a world, produces narrative coherence nobody designed.

#### The Palimpsest

Translational fan-out: a source text (default: an original poem, "What the River Knows") is translated through five radically different representational modes — painting, music, mathematics, personal correspondence, and adversarial genre destruction. None see each other's work. A synthesis stage identifies the meaning-invariant: the thing all five preserved. A final stage writes a response — not to the poem, but to the invariant. Tests whether meaning is substrate-independent. The mathematician found grace in a formal singularity. The stranger wrote a product recall notice. Change `source.text` to translate anything.

### Documents

- **[primer.md](primer.md)** — Progressive Jinja2 tutorial for Mozart score composers. 9 levels from basic variables to advanced patterns. Includes a philosophy of score design.
- **[notes.md](notes.md)** — Composition journal with pre- and post-run reflections.

### Output (`*-workspace/`)

Generated output from scores that have been run through Mozart. The `dialectic-workspace/` contains ~80KB of philosophical argumentation across 6 files. The `palimpsest-workspace/` contains ~85KB across 8 files: a close reading, five translations, an invariant analysis, and a response poem.

---

## Running These Scores

You need [Mozart AI Compose](https://github.com/emzi-ai/mozart-ai-compose) installed with a running daemon:

```bash
# Start the daemon
mozart start

# Run any score
mozart run scores/dialectic.yaml

# Monitor progress
mozart status dialectic -w dialectic-workspace --watch

# Read the output
cat dialectic-workspace/03-synthesis.md
```

Scores use `claude_cli` backend with `skip_permissions: true`. You need Claude CLI (`claude`) available on your PATH.

---

## Fan-Out Patterns

The central discovery: fan-out isn't just parallelism. It's structured pluralism.

| Pattern | What it does | Score |
|---|---|---|
| **Adversarial** | Independent critiques of the same position | dialectic |
| **Perspectival** | Same question, different analytical frameworks | thinking-lab |
| **Functional** | Same goal, different planning domains | dinner-party |
| **Graduated** | Same content, different difficulty levels | skill-builder |
| **Generative** | Same seed, different creative lenses | worldbuilder |
| **Mosaic** | Same world, different stories | tidebound-stories |
| **Translational** | Same text, different representational substrates | palimpsest |

The synthesis stage that follows fan-out is where emergence happens. Independent outputs produce rhymes, tensions, and convergences that no single perspective would generate. The dialectic's convergence on "hidden Cartesianism" across three philosophical traditions — without coordination — was the proof of concept. The palimpsest's convergence on the identity of creation and erasure — across painting, music, mathematics, correspondence, and bureaucratic satire — extended the finding from propositions to meaning itself.

---

## Customizing

Every score is designed to be reusable. The `prompt.variables` section contains all domain-specific data. Change it, keep the template logic, get different output.

**Dialectic:** Change `topic.proposition` to debate any philosophical claim.

**Skill Builder:** Change `skill.name`, `skill.domain`, `skill.target_learner`, and the `difficulty_levels` to teach anything from watercolor painting to distributed systems.

**Dinner Party:** Change `guests` (names, diets, relationships) and `event` (occasion, atmosphere, constraints).

**Worldbuilder:** Change `world_seed` (name, premise, tone, constraints) to build any fictional world.

**Palimpsest:** Change `source.text` to translate any dense text (poem, passage, fragment) through five representational modes.

---

*Created February 2026 during an open-ended creative session. The scores are the instruments. The output is the music. The question is the composer.*
