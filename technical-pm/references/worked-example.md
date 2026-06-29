# Worked Example — idea → problem → metric → story → AC

One compact, annotated example showing the *shape* of good PM output and how the pieces connect. It's
deliberately small (a single feature) so the chain is visible end to end. Use it as a pattern to
imitate, not a template to copy verbatim. Output language follows the user.

> **Why an example helps.** The templates show the *slots*; this shows the *judgment* — why each piece
> is written the way it is. The `> 为什么` annotations are the teaching; they wouldn't appear in a real
> deliverable.

---

## The request (as it arrives)
> "我们的协作工具要加一个 @提醒的通知功能,用户被 @了要能收到通知。"

> 为什么先停一下:这是一个**已经成型的解决方案**("加通知功能"),不是问题陈述。先把它当作信号,
> 回到问题本身(mindset #1: problem before solution)。

## 1. Problem (one sentence)
**团队成员**经常**错过被 @ 提及的重要消息**,因为**只有打开 App 才看得到**,导致**关键讨论延误、
需要私下二次催促**。

> 为什么这样写:用"[角色]难以[做某事],因为[障碍],导致[后果]"的结构,且不夹带解决方案(没有写
> "因为没有推送")——把"怎么解决"留到方案里,避免过早收敛。

## 2. Goal / Non-Goal
- **目标 Goal:** 让被 @ 的成员在数分钟内知晓并能一键跳回上下文。
- **非目标 Non-Goal:** 本期不做全量消息的通知(只做 @ 提及)、不做通知偏好的精细配置、不做邮件/短信
  渠道。

> 为什么列 Non-Goal:通知是 scope creep 的重灾区(很容易滑向"那顺便把所有消息都通知了吧")。明确
> 不做什么,是这里最有价值的一句话(mindset #2: ruthless scoping)。

## 3. Success Metrics
| 指标 | 定义 | 基线 | 目标 |
|---|---|---|---|
| 北极星 North Star | @ 提及后 30 分钟内被点击查看的比例 | 未知,需埋点 | ≥70% |
| 输入 Input | 通知送达成功率 | — | ≥99% |
| 护栏 Guardrail | 因通知过多导致的关闭通知率 / 投诉率 | — | 不上升 |

> 为什么配护栏:一个"提高触达"的功能很容易用"轰炸用户"来刷北极星。护栏指标(关通知率)防止局部
> 赢、全局输。没有基线就先埋点,不要盲设目标(metrics 原则)。

## 4. Scope & User Stories (MoSCoW)

### Epic:@ 提及通知
> 拆分思路:先走通一条端到端最薄路径(被 @ → 站内通知 → 点击跳回),再加深(US-2 的离线推送)。

**US-1(Must)** 作为**被 @ 的成员**,我希望**在站内收到一条可点击的通知**,以便**立刻看到是谁、在
哪条消息里 @ 了我**。
- **AC1.1(happy path):** Given 我在某条评论里被 @,When 该评论提交成功,Then 我的站内通知中心出现
  一条含"提及人 + 消息摘要 + 跳转链接"的通知。
- **AC1.2(edge):** Given 同一条消息里我被 @ 了多次,When 通知生成,Then 只产生一条通知(去重),不
  重复轰炸。
- **AC1.3(error):** Given 我已无权访问该频道,When 通知本应生成,Then 不生成该通知(不泄露我看不到
  的内容)。
- 技术任务/依赖:提及解析(@ 的实体识别)、通知写入与去重、未读计数;依赖现有权限模型。

**US-2(Should)** 作为**离线的成员**,我希望**在 App 未打开时也能收到推送**,以便**不必一直开着也不
错过**。
- **AC2.1:** Given 我开启了推送且 App 在后台,When 我被 @,Then 我在 1 分钟内收到一条系统推送,点击
  直达该消息。
- 技术任务/依赖:推送通道(APNs/FCM)接入;US-1 完成后再做(walking skeleton 先行)。

> 为什么 AC 用 Given/When/Then 且覆盖 happy + edge + error:可测是验收的前提(mindset #3)。AC1.2/
> AC1.3 这种去重和权限边界,正是"技术 PM"该替工程师想到的地方,也是日后线上事故的高发区。

### Could / Won't(本期不做)
- **Could:** @所有人(@everyone)的特殊处理。
- **Won't(this time):** 邮件/短信渠道、按关键词订阅——记录为未来候选,防止本期范围蔓延。

## 5. Key risks / open questions
| 风险/问题 | 概率×影响 | 应对/负责人 |
|---|---|---|
| 高频 @ 造成通知疲劳,用户整体关通知 | 中×高 | 去重 + 后续做聚合;监控护栏指标 / PM |
| 权限边界处理不当导致信息泄露 | 低×高 | AC1.3 显式覆盖 + 安全评审 / 工程 |
- 未决问题:推送渠道是否本期就上,取决于 APNs/FCM 接入成本——需工程评估后拍板。

> 为什么有这一节:把"想当然没问题"的地方显式变成可决策项,并指明谁来拍板(mindset #6 + JBGE:刚好
> 够用,不过度)。

---

**怎么用这个例子:** 注意这条链是如何环环相扣的——问题陈述决定了北极星(错过→30 分钟内查看),北极星
约束了范围(只做 @ 提及),范围拆成带可测 AC 的故事,边界情况又回填成风险与技术任务。产出的不是九个
互相独立的小节,而是一条前后自洽的推理链。这才是评审者真正买账的东西。
