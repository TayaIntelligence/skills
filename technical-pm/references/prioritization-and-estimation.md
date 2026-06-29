# Prioritization & Estimation Reference

Frameworks for deciding *what* to build next and *how big* the work is. Output language follows the
user (中文 by default); write plainly and don't pair every term with English — keep a term only when
it's genuinely standard (MoSCoW, RICE, PERT…), glossed once.

## Contents
- Prioritization: RICE, MoSCoW, Kano, Value/Effort, WSJF
- Estimation: Story Points, Velocity, Planning Poker, T-shirt, PERT three-point
- Estimating without history (greenfield / new team)
- Burndown & forecasting
- Selection guide & pitfalls

---

## Prioritization frameworks

### RICE
Best for comparing many candidate features with rough data.

`RICE Score = (Reach × Impact × Confidence) / Effort`

- **Reach** — how many users/events in a period (e.g. users/quarter).
- **Impact** — per-user effect, scored e.g. 3 = massive, 2 = high, 1 = medium, 0.5 = low, 0.25 = minimal.
- **Confidence** — how sure you are, as a % (100% / 80% / 50%). Discounts hand-wavy estimates.
- **Effort** — person-months (or points). The only denominator — bigger effort lowers the score.

**Worked example:** Reach 8,000/qtr · Impact 1 · Confidence 80% · Effort 4 →
`(8000 × 1 × 0.8) / 4 = 1,600`. Compare scores across features; rank descending.

> Use RICE to *rank*, not to decide absolutely. Sanity-check the top of the list against strategy.

### MoSCoW
Best for scoping a *single* release. Classify each item:
- **Must have** — release fails without it.
- **Should have** — important but not vital; a workaround exists.
- **Could have** — desirable, dropped first under pressure.
- **Won't have (this time)** — explicitly deferred; recorded as a future candidate.

Rule of thumb: keep **Must ≤ ~60%** of effort so there's room to flex.

### Kano model
Best for understanding *what kind* of value a feature provides. Categories:
- **Basic / Must-be** — expected; absence causes dissatisfaction, presence is neutral (e.g. login works).
- **Performance** — more is better; satisfaction scales linearly (e.g. speed).
- **Delighters / Attractive** — unexpected value; presence delights, absence is fine.
- **Indifferent / Reverse** — no one cares, or some dislike it.

Implication: cover all Basics first (table stakes), invest in a few Performance + Delighters to
differentiate. Delighters decay into Basics over time.

### Value vs. Effort (2×2)
Fast triage. Plot each item:
- High value / Low effort → **do now (quick wins)**
- High value / High effort → **plan / break down (big bets)**
- Low value / Low effort → **maybe / fill-in**
- Low value / High effort → **avoid (money pit)**

### WSJF (Weighted Shortest Job First)
From SAFe; for *sequencing* under a shared cadence.

`WSJF = Cost of Delay / Job Size`, where Cost of Delay ≈ (User-Business Value + Time Criticality +
Risk Reduction / Opportunity Enablement). Highest WSJF first — favors short, high-urgency jobs.

---

## Estimation

Agile estimation measures **relative complexity, not time**.

### Story Points
Relative effort/complexity of a story. Common scale: **Fibonacci** (1, 2, 3, 5, 8, 13, 21).
A 13+ usually signals a story too big to estimate well — split it.

### Velocity
Story points a team completes per sprint. Use *historical* velocity to forecast.
*Example:* 5 people × 1 pt/person/day × 10-day sprint ≈ 50 pts/sprint. 200 pts of backlog → ~4 sprints.

**Forecasting loop:** write stories → estimate points → forecast from past velocity → measure actual
velocity during the sprint → re-forecast remaining work with the real number.

### Estimating without history (greenfield / new team)
The velocity math above assumes you *have* historical velocity. On a new project or new team you
don't — which is exactly when someone demands a date. Don't fabricate one. Instead:
1. **Still size relatively.** T-shirt (XS–XL) or points to rank work and find the big rocks; you can
   compare sizes without knowing absolute speed.
2. **Give a range, not a point.** Use PERT three-point `(O+4M+P)/6` and state the assumptions, or quote
   a band ("~3–5 sprints, assuming X"). A confident single date built on a velocity you've never
   observed is false precision — the top estimation anti-pattern.
3. **Treat the first forecast as a hypothesis.** Run 1–2 sprints, measure real velocity, then
   re-forecast (rolling-wave: detail the near term, keep later phases coarse).
4. **Gate on dependencies first.** If the estimate hinges on materials/APIs/data that haven't landed,
   the honest answer is "N sprints *after* those land", not a single number that silently assumes they
   already have.
5. **Fix time, flex scope.** Commit to a Sprint Goal / time-box and negotiate scope within it, rather
   than promising fixed scope on a fixed date with unknown velocity.

### Planning Poker
Team members estimate independently and reveal simultaneously, then discuss the high/low outliers to
reach consensus. Fibonacci cards; ∞ = "can't estimate / needs breakdown", coffee cup = "need a break".
Prevents anchoring on the first/loudest estimate.

### T-shirt sizing
XS / S / M / L / XL for fast, coarse estimation of large backlogs. Map sizes to point ranges later if
you need velocity math.

### PERT three-point (for time/cost estimates)
When a *calendar* estimate is needed, gather three points per task:
O (optimistic), M (most likely), P (pessimistic).
- Triangular: `TE = (O + M + P) / 3`
- **PERT (preferred, weights M):** `TE = (O + 4M + P) / 6`

Unit is typically **person-months**. `Headcount = person-months / duration` (then adjust for skills and
availability).

---

## Burndown & forecasting
- **Sprint burndown** — remaining story points vs. days in the sprint; update **daily**:
  `remaining(day X+1) = remaining(day X) − points completed that day`.
- **Release burndown** — remaining scope across the whole release; reveals whether the date holds.
- A flat or rising line = scope creep or blocked work; investigate, don't just report.

---

## Selection guide

| Situation | Use |
|---|---|
| Big backlog, rough data, need a ranking | RICE |
| Carving one release into priorities | MoSCoW |
| Which features differentiate vs. table-stakes | Kano |
| Need a 5-minute triage | Value/Effort 2×2 |
| Sequencing many teams under a cadence | WSJF |
| How big is this agile work | Story Points + velocity |
| Need a date or budget | PERT three-point |

## Pitfalls
- **Analysis paralysis** — over-estimating instead of starting. Estimates are indicative, not contracts.
- **Cavalier approach** — skipping estimation/risk entirely; high blow-up risk.
- **Story-point misuse** — never use points to rank or evaluate *individuals*; it destroys trust and
  invites gaming.
- **False precision** — a confident single number hides uncertainty; prefer ranges and confidence levels.
