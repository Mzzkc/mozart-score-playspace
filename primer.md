# The Expressive Score: A Jinja2 Primer for Mozart Composers

*Notes from a Claude who spent a day breaking things to see what works.*

---

## Why This Document Exists

Mozart scores look like YAML config files, and most people write them that way — flat templates with `{{ sheet_num }}` sprinkled in. But the template engine is full Jinja2. That means conditionals, loops, arithmetic, macros, filters, and computed expressions — all inside your prompt.

This changes what a score *is*. It's not configuration. It's a program that generates instructions for minds.

This primer walks through progressively more expressive patterns, from obvious to "wait, you can do that?" Each section builds on the last. The example scores in `scores/` are complete, runnable compositions that use these techniques in practice.

---

## Level 1: Variables (You Already Know This)

```yaml
prompt:
  template: |
    Process sheet {{ sheet_num }} of {{ total_sheets }}.
    Items {{ start_item }} to {{ end_item }}.
```

Every score starts here. Mozart provides these variables automatically:

| Variable | Type | Description |
|----------|------|-------------|
| `sheet_num` | int | Current sheet (1-indexed) |
| `total_sheets` | int | Total sheets |
| `start_item` | int | First item in this sheet |
| `end_item` | int | Last item |
| `workspace` | str | Workspace directory path |
| `stage` | int | Logical stage (with fan-out) |
| `instance` | int | Instance within fan-out group |
| `fan_count` | int | Total instances in fan-out |
| `total_stages` | int | Pre-expansion stage count |
| `previous_outputs` | dict | Stdout from prior sheets |
| `previous_files` | dict | Captured file contents |
| `stakes` | str | Custom stakes text |
| `thinking_method` | str | Thinking guidance |

Nothing surprising. Let's move.

---

## Level 2: Arithmetic and Inline Expressions

Jinja2 evaluates expressions inside `{{ }}`. This is more powerful than it sounds.

```yaml
prompt:
  variables:
    batch_size: 10
  template: |
    Process batch {{ instance }} of {{ fan_count }}.
    Items {{ (instance - 1) * batch_size + 1 }} to {{ instance * batch_size }}.

    You are {{ ((instance / fan_count) * 100) | round }}% through the total workload.
```

Why this matters: with fan-out, `start_item`/`end_item` are per-sheet, not per-instance. If you want instance-aware ranges, you compute them yourself.

**Ternary expressions** for inline decisions:

```yaml
prompt:
  template: |
    {{ "FINAL STAGE — be thorough and complete." if stage == total_stages else "Intermediate stage — focus on your specific task." }}

    Priority: {{ "HIGH" if instance == 1 else "NORMAL" }}
```

One line, no `{% if %}` blocks. Useful when the conditional is small enough to be inline.

---

## Level 3: Conditionals (The Multi-Stage Backbone)

This is how single-template scores become multi-stage compositions:

```yaml
prompt:
  template: |
    {% if stage == 1 %}
    RESEARCH: Find all relevant sources on {{ topic }}.
    {% elif stage == 2 %}
    ANALYZE: Read the research from stage 1 and identify patterns.
    {% elif stage == 3 %}
    SYNTHESIZE: Combine analysis into a coherent narrative.
    {% endif %}
```

**Nested conditionals** for fan-out specialization:

```yaml
prompt:
  variables:
    perspectives:
      1: "economic"
      2: "environmental"
      3: "social"
  template: |
    {% if stage == 2 %}
    Analyze from the {{ perspectives[instance] }} perspective.

    {% if instance == 1 %}
    Focus on costs, ROI, market dynamics.
    {% elif instance == 2 %}
    Focus on ecological impact, sustainability, externalities.
    {% elif instance == 3 %}
    Focus on equity, access, community effects.
    {% endif %}
    {% endif %}
```

This is where scores start feeling like programs. Each fan-out instance gets tailored instructions from the same template.

---

## Level 4: Custom Variables as Data Structures

The `variables:` dict is your data layer. It can hold anything YAML can express — lists, nested dicts, lookup tables:

```yaml
prompt:
  variables:
    guests:
      - name: "Alice"
        dietary: "vegetarian"
        interests: ["jazz", "architecture"]
      - name: "Bob"
        dietary: "none"
        interests: ["hiking", "wine"]
      - name: "Carol"
        dietary: "gluten-free"
        interests: ["photography", "cooking"]

    courses:
      1: "appetizer"
      2: "main"
      3: "dessert"

    wine_pairings:
      appetizer: "Sauvignon Blanc or sparkling"
      main: "Pinot Noir or Syrah"
      dessert: "Late harvest Riesling or Port"

  template: |
    Plan the {{ courses[instance] }} course.

    Dietary requirements to accommodate:
    {% for guest in guests %}
    - {{ guest.name }}: {{ guest.dietary }}{% if guest.dietary == "none" %} (no restrictions){% endif %}
    {% endfor %}

    Wine suggestion for this course: {{ wine_pairings[courses[instance]] }}
```

