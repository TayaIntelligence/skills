# Discovery & Requirements Reference

Techniques for getting from a vague idea to a sharp, validated problem and a well-scoped solution.
Output language follows the user.

## Contents
- Problem framing
- 5 Whys
- Jobs To Be Done (JTBD)
- Personas
- Separating problem from solution
- Requirement quality (INVEST + AC)
- MVP definition techniques

---

## Problem framing
A crisp problem statement unblocks everything downstream. Aim for one sentence:

> **[User/persona]** struggles to **[do something]** because **[obstacle]**, which leads to
> **[negative consequence]**.

If you can't fill this in, you're not ready to design a solution — keep asking. A good problem
statement is solution-agnostic (it doesn't smuggle in the answer) and points at a real consequence.

## 5 Whys
When a request arrives as a *solution* ("add a CSV export button"), ask "why?" repeatedly to reach the
real need:
- Why a CSV button? → to get data into their reporting tool
- Why into their tool? → to build a weekly board deck
- Why manually? → no live connection exists
…which might reveal the real job is "keep leadership informed weekly" — possibly better solved by a
scheduled report or integration than a button. Stop when further "why" leaves the product's scope.

## Jobs To Be Done (JTBD)
Frame needs as *jobs* users hire the product to do, independent of any feature:

> When **[situation]**, I want to **[motivation]**, so I can **[expected outcome]**.

JTBD keeps focus on the outcome the user is trying to achieve, which is more stable than any specific
feature idea. Different features can serve the same job; pick the one with the best value/effort.

## Personas
A lightweight persona is enough: who they are, their goal, their context/constraints, and their
frustration today. Use them to pressure-test scope ("does this story serve our primary persona, or a
fringe case?"). Don't over-invest in elaborate persona docs — they should sharpen decisions, not
decorate slides.

## Separating problem from solution
Most stakeholder requests are pre-baked solutions. The PM move is to:
1. Accept the request *as a signal* of an underlying problem.
2. Restate the problem in solution-free language.
3. Generate 2–3 candidate solutions (the requested one + alternatives).
4. Compare on value, effort, and risk before committing.

This is where a technical PM earns trust — often a simpler technical approach solves the same problem.

---

## Requirement quality
Good requirements are **testable**. "The system should be fast" is not a requirement; "search results
return in < 500ms at the 95th percentile" is.

**INVEST** for user stories — Independent, Negotiable, Valuable, Estimable, Small, Testable. Fails
"Small/Estimable" → it's an epic, split it.

**Acceptance Criteria** — write per story, preferably:
> **Given** [context/precondition], **When** [action], **Then** [observable result].

Cover the happy path *and* key edge/error cases. AC are story-specific; the **Definition of Done** is
the team-wide quality bar (tests pass, integrated, documented, deployable) applied to every story.

---

## MVP definition techniques
The MVP is the **smallest thing that delivers real value and lets you learn** — not a half-built
product.

- **Walking skeleton** — build the thinnest end-to-end path first (all layers, minimal features), then
  add depth. De-risks integration early.
- **Slice vertically, not horizontally** — ship one complete feature for real users, not "all the
  backend, no UI". A vertical slice is usable and testable.
- **Define the learning goal** — what hypothesis does this MVP test? Scope to answer that, nothing more.
- **Name what's out** — explicitly list deferred features so "MVP" doesn't quietly expand (scope creep
  / gold-plating are the top killers of MVPs).
- **Rolling-wave planning** — detail only the near-term work; keep later phases coarse and decide at
  the "last responsible moment" to avoid rework.
