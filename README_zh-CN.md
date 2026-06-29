# Taya Skills

[English](README.md)

本仓库收集可复用的 **Agent Skills**。每个技能都是一个自包含的能力包，用来教助手以某种专业方式工作。一个技能目录通常包含：

- `SKILL.md`：核心指令、工作方式和流程路由。
- `assets/`：可复用模板。
- `references/`：按需加载的深入参考资料。
- `agents/`：可选的助手界面元数据。

当前仓库包含一个技能：**`technical-pm`**。

## `technical-pm`

`technical-pm` 会让助手以资深技术产品经理的方式工作。它适用于先回答 **做什么、为什么做** 的问题，再进入实现的场景。

你可以给它一个模糊想法、原始功能需求、会议纪要或已有需求文档。它会把这些材料整理成清晰、收敛、可验证、工程团队能直接评审和拆解的产品产物。

### 能解决什么

| 需求 | 产出 |
|---|---|
| 需求发现与界定 | 清晰的问题陈述、目标用户、目标、约束和明确不做的范围 |
| PRD / 需求规格 | 问题、范围、用户故事、验收标准、成功指标、风险和未决问题 |
| 旧文档整理 | 从会议纪要、旧 PRD、竞品资料中重构 PRD，并标出冲突、假设和空缺 |
| 工作拆分 | 史诗、用户故事、任务，以及 `Given / When / Then` 格式的验收标准 |
| 优先级排序 | 使用 RICE、MoSCoW、Kano、价值/成本、WSJF 等方法排序，并展示理由 |
| 估算与计划 | 故事点、版本切片、迭代计划、PERT 三点估算和依赖项 |
| 指标设计 | 北极星指标、输入指标、护栏指标，以及基线和目标 |
| 风险管理 | 概率 x 影响的风险登记册，包含触发条件、负责人和应对策略 |
| 干系人沟通 | 面向不同受众的状态更新和决策文档 |

它也专门覆盖 AI / 模型驱动功能：评测集、阈值、精确率 / 召回率取舍、错误代价不对称、人工复核和数据依赖。

### 什么时候使用

当你需要做下面这些事情时，使用 `technical-pm`：

- 把模糊想法写成 PRD；
- 界定 MVP；
- 把功能拆成可开发的用户故事；
- 判断下一步该做什么；
- 给待办列表排优先级；
- 估算迭代或版本计划；
- 定义产品成功指标；
- 识别交付或产品风险；
- 准备干系人汇报或决策文档。

当你说“当个 PM”“戴上产品帽子”，或在问题和范围还不清晰时就开始做实现，这个技能也应该被使用。

### 产出语言

技能会跟随用户语言。中文用户的产品产物默认使用中文，只保留团队常用的框架术语，例如 `MVP`、`PRD`、`RICE`、`MoSCoW`。

## 安装方式

把 `technical-pm/` 目录复制到你的助手环境使用的 skills 目录中。

如果使用 Claude Code，通常是：

```bash
mkdir -p ~/.claude/skills
cp -R technical-pm ~/.claude/skills/
```

然后开启新对话，显式调用：

```text
$technical-pm
```

也可以用自然语言触发，例如：

```text
请作为技术产品经理，把这个功能想法整理成一份范围清晰的 PRD。
```

助手会先读取 `SKILL.md`，再按需加载相关模板和参考资料。

## 仓库结构

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

## 新增更多技能

如需新增技能，在仓库顶层创建一个新目录，并至少包含 `SKILL.md`。建议保持技能自包含：模板放在 `assets/`，详细说明放在 `references/`，只有目标助手界面需要时才添加 `agents/` 元数据。
