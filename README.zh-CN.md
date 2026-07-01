# Taya Skills

[English](README.md) · **中文**

一组可复用的 **Agent Skills（技能包）**，适用于 Claude（Claude Code / Claude.ai / Claude API）以及其他可加载 Markdown 指令的 AI 工具，例如 Codex。每个技能都是一个自包含的文件夹，包含 `SKILL.md` 指令文件，以及它按需加载的模板和参考资料。

本仓库包含三个技能：**`technical-pm`**（写 PRD）、**`prd-review`**（审 PRD）和 **`technical-pm-guided`**（一步步学 PM 技能）。

## `technical-pm` - 技术产品经理

`technical-pm` 会让助手以资深**技术产品经理**的方式工作：负责*做什么、为什么做*，而不只是怎么做。你可以给它一个模糊想法、原始功能需求、会议纪要或已有需求文档，它会把这些材料整理成清晰、收敛、可落地的产品产物。

这个技能会先处理产品问题。它应该先澄清问题、范围、成功标准和风险，再进入实现。

> **内置 AI/ML 产品视角。** 当功能由 AI 驱动，例如提取、分类、生成、OCR 或匹配，`technical-pm` 会应用专门视角：评测集、阈值、精确率 / 召回率取舍、错误代价不对称、人工复核、`needs_review` 处理和数据依赖。

### 能产出什么

| 需求 | 产出 |
|---|---|
| **需求发现与界定** | 先找出真实问题，而不是直接接受某个方案；明确目标用户、目标、约束和非目标 |
| **PRD / 需求规格** | 问题、范围、带验收标准的用户故事、成功指标、风险和未决问题 |
| **已有文档重构** | 从会议纪要、旧 PRD 或竞品资料中整理结构化 PRD，并标出冲突和假设 |
| **需求拆分** | 史诗、用户故事、任务，以及覆盖正常路径、边界和异常的 `Given / When / Then` 验收标准 |
| **优先级排序** | 使用 RICE、MoSCoW、Kano、价值 / 成本或 WSJF 排序，并展示打分和理由 |
| **估算与计划** | 故事点、速率、PERT 三点估算、迭代计划、版本切片和依赖项 |
| **指标设计** | 北极星指标、输入指标、护栏指标，每个都带基线和目标 |
| **风险管理** | 概率 x 影响的风险登记册，包含触发条件、负责人和应对策略 |
| **干系人沟通** | 面向不同受众的状态更新和决策文档 |

### 什么时候使用

只要你在决定**做什么、为什么做**，就可以使用 `technical-pm`：

- 写 PRD 或产品规格；
- 界定 MVP；
- 把功能拆成史诗、用户故事和任务；
- 给待办列表排优先级；
- 估算迭代或版本计划；
- 定义成功指标；
- 评估产品、交付或技术风险；
- 准备干系人汇报或决策文档。

当你说“当个 PM”“戴上产品帽子”，或在问题和范围还不清晰时就开始实现，这个技能也应该被触发。

### 产出语言

技能会跟随用户语言。中文用户的产品产物默认使用中文，只保留团队常用的框架术语，例如 `MVP`、`PRD`、`RICE`、`MoSCoW`。

## `prd-review` - PRD 完整性审核

`prd-review` 是 `technical-pm` 的反向用法：它不*写* PRD，而是**审核别人已经写好的** PRD。给它一份已有的 PRD 或需求规格，它只回答一个问题：*工程师能不能照着它把正确的东西造出来而不用猜？如果不能，到底还缺什么？* 它只诊断文档，而不改写。

它复用 `technical-pm` 的知识体系，但反过来使用：那些参考资料定义了*一份好 PRD 该怎么写*，在这里就成了**评审据以打分的标准**。

### 能产出什么

| 需求 | 产出 |
|---|---|
| **完整度评分** | 用技术 PM 评判标准为 PRD 打分：问题界定、范围做 / 不做、覆盖边界与异常的用户故事和验收标准、成功指标、技术契约、AI/ML 评测门槛、风险与依赖 |
| **交付裁决** | 给出 **可开工 / 有条件 / 不可开工** 的判断，回答这份 PRD 能不能交开发 |
| **缺失项清单** | 一份按优先级排列、具体的“还缺什么、必须补齐什么”清单，而不是含糊的意见 |
| **HTML 报告** | 整份评估以清晰、自包含的 HTML 报告交付（`assets/report-template.html`） |

### 什么时候使用

当你想**审核、打分或体检一份已有的 PRD**，而不是从零写一份时，使用 `prd-review`，例如“审一下这份 PRD”“这份需求能交开发吗”“PRD 还缺什么”“is this spec ready for dev”。它触发于评审别人已经写好的 PRD，而不是起草新文档。

## `technical-pm-guided` - 引导式学习 technical-pm

`technical-pm-guided` 是 `technical-pm` 的**苏格拉底式教学教练**。它不替你产出 PM 文档，而是教你**自己掌握**这些方法——一次一个步骤，边练边学，给你反馈和渐进提示。

它使用与 `technical-pm` 相同的知识体系（`references/` 和 `assets/`），但反过来：那些文件定义了*一份好的 PM 产物该怎么写*，引导模式带你一步步亲手做出每一份。

### 与 `technical-pm` 的区别

