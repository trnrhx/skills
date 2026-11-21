# Consumer Research

Simulate a representative synthetic consumer panel to test product concepts, estimate purchase intent distributions, and surface candid qualitative feedback. Based on the Semantic Similarity Rating (SSR) methodology (Maier et al., 2025), this workflow avoids the regression-to-the-mean bias of direct numerical LLM ratings by eliciting unconstrained free-text reactions from demographically grounded personas and mapping those reactions onto a 5-point Likert distribution.

## When to Use

- Testing an early-stage product concept, service, feature, or marketing pitch before investing in human panels
- Comparing two or more product variants, positioning angles, or price tiers (A/B concept ranking)
- Identifying likely target segments, price sensitivity, and candid objections for a new idea
- Running a rapid synthetic focus group to generate qualitative product refinements

## Workflow

### Step 1: Clarify the Stimulus and Test Scope

If the user has not provided enough detail to evaluate the concept, ask briefly for:

- **Product concept**: Core value proposition, key features, and target price point (or concept image/slide if available)
- **Research goal**: Single-concept evaluation, multi-concept ranking, or pricing/positioning test
- **Primary metric**: Default to **Purchase Intent (PI)** ("How likely would you be to purchase this product?"). Optionally add **Concept Relevance** ("How relevant is this concept to you?").

If the user already provided a clear product description, proceed directly to Step 2.

### Step 2: Identify the Target Market and Build the Synthetic Panel

Define the likely market for the product and construct a diverse panel of **8 to 12 synthetic consumer personas** (or more if requested) that reflect the real-world buyer distribution, including both core target buyers and adjacent skeptics.

Condition every persona with explicit attributes. Prioritize **age** and **income/budgetary comfort**, as these drive the strongest, most human-like variance in purchase intent:

- **Age & Life Stage**: Specific age and household context (e.g., 26 single renter, 42 parent of two, 67 retiree)
- **Income & Budget Attitude**: Concrete financial posture (e.g., tight budget/price-sensitive, comfortable middle-income, value-conscious, or discretionary/premium spender)
- **Category Background & Habits**: Current solution they use, switching friction, and domain familiarity
- **Core Attitude**: Skepticism level, brand loyalty, or openness to novelty

Never omit demographic conditioning. Unconditioned LLM personas default to generic positivity and lose ranking signal across concepts.

### Step 3: Elicit Unconstrained Free-Text Reactions

For each persona, generate a brief, candid **2 to 4 sentence free-text response** to the purchase intent question from that persona's first-person perspective.

- **Do NOT ask the persona for a 1-5 number.** Direct numerical elicitation causes LLMs to cluster artificially around "3" and destroys distributional realism.
- Encourage honest, persona-authentic reactions: mention specific features that appeal, price/budget friction, skepticism toward buzzwords, or why their current alternative is good enough.

### Step 4: Map Text to Likert Distributions (Simplified SSR)

Act as a separate, neutral **Semantic Rating Evaluator** to map each persona's free-text response onto a 5-point Likert probability distribution (PMF) summing to 100%, rather than a single rigid integer. This preserves the natural ambiguity of human language (e.g., "I'd probably try it if it's on sale" splits weight across 3 and 4).

Evaluate each response against these standardized anchor statements:

- **1 (Definitely Not)**: "It is very unlikely I would buy this; I have no interest or it does not fit my needs/budget."
- **2 (Probably Not)**: "I probably would not buy this; the drawbacks or price outweigh the appeal."
- **3 (Might or Might Not)**: "I am indifferent or unsure; I might consider it only under specific conditions."
- **4 (Probably Yes)**: "I am interested and would likely buy or try this if available."
- **5 (Definitely Yes)**: "It is very likely I would buy this; it strongly fits my needs and feels worth the price."

For each persona:

1. Assign a probability distribution `[p1, p2, p3, p4, p5]` summing to `1.00` based on semantic alignment with the five anchors.
2. Compute the persona's expected score: `E[PI] = 1*p1 + 2*p2 + 3*p3 + 4*p4 + 5*p5`.

*(Optional: If the user is running this in a coding environment with an embedding model available, offer a short Python snippet using cosine similarity against the anchor embeddings with minimum-similarity subtraction and normalization, matching the exact SSR formula.)*

### Step 5: Aggregate Metrics and Synthesize Insights

Present the results in a clean, decision-ready report:

1. **Executive Verdict**: Overall concept viability, Mean Purchase Intent (1.00 to 5.00), and Top-2-Box share (combined % weight on 4 and 5).
2. **Panel-Level Likert Distribution**: A compact table or ASCII bar chart showing the aggregated % across ratings 1 through 5.
3. **Subgroup Breakdown**: Mean PI stratified by **Age cohort** and **Budget/Income tier** to highlight where the concept wins or stalls.
4. **Persona Panel Log**: A concise table with each persona's profile, candid free-text quote, Likert PMF, and expected PI score.
5. **Qualitative Synthesis**:
   - **Top Value Drivers**: What resonated most strongly
   - **Key Objections & Friction**: Price pushback, skepticism, or missing proof points
   - **Actionable Concept Refinements**: 3 concrete changes to improve purchase intent in the next iteration

## Gotchas

- **Never use direct Likert rating**: Asking a persona "Rate this 1 to 5" produces narrow, artificial clusters around 3. Always separate free-text generation (Step 3) from anchor-based scoring (Step 4).
- **Avoid generic personas**: Vague labels like "Tech Enthusiast" produce flat, overly positive scores. Always ground personas in age, budget constraints, and current alternatives.
- **Respect domain boundaries**: Synthetic panels work best for consumer categories well-represented in public discourse (CPG, software, services, retail). Flag lower confidence for highly niche, novel, or B2B technical markets.
- **Do not over-interpret minor demographic splits**: Age and income/budget patterns align well with human data, whereas gender and geographic region splits are noisier in synthetic panels. Treat synthetic results as directional concept screening, not a final substitute for human validation.
