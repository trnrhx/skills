---
name: ui-design-architect
description: End-to-end UI and web design workflow for building distinctive, accessible, production-grade interfaces without generic AI styling. Works in CLI agents (Claude Code, Gemini CLI) and Chat/Web UIs (Artifacts, Canvas).
---

# UI Design Architect

## Core Principle
Never default to generic AI templates. Every interface must pass through structured context lock, an explicit pre-code design plan, multi-variant exploration, an engineering/accessibility audit, and a visual QA or live-tuning loop.

## Environment Adaptation
Detect the active environment and adapt execution automatically:
- **CLI / Local Repo (Claude Code, Gemini CLI, Cursor):** Write modular files, start the local dev server, use browser/Playwright tools to inspect desktop (`1440px`) and mobile (`390px`) screenshots, and build a multi-route comparison view.
- **Chat / Web (Artifacts, Canvas, Standard Chat):** Output a single self-contained HTML/CSS/JS or React artifact with a top-bar **Variant Switcher** (for comparing styles side-by-side) and an embedded **Floating Tweaks Bar**. Run a strict static responsive audit in place of headless screenshots.

---

## Phase 1: Intake & Context Lock
Before writing code, establish five inputs. If the user omitted critical details, ask at most 3 concise questions—or state crisp, non-generic assumptions and proceed if the user asked for immediate output:

1. **Product & Audience:** What is being built and who uses it?
2. **Primary User Action (CTA):** The single most important action on the screen.
3. **Aesthetic Direction or UI Library:** A target vibe/reference (URL, screenshot, or aesthetic family) OR a component library style:
   - *Coss UI* (clean, monochrome-first SaaS; requires explicit accent color)
   - *Align UI* (modern, high-contrast, vibrant SaaS)
   - *HeroUI* (soft, Apple-like, consumer-friendly, rounded)
   - *Ant Design* (dense, squared-off, enterprise/corporate)
4. **Always Rules:** Non-negotiable structural or brand elements.
5. **Never Rules (Anti-Slop Bans):** Explicit exclusions.

---

## Phase 2: Design System & Plan Gate (Pre-Code)
Output a concise **Design Spec** before writing any implementation code:

- **Palette (4–6 Semantic Tokens):** `--bg`, `--surface`, `--ink`, `--muted`, `--accent`, `--border` (plus dark-mode parity if applicable).
- **Typography Pairing:** Distinct Display font for headlines + high-legibility Sans or Mono for body/UI. Never use display fonts for small functional data, metrics, or tables.
- **Layout & Rhythm:** Grid structure, border-radius scale, elevation strategy, and whitespace density.
- **Anti-Slop Self-Check (Mandatory Gate):** Verify the plan contains NONE of the following unless explicitly requested:
  - Default Inter/Roboto-only typography with uniform weights
  - Purple-to-blue or orange-to-pink SaaS hero gradients
  - Generic 3-column equal-height feature cards with centered circle icons
  - Decorative 3D SaaS blobs or dated drop shadows
  - Low-contrast pastel status badges or unreadable light text on colored pills
  - *Rule:* If any choice feels like the category default, replace it before coding.

---

## Phase 3: Multi-Variant Exploration (The Funnel)
Avoid one-shotting final designs. Guide the user through a progressive funnel unless they request a single locked build:

1. **Step A — Wide Net (3–5 Distinct Directions):** Generate 3 to 5 genuinely different aesthetic directions (varying typography, layout structure, and surface treatment—not just palette swaps). Present them side-by-side via a comparison page (CLI) or tabbed switcher (Chat/Web).
2. **Step B — Layout Refinement (3 Variations):** Once the user picks a direction, generate 3 structural variations of the body/content layout.
3. **Step C — Asset & Detail Lock:** Lock the chosen layout, then refine hero imagery, data visualizations, and micro-copy.

---

## Phase 4: Engineering, Polish & Accessibility Audit
Before declaring any build finished, run a harsh self-critique (score each view `1–10` on visual hierarchy, craft, and usability) and fix every failure across two checklists:

### Baseline UI Micro-Rules
- **Numerals:** Apply `font-variant-numeric: tabular-nums` to all metrics, counters, prices, and table columns.
- **Typography:** Tighten letter-spacing only on large display headings; keep body copy at `15–16px` minimum with `1.5–1.6` line-height.
- **Dialogs:** Use proper modal confirmation dialogs for destructive actions (never bare inline deletes).
- **Motion:** Keep micro-interactions snappy (`150–200ms`, ease-out). Use heavier, deliberate easing only for page/hero reveals.

### Usability & Accessibility Rules
- **Semantics:** Use real `<input type="checkbox">`, `role="switch"`, `<button>`, and `<label>` elements—never clickable `<div>`s for controls.
- **Keyboard Focus:** Ensure clear `:focus-visible` rings on all interactive elements, and add `scroll-margin-top` so sticky headers never obscure focused controls.
- **Contrast & Touch:** Enforce WCAG AA contrast (`>= 4.5:1` for text, `>= 3:1` for UI components) and `>= 44x44px` touch targets on mobile.

---

## Phase 5: Visual QA & Live Tweaks Bar

### Floating Tweaks Bar (Dev / Preview Mode)
Unless the user asks for production-stripped output, inject a collapsible, bottom-right **Tweaks** button that opens a lightweight overlay bound to CSS custom properties:
- **Controls:** Heading font selector, body font selector, base type scale slider, accent color swatches/picker, border-radius scale, spacing density, and animation speed/weight.
- **Export:** Include a **Copy CSS Tokens** button so the user can paste their finalized visual settings straight back into the prompt or codebase.

### Responsive & Visual QA
- **CLI Mode:** Launch the app in a real browser, capture screenshots at `390x844` (mobile) and `1440x900` (desktop), and inspect for horizontal scrollbars, overflowing text, clipped badges, or unclickable mobile navigation. Fix all defects and re-verify.
- **Chat/Web Mode:** Audit CSS for fixed widths that break `< 400px` viewports, add `min-w-0` on flex/grid children, verify mobile navigation collapse, and invite the user to share a screenshot for targeted fixes.

---

## On-Demand Refinement Commands
Users can invoke these targeted passes at any stage:
- `explore` — Generate 3–5 distinct aesthetic directions side-by-side.
- `bolder` — Push safe hierarchy, scale contrast, and visual personality further without breaking legibility.
- `quieter` — Strip visual noise, soften contrast, and increase breathing room.
- `typeset` — Fix font pairing, type scale, line lengths (`55–75ch`), and tabular numeral alignment.
- `layout` — Rebalance spacing, alignment, grid rhythm, and mobile breakpoints.
- `audit` — Run the full Phase 4 Baseline UI + Accessibility audit and fix all issues.
- `tweaks` — Add or expand the floating CSS-variable Tweaks Bar.
- `polish` — Final pre-ship pass across micro-copy, hover/focus states, transitions, and responsive edge cases.
