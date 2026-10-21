---
name: ai-tells-to-guardrails
description: Generate custom, copy-pasteable system instructions and prompt guardrails from identified AI-generated text or an AI detection audit to prevent those specific LLM cliches, syntactic templates, and stylistic tells in future outputs. Trigger when the user wants to turn detected AI tells or bad AI writing into negative constraints, system prompt rules, or anti-slop instructions.
---

# AI Tells to Guardrails

Convert specific AI-generated text samples or diagnostic findings (such as outputs from `ai-text-detector`) into enforceable, copy-pasteable system instructions that eliminate both the literal cliches and the underlying syntactic patterns that produced them.

## When to Use

- Immediately after running an AI detection audit on a webpage or article to immunize your own prompts against those patterns
- When the user provides a sample of "AI-sounding" writing and asks for system prompt rules to stop an LLM from writing like that
- When hardening a custom agent, voice card, or system instruction against specific genre tropes (e.g., LinkedIn bro-etry, SEO blog slop, synthetic fiction, or overly formal corporate summaries)

## Conversion Workflow

### Step 1: Extract the Pattern Taxonomy

Do not merely ban the exact sentences found in the source text. Generalize every detected flaw into three layers:

1. **Literal Tokens:** Specific overused words, transitions, and stock metaphors present in the sample.
2. **Syntactic Templates:** Sentence-level structural formulas (e.g., negative parallelisms, trailing participial clauses, rhetorical one-word questions, forced triads).
3. **Macro-Architecture & Pacing:** Document-level habits (e.g., throat-clearing openers, `Bold Label: Explanation` bullet lists, uniform paragraph lengths, neat moralizing conclusions).

### Step 2: Pair Every Negative Constraint with a Positive Replacement

LLMs struggle with pure negative constraints ("Do not write like AI") and often drift to the nearest statistical neighbor. For every banned pattern, specify the **mechanical replacement**:

- *Instead of trailing "-ing" significance clauses* -> Stop the sentence at the concrete object or state the exact causal mechanism in a separate sentence.
- *Instead of "Not X, but Y" contrasts* -> State directly what the subject *is* or *does* without using a foil.
- *Instead of uniform `Bold: Description` bullets* -> Write in continuous prose paragraphs, or vary list item structures without bolded lead-ins.
- *Instead of abstract stock verbs ("foster," "navigate," "underscode")* -> Use physical, domain-specific, or plain-English verbs ("build," "pay for," "cut," "measure," "break").

### Step 3: Output the Ready-to-Use Guardrail Block

Deliver the result in two parts:

1. **Quick Pattern Mapping Summary:** A brief list showing how the specific flaws from the analyzed text were translated into rules.
2. **Copy-Paste System Instruction Block:** A clean, self-contained Markdown code block that the user can drop directly into any system prompt, `SKILL.md`, or custom instruction field. Structure the block with:
   - **Banned Lexical Tokens** (grouped by verbs, nouns/adjectives, and transitions)
   - **Banned Syntactic Formulas** (with explicit `Bad -> Good` inline micro-examples derived from the audited text)
   - **Structural & Formatting Rules** (paragraph burstiness, list limits, punctuation caps such as em-dash limits)
   - **Positive Voice Directives** (concrete specificity, direct openings, abrupt/natural endings)

## Gotchas

- **Avoid Meta-Language in the Output Prompt:** Never write rules like "Sound human" or "Avoid AI cliches" inside the generated system instruction. LLMs do not know what "sounding human" means statistically; give them explicit mechanical constraints (e.g., "Maximum one em-dash per 500 words," "Never end a sentence with a participial phrase starting with fostering, ensuring, or underscoring").
- **Prevent Token-Bleed:** Keep banned word lists grouped cleanly and always finish the system instruction block with positive structural directions so the model's attention ends on what *to* generate rather than a wall of forbidden tokens.
