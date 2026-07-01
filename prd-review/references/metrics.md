# Product Metrics Reference

How to define how success is measured — *before* building. Output language follows the user.

## Contents
- North Star metric
- AARRR (pirate metrics)
- HEART framework
- Leading vs. lagging, input vs. output
- Guardrail metrics
- Metrics for AI/ML & internal-efficiency products
- What makes a good metric / anti-patterns

---

## North Star metric (NSM)
A single metric that best captures the **core value users get** from the product. Everything else
should ladder up to it. It should reflect *value delivered*, not just activity.

Examples by product type:
- Marketplace → transactions completed
- Communication tool → messages sent between active users
- Content platform → time spent on meaningful content / weekly active creators
- B2B SaaS → weekly active teams completing the core workflow
- Internal / efficiency / automation tool → **straight-through rate** (tasks completed with **no human
  correction**) or **hours saved per week**

Test: if this number goes up, are users genuinely better off? If yes, it's a good NSM.

## AARRR — "Pirate Metrics" (funnel view)
Track the user lifecycle as a funnel:
1. **Acquisition** — how users find you.
2. **Activation** — first successful experience ("aha" moment).
3. **Retention** — they come back.
4. **Referral** — they bring others.
5. **Revenue** — they (or someone) pay.

Use to locate the *biggest leak*. Retention is usually the truest signal of product-market fit — fix
it before pouring effort into acquisition.

## HEART framework (UX quality, from Google)
For measuring user experience quality of a feature/product:
- **Happiness** — satisfaction (surveys, NPS, ratings).
- **Engagement** — depth/frequency of use.
- **Adoption** — new users taking up the feature.
- **Retention** — users staying over time.
- **Task success** — efficiency, error rate, completion.

For each chosen dimension, define **Goals → Signals → Metrics** (what success means → observable
behavior → the number you track).

---

## Leading vs. lagging, input vs. output
- **Lagging / output** metrics (revenue, retention) confirm results but move slowly and are hard to act
  on directly.
- **Leading / input** metrics (activation rate, # of key actions per user) move first and are
  controllable — these are what a team steers by week to week.

Deliverable pattern: **1 North Star (output)** + **2–4 input metrics** the team can actually influence.

## Guardrail metrics
Metrics you commit *not to harm* while chasing a goal. Prevent local wins that cause global damage.
Examples: latency, error rate, support ticket volume, churn, unsubscribe rate. Always pair an
optimization target with at least one guardrail.

---

## Metrics for AI/ML & internal-efficiency products
The classic funnel (AARRR) assumes a consumer/growth product with acquisition and revenue. Two very
common product types don't fit it — measure them on their own terms.

**Internal / efficiency / automation tools** (the goal is to save work, not win users):
- **North Star:** straight-through rate (% of items processed end-to-end with no human fix) or hours
  saved per period. These capture the actual value — labor removed.
- **Input:** per-step automation/accuracy rate; adoption among the internal users who must use it.
- **Guardrail:** error/rework rate (a "win" that creates downstream cleanup is a loss); manual-review
  burden (it should fall, not just shift). Acquisition / referral / revenue simply don't apply — don't
  force them.

**AI/ML features** need precision/recall, eval sets, confidence thresholds, error-cost asymmetry, and
a `needs_review` guardrail rather than a single "accuracy" number. These deserve their own treatment —
see `references/ai-ml-products.md`. Key reminder that belongs here: if there's **no baseline and no way
to measure** the model yet, the first deliverable is to **instrument + build an eval set**, not to set
a target blind.

## What makes a good metric
- **Specific & unambiguous** — everyone computes it the same way (write the exact definition).
- **Actionable** — a team can move it through their work.
- **Attributable** — changes can be traced to the work, not just background noise.
- **Hard to game** — gaming it shouldn't be easier than delivering real value.
- **Has a baseline & target** — "increase activation from 28% → 40% in Q3", not "improve activation".
  If there's no baseline, the first task is to **instrument and measure**, not to set a target blind.

## Anti-patterns
- **Vanity metrics** — big numbers that don't inform decisions (raw pageviews, total signups ever,
  cumulative downloads). If a metric can only go up and never informs a choice, drop it.
- **Single-metric tunnel vision** — optimizing one number while guardrails quietly degrade.
- **Metric without a decision** — if no decision changes based on the metric, don't track it.
- **Confusing activity with value** — "users clicked a lot" ≠ "users succeeded".