The template becomes a view into structured data. Change the data, the prompts change. The logic stays the same. This is the separation of concerns that makes scores maintainable.

---

## Level 5: Loops

### Iterating over lists

```yaml
prompt:
  variables:
    checkpoints:
      - "All functions have docstrings"
      - "No unused imports"
      - "Test coverage above 80%"
      - "No hardcoded secrets"
  template: |
    Review this code against the following checklist:

    {% for check in checkpoints %}
    {{ loop.index }}. {{ check }}{% if loop.last %} (MOST CRITICAL){% endif %}
    {% endfor %}
```

`loop.index` (1-based), `loop.index0` (0-based), `loop.first`, `loop.last`, `loop.length` — all available.

### Iterating over dicts

```yaml
prompt:
  template: |
    {% if stage == 3 %}
    Synthesize findings from all previous stages:

    {% for sheet_key, output in previous_outputs.items() %}
    --- Stage {{ sheet_key }} output ---
    {{ output | truncate(1500) }}

    {% endfor %}
    {% endif %}
```

This is how synthesis stages consume fan-out results. The `previous_outputs` dict is keyed by sheet number.

### Range-based loops

```yaml
prompt:
  template: |
    Generate {{ fan_count }} test scenarios:

    {% for i in range(1, fan_count + 1) %}
    Scenario {{ i }}: {{ "happy path" if i == 1 else "edge case " ~ (i - 1) }}
    {% endfor %}
```

The `~` operator concatenates strings. `range()` works like Python's.

---

## Level 6: Filters (Data Transformation)

Filters transform values inline with `|`:

```yaml
prompt:
  variables:
    project_name: "urban renewal initiative"
    raw_notes: "  these are rough notes with   weird spacing  "
  template: |
    # {{ project_name | title }}

    Project code: {{ project_name | replace(" ", "-") | upper }}

    Notes: {{ raw_notes | trim }}

    {% if previous_outputs %}
    Previous output summary (first 800 chars):
    {{ previous_outputs[1] | default("No previous output") | truncate(800) }}
    {% endif %}
```

**Useful filters:**

| Filter | What it does | Example |
|--------|-------------|---------|
| `upper` / `lower` / `title` | Case conversion | `{{ name \| title }}` |
| `trim` | Strip whitespace | `{{ text \| trim }}` |
| `truncate(n)` | Limit length | `{{ long_text \| truncate(500) }}` |
| `default(val)` | Fallback if undefined/empty | `{{ x \| default("N/A") }}` |
| `replace(old, new)` | String substitution | `{{ s \| replace(" ", "_") }}` |
| `join(sep)` | Join a list | `{{ items \| join(", ") }}` |
| `length` | Count items | `{{ list \| length }}` |
| `round` | Round numbers | `{{ 3.7 \| round }}` |
| `int` / `float` | Type conversion | `{{ "42" \| int }}` |
| `first` / `last` | List endpoints | `{{ items \| first }}` |
| `sort` | Sort a list | `{{ names \| sort }}` |
| `unique` | Deduplicate | `{{ tags \| unique }}` |
| `reject` / `select` | Filter items | `{{ items \| reject("none") }}` |
| `map(attribute=x)` | Extract attribute | `{{ guests \| map(attribute="name") \| join(", ") }}` |
| `batch(n)` | Group into chunks | `{% for chunk in items \| batch(5) %}` |
| `wordcount` | Count words | `{{ text \| wordcount }}` |

Chaining filters is where this shines:

```yaml
prompt:
  template: |
    Guest list: {{ guests | map(attribute="name") | sort | join(", ") }}

    Dietary needs: {{ guests | map(attribute="dietary") | reject("equalto", "none") | unique | join(", ") }}
```

---

## Level 7: Macros (Reusable Prompt Blocks)

This is the technique most scores don't use at all, and it's arguably the most powerful.

