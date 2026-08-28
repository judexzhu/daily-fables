---
layout: fable
title: "碎碗有数 · The Porcelain Allowance"
title_zh: "碎碗有数"
title_en: "The Porcelain Allowance"
concept: "Error budget (SLO-based reliability target)"
tags: [sre, distributed-systems]
illustration: /assets/art/2026-08-28-error-budget.jpg
---
<section class="zh" markdown="1">
清河镇上有一家瓷器行，掌柜姓陶，做了三十年生意。他家的碗碟从窑口运到铺面，中间隔着六十里山路、两段水路、三次装卸。每一百件里总有几件碎在路上。

陶掌柜年轻时脾气暴，碎一件就骂一顿，骂完了逼着伙计用最好的稻草、最慢的牛车、最小心的装法再跑一趟。伙计们怕碎，走得极慢，一个月只能跑两趟。铺面经常断货，客人等不及就去了别家。

后来陶掌柜想通了一件事：碎碗不是错，碎**太多**才是错。

他坐下来算了一笔账。一百件碗碟，客人能接受碎三件——也就是说，只要到手的完好率不低于九成七，这门生意就做得下去。他把这个数刻在门板上：**九七**。

然后他又算了另一笔账。既然九成七就够了，那每一百件里有**三件碗的余量**可以碎。这三件碎碗，不是损失——是**预算**。

从那天起，规矩变了。

伙计跑完一趟回来，先数碎了几件。只要还没超三件，陶掌柜不但不骂，还鼓励他们试新路线——翻山走近道能不能省半天？换小船走浅水能不能避开风浪？这些试验一定会多碎几件，但只要月底一算，碎碗总数没超预算，那就值。

有一个月，伙计们特别大胆，试了一条险路。月中一数，已经碎了两件半（有一件裂了，勉强算半件）。陶掌柜拍了拍手："预算快用完了。这个月后半段，老老实实走老路，不许再试新花样。"

伙计们立刻收回来，稳稳当当走完后半个月。月底一算：碎了二又四分之三件，九成七守住了。

最有意思的是另一种情况。有连着两个月，一件碗都没碎。陶掌柜反而不高兴了。他把领头的伙计叫来："你们走得太小心了。碎碗预算一点没花，说明你们没试过任何新东西。下个月我要你们拿出这两个月攒下来的余量，去试那条传说中能省一天的峡谷水路。"

还有更微妙的一层。陶掌柜不光管运碗的人，也管造碗的窑工。窑工总想试新釉色、新器形，但新配方失败率高，出窑碎得多。陶掌柜说："你们也有预算。窑口的碎率和路上的碎率共用同一笔预算——你们多碎一件，路上就得少碎一件。要是窑口这个月碎太多，路上那帮人就不许冒险了。"

于是窑工和伙计学会了一件事：**速度和稳定不是对立的，它们共享一个碎碗预算**。花预算的时候大胆，预算快用完时收手。长远看，铺面的货架从没断过，客人从没等太久，而路线和釉色一年比一年好。

——到这儿你大概已经认出来了：这不是瓷器行的故事。九成七是 *SLO*（*Service Level Objective*），三件碎碗是 *error budget*，试新路线是功能发布和架构变更，走老路是 *reliability freeze*，窑工和伙计共享预算是开发团队和 SRE 团队在同一个 *error budget policy* 下协作。这是 **error budget** 的故事。

### 这是什么

*Error budget* 是 Google SRE 实践的核心量化工具。逻辑极简：

1. 先定一个 *SLO*——比如"99.9% 的请求在 300ms 内返回成功"。
2. 那么每个月允许的"坏请求"数量就是总请求的 0.1%。这个 0.1% 就是 *error budget*。
3. 只要 error budget 还有余量，团队可以自由发布新功能、跑实验、做迁移——这些操作一定会带来风险。
4. 一旦 error budget 即将耗尽（这个月的坏请求快到上限了），立刻冻结变更——只做修复可靠性的工作，直到预算恢复。

