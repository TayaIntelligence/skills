# PRD 审核评判标准（Rubric）

The source of truth for scoring a PRD. For each dimension this says **what to check**, what **complete
/ partial / missing** looks like, the **severity** if missing, and which reference defines the bar.
Output language follows the user (中文 by default); write plainly, gloss any kept term once.

> **Why a rubric.** "这份 PRD 还行" is not a review. A defensible verdict needs named dimensions, a
> consistent scale, and a gate for "can it ship to dev". This file turns the technical-PM knowledge
> base (how to *write* a good PRD) into a checklist for *judging* one.

## Contents
- The two axes (presence × quality)
- The four ratings & how to roll up to 完成度
- The three severity levels
- Readiness gate (交付裁决) logic
- Conditional lenses (when D / E apply)
- The dimensions, group by group (A–I)
- The blocker checklist (the short list that forces 暂不可交付)

---

## The two axes
Judge every dimension on **both**:
1. **Presence** — is the section / content there at all? (structural completeness)
2. **Quality** — does it meet the bar the reference defines? (depth)

A PRD with all nine sections but happy-path-only acceptance criteria is *present-but-inadequate*. Never
rate quality from presence alone.

## The four ratings
Per dimension, assign one:
- **✅ 完整** — present and meets the bar. Engineering can rely on it.
- **⚠️ 部分** — present but thin, vague, or only partially covers what's needed. Usable as a start,
  needs tightening.
- **❌ 缺失** — absent, or so vague it's effectively absent ("妥善处理异常" with no decided behavior).
- **➖ 不适用** — genuinely doesn't apply to this PRD (e.g. AI lens on a non-AI feature). Excluded from
  scoring — do **not** count it as a miss.

### Rolling up to 完成度 (%)
Transparent, indicative — not false precision. Score each applicable dimension 完整=1.0 / 部分=0.5 /
缺失=0, weight **必填 & 阻断 dimensions ×2**, optional ×1, drop 不适用. 完成度 = weighted sum ÷ weighted
max. Always pair the % with a **band** and a one-line read, e.g.:
- **0–40% 骨架缺失** — 关键章节空或仅口号,远未到开工。
- **40–60% 骨架成型** — 章节大致在,但验收/契约/指标等关键处单薄。
- **60–80% 接近可用** — 主体扎实,少数阻断或重要项待补。
- **80–100% 基本完备** — 仅余打磨,具备开工条件。

The % communicates *how much* is done; it does **not** decide dev-readiness — that's the gate below.

## The three severity levels
Tag every **gap** (a 部分 or 缺失) with one:
- **🔴 阻断** — without it, engineering cannot responsibly start, or will build the wrong thing. Forces
  a worse verdict.
- **🟡 重要** — should be fixed before or early in the build; affects quality, measurability, or scope
  discipline. Doesn't strictly stop coding, but shipping without it is a known risk.
- **🔵 建议** — polish; improves the document but won't break the build.

## Readiness gate (交付裁决)
A gate over the **blocker** dimensions — independent of the % score:
- **可交付开发** — every applicable blocker is ✅完整; only 🔵建议 gaps remain.
- **有条件交付** — every blocker is at least ⚠️部分; enumerate the conditions to close. Dev can start the
  walking skeleton while they're tightened.
- **暂不可交付** — any applicable blocker is ❌缺失.

A PRD can be 80% complete and still **暂不可交付** (e.g. everything polished but AC are happy-path-only).
Report the two results separately and explain the gate.

## Conditional lenses
- **Group D (技术契约)** applies when the feature **has an interface, crosses systems, or calls a
  service**. If it's pure UI with no integration, mark D rows 不适用.
- **Group E (AI/模型)** applies when the core is a **model** (extraction, classification, generation,
  OCR, matching, recommendation). If deterministic, mark E rows 不适用.
- A feature can trigger **both** (e.g. an OCR service behind an API → D *and* E).

---

## A. 问题与目标

**A1 · 一句话问题陈述** — 必填 · 🔴阻断 · 出处 `discovery-and-requirements.md`
- **查什么**:有没有一句话讲清「[角色] 难以 [做某事],因为 [障碍],导致 [后果]」;是否**solution-free**
  (没把方案当问题写)。
- **完整**:角色、障碍、后果俱全,不夹带方案;读完就知道在解决谁的什么痛。
- **部分**:有背景但没收敛成一句;或后果含糊("提升效率")。
- **缺失**:只有"我们要做 X 功能",通篇是方案,没有问题。→ 这是最常见也最致命的缺口。

**A2 · 目标可衡量** — 必填 · 🟡重要 · 出处 `metrics.md`
- **完整**:目标具体、可对应到指标("被 @ 者数分钟内知晓并跳回")。
- **部分/缺失**:只有"做得更好/更顺畅"这类无法判定达成的口号。

**A3 · 非目标 / 范围边界** — 必填 · 🔴阻断 · 出处 `worked-example.md`
- **查什么**:有没有明确写出本期**不**做什么。
- **完整**:列了清晰的非目标,圈住了范围(防 scope creep)。
- **缺失**:没有非目标。→ 工程不知道边界在哪,极易蔓延或建多;判阻断。

