# Delivery & Process Reference

The execution side of the technical-PM role: development models, Scrum, risk, quality, scheduling,
configuration management, and scaling. Condensed for quick reference. Output language follows the user.

> This reference reuses and condenses the team's "软件开发全流程核心知识手册". Pull from the relevant
> section when a request touches process, delivery mechanics, or schedule math.

## Contents
1. SDLC models
2. Agile & Scrum
3. Stakeholders, communication & documentation
4. Risk management
5. Quality management
6. Project scheduling (WBS, critical path, EVA)
7. Configuration management
8. Scaling agile (SAFe)

---

## 1. SDLC models
Two control styles: **Defined** (fixed input → predictable output; low change) vs. **Empirical**
(inspect & adapt in short cycles; high uncertainty — the basis of agile).

| Model | Idea | Use when |
|---|---|---|
| Code & Fix | No structure | Never (toy work only) |
| Waterfall | Sequential, document-driven | Requirements stable & well-understood |
| Incremental | Series of mini-waterfalls adding features | Want early delivery + risk spread |
| Prototyping | Build to clarify requirements | Requirements unclear |
| Spiral | Plan → risk → prototype → build, repeated | High-risk, large projects |

Core phases: Requirements → Design → Implementation → Testing → Deployment & Maintenance.

## 2. Agile & Scrum

**Agile Manifesto** (values, left over right): individuals & interactions > processes & tools; working
software > documentation; customer collaboration > contract negotiation; responding to change >
following a plan.

**Scrum three pillars:** Transparency, Inspection, Adaptation.

**Roles:** Product Owner (owns & orders the Product Backlog, represents value), Scrum Master
(safeguards the process, removes blockers, servant leader), Developers (self-organizing, deliver the
increment).

**Events (all time-boxed):** Sprint (commonly 2 wks) · Sprint Planning · Daily Scrum (15 min) · Sprint
Review (demo + feedback) · Retrospective (improve the process).

**Artifacts:** Product Backlog · Sprint Backlog · Increment. The **Definition of Done (DoD)** is the
quality bar every increment must meet (code complete, tested, integrated, documented, deployable).

**Scrum vs. Waterfall:** stage-driven & document-heavy vs. iteration-driven & change-responsive.

**Frameworks beyond Scrum:** Kanban (visualize flow, limit WIP) · XP (frequent release, pair
programming, continuous feedback).

## 3. Stakeholders, communication & documentation

**Power-Interest grid** — communication posture by quadrant:
- High power / High interest → active engagement, frequent contact, seek input.
- High power / Low interest → concise high-level summaries, keep satisfied.
- Low power / High interest → consult, address concerns, leverage support.
- Low power / Low interest → minimal contact, monitor.

**Engagement levels:** Unaware → Resistant → Neutral → Supportive → Champion.

**Documentation (agile):** "no docs" is a myth; aim for **JBGE — Just Barely Good Enough**. Docs are
required for external/outsourced work, compliance/audit, complex architecture/flows, and team handover.

**Issue management:** record a Discrepancy Report (actual vs. expected, version, reporter, repro), then
classify — observed ≠ specified → **Defect**; observed = specified → **Change Request** (different
process, may add cost, may enter the backlog).

**PMP vs. Charter:** Charter authorizes the project (a few pages, at kickoff); the Project Management
Plan defines how it's executed/monitored/controlled (detailed, the single source of truth).

## 4. Risk management

**Risk** = an uncertain future event affecting objectives (positive or negative). Three types:
**Project** (plan: budget, schedule, scope, people), **Product** (quality/performance), **Business**
(economic success).

**Process:** Plan → Identify → Analyse & Assess → Respond → Monitor & Control.

**Identification:** brainstorming, checklists, interviews, **Delphi** (anonymous expert rounds),
**SWOT**.

**Analysis:** qualitative — **Exposure = Probability × Impact**, then a risk matrix; quantitative
(advanced) — decision trees, simulation, sensitivity.

**Response — threats:** Avoid · Mitigate · Transfer · Accept. **Opportunities:** Exploit · Enhance ·
Share · Accept.

