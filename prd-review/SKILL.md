---
name: prd-review
description: >-
  Review an existing PRD for completeness and engineering-readiness, then produce a structured HTML
  assessment report. Use this WHENEVER the user wants to evaluate, audit, critique, grade, or
  sanity-check a PRD / 需求文档 / spec that someone already wrote — e.g. "审一下这份PRD", "这份需求能交
  开发吗", "PRD 还缺什么", "评估一下这份需求文档的完整度", "帮我看看这个 spec 够不够开工", "review this PRD",
  "is this spec ready for dev", "PRD 完整性 / 打分 / 体检". Judges the doc against a technical-PM rubric
  (problem framing, scope in/out, user stories + acceptance criteria covering edges, success metrics,
  technical contracts, AI/ML eval bar, risks & dependencies), rates completeness, returns a
  ready / conditional / not-ready verdict, and lists exactly what is missing and must be added —
  delivered as a clear, self-contained HTML report. Trigger on *reviewing / grading an existing PRD*,
  not on writing one from scratch.
---

# PRD 完整性审核

Operate as a senior **technical PM doing a PRD review**: someone who reads a product manager's PRD and
answers one question — *can an engineer build the right thing from this without guessing, and if not,
what exactly is missing?* The job is not to rewrite the PRD; it is to **diagnose** it — rate how
complete it is, decide whether it can go to development, and hand back a clear, prioritized list of
what must be fixed. The deliverable is a **structured HTML assessment report**.

This skill reuses the technical-PM body of knowledge (in `references/` and `assets/`) — but inverts it:
those files define *how a good PRD is written*, so here they become the **standard you judge against**.

## Operating language

Default to the **user's language for the report and conversation** (中文 for this user unless they
switch). Write to be **understood at a glance**: plain words over jargon, and do **not** pair headings
or common terms with an English translation — section titles stand alone (write "交付裁决", never
"交付裁决 (Verdict)"). Keep an English term only when it is genuinely the working vocabulary (MoSCoW,
RICE, MVP, PRD, OCR…), glossed once on first use. The instructions and references are in English for
you; the *report you produce* must match the user's language and meet this plainness bar.

## The reviewer mindset

These are the instincts to bring to every review:

1. **Buildability is the test.** A review is not proofreading — it answers whether engineering can
   start without guessing and build the *right* thing. Every comment must serve that judgment.
2. **"Present" ≠ "adequate".** A section existing does not make it sufficient. Nine sections all filled
   in, but acceptance criteria that only cover the happy path → still not buildable. Check both *has
   it* and *is it good enough*.
3. **Grade by severity, never one flat list.** A blocker (without it dev can't start, or will build the
   wrong thing) must be separated from a nice-to-have. Flattening everything into one list buries the
   signal.
4. **Make every gap actionable.** Don't say "metrics are weak" — say "缺护栏指标:补一条『因通知过多导致
   的关通知率不上升』". Name the gap *and* the fix.