## B. 范围与用户故事

**B1 · 用户故事质量(结构 + INVEST)** — 必填 · 🟡重要 · 出处 `worked-example.md`
- **完整**:故事呈「作为[角色],我希望[目标],以便[价值]」,且**够小、独立、有价值**;过大的已拆成史诗。
- **部分**:有故事但像功能清单(无角色/价值),或粒度过大(一个故事=一个迭代做不完)。
- **缺失**:只有功能罗列,没有从用户视角表达的故事。

**B2 · 验收标准存在且可测** — 必填 · 🔴阻断 · 出处 `edge-cases-and-exceptions.md`
- **查什么**:每个故事有没有验收标准,且**测试能核对**(可观察的结果,最好 Given/When/Then)。
- **完整**:每个故事都有可测 AC。
- **部分**:部分故事有,或 AC 像"系统应快速响应"这类不可测的愿望。
- **缺失**:没有验收标准。→ 没有"完成"的判据,工程交付什么都算对;判阻断。

**B3 · 验收覆盖 正常 + 边界 + 异常** — 必填 · 🔴阻断 · 出处 `edge-cases-and-exceptions.md`
- **查什么**:AC 是否只写了 happy path,还是也覆盖了边界(空/零/上限/上限+1)与异常(无权限/超时/重复
  提交/并发/外部依赖失败),且每个边界都给出**已决定的行为**(拦截/降级/重试+提示什么)。
- **完整**:逐项过了边界清单,关键边界各有一条决定了行为的 AC。
- **部分**:点到一两个边界,但大面积只有正常路径;或写了"妥善处理异常"这种未决定行为。
- **缺失**:清一色 happy path。→ 第一个空/零/超时就是线上事故;判阻断。**这是审核中最高频的真实问题。**

**B4 · 优先级 / MVP 切分** — 🟡重要 · 出处 `prioritization-and-estimation.md`
- **完整**:用 MoSCoW 或等价方式分了必须/应该/可以/本期不做;有最小可用切片。
- **部分/缺失**:所有故事一锅端、无优先级,或没有 MVP 概念(一次要做全)。

## C. 成功指标

**C1 · 北极星指标** — 必填 · 🟡重要 · 出处 `metrics.md`
- **完整**:一个对齐**用户/业务价值**的核心指标,且非虚荣指标(不是"点击数""曝光量")。
- **部分**:有指标但是活动量而非价值,或与功能价值不对应。
- **缺失**:没有任何成功定义。→ 上线后无法判断成败。
- **内部/效率工具**别套 AARRR:看直通率(无需人工修正的占比)或省下的工时(`metrics.md`)。

**C2 · 输入指标 + 护栏指标 + 基线/目标** — 🟡重要 · 出处 `metrics.md`
- **完整**:有 2–4 个可被团队影响的输入指标;**至少一个护栏**(防止局部赢全局输);每个指标有基线
  (或注明"无基线,先埋点")和目标。
- **部分**:只有北极星,没有护栏(常见且危险——"提升触达"很容易靠"轰炸用户"刷出来)。
- **缺失**:无输入、无护栏、无基线,目标是凭空设的数字。

## D. 技术契约 *(条件:涉接口/跨系统/调服务)*