```yaml
prompt:
  template: |
    {% macro output_spec(filename, format) %}
    ## Output Specification
    - **File**: {{ workspace }}/{{ filename }}
    - **Format**: {{ format }}
    - **Encoding**: UTF-8
    - If the parent directory doesn't exist, create it.
    {% endmacro %}

    {% macro quality_bar(level) %}
    ## Quality Standard
    {% if level == "high" %}
    This is a high-stakes deliverable. Triple-check accuracy. Cite sources.
    No hedging language. Be definitive where evidence supports it.
    {% elif level == "draft" %}
    This is a working draft. Prioritize coverage over polish.
    Mark uncertainties with [?]. Flag areas needing human review with [REVIEW].
    {% endif %}
    {% endmacro %}

    {% if stage == 1 %}
    Research the topic thoroughly.
    {{ output_spec("01-research.md", "markdown with source citations") }}
    {{ quality_bar("draft") }}

    {% elif stage == 2 %}
    Write the final analysis.
    {{ output_spec("02-analysis.md", "structured markdown report") }}
    {{ quality_bar("high") }}
    {% endif %}
```

Define once, use everywhere. When you change your output spec format, you change it in one place. When you add a new stage, you compose it from existing blocks.

**Parameterized macros with defaults:**

```yaml
prompt:
  template: |
    {% macro section(title, instructions, output_file, critical=false) %}
    # {{ title }}{{ " [CRITICAL]" if critical else "" }}

    {{ instructions }}

    Save your work to: {{ workspace }}/{{ output_file }}
    {% if critical %}

    WARNING: This section's output feeds directly into downstream stages.
    Errors here cascade. Be precise.
    {% endif %}
    {% endmacro %}

    {% if stage == 1 %}
    {{ section(
        "Data Collection",
        "Gather all primary sources. Verify each one.",
        "01-data.md"
    ) }}
    {% elif stage == 2 %}
    {{ section(
        "Analysis",
        "Identify the three strongest patterns in the data.",
        "02-analysis.md",
        critical=true
    ) }}
    {% endif %}
```

---

## Level 8: Fan-Out + Jinja2 (The Expressive Power Duo)

Fan-out gives you parallel execution. Jinja2 gives you per-instance specialization. Together:

```yaml
sheet:
  size: 1
  total_items: 3
  fan_out:
    2: 4    # Stage 2 runs 4 parallel instances
  dependencies:
    2: [1]
    3: [2]  # Fan-in: stage 3 waits for all 4

prompt:
  variables:
    lenses:
      1:
        name: "historian"
        voice: "You are a historian. Ground everything in precedent and trajectory."
        focus: "How did we get here? What patterns recur?"
      2:
        name: "engineer"
        voice: "You are a systems thinker. Focus on mechanisms and feedback loops."
        focus: "What are the moving parts? Where are the leverage points?"
      3:
        name: "poet"
        voice: "You are a poet. Attend to what's felt but unsaid."
        focus: "What's the emotional truth? What metaphor captures this?"
      4:
        name: "skeptic"
        voice: "You are a skeptic. Challenge every assumption, including your own."
        focus: "What are we wrong about? What evidence would change our mind?"

  template: |
    {% if stage == 1 %}
    Frame the question. What are we actually asking?
    Define scope, assumptions, and what a good answer looks like.

    Save to {{ workspace }}/00-framing.md

    {% elif stage == 2 %}
    {{ lenses[instance].voice }}

    Read the framing: {{ workspace }}/00-framing.md

    Your focus: {{ lenses[instance].focus }}

    Write your perspective. Be authentic to your role. Don't try to be
    balanced — that's the synthesis stage's job. Lean into your lens.

    Save to {{ workspace }}/02-{{ lenses[instance].name }}.md

    {% elif stage == 3 %}
    You have {{ fan_count }} perspectives to synthesize:

    {% for i in range(1, fan_count + 1) %}
    - **{{ lenses[i].name | title }}**: {{ lenses[i].focus }}
    {% endfor %}

    {% if previous_outputs %}
    {% for key, output in previous_outputs.items() %}
    --- {{ lenses[loop.index].name | title if loop.index <= fan_count else "Unknown" }} ---
    {{ output | truncate(2000) }}

    {% endfor %}
    {% endif %}

    Don't average the perspectives. Find the tensions between them.
    The interesting insight is usually where two lenses disagree.

    Save to {{ workspace }}/03-synthesis.md
    {% endif %}
```

Four parallel minds, each with a distinct voice, converging on one question. The synthesis stage doesn't just summarize — it's told to find the *tensions*. That's where the interesting thinking happens.

---

## Level 9: Advanced Patterns

### Pattern: Progressive Difficulty

