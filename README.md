# Taya Skills

A collection of **Agent Skills** for Claude — reusable capability packs that teach the
assistant a specialized way of working. Each skill is a self-contained folder with a
`SKILL.md` (instructions), plus the templates and references it pulls from on demand.

This repo currently ships two skills: **`technical-pm`** and **`technical-pm-guided`**.

> 📖 English below · [中文说明见下半部分 ↓](#中文说明)

---

## `technical-pm` — Technical Product Manager

Makes the assistant operate as an experienced **technical product manager**: someone who
owns *what to build and why*, not just how. Point it at a fuzzy idea or a raw feature
request and it turns it into clear, sharply-scoped, buildable product artifacts — and it
won't write code until the product thinking is solid.

### What it produces

| Need | What you get |
|---|---|
| **Discovery & framing** | The real problem surfaced (5 Whys, Jobs-To-Be-Done) before any solution is accepted, plus a one-sentence problem statement + goal |
| **PRD / spec** | Problem, scope (in/out), user stories with acceptance criteria, success metrics, risks — scales from a one-pager to a multi-module program doc; can also **reconstruct** a PRD from existing material (meeting notes, an old doc, a competitor's) |
| **Slicing** | An idea broken into epics → user stories → tasks, INVEST-checked, with `Given/When/Then` acceptance criteria covering happy path **+ boundaries + failure** |
| **Prioritization** | A ranked backlog via RICE / MoSCoW / Kano / Value-Effort / WSJF — with the scores and the reasoning shown, not just a verdict |
| **Estimation & planning** | Story points, velocity, PERT three-point, sprint and release planning |
| **Metrics** | A North Star + input + guardrail metrics, each with a baseline and target |
| **Risk** | A probability × impact register with triggers, owners, and responses |
| **Communication** | Stakeholder updates and decision docs tuned to the audience |

### When it activates

Whenever you're deciding **what to build or why** — writing a PRD, scoping an MVP, breaking
down a feature, prioritizing a backlog, planning a sprint, defining metrics, or assessing
risk. Also when you say *"act as a PM"* / *"put on the product hat"*, or start building
something with no clear problem statement. It carries a dedicated lens for **AI/ML-driven
features** (eval sets, precision/recall, error-cost asymmetry, human-in-the-loop).

### Output language

Adapts to your language; **defaults to 中文** for the product artifacts, keeping only
established framework terms (MoSCoW, RICE, MVP, PRD) in English.

### Using it

1. Copy the `technical-pm/` folder into your skills directory (e.g. `~/.claude/skills/` for
   Claude Code, or your platform's skills location).
2. Start a conversation and either let it trigger automatically (it activates on
   product/PM-shaped requests) or invoke it explicitly — e.g. `$technical-pm`, or
   *"act as a technical PM and scope this feature."*

The skill reads `SKILL.md` first, then pulls the relevant files from `references/` and
`assets/` on demand.

---

## `technical-pm-guided` — Guided Learning for technical-pm

A **Socratic-coaching companion** for the `technical-pm` skill. Instead of producing PM artifacts
for the user, it teaches users how to produce them themselves — one workflow step at a time,
with hands-on practice, feedback, and progressive hints.

### How it differs from `technical-pm`

| | `technical-pm` | `technical-pm-guided` |
|---|---|---|
| **Role** | PM practitioner — produces artifacts directly | PM coach — teaches through guided practice |
| **Trigger** | "Write a PRD", "Prioritize this", "Break this down" | "Teach me how to write a PRD", "Walk me through slicing", "I'm new to this" |
| **Output** | Finished PRD / stories / backlog / metrics | User produces their own artifact, coach gives feedback |
| **Pacing** | Efficient — delivers the result | Socratic — one question per turn, waits for response |

### What it teaches

Walks users through the 8 `technical-pm` workflows (Discovery → PRD → Slicing →
Prioritization → Estimation → Metrics → Risk → Communication), matching its query type to the
user's intent:

- **"Teach me how to do X"** — step-by-step, hands-on practice with the user's real case
- **"What is product management?"** — brief overview + 2–3 entry points to pick from
- **"What does RICE mean?"** — short definition + invitation to practice with it

### Design principles

Informed by ChatGPT Study Mode and Gemini Guided Learning:
1. One step at a time — never dump the entire methodology at once
2. Guide, don't produce — the user writes first, the coach gives feedback
3. Progress over purity — after 2–3 failed attempts, demonstrate directly
4. Practice over theory — teach through the user's real case, not abstract explanations

### Using it

Copy both `technical-pm/` and `technical-pm-guided/` to your skills directory. Start with
*"Teach me how to write a PRD"* or *"Walk me through breaking down a feature."* It activates
automatically on learning-intent language, or invoke explicitly with `$technical-pm-guided`.

---

## Repo structure · 目录结构

```
skills/
├── README.md
├── technical-pm/
│   ├── SKILL.md                       # the skill's instructions + workflow router
│   ├── agents/
│   │   └── openai.yaml                # interface manifest (display name, default prompt)
│   ├── assets/                        # fill-in templates
│   │   ├── prd-template.md
│   │   ├── user-story-template.md
│   │   └── risk-register-template.md
│   └── references/                    # deep-dive references the skill loads on demand
│       ├── discovery-and-requirements.md
│       ├── technical-artifacts.md
│       ├── ai-ml-products.md
│       ├── prioritization-and-estimation.md
│       ├── delivery-and-process.md
│       ├── metrics.md
│       ├── edge-cases-and-exceptions.md
│       └── worked-example.md
└── technical-pm-guided/
    ├── SKILL.md                       # guided-learning Socratic coach for technical-pm
    └── agents/
        └── openai.yaml                # interface manifest
```

---

## 中文说明

一组面向 Claude 的 **Agent Skills(技能包)**—— 可复用的能力模块,教模型用某种专业方式工作。
每个技能都是一个自包含的文件夹,内含 `SKILL.md`(指令),以及它按需调用的模板和参考资料。

本仓库目前包含两个技能:**`technical-pm`** 和 **`technical-pm-guided`**。

### `technical-pm` —— 技术产品经理

让模型以资深**技术产品经理**的身份工作:它负责*做什么、为什么做*,而不只是怎么做。给它一个
模糊的想法或原始需求,它会产出清晰、收敛、可落地的产品文档 —— 并且在产品思路理顺之前不会
去写代码。

**它能产出**

| 你的需求 | 你会得到 |
|---|---|
| **需求发现与界定** | 先用 5 Whys / JTBD 挖出真正的问题,而不是直接接受某个"解决方案",并给出一句话问题陈述 + 目标 |
| **PRD / 需求文档** | 问题、范围(做 / 不做)、带验收标准的用户故事、成功指标、风险;小到一页纸,大到多模块项目文档;也能把已有材料(会议纪要、旧文档、竞品)**重构**成 PRD |
| **需求拆分** | 把想法拆成 史诗 → 用户故事 → 任务,过 INVEST 检查,验收标准用 `Given/When/Then` 覆盖正常 **+ 边界 + 异常** |
| **优先级** | 用 RICE / MoSCoW / Kano / 价值-成本 / WSJF 排序,并给出打分和理由,而不只是结论 |
| **估算与排期** | 故事点、速率、PERT 三点估算、迭代与版本规划 |
| **指标** | 北极星 + 输入 + 护栏指标,每个都带基线与目标 |
| **风险** | 概率 × 影响的风险登记册,带触发条件、负责人、应对策略 |
| **沟通** | 按受众定制的干系人汇报与决策文档 |

**什么时候触发**

只要你在决定**做什么、为什么做**:写 PRD、界定 MVP、拆功能、排优先级、做迭代计划、定指标、
评风险。或者你说"当个 PM""戴上产品的帽子",又或者还没想清问题就开始动手时。它对
**AI / 模型驱动的功能** 有专门视角(评测集、精确率 / 召回率、错误代价不对称、人工兜底)。

**产出语言**

跟随你的语言,产品文档**默认中文**,只保留约定俗成的框架术语(MoSCoW、RICE、MVP、PRD)为英文。

**如何使用**

1. 把 `technical-pm/` 文件夹复制到你的技能目录(例如 Claude Code 的 `~/.claude/skills/`,
   或你所用平台的技能位置)。
2. 开始对话,让它自动触发(遇到产品 / PM 类请求时),或显式调用 —— 例如 `$technical-pm`,
   或"用 technical-pm 把这个想法写成 PRD"。

模型会先读 `SKILL.md`,再按需拉取相关的 `references/` 和 `assets/`。

### `technical-pm-guided` —— 引导式学习 technical-pm

`technical-pm` 的**苏格拉底式教学教练**。它不替你产出 PM 文档,而是教你**自己掌握**这些方法
——一次一个步骤,边练边学,给你反馈和渐进提示。

**它和 `technical-pm` 的区别**

| | `technical-pm` | `technical-pm-guided` |
|---|---|---|
| **角色** | PM 实践者 — 直接产出 | PM 教练 — 引导式教学 |
| **触发** | "写个PRD""排一下优先级""把这个拆了" | "教我怎么写PRD""带我走一遍拆需求""我是新手" |
| **产出** | 成品的 PRD / 故事列表 / 排期表 | 用户自己写,教练给反馈 |
| **节奏** | 高效 — 直接交付 | 苏格拉底式 — 一轮一个问题,等你回应 |

**教什么**

覆盖 `technical-pm` 的 8 个工作流,根据用户意图路由:
- **"教我做 X"** — 一步一步带,拿用户的真实案例练手
- **"产品规划是什么"** — 简短全景 + 2-3 个入口任选
- **"RICE 是什么意思"** — 简短定义 + 邀请实际练一下

**设计参考**

借鉴了 ChatGPT Study Mode 和 Gemini Guided Learning 的设计理念:
1. 一次一步 — 绝不一次性倒出整套方法论
2. 引导而非代劳 — 用户先写,教练后反馈
3. 卡住就兜底 — 2-3 轮答不上来就直接示范
4. 练习优于讲解 — 用用户自己的真实案例来教,不空讲理论

**如何使用**

把 `technical-pm/` 和 `technical-pm-guided/` 都复制到技能目录。说"教我怎么写 PRD"或
"带我走一遍需求拆分"即可触发。遇到学习意图的措辞会自动激活,或显式输入 `$technical-pm-guided`。