它的精髓在于**把可靠性变成可以花的货币**：

- 产品团队想快速迭代？可以，但要花 error budget。
- SRE 团队想控制风险？可以，但不能把 error budget 囤着不花——那意味着系统过于保守，速度不够快。
- 两个团队的利益通过同一个数字对齐：花得太快要踩刹车，花得太慢要踩油门。

实现上，error budget 通常通过 *burn rate*（消耗速率）来监控：如果当前的错误速率会让你在窗口期结束前用光预算，就触发告警。这就是 *multi-window, multi-burn-rate alert* 的基础——Google SRE Workbook 第五章的核心。

### 为什么重要

没有 error budget 之前，"要快还是要稳"是一场永远吵不完的架——产品经理要速度，SRE 要可靠性，谁也说服不了谁。Error budget 把它变成了一个可量化的决策：不用吵，看数字。

它也解决了一个更隐蔽的问题：**过度可靠**。如果你的 SLO 是 99.9% 但实际跑到了 99.99%，那意味着你在可靠性上花了十倍的代价却没有用——你的用户根本分辨不出 99.9% 和 99.99% 的区别，因为他们自己的网络和设备的可靠性远不到 99.99%。这些浪费掉的余量本可以拿来更快地发布功能。

在 Kubernetes / OpenShift 上，error budget 直接对接告警策略。Prometheus 的 `slo-generator` 或 Google Cloud 的 Service Monitoring 可以自动计算 budget 消耗，结合 `PodDisruptionBudget` 和 rollout 策略，在 budget 不足时自动阻止新版本部署。

### 隐喻对应表

- 瓷器行 —— 运行中的服务
- 一百件碗碟 —— 请求总量
- 门板上的"九七" —— SLO（Service Level Objective）
- 三件碎碗的余量 —— error budget
- 伙计试新路线 —— 功能发布、架构变更、实验
- 走老路、不许新花样 —— reliability freeze（变更冻结）
- 月底算碎碗总数 —— error budget 消耗计算
- 连续两月一件没碎 —— 系统过度保守，创新速度不足
- 窑工和伙计共享预算 —— 开发团队与 SRE 共享同一个 error budget policy
- 窑口碎太多路上就不能冒险 —— 一方消耗过多预算，另一方必须收敛
</section>
<section class="en" markdown="1">
There was a porcelain shop in Qinghe Town, run by a man surnamed Tao, who had been in the business for thirty years. His bowls and dishes traveled sixty li of mountain roads, two stretches of river, and three loading docks between the kiln and his shop. Out of every hundred pieces, a few always broke along the way.

When Tao was young, he had a temper. One broken piece and he would shout at the porters, then force them to re-pack with the finest straw, the slowest ox-cart, the most cautious method, and run the route again. The porters, terrified of breakage, moved painfully slowly. They could only manage two trips a month. The shop shelves emptied. Customers tired of waiting and went elsewhere.

Eventually Tao understood something: broken bowls were not the mistake. Breaking **too many** was.

He sat down and did the arithmetic. Out of a hundred pieces, customers could tolerate three broken — meaning as long as the intact rate stayed at or above ninety-seven percent, the business would hold. He carved the number on the door plank: **97**.

Then he did a second calculation. Since ninety-seven percent was enough, he had an **allowance of three bowls per hundred** that could break. Those three broken bowls were not losses — they were a **budget**.

From that day, the rules changed.

When porters returned from a trip, they first counted breakage. As long as it was under three, Tao didn't scold — he even encouraged them to try new routes. Could the mountain shortcut save half a day? Could a smaller boat on the shallows dodge the wind? These experiments would certainly break a few more pieces, but if the month-end tally stayed within budget, it was worth it.

One month, the porters were especially bold and tried a risky route. At mid-month they counted: two and a half broken (one was cracked — call it half). Tao clapped his hands: "Budget's nearly spent. Second half of this month, take the old road. No more experiments."

The porters pulled back immediately and ran the safe route for the remaining two weeks. Month-end tally: two and three-quarters broken. Ninety-seven percent held.

