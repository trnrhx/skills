# Custom AI Skills Repository

A modular library of specialized agent skills and behavioral specifications (`SKILL.md`) designed to steer large language models (LLMs) and autonomous agents toward higher operational rigor, deterministic task routing, and human-like output quality.

---

## Overview

This repository houses self-contained, domain-specific instruction sets that extend agent capabilities beyond generic conversational defaults. Rather than relying on massive, monolithic system prompts, each skill functions as an on-demand plugin that agents can dynamically discover, load, and execute based on user intent and task complexity.

The skills within this collection generally fall across two primary operational domains:

* **Execution & Ambition Governance:** Frameworks that enforce depth, prevent premature task truncation, scale problem-solving altitude, and coordinate structured multi-agent workflows.
* **Content Quality & Stylistic Guardrails:** Heuristics and constraints designed to identify machine tells, eliminate repetitive synthetic tropes and cliché nomenclature, and enforce authentic voice.

---

## Repository Structure

Each custom skill resides in its own isolated directory named after its canonical identifier, containing a primary `SKILL.md` definition file alongside any relevant reference documentation:

```text
skills/
├── <skill-identifier>/
│   ├── SKILL.md              # Core skill frontmatter and behavioral guidelines
│   ├── references/           # (Optional) Domain references, examples, or schemas
│   └── scripts/              # (Optional) Supporting deterministic scripts/tools
├── readme.md                 # Repository index and usage guidelines

```

---

## Skill Architecture (`SKILL.md`)

All skills adhere to the standard agent skill specification:

1. **YAML Frontmatter:** Declares the skill name, semantic scope, and explicit trigger conditions used by agent discovery providers to determine when activation is appropriate.
2. **Operational Directives:** Clear, declarative behavioral constraints, procedural steps, evaluation criteria, and edge-case handling.
3. **Execution Guardrails:** Clear boundaries defining what the skill must enforce, avoid, or reject.

```yaml
---
name: skill-identifier
description: Clear, 1–2 sentence description of what the skill does and specific triggers for when the agent should activate it.
---

## Objective
Direct statement of the skill's purpose.

## Operational Rules
- Concrete rule 1
- Concrete rule 2

## Guardrails & Output Standards
- Required format specifications, exclusions, or validation steps.

```

---

## Integration & Consumption

These skills can be consumed across multiple agent runtimes and orchestration harnesses:

* **Agent Runtimes (Claude Code, OpenClaw, Codex CLI):** Point your runtime's skill provider directly to the repository root directory. The runtime parses subdirectories to expose available capabilities dynamically.
* **Orchestration Pipelines:** Ingest individual `SKILL.md` files programmatically into context windows as ephemeral role overlays or sub-agent prompt templates.
* **Direct Instruction Overlay:** Embed select skill bodies directly into custom system instructions or workspace rule configurations.

---

## Adding New Skills

To add a new capability without disrupting downstream discovery engines:

1. Create a root-level directory using lowercase alphanumeric characters and hyphens:
```bash
mkdir <new-skill-identifier>

```


2. Create `SKILL.md` within the directory:
```bash
touch <new-skill-identifier>/SKILL.md

```


3. Populate the frontmatter with an exact name match (`name: <new-skill-identifier>`) and specific trigger keywords in the `description`.
4. Keep core instructions declarative, concise, and focused on a single operational domain. Move supplementary checklists or schemas into a `references/` subfolder if the file exceeds standard context budgets.

---