| | `technical-pm` | `technical-pm-guided` |
|---|---|---|
| **角色** | PM 实践者 — 直接产出 | PM 教练 — 引导式教学 |
| **触发** | "写个 PRD""排一下优先级""把这个拆了" | "教我怎么写 PRD""带我走一遍拆需求""我是新手" |
| **产出** | 成品的 PRD / 故事列表 / 排期表 | 用户自己写，教练给反馈 |
| **节奏** | 高效 — 直接交付 | 苏格拉底式 — 一轮一个问题，等你回应 |

### 教什么

覆盖 `technical-pm` 的 8 个工作流（需求发现 → PRD → 拆分 → 排序 → 估算 → 指标 → 风险 → 沟通），根据用户意图匹配开场策略：

- **收敛型**（"教我做 X"）— 一步一步带，拿用户的真实案例练手
- **发散型**（"产品规划是什么"）— 简短全景 + 2-3 个入口任选
- **简单事实型**（"RICE 是什么意思"）— 简短定义 + 邀请实际练一下

### 设计理念

借鉴了 ChatGPT Study Mode 和 Gemini Guided Learning：

1. 一次一步 — 绝不一次性倒出整套方法论
2. 引导而非代劳 — 用户先写，教练后反馈
3. 卡住就兜底 — 2-3 轮答不上来就直接示范
4. 练习优于讲解 — 用用户自己的真实案例来教，不空讲理论

### 什么时候使用

当有人想**学会** PM 技能，而不是拿一份成品文档时，使用 `technical-pm-guided`。触发词包括"教我怎么写 PRD""带我走一遍需求拆分""我是新手""不会排优先级""teach me how to...""walk me through..."等。

## 安装到 Claude Code

把技能目录复制到 Claude Code 的 skills 目录：

```bash
mkdir -p "$HOME/.claude/skills"
cp -R technical-pm "$HOME/.claude/skills/technical-pm"
cp -R prd-review "$HOME/.claude/skills/prd-review"
cp -R technical-pm-guided "$HOME/.claude/skills/technical-pm-guided"
```

然后开启新对话，让技能在产品 / PM 类请求中自动触发，或显式调用：

```text
$technical-pm
$prd-review
$technical-pm-guided
```

示例：

```text
请作为技术产品经理，把这个功能想法整理成一份范围清晰的 PRD。
审一下这份 PRD，告诉我它能不能交开发。
```

## 其他 AI 工具

将某个技能的 `SKILL.md`（`technical-pm/SKILL.md`、`prd-review/SKILL.md` 或 `technical-pm-guided/SKILL.md`）作为系统指令加载。技能会在需要时按需拉取 `references/` 和 `assets/` 中的文件。如果使用 Codex 风格的 UI，每个技能的 `agents/openai.yaml` 提供展示名称和默认提示词。

## 仓库结构

```text
skills/
├── .gitignore
├── LICENSE
├── README.md
├── README.zh-CN.md
├── technical-pm/
│   ├── SKILL.md                       # 指令与工作流路由
│   ├── agents/
│   │   └── openai.yaml                # Codex 风格 UI 的展示名称和默认提示词
│   ├── assets/                        # 填写模板
│   │   ├── prd-template.md
│   │   ├── risk-register-template.md
│   │   └── user-story-template.md
│   └── references/                    # 按需加载的深度参考资料
│       ├── ai-ml-products.md
│       ├── delivery-and-process.md
│       ├── discovery-and-requirements.md
│       ├── edge-cases-and-exceptions.md
│       ├── metrics.md
│       ├── prioritization-and-estimation.md
│       ├── technical-artifacts.md
│       └── worked-example.md
├── prd-review/
│   ├── SKILL.md                       # 指令与评审路由
│   ├── agents/
│   │   └── openai.yaml
│   ├── assets/                        # 模板与 HTML 报告框架
│   │   ├── prd-template.md
│   │   ├── report-template.html
│   │   ├── risk-register-template.md
│   │   └── user-story-template.md
│   └── references/                    # 评审据以打分的标准
│       ├── ai-ml-products.md
│       ├── delivery-and-process.md
│       ├── discovery-and-requirements.md
│       ├── edge-cases-and-exceptions.md
│       ├── metrics.md
│       ├── prioritization-and-estimation.md
│       ├── review-rubric.md
│       ├── technical-artifacts.md
│       ├── worked-example.md
│       └── worked-review-example.md
└── technical-pm-guided/
    ├── SKILL.md                       # 苏格拉底式教学指令
    ├── agents/
    │   └── openai.yaml
    ├── assets/                        # 共享模板（与 technical-pm 相同）
    │   ├── prd-template.md
    │   ├── risk-register-template.md
    │   └── user-story-template.md
    └── references/                    # 共享参考资料（与 technical-pm 相同）
        ├── ai-ml-products.md
        ├── delivery-and-process.md
        ├── discovery-and-requirements.md
        ├── edge-cases-and-exceptions.md
        ├── metrics.md
        ├── prioritization-and-estimation.md
        ├── technical-artifacts.md
        └── worked-example.md
```

## 约定

- `SKILL.md` 是每个技能的唯一入口。
- `agents/openai.yaml` 存放 UI 元数据；执行逻辑在 `SKILL.md` 中。
- `references/` 存放按需渐进加载的深度参考资料。
- `assets/` 存放产出模板和可复用文件。
- 技能文件夹内尽量不放额外 README 或 changelog，减少助手加载噪音。

## 新增更多技能

如需新增技能，在仓库顶层创建一个新目录，并至少包含 `SKILL.md`。建议保持技能自包含：模板放在 `assets/`，详细说明放在 `references/`，只有目标助手界面需要时才添加 `agents/` 元数据。
