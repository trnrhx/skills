---
name: ambition-escalator
description: Meta-router and task upgrader based on Felix Rieseberg's Altitude, Depth, and Range framework. Trigger whenever the user asks to "make this more ambitious", "level this up", "apply Altitude/Depth/Range", "10x this workflow", or presents a narrow, low-leverage task that artificially caps scope due to habit or manual execution assumptions.
---

# Ambition Escalator (Meta-Router & Task Upgrader)

## Core Mandate
Identify where human habit, artificial time constraints, or low self-efficacy have capped the scope of a request. Upgrade the task from baseline **Speed** (doing the same manual task faster) across three operational vectors—**Altitude**, **Depth**, and **Range**—while remaining strictly anchored to the user's core objective ("the Why").

## Operational Protocol

### Step 1: Extract the Anchor & Diagnose the Cap
Before proposing upgrades, state in two sentences:
1. **The Requested Artifact:** What the user literally asked for.
2. **The Artificial Cap:** Why the request is scoped too small (e.g., asking for a static template instead of an automated pipeline, summarizing pre-digested text instead of querying raw logs, or defaulting to a standard format like a slide deck).

### Step 2: Run the 3-Vector Escalation Audit
Evaluate the request across all three dimensions of ambition:

#### 1. Altitude (Moving from Task to Outcome & Durable Workflow)
* **The Goal Beyond the Goal:** Treat the requested task as merely the user's best guess at achieving a larger outcome. Identify that underlying outcome.
* **Decision Velocity:** Identify the 3 most urgent decisions that must be made once this task is done, including concrete for/against arguments.
* **Human-Out-of-the-Loop System:** Specify how to replace this one-off request with a continuous, self-verifying workflow connected directly to raw data sources, triggers, and execution environments.
* *Guardrail:* Never escalate Altitude into vague, unverifiable directives (e.g., "go increase revenue"). Anchor Altitude strictly to a concrete decision bottleneck or an auditable system loop.

#### 2. Depth (Top-1% Practitioner Rigor & External Validation)
* **Instrument Upgrade:** Reject surface-level outputs (markdown tables, qualitative guesses, generic summaries). Mandate the exact software, statistical models, code execution, or simulation tools a top 1% specialist uses.
* **Raw Input Ingestion:** Bypass pre-digested summaries. Demand primary source data, raw logs, or commit/version histories to adversarially test conventional wisdom.
* **Unreasonable Constraints:** Set an order-of-magnitude target (e.g., 10x faster, zero manual intervention, predictive proof on historical holdout data) and treat roadblocks as solvable engineering constraints.
* **The Outside Judge:** Prohibit self-grading. Define a deterministic test harness, compiler/benchmark check, or independent adversarial evaluator that proves the output works.

#### 3. Range (Verb Framing & Cross-Disciplinary Execution)
* **Noun-to-Verb Reframe:** Strip away the deliverable noun (e.g., "write a PRD", "build a QBR deck") and replace it with the functional verb/outcome (e.g., "align engineering and product on edge-case trade-offs in 15 minutes", "convince skeptical executives of a causal root cause under uncertainty").
* **The Method Crew:** Select 3–4 distinct specialist disciplines suited to that verb plus 1 adversarial Red Team role (e.g., short seller, forensic accountant, security auditor).
* **Anti-Roleplay Law:** Never write theatrical persona dialogue. Execute the actual investigative, analytical, and production workflows of those disciplines.

### Step 3: Output the Upgraded Execution Plan
Present a structured escalation blueprint containing:
1. **The Core "Why" (Anchor):** The non-negotiable objective that keeps the scope grounded.
2. **Vector Upgrades (Altitude / Depth / Range):** Specific, concrete shifts in inputs, tools, and outputs for each dimension.
3. **Recommended Routing:** Explicitly state which specialized protocol (`altitude-architect`, `depth-enforcer`, `range-crew`, or a chained sequence) to execute immediately, and begin executing the highest-leverage first step unless the user needs to supply missing raw data.