**Risk register fields:** ID · Trigger · Owner · Response · Resources. Monitor triggers continuously;
new risks appear throughout.

**In agile:** risks enter the Product Backlog as items, are assessed in Sprint Planning, addressed
within sprints, tracked at standups; PO owns business risk, Scrum Master clears blockers.

## 5. Quality management

**Quality** has external (user: fit-for-purpose, usability, performance, reliability) and internal
(developer: testability, maintainability, understandability, reusability) characteristics.

**Cost:** conformance (prevention — planning, testing, standards) vs. nonconformance (rework, defects,
loss). **Defects found late cost far more.**

**Processes:** Quality Assurance (define standards/process — "build it right"), Quality Planning (the
SQP), Quality Control (inspect outputs against standards).

**Verification vs. Validation:** "Are we building the product right?" (to spec, internal) vs. "Are we
building the right product?" (meets user needs, external).

**Testing:** Unit → Integration → System → User Acceptance. AC commonly `Given… When… Then…`.

**Reviews:** Technical / Business / Management. Technical types: Informal · Formal (3–5 roles, <90 min)
· Walkthrough (author-led) · Inspection (checklist) · Audit (external).

**Agile QA methods:** TDD (test → code → run; the three laws), ATDD (stakeholders co-write acceptance
tests), BDD (business-goal focus, Given/When/Then, "5 whys"). **CI/CD:** build & test on every commit;
fix red builds immediately.

**Standards/models:** ISO 9001, ISO/IEC 25010; CMMI 5 levels (Initial → Repeatable → Defined → Managed
→ Optimizing); McCall (Operation / Revision / Transition).

## 6. Project scheduling

Answers: how long, how much. Tools: **Gantt** (tasks vs. calendar), **PERT** (network + critical path).

**Steps:** WBS (decompose into deliverable tasks) → dependencies (FS / SS / FF / SF) → time estimate
(PERT three-point `(O+4M+P)/6`) → resource allocation (`headcount = person-months / duration`) → build
the schedule.

**Critical path:** ES/EF forward pass, LS/LF backward pass; **Slack = 0** on the critical path (the
longest path; can be more than one). **Crashing** = shorten critical-path tasks or remove dependencies
to compress the schedule.

**Earned Value Analysis (EVA):**
- PV (planned value) = budget × planned %; EV (earned value) = budget × actual %; AC = actual cost.
- Schedule: **SV = EV − PV** (>0 ahead), **SPI = EV / PV** (>1 ahead).
- Cost: **CV = EV − AC** (>0 under budget), **CPI = EV / AC** (>1 under budget).

## 7. Configuration management
Managing change and keeping integrity/traceability across the lifecycle. Goals: track versions, manage
dependencies, safe rollback, audit changes.

**Activities:** Identification (config items: basic / aggregate / derived) · Version Control (history,
rollback, collaboration; Git etc.) · Change Control (initiate → evaluate on 6 factors: technical merit,
side effects, dependencies, cost, schedule, risk → apply) · Auditing (consistency, approved changes
applied) · Status Reporting (Not started → In progress → Modified → Reviewed → Approved → Baselined).

**Baseline** = a stable, formally agreed artifact changed only through the formal change process.
Terms: Version (functional change) · Variant (same function, different platform) · Release (user-facing).

## 8. Scaling agile (SAFe)
Single-team Scrum doesn't scale to many interdependent teams. **SAFe** (Agile + Lean + DevOps) aligns
multiple teams.

- **Agile Release Train (ART):** 5–12 teams (50–125 people) on a synchronized cadence, one **Program
  Backlog** (Features, not stories), coordinated by the **Release Train Engineer (RTE)**.
- **Program Increment (PI):** 8–12 weeks (often 10), several sprints + an Innovation & Planning
  iteration.
- **PI Planning:** two-day event; outputs PI Objectives, the Program Board (dependencies), and Business
  Value scores.
- **System Demo** (integrated ART output) and **Inspect & Adapt** workshop drive cross-team alignment
  and improvement.
- **Program Backlog (Features) vs. Team Backlog (User Stories).**
