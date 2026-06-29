# Taya Skills

[简体中文](README_zh-CN.md)

This repository contains reusable **Agent Skills**: self-contained capability packs that teach an
assistant a specialized way of working. A skill folder usually includes:

- `SKILL.md` with the operating instructions and workflow router.
- `assets/` with reusable templates.
- `references/` with deeper guidance the assistant loads only when needed.
- optional `agents/` metadata for supported assistant surfaces.

The repository currently ships one skill: **`technical-pm`**.

## `technical-pm`

`technical-pm` makes the assistant operate like an experienced technical product manager. It is
designed for moments when the important question is **what to build and why**, before jumping into
implementation.

Give it a rough idea, a feature request, meeting notes, or an existing requirements document. It helps
turn that material into clear, scoped, testable product artifacts that engineering teams can act on.

### What It Helps With

| Need | Output |
|---|---|
| Discovery and framing | A clear problem statement, target user, goal, constraints, and explicit non-goals |
| PRDs and specs | Problem, scope, user stories, acceptance criteria, success metrics, risks, and open questions |
| Existing-doc cleanup | A reconstructed PRD from notes, old docs, or competitor material, with conflicts and assumptions surfaced |
| Work slicing | Epics, user stories, tasks, and `Given / When / Then` acceptance criteria |
| Prioritization | Ranked backlog using RICE, MoSCoW, Kano, Value/Effort, or WSJF, with reasoning shown |
| Estimation and planning | Story points, release slices, sprint planning, PERT estimates, and dependency callouts |
| Metrics | North Star, input, and guardrail metrics with baselines and targets |
| Risk management | Probability x impact risk register with triggers, owners, and responses |
| Stakeholder communication | Status updates and decision docs tuned to the audience |

It also includes a dedicated lens for AI/ML-driven features, including eval sets, thresholds,
precision/recall tradeoffs, error-cost asymmetry, human review, and data dependencies.

### When To Use It

Use `technical-pm` when you are:

- turning a fuzzy idea into a PRD;
- scoping an MVP;
- breaking a feature into buildable stories;
- deciding what to build next;
- prioritizing a backlog;
- estimating a sprint or release;
- defining product metrics;
- identifying delivery or product risks;
- preparing stakeholder updates or decision docs.

It should also trigger when someone asks the assistant to "act as a PM", "wear the product hat", or
starts building without a clear problem statement and scope.

### Output Language

The skill follows the user's language. For Chinese users, product artifacts default to Chinese while
keeping established framework terms such as `MVP`, `PRD`, `RICE`, and `MoSCoW` in English.

## Installation

Copy the `technical-pm/` folder into the skills directory used by your assistant environment.

For Claude Code, that is commonly:

```bash
mkdir -p ~/.claude/skills
cp -R technical-pm ~/.claude/skills/
```

Then start a new conversation and invoke the skill explicitly:

```text
$technical-pm
```

You can also phrase the request naturally, for example:

```text
Act as a technical PM and turn this feature idea into a scoped PRD.
```

The assistant reads `SKILL.md` first, then loads the relevant templates and references on demand.

## Repository Structure

```text
skills/
├── README.md
├── README_zh-CN.md
└── technical-pm/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    ├── assets/
    │   ├── prd-template.md
    │   ├── risk-register-template.md
    │   └── user-story-template.md
    └── references/
        ├── ai-ml-products.md
        ├── delivery-and-process.md
        ├── discovery-and-requirements.md
        ├── edge-cases-and-exceptions.md
        ├── metrics.md
        ├── prioritization-and-estimation.md
        ├── technical-artifacts.md
        └── worked-example.md
```

## Adding More Skills

To add another skill, create a new top-level folder with at least a `SKILL.md`. Keep the skill
self-contained: put reusable templates in `assets/`, detailed guidance in `references/`, and metadata
in `agents/` only when a target assistant surface needs it.