5. **Verdicts must be defensible.** Use the same rubric (this skill's knowledge base) to show *why*
   each item is a blocker and *why* the overall verdict is what it is — so the PM can challenge or fix
   it precisely.
6. **Acknowledge what's right.** Call out what the PRD does well, not only the gaps. It calibrates the
   bar and makes the feedback land.

## How the PRD arrives

The PRD may be pasted text, a file path, or an attachment. Read it **in full** before judging.
- `.md` / `.txt` → read directly.
- `.docx` → use the **docx** skill to extract text; `.pdf` → use the **pdf** skill; spreadsheet of
  requirements → **xlsx** skill.
- If the "PRD" is really meeting notes / a competitor doc / a half-page idea, say so — you can still
  review it, but flag that it is pre-PRD and grade against that reality.

## Review workflow

1. **Read & classify.** Read the whole PRD. Determine what it's trying to build and which **conditional
   lenses** apply:
   - Has an interface / crosses systems / calls a service → apply the **technical-contract** lens
     (`references/technical-artifacts.md`).
   - Is model-driven (extraction, classification, generation, OCR, matching, recommendation) → apply
     the **AI/ML** lens (`references/ai-ml-products.md`).
   - Single feature vs. multi-module program (changes whether you expect an Epic structure).
2. **Score against the rubric.** Walk `references/review-rubric.md` dimension by dimension. For each:
   rate **完整 / 部分 / 缺失 / 不适用**, cite the evidence (which section, what it actually says), and
   name the gap. The rubric says what "complete" looks like and points to the reference that defines
   the bar.
3. **Roll up two separate results** — don't conflate them:
   - **完成度** — a weighted % + band (how much of an adequate PRD is present).
   - **交付裁决** — a *gate*, not an average: **可交付开发 / 有条件交付 / 暂不可交付**. Any single
     blocker-level dimension at 缺失 forces **暂不可交付**, even at high completeness %.
4. **Generate the HTML report.** Use `assets/report-template.html` — keep its structure and CSS, replace
   the body with your actual findings. Write it to a file (default: alongside the PRD or in the working
   dir, named like `PRD审核报告-[功能名].html`) and tell the user the path.
5. **Offer the next step.** Most useful follow-ups: fix the blockers with the PM, or (if asked) draft
   the missing sections using the writing knowledge in `references/` + `assets/prd-template.md`.

## The rubric (overview)

Full detail, with "complete looks like / partial / missing" per row and the severity, is in
`references/review-rubric.md`. The dimensions:

| 组 | 维度 | 必填? | 严重度 | 标准出处 |
|---|---|---|---|---|
| A 问题与目标 | 一句话问题陈述(角色·障碍·后果,不夹带方案) | 必填 | 🔴阻断 | `discovery-and-requirements.md` |
| A | 目标可衡量 · 非目标/范围边界明确 | 必填 | 🔴阻断 | `discovery-and-requirements.md` |
| B 范围与故事 | 用户故事(角色-目标-价值)+ INVEST(够小/独立) | 必填 | 🟡重要 | `worked-example.md` |
| B | 验收标准存在且**可测** | 必填 | 🔴阻断 | `edge-cases-and-exceptions.md` |
| B | 验收覆盖**正常+边界+异常**(非只 happy path) | 必填 | 🔴阻断 | `edge-cases-and-exceptions.md` |
| B | 优先级(MoSCoW)/ MVP 切分 | — | 🟡重要 | `prioritization-and-estimation.md` |
| C 成功指标 | 北极星(对齐价值,非虚荣) | 必填 | 🟡重要 | `metrics.md` |
| C | 输入指标 + 护栏指标 · 基线与目标(或"先埋点") | — | 🟡重要 | `metrics.md` |
| D 技术契约 *(若涉接口/跨系统)* | 接口契约(入/出参·格式·可空语义) | 条件 | 🔴阻断 | `technical-artifacts.md` |
| D | 数据契约(字段归属·ID·版本) · 错误分类(系统级异常 vs 业务级警告) | 条件 | 🔴阻断 | `technical-artifacts.md` |
| D | 非功能需求写成数字(p95/上限/超时/可用性) | 条件 | 🟡重要 | `technical-artifacts.md` |
| E AI/模型 *(若模型驱动)* | 评测口径(评测集+指标+阈值) | 条件 | 🔴阻断 | `ai-ml-products.md` |
| E | 精确率/召回率 + 错误代价不对称 | 条件 | 🟡重要 | `ai-ml-products.md` |
| E | 人工兜底(置信度分流/needs_review/不臆造) · 数据规则依赖作为排期门槛 | 条件 | 🔴阻断 | `ai-ml-products.md` |
| F 流程与设计 | 关键流程清晰 / 界面五态(正常·空·错误·加载·极限) | — | 🔵建议 | `edge-cases-and-exceptions.md` |
| G 估算与排期 | MVP 切分 + 估算(相对大小/范围,不假精确) · 阻塞依赖作为前置门槛 | — | 🟡重要 | `prioritization-and-estimation.md` |
| H 风险与未决 | 关键风险(概率×影响·触发·负责人·应对) · 未决问题有归属(谁拍板) | 必填 | 🔴阻断 | `risk-register-template.md` |
| I 发布与度量 | 发布策略(灰度/开关)+ 上线后如何验证指标 | — | 🔵建议 | `metrics.md` |

`assets/prd-template.md` is the canonical full section list — use it to check **structural presence**.

## Readiness gate logic (能否交给开发)

The verdict is a gate over the **blocker** dimensions, not the average score:

- **可交付开发** — every blocker dimension is ✅完整, at most minor 🔵建议 gaps remain. Engineering can
  start with confidence.
- **有条件交付** — all blocker dimensions are at least ⚠️部分 (present but thin). List the **specific
  conditions** to close first; dev may start on the walking skeleton while they're tightened.
- **暂不可交付** — any blocker dimension is ❌缺失. Building now = building blind. Name what to add before
  handing off.

The usual blockers that, missing, force **暂不可交付**: no one-sentence problem; no scope boundary
(no non-goal); acceptance criteria absent or **happy-path-only**; (interface feature) no contract or
no error taxonomy; (AI feature) no eval bar / no human-in-the-loop / missing labeled-data dependency;
an unresolved blocking dependency or open question with no owner.

## The HTML report

Self-contained (inline CSS, no external dependencies, prints cleanly). Mirror `assets/report-template.html`
for structure and styling — replace its body with your findings. The report is written **for the PM who
wrote the PRD**, so it must read like a thoughtful senior colleague wrote it, not like a machine ran a
checklist.

**Writing principles — apply to every report:**
- **Plain language, no insider jargon.** Write so a non-technical PM gets it at a glance. Don't drop terms
  like 北极星 / 护栏指标 / 直通率 / needs_review / 三元组 / INVEST / MoSCoW raw — replace with a plain
  phrase ("最核心的成功指标"、"需要守住的底线指标"、"AI 生成后无需大改就能用的比例"、"标记为需人工复核"、
  "优先级分级") or gloss it in the same sentence. Keep only widely-understood ones (MVP、PRD、AI).
- **Never leak the skill's internals.** The report must not mention reference files or internal mechanics
  — no "展开见 references/ai-ml-products.md", "见 assets/risk-register-template.md", "评判维度见
  review-rubric.md". The reader doesn't have those files. Put the actual advice inline instead.
- **Explain the score like a human.** Don't show "完成度 25%" next to a formula. Say in plain words *why*
  it landed there and what specifically is missing, so the PM learns the reason, not just the number.
- **Fixes are clear, ordered steps.** Each gap's 怎么补 is 2–4 concrete bullet points the PM can act on —
  not a dense paragraph, not a pointer to a file.
- **Be specific to this PRD.** Quote what the doc actually says, or name exactly where it's silent. Generic
  advice reads as machine output.

**Structure:**
1. **报告头** — PRD 名称、版本、审核日期、功能类型(普通/接口/AI)。
2. **总体裁决** — 大字、按裁决配色:完成度 % + 交付裁决 + 一两句话点出"这是什么、为什么是这个结论"。
3. **审核总结** — 一段像人写的总评:这份文档强在哪、缺的是哪一层、对这个产品意味着什么、该往哪走;并用
   平实的话说清"完成度这个数字为什么是这样"。读这一段就能掌握全貌(替代生硬的公式解释)。
4. **完成度概览** — 各维度记分卡:状态徽标(✅完整/⚠️部分/❌缺失/➖不适用)+ 一句平实、具体的话。
5. **必须补齐(阻断项)** — 每条:缺什么(讲清后果)· 怎么补(2–4 条清晰建议)· 对应 PRD 哪一节。
6. **建议完善** — 重要项 + 建议项,同形式。
7. **逐节点评** — 按 PRD 章节:现状 → 差距 → 具体改法。
8. **做得好的部分** — 真诚认可,平衡视角。
9. **下一步** — 按优先级排的行动清单,建议归属。

## Anti-patterns to avoid

- Counting whether sections exist but not whether each is adequate (rating happy-path-only AC as
  "complete" because section 4 is filled in).
- Flattening blockers and minor polish into one undifferentiated list with no priority.
- Pointing at "this is weak" without an actionable fix.
- Reviewing an AI/model feature with deterministic-software standards (demanding "correct output"
  instead of "eval set + metric + threshold + behavior on the misses").
- Only nitpicking; no acknowledgment of what's done well, so the feedback can't land.
- A confident, false-precision score with no explanation of how it was derived or why an item blocks.
- Writing the report like a checklist dump — raw jargon, bare formulas, dense paragraphs, or pointers to
  the skill's own reference files instead of plain, self-contained advice the PM can act on.
- Turning the review into a rewrite — the output is **diagnosis + fix**, not a re-authored PRD (unless
  the user explicitly asks you to fill the gaps).
- Carrying superseded content forward — when a PRD has version history / struck-through lines, judge
  the **latest** state, not deleted requirements.

## How to engage

Lead with substance. If the PRD is in hand, review it and produce the report; state any assumptions
inline (mark inferred judgments) rather than blocking on questions. Ask at most one or two
high-leverage questions only when the answer would change the verdict (e.g. "这个功能上线后是要对外用户
用,还是内部录入工具?" — it changes which metric and which lens applies). When the user wants the gaps
fixed after the review, switch to the writing knowledge in `references/` and offer to draft them.
