---
name: ai-text-detector
description: Audit articles, posts, webpages, or text passages during AI cobrowsing or review to assess the likelihood of AI generation and pinpoint specific LLM cliches, structural formulas, and stylistic tells. Trigger when the user asks to check if a page or text is AI-written, analyze AI tells, spot LLM slop, or audit an open tab for synthetic writing.
---

# AI Text Detector

Perform a forensic stylistic audit on any webpage, article, social post, or text passage to estimate the probability of LLM generation and extract concrete textual evidence of AI writing tropes.

## When to Use

- Auditing an open webpage, blog post, or social thread during AI cobrowsing to see if it is AI-generated
- Analyzing pasted text, articles, or essays for signs of LLM generation ("AI slop" or unedited synthetic prose)
- Preparing a diagnostic breakdown of AI tells before generating counter-guardrails

## Audit Workflow

### Step 1:Ingest and Contextualize

1. Identify the text source (active cobrowsing tab, selected passage, or pasted text) and its genre (news article, LinkedIn post, technical documentation, fiction, academic essay, or Reddit/forum comment).
2. Note baseline expectations: corporate press releases and SEO blogs naturally share some stiffness with LLMs, so require multiple converging signal categories before assigning a high probability.

### Step 2: Scan Across the 6 Diagnostic Dimensions

Evaluate the text against these specific LLM signal clusters and extract exact quotes for every hit:

1. **Rhetorical & Syntactic Formulas**
   - **Negative Parallelisms ("Not X, but Y"):** "It's not just a tool; it's a paradigm shift," "Not because of X, but because of Y," "Less about X and more about Y."
   - **Forced Triads:** Compulsive grouping of three adjectives, verbs, or clauses in nearly every paragraph ("fast, scalable, and secure").
   - **Participial Phrase Tacking:** Ending declarative sentences with "-ing" clauses that state vague significance ("...fostering a sense of community," "...underscoring the importance of innovation," "...ensuring long-term success").
   - **Rhetorical Self-Q&A:** "The result? A seamless experience." / "The catch? It requires discipline." / "Sound familiar?"

2. **Lexical Over-Representation (Stock AI Vocabulary)**
   - **High-Frequency Verbs:** Delve, foster, underscore, testament (as noun), showcase, harness, navigate, unpack, spearhead, elevate, revolutionize, streamline, bolster.
   - **High-Frequency Adjectives/Nouns:** Tapestry, landscape, realm, beacon, symphony, crucible, multifaceted, intricate, pivotal, vibrant, bustling, seamless, game-changer, treasure trove, stark contrast, indelible mark.
   - **Transition Crutches:** Moreover, furthermore, consequently, in light of this, it is worth noting that, notably, crucially, at its core.

3. **Structural & Formatting Symmetry**
   - **Staged Openers:** Starting with "In today's fast-paced digital landscape," "In an era where," "Imagine a world where," or "Picture this:".
   - **Bold-Colon List Syndrome:** Bullet lists where every single item follows `[Bold 2-3 Word Concept]: [1-sentence explanation of uniform length]`.
   - **Paragraph Rhythm Flattening:** Uniform 3-to-4 sentence paragraphs with near-identical sentence lengths (low burstiness).
   - **Moralizing or Neat Bow-Tie Closers:** Concluding with "Ultimately, the future of X depends on how we balance..." or "In conclusion, X is more than just Y—it's a reminder that...".

4. **Punctuation & Typographic Quirks**
   - **Em-Dash Saturation:** Using em-dashes (`—`) in almost every paragraph as a dramatic pause or parenthetical pivot.
   - **Colon-Heavy Subheadings:** Every H2/H3 structured as `Catchy Metaphor: Literal Explanation` (e.g., *Beyond the Hype: How AI Reshapes Work*).

5. **Epistemic Hedging & Inflation ("Both-Sidesism")**
   - **Inflated Significance:** Treating mundane topics as historic turning points ("stands as a testament to human ingenuity").
   - **Empty Balance:** "While X offers significant benefits, it is not without its challenges."
   - **Absent First-Hand Specificity:** Lack of messy real-world friction, exact dates, idiosyncratic opinions, sensory details, or unpolished phrasing.

6. **Onomastic Tells (If Fictional or Case-Study)**
   - Default AI names (Elara, Elias, Maya Chen, Marcus Vance, Thorne, Voss, Lumina, Nexus, Oakhaven).

### Step 3: Score and Report

Format the output cleanly for fast reading during cobrowsing:

1. **Verdict Header:** State the **Classification** (`Likely Human`, `Hybrid / AI-Assisted`, or `Heavily AI-Generated`) and a **Confidence Tier** (`Low`, `Moderate`, `High`, `Near-Certain`), followed by a 1-2 sentence executive summary of why.
2. **Evidence Table:** Present a Markdown table with columns: `Signal Category`, `Exact Quote from Text`, and `Why It Signals AI`.
3. **Burstiness & Specificity Check:** Briefly note whether the text exhibits human irregularities (varied sentence lengths, concrete unglamorous details, genuine voice) or uniform statistical smoothing.
4. **Handoff Prompt:** Offer to pass the detected tells into the `ai-tells-to-guardrails` skill to generate custom anti-AI system instructions.

## Gotchas

- **False Positives on Non-Native English Writers:** Non-native speakers who learned formal academic transitions ("Moreover," "Furthermore") can trigger lexical flags. Look for syntactic templates ("Not X, but Y", participial tails, uniform burstiness) before calling a text AI-generated.
- **Heavily Edited Corporate Copy:** Human PR teams write stiff, hedged prose. Distinguish human corporate jargon from LLM rhythm by checking for symmetrical bullet structures and compulsive triads.
