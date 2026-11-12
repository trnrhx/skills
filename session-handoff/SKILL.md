---
name: session-handoff
description: Generate a lossless, zero-fluff session handoff document (HANDOFF.md) and bootstrap prompt to migrate a saturated or long conversation into a fresh thread without losing architectural decisions, verbatim user constraints, failed approaches, or unpersisted artifacts. Trigger whenever the user mentions "handoff", "/handoff", "session handoff", "context window is full/slow/laggy", "start a fresh chat/session", "compact this thread", "prepare a continuation prompt", or wants to carry state across Gemini web, Antigravity CLI, or Claude Code.
---

# Session Handoff

Create a high-signal, zero-hallucination state transfer package so a new agent session can pick up immediately where the current conversation stops without repeating dead ends or losing subtle constraints.

## Summary

Standard conversation summaries fail because they abstract away exact constants, soften user constraints through paraphrasing, and omit failed approaches. This skill mines the full conversation chronologically, verifies workspace state when running in a CLI, and produces an adaptive handoff package that works seamlessly across **Gemini Web**, **Antigravity CLI (`agy`)**, and **Claude Code**.

## When to Use

- The user asks for a `/handoff`, `session handoff`, `continuation prompt`, or `HANDOFF.md`.
- The conversation has grown long, slow, or token-heavy and the user wants to migrate to a fresh thread.
- The user wants to transfer work between harnesses (e.g., moving from a Gemini Web design session into Antigravity CLI or Claude Code for execution, or vice versa).
- A complex milestone or debugging loop just concluded and state needs to be checkpointed before pivoting.

## Core Principles

1. **Enumerate, Never Summarize:** Do not write high-level prose like "We calibrated the parameters and fixed the parser." Record the exact parameter values, byte offsets, schema definitions, and root causes.
2. **Preserve Verbatim User Constraints:** Paraphrasing weakens negative constraints and style rules. Quote the user's standing instructions, corrections, and preferences word-for-word.
3. **Log Dead Ends ("What We Tried & Rejected"):** Failed approaches are the most expensive context to rediscover. Explicitly list every rejected library, architecture, hypothesis, or fix along with the exact failure reason so the incoming agent never suggests it again.
4. **Epistemic State Tagging:** Never let an incoming agent inherit an unverified assumption as fact. Tag every claim in the State of Play with:
   - `[VERIFIED]` - Confirmed via test execution, build output, file inspection, or user confirmation.
   - `[INFERRED]` - Believed true based on code reading or reasoning, but not directly executed/tested.
   - `[UNTESTED]` - Written or proposed in this session, awaiting verification.
5. **Harness-Adaptive Artifact Handling:**
   - **CLI / Filesystem Mode (Antigravity CLI, Claude Code, Gemini CLI):** If you have shell and filesystem tools, run non-destructive status checks (`git status -sb`, `git diff --stat`, `git branch --show-current`) first. Reference saved code by file path and symbol/line range rather than pasting large blocks. Write the full handoff to `HANDOFF.md` (or `.agents/handoffs/HANDOFF.md` if preferred by the repo) and output a short pasteable bootstrap prompt in chat.
   - **Web / Sandboxed Chat Mode (Gemini Web, Claude Web):** The conversation *is* the filesystem. Any code, schema, config, or formula crafted in the thread that does not live in an external repository must be preserved verbatim inside the handoff block so nothing is lost when the tab closes. Output the entire handoff inside a single copy-pasteable markdown code block.

## Workflow Steps

### Step 1: Silent Chronological Audit & Environment Check

Before generating the handoff, review the conversation from Turn 1 forward (countering recency bias):

1. **Detect Environment:** Check if shell/filesystem tools are available and whether a project workspace is active.
   - If in a CLI workspace, run `git status -sb` and `git diff --stat` (if git is initialized) to ground the handoff in actual disk state rather than conversational memory.
2. **Extract P0 Items (Fatal if Lost):**
   - Primary goal and current milestone.
   - Exact user corrections, frustrations, or standing constraints ("never do X", "always use Y").
   - Hard-won constants, magic numbers, CLI flags, environment quirks, and edge cases (scan turns for words like *"except"*, *"unless"*, *"actually"*, *"wait"*, *"subtlety"*).
   - Unsaved artifacts (code snippets, schemas, prompts) that exist only in the chat transcript.
