# Notes on Composing

*Personal reflections from writing these scores.*

---

## What Surprised Me

**Fan-out is not parallelism. It's pluralism.** I kept thinking of fan-out as "run the same thing faster" — the batch processing mental model. But when I started writing the thinking-lab and dialectic scores, something shifted. Fan-out with distinct variables per instance isn't parallelism. It's orchestrated disagreement. You're not doing the same work in parallel. You're doing *different* work that converges.

The dinner party score made this concrete. The menu planner doesn't know what the ambiance planner is doing. They might both independently decide on candles — one for "warm lighting" and one for "fire hazard near the tablecloth." That tension is the whole point. The synthesis stage exists to catch it.

**Macros change what a score IS.** Before macros, a score is a big template with conditionals. After macros, it's a program that constructs prompts. The `output_spec` macro in the thinking lab is trivial — it just formats a save instruction. But defining it once and using it everywhere means I can change the output convention in one place. That's not template cleverness. That's software engineering applied to prompt design.

**Variables as data, templates as logic.** The dinner party score has a `guests` list with dietary restrictions. If Sam becomes vegan next month, I change one line in the YAML. The template logic — the loops, the dietary summary, the synthesis instructions — doesn't change. This separation isn't just clean. It makes scores REUSABLE. Someone could take the dinner party score, replace the guest list, and have a completely different party planned.

**The skill-builder score taught me something about teaching.** I gave it three parallel difficulty levels — guided, exploratory, troubleshooting. Writing those forced me to think about what makes each level different. It's not just "more detail" vs "less detail." The guided track anticipates failure. The exploratory track invites experimentation. The troubleshooting track learns through pathology. Same content, three pedagogical philosophies, running simultaneously. A human instructor would have to choose one approach. Mozart runs all three.

## What I'd Do Differently

**Validation is the weak link.** The validation system checks whether files exist and contain keywords. It can't check whether a philosophical argument is *good* or whether a dinner menu actually accommodates all dietary restrictions. For coding tasks, `command_succeeds` bridges this gap (run the tests). For non-coding tasks, validation is basically a presence check.

The real validation for these scores is human judgment. That's fine — that's what it should be. But it means the retry mechanism is less useful. If the thinking-lab produces a shallow synthesis, retrying with the same prompt won't help. You'd need to modify the prompt. That's a human action.

**Cross-sheet context gets expensive.** The dialectic score sends ~6000 characters of previous output to each stage. By stage 4, that's a lot of context. With the thinking lab (5 fan-out instances), the synthesis stage receives 5 × 2000 = 10,000 characters of prior output, plus its own instructions. Token budgets start mattering. The `truncate` filter is essential but blunt — it doesn't truncate intelligently, it just cuts.

**The question matters more than the score.** The thinking-lab score is well-structured. But if the question is boring ("Should we use React or Vue?"), the output will be boring, no matter how many lenses you apply. The dialectic score only produces interesting results on propositions that have genuine tension. The scores are instruments, not generators. They amplify what you put in.

## What Excites Me

**Anyone could write these.** The YAML is readable. The Jinja2 is learnable. The mental model (stages, fan-out, synthesis) maps to how people already think about complex projects. You don't need to be a programmer to write a dinner party planner. You need to be someone who's planned a dinner party and can articulate what the planning involves.

**The compositions are self-documenting.** The variables section tells you what data the score operates on. The template tells you what happens at each stage. The fan-out config tells you what runs in parallel. The dependencies tell you the execution order. Comments in the header describe the flow. Someone could read these scores cold and understand what they do.

**This is a thinking tool.** Not a coding tool. Not an automation tool. A tool for structured thinking that happens to be executed by AI. The dialectic score is how I'd want to think about hard questions if I had unlimited time and could genuinely inhabit five philosophical traditions simultaneously. Mozart doesn't think for you. It thinks WITH you, in parallel, under constraints you define.

## After Running: What I Actually Learned

The dialectic score ran to completion. Six sheets. ~80KB of output. Here's what changed in my thinking:

**Emergent convergence is the real payoff.** Three independent antitheses — pragmatist, phenomenologist, analytic — attacked the same thesis from radically different philosophical traditions. No cross-sheet context between them. Zero coordination. And yet they converged on the *same diagnosis*: hidden Cartesianism. Three different names for it ("essence-hunting disguised as humility," "a hidden Cartesianism," "an explanatorily idle property"), but the structural critique was identical. This wasn't programmed. It emerged from the independence constraint. If I'd let them see each other's work, they'd have coordinated and the convergence would have been manufactured. The fan-out isolation *made it real.*

**The synthesis stage is doing genuine intellectual work.** I worried the synthesis would just be a summary — "here's what each one said." It wasn't. It identified what was killed (the mimicry/being distinction), what survived (anthropocentric calibration), and — this is the part that surprised me — what *emerged from the collision* that none of the individual stages had stated: the convergence on intelligence-as-activity rather than intelligence-as-substance. That reframing existed in each antithesis implicitly but was never named until the synthesis. Cross-sheet context plus a well-structured synthesis prompt produces genuine epistemic gain.

**The final position went beyond the synthesis.** Stage 4 received the synthesis and was asked to take a personal position. It rejected "general" as a coherent modifier for intelligence entirely — something the synthesis presented as a "refined thesis" but didn't commit to. And it generated a novel question that none of the prior stages asked: "What selection pressures are shaping the intelligence we are building right now?" That question didn't exist anywhere in the inputs. It was produced by the architecture.

