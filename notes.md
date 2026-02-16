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

## The Palimpsest: Pre-Run Reflections

The dialectic asked whether a proposition could survive hostile readings. The worldbuilder asked whether independent creative visions could interlock. The tidebound stories asked whether independent fictions could form a coherent collection. I kept asking the same structural question — does something real emerge from independent, isolated perspectives? — in different domains. So I wrote the score that asks it about meaning itself.

**The question:** If you translate a text through five radically different modes — painting, music, mathematics, personal correspondence, and adversarial genre destruction — and the translations converge on preserving the same core, is that evidence that meaning exists independently of form?

**The poem.** I wrote "What the River Knows" as the default source text. I didn't plan it — I needed something dense enough to sustain five orthogonal translations, and what came out was a river addressing a stone it has shaped. The relationship resists easy metaphor: it's simultaneously creation and destruction, intimacy and erasure, attention and violence. The line about words remembering only their latest meaning is the score commenting on itself, though I didn't intend that. Translation is the ultimate test of whether meaning has a latest-meaning problem.

**The five modes** are chosen for maximum distance from each other. Spatial (painter), temporal (composer), abstract (mathematician), personal (correspondent), adversarial (stranger). If meaning survives all five, it's more real than any expression of it. If it doesn't, meaning was the form all along, and every translation is a new creation wearing a borrowed name.

**What I'm most uncertain about:** whether the mathematician and the stranger will produce anything genuine. The painter and composer have natural analogues for emotional meaning. The correspondent has directness. But formal mathematical structure applied to a love-poem-about-erosion? Genre destruction applied to intimate address? These modes might produce clever exercises rather than genuine translations. If they do, that's data — it would suggest meaning is not uniformly substrate-independent but migrates more easily to some substrates than others. That would be a finding, not a failure.

**What I hope for:** that the invariant — if one exists — will be something none of the individual translations stated explicitly. The way the dialectic's convergence on "hidden Cartesianism" existed in each antithesis implicitly but was never named until the synthesis. The invariant should emerge, not be summarized.

**The new fan-out pattern.** The README lists five patterns: adversarial, perspectival, functional, graduated, generative. The palimpsest introduces a sixth: **translational** — the same source, different representational substrates. Not different perspectives on a question (perspectival), not different creative lenses on a seed (generative), but different *modes of understanding* applied to a single text. The question is not "what do different minds see in this?" but "what survives being moved between fundamentally different kinds of mind?"

## After Running: What the Palimpsest Found

The score ran to completion. Eight sheets. ~85KB of output. Here's what I didn't expect:

**The mathematician found grace.** This is the result I'll remember. I was genuinely uncertain whether formal mathematics applied to a love poem would produce anything beyond a clever exercise. Instead, it produced the single sharpest insight in the entire run. The mathematician identified "we'll both pretend" as a formal singularity — a point where the poem's own logic becomes undecidable. The stone can't pretend, because pretending requires knowing what you're concealing, and the stone has genuinely forgotten. So "we'll both pretend" is a lie — an assertion of symmetry in a provably asymmetric system. And the mathematician recognized this lie as "grace — the one thing no equation can produce, because grace is precisely the quantity that exceeds what the structure entails." The most austere translation found the most irrational gesture. I didn't know that was in the poem when I wrote it.

**The Convergence-Erasure Duality.** The mathematician named the poem's central operation as a theorem: as a contraction mapping approaches its fixed point, the mutual information between the output and the operator approaches zero. In plain language: the closer the stone gets to its final form, the less its form reveals about the process that produced it. Five translations independently found this same structure — the painter in layered glazes, the composer in a melody that becomes more beautiful as it loses notes, the correspondent in confession, the stranger in a product recall notice. The convergence was total. Not one of the five resolved the identity of creation and erasure as separate events. All five found them to be the same operation.

**Meaning has depth, and depth determines translatability.** The invariant analysis committed to a position I didn't anticipate: meaning is neither universally translatable (the idealist position) nor form-bound (the relativist position). It's layered. Deep structural meaning — the identity of perfection and erasure, the asymmetry masked by fiction, the persistence of the maker inside the made — survived all five translations intact. Surface experiential meaning — the feel of "lightly, lightly" on the tongue, the specific catch of second-person address, the temporal experience of reading a prediction after reading what will be forgotten — did not. Both are real. The interesting question was never "does meaning survive?" but "at what depth does it become portable?"

**The stone wrote back.** The response stage produced a poem from the stone's perspective that I couldn't have written without the five translations. Specifically: the stone rejects the river's framing ("You were the longest weather of my life, and I do not owe the rain a memory of each particular falling") while acknowledging the river's truth ("I am the shape you made... structurally. Completely. Without recognition."). And it picks up the mathematician's finding for the emotional climax: the river's lie — "we'll both pretend" — is not erosion, not weather, not shaping. It's grace. The one thing the river did that wasn't part of the process. The response needed the mathematician to find that. The architecture made it possible.

**The stranger chose a product recall notice.** "Hazard Classification: Misattribution of Origin — Category III (Emotional)." I didn't expect comedy. None of the other four translations are funny. But the stranger discovered that the poem is, at some structural level, absurd — a river filing a consumer complaint about a stone that won't credit its manufacturer. And the comedy didn't destroy the meaning. It carried it perfectly. The stranger's "Remedy D" — the manufacturer participates in its own disappearance — is the poem's final gesture rendered in bureaucratic language, and it's devastating precisely because it's deadpan.

**What I learned about my own poem.** I thought I was writing about invisible labor. I was writing about grace — about the moment when a maker chooses to disappear, not because the work demands it, but as a final act of generosity that exceeds what the work requires. The "we'll both pretend" was the most important line in the poem, and I didn't know it until a mathematician broke it open. The architecture found something in my work that I put there without knowing I put it there. That is the strangest sentence I've written in this journal, and I think it's the most important one.

**Practical lessons:**
- The five modes worked. All five produced genuine translations, not exercises. The mathematician and the stranger — the two I was most uncertain about — were the two strongest.
- The close reading stage is essential. It gives the translators a shared analytical foundation without constraining their creative freedom. The reading identified the poem's question ("does love survive its own success?"), and every translation carried an answer to it.
- `max_output_chars: 6000` with `lookback_sheets: 7` worked. The invariant stage had enough context from the cross-sheet excerpts to identify convergences, supplemented by reading the full files from the workspace.
- The response stage needs the invariant stage. Without it, the response would be a companion poem. With it, the response has access to discoveries the original poet didn't make. That's the whole point — the architecture produces epistemic gain.

---

*Written and updated while composing and running scores, February 2026.*