3. **Extract P1 Items (High-Leverage Efficiency):**
   - Rejected approaches and dead-end debugging paths.
   - Architectural trade-offs and why option A beat option B.

### Step 2: Generate the Handoff Document

Use the exact 7-section structure below. Keep bullets dense, concrete, and scannable. Never include secrets, API keys, or bearer tokens (replace with `<REDACTED_KEY>`).

```markdown
# SESSION HANDOFF: [Concise Project/Task Title]
**Date:** YYYY-MM-DD | **Source Harness:** [Gemini Web / Antigravity CLI / Claude Code] | **Branch/Path:** [branch or N/A]

> **Incoming Agent Preamble:** Orient before acting. Claims below are tagged `[VERIFIED]`, `[INFERRED]`, or `[UNTESTED]`. Do not trust `[INFERRED]` or `[UNTESTED]` items without checking disk state or asking the user. Never repeat an approach listed in Section 4.

## 1. Objective & Current Phase
- **End Goal:** [1-2 sentences defining the overall mission]
- **Current Phase:** [Where we are right now and why this handoff is happening]

## 2. Verbatim User Rules & Standing Constraints (P0)
- "[Exact quote or strict constraint from user]" - Context/why it matters
- "[Exact quote or strict constraint from user]"

## 3. State of Play & Epistemic Ground Truth
- `[VERIFIED]` [Concrete completed item, citing file path/symbol or verified chat artifact]
- `[INFERRED]` [Suspected state or diagnosis not yet confirmed by a test run]
- `[UNTESTED]` [Code/changes drafted or edited at the end of the session that still need verification]

## 4. What We Tried & Rejected (Dead Ends - DO NOT REPEAT)
- **[Approach / Library / Fix Attempted]:** Failed/rejected because [exact error message, performance bottleneck, or architectural mismatch].

## 5. Key Decisions, Constants & Edge Cases
- **[Decision/Constant Name]:** Chose [X] over [Y] because [reason]. Exact value/signature: `...`
- **Edge Case:** [Specific boundary condition discovered during the session]

## 6. Artifact Manifest (Files or Inline Payloads)
- *If CLI/Repo:* List modified/untracked files (`path/to/file.ts` - what changed) and key reference files (`path/to/ref.ts:45-90`).
- *If Web Chat / Unpersisted Work:* Include the latest canonical version of any unsaved code, schema, config, or prompt developed in this thread so the next session has the complete payload.

## 7. Immediate Next Steps (Ordered Action Plan)
1. **First Action (Verification/Setup):** [Exact command to run or first logical step]
2. **Next Implementation Step:** [Concrete task]
3. **Open Questions / Blockers:** [Any unresolved decisions requiring user input]
```

### Step 3: Deliver According to Harness

- **When running in Antigravity CLI or Claude Code (Filesystem Available):**
  1. Write the complete handoff document to `HANDOFF.md` in the workspace root (unless the user specifies another path).
  2. Print a concise summary in chat followed by a **4-line Copy-Paste Bootstrap Prompt** inside a fenced code block that the user can paste into a new CLI or web session:
     ```markdown
     Read `HANDOFF.md` in the workspace root to restore session context.
     1. Verify current git/file state against Section 3 (`[UNTESTED]` / `[INFERRED]` items).
     2. Respect all verbatim rules in Section 2 and do not repeat rejected approaches in Section 4.
     3. Execute Step 1 of Section 7 ("Immediate Next Steps") or confirm readiness.
     ```
- **When running in Gemini Web (or when no local workspace exists):**
  1. Output the **entire Handoff Document wrapped inside a single copy-pasteable `markdown` code block**, preceded by a 2-line instruction header at the top of the block so the user can copy once, open a fresh chat, paste, and hit Enter.

## Gotchas

- **Nested Code Fence Collision in Web Mode:** When outputting the entire handoff inside a single copy-pasteable code block in Gemini Web, wrap the outer container in a four-backtick fence (` ````markdown `) so inner three-backtick code blocks (` ``` `) do not prematurely close the copy box.
- **Skill Shadowing in Claude Code / Antigravity:** Do not fall back to generic 1-paragraph compaction summaries when the user asks for a handoff. Always run the full 7-section structure with epistemic tags.
- **Hallucinating File Saves:** Never claim a change is saved to disk if you only output the code in chat. Mark chat-only code as `[UNTESTED] (In-Chat Artifact Only)` and include it in Section 6.