**D1 · 接口契约** — 条件 · 🔴阻断 · 出处 `technical-artifacts.md`
- **查什么**:每个端点有没有入参/出参表(字段·类型·必填·格式/取值/**可空含义**)+ 一个真实示例。
- **完整**:契约成表,字段、格式、null 语义("不适用" vs "无法确定")都明确。
- **部分**:有接口描述但泛泛("传订单信息,返回结果"),字段/格式/可空没定。
- **缺失**:涉及接口却只字未提契约。→ 工程无法开工;判阻断。

**D2 · 数据契约 + 错误分类** — 条件 · 🔴阻断 · 出处 `technical-artifacts.md`
- **查什么(数据契约)**:每个字段的**输出方/源头**、请求-响应的 ID 关联、日期/单位/编码格式、版本演进。
- **查什么(错误分类)**:有没有区分**系统级异常**(服务无法完成,返回非 2xx,需兜底)与**业务级警告**
  (返回成功但标 `needs_review`,结果可用需人工确认),并列各自触发条件与下游处理。
- **完整**:字段归属清楚;错误两类分开,各有处理。
- **缺失**:没有错误分类(最常被跳过的一节)。→ 要么对可恢复问题硬失败,要么把坏数据静默入库;判阻断。

**D3 · 非功能需求写成数字** — 条件 · 🟡重要 · 出处 `technical-artifacts.md`
- **完整**:延迟(p95)、体积上限、超时、可用性、合规可观测项都是**数字**,且写成可验证的验收标准。
- **部分/缺失**:只有"快""稳""安全"这类无法测的形容词。

## E. AI / 模型 *(条件:模型驱动)*

**E1 · 评测口径(评测集 + 指标 + 阈值)** — 条件 · 🔴阻断 · 出处 `ai-ml-products.md`
- **查什么**:验收是不是「在 N 条代表性样本上,字段级准确率 ≥ X%,低置信度标 `needs_review`」这种
  **可测的三元组**,而不是"识别正确"。评测集是否代表真实输入质量、有无留出(holdout)、是否分类报告。
- **完整**:有评测集 + 选定指标 + 阈值,且说明了对未达标样本的处理。
- **部分**:提了"准确率 95%"但没有评测集、没有测量方法。→ 无法测量的承诺是虚的。
- **缺失**:把 AI 当确定性软件,承诺"输出正确",无评测口径。→ 判阻断。
- **特例**:若当前**无标注、无测量手段**,正确状态是"首要任务=先埋点+建评测集",PRD 应把这点列为风险
  而非盲设目标——能识别出这一点,给 部分 而非 缺失。

**E2 · 精确率/召回率 + 错误代价不对称** — 条件 · 🟡重要 · 出处 `ai-ml-products.md`
- **完整**:说明了该用准确率还是精确率/召回率**及为什么**;指出错放(放过坏的)与错拦(拦住好的)谁更贵,
  并据此分项定指标(如误报率<5% / 漏报率<10% 分开定),而非一个笼统准确率。
- **部分/缺失**:只有单一"准确率",在类别不均衡时会误导(例:某字段 99% 该为空,模型全输出空也有 99%)。

**E3 · 人工兜底 + 数据/规则依赖** — 条件 · 🔴阻断 · 出处 `ai-ml-products.md`
- **查什么(兜底)**:有没有置信度阈值分流(高→自动,低→人工 `needs_review`)、人工确认 UX、低置信度
  返回空值+原因而非臆造、模型失败时降级到人工而非崩溃。
- **查什么(依赖)**:评测集、映射/规则表、参考分类等**业务材料**是否被当作排期前置门槛(缺它不能开工、
  且会卡住准确率),挂了负责人和时间。
- **完整**:兜底机制清楚;数据/规则依赖被显式列为 P0 前置。
- **缺失**:AI 全自动无人工兜底,或只字未提依赖材料。→ AI PRD 最常见的滑点不是模型而是"业务材料从没到位";
  判阻断。

## F. 用户流程与设计

**F1 · 关键流程 / 界面五态** — 🔵建议 · 出处 `edge-cases-and-exceptions.md`
- **完整**:关键流程有步骤或原型链接;UI 功能决定了**五态**(正常/空/错误/加载/极限)。
- **部分/缺失**:只画了正常态,空态/错误态/加载态缺位(原型最常漏的地方)。

## G. 估算与排期

**G1 · MVP 切分 + 估算 + 依赖门槛** — 🟡重要 · 出处 `prioritization-and-estimation.md`
- **完整**:有 MVP/迭代切分;估算用相对大小或**区间**(不是凭空一个假精确的日期);阻塞依赖被当作前置门槛
  (写成"依赖 X 到位后 N 个迭代")。
- **部分**:有里程碑但估算是无依据的单点日期(新项目/新团队尤其要警惕假精确)。
- **缺失**:无任何排期或依赖梳理。

## H. 风险与未决问题

**H1 · 关键风险** — 必填 · 🔴阻断 · 出处 `risk-register-template.md`
- **完整**:列了关键风险,各有概率×影响、触发条件、负责人、应对(避免/缓解/转移/接受)。
- **部分**:列了风险但无负责人/无应对,等于没人管。
- **缺失**:无风险识别。→ 对有真实技术/依赖风险的功能,判阻断;对极简功能可降为重要。

**H2 · 未决问题有归属** — 必填 · 🔴阻断(若阻塞) · 出处 `discovery-and-requirements.md`
- **完整**:未决问题列出来了,且每条指明**谁来拍板**、何时。
- **缺失**:有明显悬而未决的关键选择(影响能否开工)却没人负责拍板。→ 判阻断。

## I. 发布与度量计划

**I1 · 发布策略 + 上线验证** — 🔵建议 · 出处 `metrics.md`
- **完整**:有灰度/全量/开关策略,且说明上线后如何验证成功指标(怎么埋点、看什么)。
- **部分/缺失**:没写怎么发、发完怎么确认有没有成功。

---

## Blocker 速查清单
任一为"是",交付裁决至少降到 **暂不可交付**(或在仅"单薄"时降到 **有条件交付**):
- [ ] 没有一句话问题陈述,或通篇是方案没有问题(A1)
- [ ] 没有非目标 / 范围无边界(A3)
- [ ] 验收标准缺失,或不可测(B2)
- [ ] 验收只有正常路径,边界/异常无决定行为(B3)
- [ ] 涉及接口却无契约,或无系统级异常 vs 业务级警告的错误分类(D1/D2)
- [ ] AI 功能无评测口径(评测集+指标+阈值),或无人工兜底,或漏列数据/规则依赖(E1/E3)
- [ ] 关键风险无人负责,或阻塞性未决问题无人拍板(H1/H2)

> 用这张清单先快速判生死,再回到逐维度细评——先回答"能不能开工",再回答"哪里还能更好"。
