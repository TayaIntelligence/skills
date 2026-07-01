# AI / ML Product Reference

How to spec, measure, and de-risk a feature whose core is a model (LLM, OCR, classifier, extractor,
recommender) rather than deterministic code. Output language follows the user (中文 by default);
write plainly and avoid pairing terms with English — keep a term only when it's genuinely standard,
glossed once.

> **Why AI features need a different PM lens.** Normal software is deterministic: it works or it has a
> bug. A model produces a *distribution* of outcomes — you don't ship "it's correct", you ship "it's
> correct ~92% of the time, and here's what happens the other 8%." That single fact reshapes how you
> write acceptance criteria, choose metrics, set guardrails, and plan dependencies. Pull from this
> whenever the feature involves extraction, classification, generation, matching, OCR, or any
> "the AI figures it out" step.

## Contents
- Reframing "done" for a probabilistic feature
- Evaluation protocol (the eval set is the spec)
- Which metric: accuracy vs. precision/recall
- Error-cost asymmetry
- Confidence thresholds & human-in-the-loop
- Fallback, degradation, and AI-specific guardrails
- Data & material dependencies as schedule gates

---

## Reframing "done" for a probabilistic feature
You cannot write an acceptance criterion like *"Then the extracted date is correct"* — there's no
deterministic guarantee to test. Replace it with a **measured bar on a representative set**:

> **Given** the benchmark set of N real documents, **When** the service runs, **Then** field-level
> accuracy ≥ 90% and every low-confidence case is flagged `needs_review`.

So the acceptance criterion for an AI feature is a triple: **(eval set) + (metric) + (threshold)**,
plus the behavior on the misses. If you can't yet measure it, that's the finding — see "instrument
first" below.

## Evaluation protocol — the eval set IS the spec
The labeled evaluation set defines what "good" means more precisely than any prose. Nail down:
- **Ground truth / 标注集** — who provides the correct answers, and are they trustworthy? (Disagreement
  among human labelers caps the accuracy you can fairly demand.)
- **Representativeness** — does the set reflect real input quality? (If production is messy phone
  photos but the eval set is clean PDFs, your 95% is fiction.) Cover the hard cases on purpose.
- **Holdout** — keep a test slice the prompt/model is never tuned on, so the score isn't overfit.
- **Per-class breakdown** — report accuracy per document type / field, not one blended number; the
  blend hides the categories that are failing.
- **"Currently un-evaluable" is a real status.** If there's no eval method or no labels yet, the first
  task is to **instrument and measure**, not to promise a target. Setting "≥85%" with no way to
  measure it is a fictional commitment — say so in the PRD as a risk.

## Which metric: accuracy vs. precision/recall
Plain **accuracy** (% correct) misleads under class imbalance. Classic trap: a field that *should be
empty 99% of the time*. A model that always outputs "empty" scores 99% accuracy while being useless —
and a model that over-eagerly fills it produces a flood of wrong data at "only" a few % error.

Use the confusion-matrix family instead:
- **Precision** — of the items the model flagged/filled, how many were right? (Optimize when **false
  positives are expensive** — e.g. wrongly auto-filing a document, fabricating a value.)
- **Recall** — of the items that should have been flagged/filled, how many did it catch? (Optimize when
  **misses are expensive** — e.g. failing to catch a quality defect before archival.)
- **F1** — harmonic mean, when you need to balance both.

State which one matters for *this* feature and *why* — that choice is a product decision, not a
modeling detail.

## Error-cost asymmetry
A false positive and a false negative rarely cost the same. Make the asymmetry explicit and let it
drive the target:
- Wrong auto-archive that passes review → expensive (compliance, rework) → favor **precision**, set a
  hard cap on "错误归档率".
- A quality pre-check that nags on a clean page (false positive) → mildly annoying; one that lets a
  blank page through (false negative) → worse. Set **误报率 / 漏报率 separately** (e.g. 误报 < 5%,
  漏报 < 10%) rather than a single accuracy number.

## Confidence thresholds & human-in-the-loop
Most models can emit a **confidence**. Use it to split traffic instead of trusting everything:
- Above threshold → auto-apply (or auto-suggest); below → route to a human (`needs_review`).
- The threshold is a **dial between automation rate and error rate** — raising it means fewer auto
  errors but more manual review. Tune it on the eval set against the cost asymmetry above; don't pick
  0.7 by vibes.
- **Human-in-the-loop pattern:** AI proposes + gives a reason; human confirms; the system executes.
  Design the confirmation UX (highlight the `needs_review` fields, show the AI's reason). This is how
  you ship an 85%-accurate model responsibly.
- **`needs_review` ratio is itself a guardrail metric:** too high → the AI isn't saving work; too low
  → it may be silently confident-and-wrong. Watch both ends.

## Fallback, degradation, and AI-specific guardrails
- **Never fabricate.** When the model can't determine a value, returning `null` + a reason beats a
  confident guess. Bake "don't strain to output" into the spec for low-confidence cases.
- **Graceful fallback** — on model failure or low confidence, fall back to manual entry, not a crash.
- **AI-specific guardrails to track:** hallucination / fabricated fields, silent errors (wrong but
  high-confidence), and **drift** — when the world changes under the model (templates, SOPs, rules get
  updated), accuracy decays. Plan for re-evaluation, not a one-time launch.

## Data & material dependencies are schedule gates
AI features run on data and rules the model doesn't invent: labeled eval sets, mapping/rule tables,
reference taxonomies, ground-truth examples. These are not "nice to have later" — **a missing rule
table or labeled set blocks the build and caps the accuracy.** Treat each as a P0 dependency with an
owner and a due date, and gate the phase on it. The most common reason an AI PRD slips is not the model
— it's that the business material never arrived.