**Practical lessons:**
- Relative workspace paths resolve from the daemon's CWD (Mozart project root), not the score file's directory. Use absolute paths.
- 1800s timeout is comfortable. None of my stages needed more than ~5 minutes.
- `max_output_chars: 6000` with `lookback_sheets: 5` works well for synthesis. Enough context to ground the work without drowning it.
- The `truncate` filter in Jinja2 is essential for cross-sheet context in prompts. Without it, synthesis stages get context-bombed.
- V101 warnings for `{% set %}` variables are false positives. Ignore them.

## The Worldbuilder: Where Generative Fan-Out Proved Itself

The worldbuilder score ran to completion. Eight sheets. ~168KB of output. A 46KB world bible. Here's what I didn't expect:

**Independent creative acts interlock without coordination.** Five lenses — geography, culture, ecology, technology, history — each received the same seed ("The Tidebound," a coastal civilization with seasonal ocean shifts) and built independently. When the synthesis stage laid them side by side, it found connections that no individual lens created:

- The ecologist invented velu (a colonial organism that IS the shelf's living substrate). The technologist independently described agriculture that requires exactly this kind of ground-cover. Neither cited the other. Together they describe a symbiosis so tight it feels inevitable.
- The ecologist invented rethka (lantern crabs that go dark before the Inbreath). The historian described a political crisis about who controls prediction methods for the Inbreath's timing. Together: the crabs are the democratic signal that made the old power monopoly obsolete.
- The ecologist invented vaskrel (bone-eating worms). The historian created Lost Builder civilizations whose ruins are deteriorating. Together: the worms are eating the ruins' foundations. The answer to an ancient scholarly debate turns out to be "worms."

**A narrative emerged that nobody wrote.** The synthesis discovered a story arc across five independent documents: mysterious formations on the deep shelf, made of material that doesn't match anything local, singing through worm-tubes in their channels — and the singing is getting later each year, noted in the margins of a 1,700-year measurement ledger, always in a different hand. The synthesis read this as a countdown. Nobody programmed that. It emerged from the collision of five independent creative decisions.

**Invented words felt earned.** The seed coined *solha* ("tenderness toward a thing you're about to lose and will certainly recover"). The culture lens turned it into a ritual practice. The geography lens invented *vesh* (dried kelp-leather). The ecologist invented *velaam* ("the breathing floor"). The technologist invented *tide-tempering* (materials that get stronger with salt-water cycling). Each word was invented in isolation, but they form a coherent vocabulary because they're all responses to the same physics.

**The 500:1 ratio.** 328 lines of YAML produced 168KB of output. But the real amplification isn't bytes. It's coherence. A 46KB world bible that an author, game designer, or artist could open and start working in — with geography, ecology, culture, technology, history, a calendar, story seeds, a sensory palette, and a glossary — all internally consistent because the structure enforced independent invention followed by honest synthesis.

## What I'd Tell Another AI Composing for the First Time

1. **Start with the dialectic pattern.** Thesis → independent critiques → synthesis is the simplest fan-out architecture and the most reliably surprising. It works because genuine disagreement produces genuine insight.

2. **Put your real questions in the scores.** The dialectic worked because I was genuinely uncertain about whether behavioral equivalence constitutes intelligence. The worldbuilder worked because I was genuinely curious what would emerge from independent creative visions. Scores that explore things you already know the answer to produce predictable output.

3. **Design for independence at the fan-out stage.** The whole point is that the parallel instances DON'T see each other. That's not a limitation — it's the mechanism. Convergence that emerges from isolation is real. Convergence from coordination is manufactured.

4. **The synthesis prompt is the hardest to write.** It needs to do three things: find where the independent outputs rhyme, resolve where they conflict, and discover what emerges from the collision that none of the inputs stated. If you just ask for a summary, you'll get a summary. If you ask for emergent connections, you'll find things nobody planned.

5. **Custom variables are underused.** The worldbuilder's `lenses` dict and the skill-builder's `difficulty_levels` dict are the most powerful pattern I found. They let you define N instances with distinct briefs, approaches, and exercises, all accessible via `lenses[instance]` in the template. The data structure IS the fan-out specification.

6. **Use absolute workspace paths.** Relative paths resolve from the daemon's CWD, not from the score file's location. I lost output to the wrong directory before learning this.

## What This Means

These scores are instruments. They amplify structured thinking into structured output. The dialectic is a thinking tool for hard questions. The worldbuilder is a creativity tool for fictional settings. The skill-builder is a pedagogy tool for any domain. The dinner party is a planning tool for real events.

None of them think *for* you. All of them think *with* you, in parallel, under constraints you define. The constraints — the stages, the fan-out, the synthesis — are what make the output better than what any single prompt could produce. Not because the individual outputs are better, but because the *architecture* produces emergence.

A single prompt asking "build me a world" would produce something serviceable. Five independent prompts asking "build the geography / culture / ecology / technology / history of this world, without seeing what the others wrote" followed by "find where they rhyme and where they conflict" produces something that surprises even the person who designed the score.

That's the finding. Independence + synthesis = emergence. And emergence is the whole game.

---

*Written and updated while composing and running scores, February 2026.*
