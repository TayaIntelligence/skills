# Edge Cases & Exception Handling Reference

How to make a story's "what happens when things aren't normal" explicit and complete. Output language
follows the user (中文 by default); write plainly and don't pair every term with English — keep a term
only when it's genuinely standard (BVA, ZOM…), glossed once.

> **Why this exists.** "妥善处理异常" is not a requirement — it's a decision deferred until it becomes a
> production incident. A PRD's job is to **decide the intended behavior at the edges** (block? degrade?
> retry? what message?) up front. The techniques below are *completeness prompts* so the
> obvious-in-hindsight cases (empty, zero, max, no-permission, timeout, double-submit) each get a named
> behavior in the acceptance criteria. This is about *deciding behavior*, not writing exhaustive QA
> test matrices — those stay with QA.

## Contents
- The one rule
- Two altitudes: story AC vs. system contract
- Method 1 — Negative & boundary scenarios (Given/When/Then)
- Method 2 — Boundary value analysis + equivalence partitioning (which values to pin)
- Method 3 — The category checklist (don't miss a class)
- Method 4 — Extensions: alternate & exception flows (multi-step flows)
- Product shortcut — the five states
- Anti-patterns

---

## The one rule
Every story's acceptance criteria cover **happy path + boundaries + failure/exception**, and each edge
states the system's *intended behavior*, not "handle it". Compare: "处理好超时" (undecided, untestable)
vs. "支付网关超时,展示「稍后重试」并保留购物车不清空" (a decision a tester can check). If you can't say what
the system *does*, you haven't specified the requirement yet.

## Two altitudes — don't conflate them
- **Story level (acceptance criteria)** — user-facing behavior for this story's edges: empty input,
  over-limit, no permission, duplicate submit. Lives in the story's 验收标准 and
  `assets/user-story-template.md`.
- **System-contract level** — cross-system / interface robustness: oversized / empty / corrupt
  payloads, missing upstream data, retry idempotency, partial success. Lives in PRD §6 and the
  "Boundary & edge-case checklist" of `references/technical-artifacts.md`. For AI/model features,
  error-cost asymmetry and human-in-the-loop live in `references/ai-ml-products.md`.

Pick the altitude that fits the story and cross-reference the other — don't copy the system list into
every story's AC.

## Method 1 — Negative & boundary scenarios (Given/When/Then)
Extend each story's AC past the happy path with explicit edge scenarios in the same form:
- 正常:Given 余额充足,When 提交支付,Then 下单成功并扣款。
- 边界:Given 余额恰好等于订单金额,When 提交,Then 允许并扣至 0。
- 异常:Given 余额为 0,When 提交,Then 阻止下单并提示「余额不足」。

For a family of boundary values, parameterize instead of writing four near-identical scenarios — a
Gherkin "Scenario Outline" with an examples table whose rows are `0 / 1 / 上限 / 上限+1`.

## Method 2 — Boundary value analysis + equivalence partitioning
Standard test-design techniques (ISTQB) for choosing *which* values to pin, so you cover a range
without listing every value:
- **Equivalence partitioning（等价类划分）** — split inputs into classes that behave alike (amount:
  `<0 / 0 / 1..上限 / >上限`), then specify one representative per class.
- **Boundary value analysis, BVA（边界值分析）** — defects cluster at class edges: pin **最小、最大、
  最小−1、最大+1、0**. "Robust" BVA adds a clearly-invalid value just outside the range.

This turns a vague "校验输入" into named, decided boundaries.

## Method 3 — The category checklist (completeness)
Walk this per story; keep only what applies and give each a decided behavior. It's the product-facing
condensation of Elisabeth Hendrickson's test-heuristics cheat sheet plus the Zero-One-Many (ZOM)
heuristic:
- **空 / 缺失 / null** — 空字段、未选择、可选数据缺失
- **零 / 一个 / 多个 (ZOM)** — 空列表、仅一条、海量(分页与性能)
- **数值与长度边界** — 最小、最大、最小−1、最大+1、0、负数、溢出、超长文本
- **非法格式 / 特殊字符** — 类型错误、emoji/中文、首尾空格、注入
- **重复 / 冲突** — 重复提交、同键两次、并发编辑
- **并发 / 时序** — 竞态、重试、乱序事件
- **权限 / 状态** — 未登录、角色不符、状态机非法跳转
- **外部依赖失败** — 超时、第三方报错、网络中断、部分成功
- **时间 / 时区 / 日历** — 跨天、夏令时、闰年、到期边界

## Method 4 — Extensions: alternate & exception flows
For a multi-step flow, the cleanest structure (Cockburn use-case style) is a **main success scenario**
plus numbered **extensions** hung off the step where each branches:
- **Alternative flow（备选流）** — a different path that *still reaches the goal* (用积分代替银行卡支付).
- **Exception flow（异常流）** — a step *fails and the goal is abandoned* (库存售罄 → 无法下单).

Number each extension to its step (`2a`, `2b`, `3a`) so reviewers see exactly where it branches. Use
this when a story genuinely branches; for simple stories, Method 1's edge scenarios suffice.

## Product shortcut — the five states
For UI work, decide all five states of each screen/story up front: **正常态 / 空态 / 错误态 / 加载态 /
极限态**. It's BVA in plain product language and catches the empty / error / loading gaps mockups
usually skip.

## Anti-patterns
- "妥善处理异常" with no specified behavior — undecided, so untestable.
- Only the happy path; the first empty / zero / max / timeout case surfaces in production.
- One token "边界/异常" line to tick a box instead of walking the checklist.
- Copying the system-contract edges (PRD §6) into every story's AC — cross-reference instead.
- Writing exhaustive QA test matrices in the PRD — specify *behavior*; let QA enumerate cases.