The most interesting case was the opposite. For two consecutive months, not a single bowl broke. Tao was *unhappy*. He called the lead porter in: "You're being too careful. You spent none of the breakage budget, which means you tried nothing new. Next month, I want you to take the surplus from these two months and attempt the canyon waterway that supposedly saves a full day."

There was a subtler layer still. Tao managed not only the porters but also the kiln workers. The workers always wanted to try new glazes and shapes, but new formulas had higher failure rates — more pieces cracked in the firing. Tao said: "You have a budget too. The kiln's breakage rate and the road's breakage rate share the same budget — every extra piece you break in the kiln is one fewer the porters can afford to break on the road. If the kiln runs hot this month, the porters must play it safe."

And so the kiln workers and the porters learned one thing: **speed and stability are not opposites — they share a breakage budget**. Spend boldly while the budget allows; pull back when it runs low. Over the long run, the shelves were never bare, customers never waited too long, and the routes and glazes improved year after year.

— By now you have probably recognized it: this is not a story about a porcelain shop. Ninety-seven percent is the *SLO* (*Service Level Objective*), the three breakable bowls are the *error budget*, trying new routes is feature releases and architecture changes, sticking to the old road is a *reliability freeze*, and the kiln workers and porters sharing one budget is the development team and the SRE team collaborating under a single *error budget policy*. This is the story of the **error budget**.

### What it is

The *error budget* is the core quantitative tool in Google's SRE practice. The logic is minimal:

1. Set an *SLO* — for example, "99.9% of requests return successfully within 300ms."
2. The number of "bad requests" allowed per month is 0.1% of total requests. That 0.1% is the *error budget*.
3. As long as error budget remains, the team is free to ship features, run experiments, perform migrations — all of which carry risk.
4. Once the error budget is nearly exhausted (this month's bad requests are approaching the limit), freeze all changes — work only on reliability until the budget recovers.

Its essence is **turning reliability into a spendable currency**:

- The product team wants to iterate fast? Fine — but it costs error budget.
- The SRE team wants to control risk? Fine — but they cannot hoard error budget either, because that means the system is over-provisioned for reliability and shipping too slowly.
- Both teams' incentives align through a single number: spend too fast and you hit the brakes; spend too slow and you hit the accelerator.

In practice, error budget is monitored through *burn rate*: if the current error rate would exhaust the budget before the window closes, an alert fires. This is the foundation of *multi-window, multi-burn-rate alerting* — the heart of Chapter 5 in the Google SRE Workbook.

### Why it matters

Before the error budget, "move fast or stay stable" was an argument that never ended — product managers wanted speed, SREs wanted reliability, and neither could convince the other. The error budget turns it into a quantifiable decision: don't argue, look at the number.

It also solves a subtler problem: **over-reliability**. If your SLO is 99.9% but you're actually running at 99.99%, you're spending ten times the effort on reliability that nobody needs — your users can't tell the difference between 99.9% and 99.99% because their own networks and devices aren't that reliable. That wasted margin could have been spent shipping features faster.

On Kubernetes / OpenShift, the error budget plugs directly into alerting policy. Prometheus's `slo-generator` or Google Cloud's Service Monitoring can compute budget burn automatically, and in combination with `PodDisruptionBudget` and rollout strategies, automatically block new deployments when the budget is insufficient.

### Metaphor mapping

- The porcelain shop — a running service
- One hundred bowls and dishes — total request volume
- "97" carved on the door — the SLO (Service Level Objective)
- The allowance of three breakable bowls — the error budget
- Porters trying new routes — feature releases, architecture changes, experiments
- Sticking to the old road, no experiments — reliability freeze (change freeze)
- Month-end breakage tally — error budget consumption calculation
- Two months with zero breakage — system is over-conservative, innovation velocity too low
- Kiln workers and porters sharing one budget — dev team and SRE sharing one error budget policy
- Kiln breaks too many, porters must play safe — one side over-consuming budget forces the other to conserve
</section>