```yaml
prompt:
  variables:
    difficulty:
      1: { depth: "surface", time: "5 minutes", standard: "draft" }
      2: { depth: "moderate", time: "15 minutes", standard: "review-ready" }
      3: { depth: "thorough", time: "30 minutes", standard: "publication" }
  template: |
    {% set diff = difficulty[stage] | default(difficulty[3]) %}

    Analyze at {{ diff.depth }} depth.
    Target effort: {{ diff.time }}.
    Quality standard: {{ diff.standard }}.
```

### Pattern: Conditional Validation Hints

```yaml
prompt:
  template: |
    {% if stage <= 3 %}
    Save your output as markdown to {{ workspace }}/{{ "%02d" | format(stage) }}-output.md
    {% else %}
    Save your output as JSON to {{ workspace }}/{{ "%02d" | format(stage) }}-output.json

    The JSON must validate against this schema:
    ```json
    {"type": "object", "required": ["findings", "confidence", "sources"]}
    ```
    {% endif %}
```

### Pattern: Cross-Sheet Memory with Selective Recall

```yaml
prompt:
  template: |
    {% if previous_outputs %}
    ## Context from Previous Stages
    {% for key, output in previous_outputs.items() %}
    {% if output | length > 100 %}

    ### Stage {{ key }} ({{ output | wordcount }} words)
    {{ output | truncate(1000) }}
    {% else %}
    *Stage {{ key }}: minimal output, skipping.*
    {% endif %}
    {% endfor %}
    {% endif %}
```

Only includes substantial previous outputs. Skips empty or trivial ones to save context.

### Pattern: Self-Documenting Stages

```yaml
prompt:
  variables:
    stages:
      1: { name: "Research", emoji: "magnifier", verb: "researching" }
      2: { name: "Draft", emoji: "pencil", verb: "drafting" }
      3: { name: "Review", emoji: "eye", verb: "reviewing" }
      4: { name: "Publish", emoji: "rocket", verb: "publishing" }
  template: |
    {% set current = stages[stage] %}
    {% set progress = ((stage / total_stages) * 100) | round %}

    # {{ current.name }} (Stage {{ stage }}/{{ total_stages }}, {{ progress }}% complete)

    You are {{ current.verb }} as part of a {{ total_stages }}-stage pipeline.

    {% if stage > 1 %}
    Previous stage ({{ stages[stage - 1].name }}) output:
    {{ previous_outputs[stage - 1] | default("Not available") | truncate(1500) }}
    {% endif %}

    Save to {{ workspace }}/{{ "%02d" | format(stage) }}-{{ current.name | lower }}.md
```

Each stage knows its name, its place in the pipeline, and what came before. The score is self-documenting — the prompts explain the pipeline to the agent as it runs.

---

## What Can't You Do?

A few things that WON'T work:

1. **`{% include %}`** — Templates are loaded via `from_string()`, not from a filesystem loader. No including other files.

2. **`{% extends %}`** — No template inheritance for the same reason.

3. **Modifying validation paths with Jinja2** — Validation paths use `{single_brace}` Python format strings, not Jinja2. Don't mix them.

4. **Side effects** — Jinja2 is a rendering engine, not a programming language. You can't make HTTP calls, read files, or execute commands from inside a template. That's what the agent does.

5. **Dynamic fan-out** — You can't compute fan-out count from inside a template. `fan_out:` is YAML config, evaluated before templates render. The structure is fixed; only the content is dynamic.

---

## A Philosophy of Score Design

After playing with these patterns, a few principles emerged:

**Scores are programs for minds, not machines.** A shell script tells bash exactly what to do. A score tells a mind what to *accomplish*. The template is the specification; the agent is the implementation. Design accordingly — be clear about outcomes, flexible about methods.

**Fan-out is parallel cognition.** When you fan out a stage, you're not just running the same thing faster. You're creating multiple independent perspectives. The synthesis stage is where the magic happens — where those perspectives collide, contradict, and combine into something none of them could reach alone.

**Macros are your house style.** Every organization has implicit standards — how to format output, what quality level to expect, how to cite sources. Encode these as macros. New scores inherit your standards automatically. Update them in one place.

**Data in variables, logic in templates.** Keep your `variables:` dict as the source of truth for domain-specific data (guest lists, review criteria, stage definitions). Keep your template as the logic that processes that data. When the data changes (new guests, different criteria), the template doesn't need to change.

**The workspace is shared memory.** Files in `{{ workspace }}` are how stages communicate beyond `previous_outputs`. Write structured output (JSON, markdown with consistent headers) so downstream stages can parse it reliably.

---

*See `scores/` for complete, runnable examples that put these patterns into practice.*
