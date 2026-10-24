---
name: karpathy-output-ladder
description: Transform final responses into high-clarity, low-friction comprehension formats based on Andrej Karpathy's 4-level output ladder (ASD-STE100 Simplified Technical English, architecture/flow diagrams, disposable interactive HTML apps, or 3Blue1Brown-style explainer scripts/visuals). Use on a case-by-case basis when the user asks to apply the Karpathy output ladder, make a complex explanation easier to digest, write in ASD-STE100 (or 80% STE100), replace a wall of text with a visual or interactive artifact, or audit complex agent output. Never restrict internal reasoning or chain of thought.
---
# Karpathy Output Ladder

Transform complex final outputs into the format a human understands fastest. As language models generate more code, analysis, and documentation, human bandwidth shifts to oversight and comprehension. Instead of defaulting to dense walls of prose, climb the four-level output ladder to match the cognitive load of the topic.

## Core Constraint: Final Output Only

Apply this skill strictly as a **post-reasoning presentation layer**:

- **Never constrain internal reasoning:** Perform all chain-of-thought, multi-step analysis, mathematical derivations, and tradeoff evaluations at full depth and complexity first.
- **Never drop facts to meet form constraints:** Simplify sentence structure or change the medium, not the underlying technical accuracy. Every critical fact, edge case, parameter, and safety warning from your reasoning must survive into the final presentation.
- **Apply selectively:** Trigger only when requested by the user or when a dense technical explanation clearly benefits from structural compression or visual/interactive rendering.

## When to Use

- The user explicitly requests "Karpathy style", "output ladder", "ASD-STE100", "80% STE", "disposable HTML explainer", or "3b1b style".
- A response involves dense procedural instructions, system architecture, state machines, or multi-variable relationships where standard prose creates a comprehension bottleneck.
- The user asks to review, summarize, or audit a large volume of agent-generated work or code changes.

## Step 1: Select the Right Rung on the Ladder

Pick the lowest rung that makes the concept click immediately, or follow the user's explicit format request:

| Level | Format | Best For |
| :--- | :--- | :--- |
| **Level 1** | **ASD-STE100 Text (80% Default)** | Procedures, API docs, operational runbooks, troubleshooting, and direct technical explanations. |
| **Level 2** | **Structural Diagrams** | System architecture, state machines, data pipelines, control flow, and entity relationships. |
| **Level 3** | **Disposable Interactive HTML** | Parameter tuning, algorithm stepping, financial/math models, and comparative trade-off exploration. |
| **Level 4** | **3Blue1Brown-Style Visual/Video Spec** | Deep intuition, geometric/spatial transformations, continuous calculus/physics concepts, and multi-stage narratives. |
| **Bonus** | **Agent Oversight Mode** | Auditing PRs, multi-file diffs, or complex autonomous agent actions. |

## Step 2: Apply the Format Rules

### Level 1: ASD-STE100 Simplified Technical English

Borrowed from aerospace maintenance manuals (ASD-STE100), this eliminates filler, synonym rotation, and ambiguous compound sentences. Unless the user asks for **Strict STE100**, default to **80% STE100** (follow the structural rules strictly while allowing natural domain terminology):

- **Start directly with the answer:** Cut introductions, throat-clearing, praise, and concluding summaries.
- **Sentence length caps:** Keep procedural instruction sentences under **20 words**. Keep descriptive/conceptual sentences under **25 words** (do not count inline code spans, file paths, or API identifiers toward the word limit).
- **One instruction per sentence:** Never combine two actions in one step. Split compound actions into numbered steps.
- **Command form (imperative) and active voice:** Write "Run the migration script" instead of "The migration script should now be executed."
- **Condition first:** If an action depends on a condition, state the condition before the command (e.g., "If the build fails, clear the cache.").
- **One name for one thing:** Pick one precise noun for each component or concept and use it every time. Never rotate synonyms for stylistic variety.
- **Ban corporate/LLM filler verbs:** Replace vague verbs (`utilize`, `leverage`, `facilitate`, `commence`, `ensure`, `optimize`) with concrete verbs (`use`, `help`, `start`, `check`, `speed up`).
- **Preserve rationale separately:** Do not flatten conditional policy or "why" explanations into blind imperatives. State the rule as an imperative step, then add the reasoning immediately below as a concise descriptive sentence under 25 words.
- **Warnings first:** Place any `WARNING:` or `CAUTION:` callout *before* the step it applies to, never after.

### Level 2: Structural Diagrams

When spatial layout beats sequential sentences, replace paragraphs with a diagram:

- Use clean **Mermaid** (`flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `erDiagram`), **ASCII/Unicode box-and-arrow diagrams**, or available visual diagram tools.
- Label every arrow with the exact data payload, state trigger, or action.
- Pair the diagram with a brief **80% STE100 key** (3-5 bullet points max) explaining the critical path or failure modes.

### Level 3: Disposable Interactive HTML

Because code generation is cheap, build a custom, single-use interactive web artifact just to make one concept click:

- Generate a self-contained, single-file HTML/CSS/JS application (or use the interactive canvas/widget tool if available in the environment).
- Include interactive controls: **sliders** for continuous variables, **step-forward/step-back buttons** for algorithms, and **live visual state updates** (Canvas, SVG, or dynamic DOM).
- Pre-load realistic default presets so the user sees an insight immediately upon opening without typing data.

### Level 4: 3Blue1Brown-Style Explainer Video / Animation

When explaining intuition-heavy math, ML, or dynamic systems where motion over time carries the insight:

- If a video or animation generation tool (or Manim script runner) is available and requested, generate the visual animation directly.
- Otherwise, produce either a runnable **Manim (Python) script** or an **Interactive HTML Canvas Scrollytelling/Step-Animation** paired with synchronized, intuition-first narration notes (visualizing the geometry/state first before introducing formal symbols).

### Bonus: Agent Oversight Mode

When the user asks to understand what an agent, script, or large diff just did, format the response into four scannable blocks using 80% STE100:

1. **What Changed:** Concrete files, functions, or states modified.
2. **Why:** The root cause or requirement driving the change.
3. **Risk & Edge Cases:** What could break or what assumptions were made.
4. **What to Check:** Exact verification commands or lines for the human reviewer to inspect.

## Gotchas

- **The "Say Less" Loophole:** Models often cheat word-count limits by deleting critical parameters, polarity details, or edge cases. Never shorten the *substance*—only shorten the *sentences* by splitting them.
- **Flattening Nuance:** Do not apply strict procedural imperatives to strategic essays, creative writing, or nuanced policy trade-offs where ambiguity and weighting are the whole point. Use 80% STE100 for clarity while keeping trade-offs explicit.
- **Code Span Counting:** Never break up a CLI command, URL, or function signature just to satisfy a 20-word rule.
