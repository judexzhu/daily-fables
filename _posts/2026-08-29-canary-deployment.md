---
layout: fable
title: "先尝一碗 · The First Bowl"
title_zh: "先尝一碗"
title_en: "The First Bowl"
concept: "Canary deployment (progressive rollout)"
tags: [ci-cd, sre, kubernetes]
illustration: /assets/art/2026-08-29-canary-deployment.jpg
---
<section class="zh" markdown="1">
清河镇的面馆叫"老周家"，传了三代，靠的就是一碗红汤牛肉面。卤水的配方写在一本油渍斑驳的册子上，谁也不许改动一个字。

第三代掌柜周平不信这个邪。他在后厨琢磨了半年，觉得把八角减半、加一撮花椒和半勺陈皮，汤底会更透亮、回味更长。但他不敢一刀切——万一客人不认，一天丢掉三十年的招牌。

他想了个法子。

面馆一共十二张桌子。周平挑了靠窗的两张，每天只给这两桌上新汤。其余十桌照旧。两桌客人不知道自己吃的是新配方，只知道今天的面好像"有点不一样"。

他让跑堂的阿贵盯着这两桌。退碗的比平时多不多？加面的比平时少不少？有没有人皱眉？有没有人问"今天汤怎么了"？阿贵每晚把数字报给他。

头三天，两桌的退碗率和老配方一样。第四天，有个老客连汤都喝干了，这在老周家极少见。周平又多开了两桌，变成四桌新汤、八桌老汤。

第二周，四桌的数字依然稳当。周平再开两桌。六比六。

第三周出了状况。有一桌客人说汤太麻，嘴发木。周平一查，是那天花椒磨得太细，出味过重。他没有把全部六桌退回老配方——他只把花椒从细磨改成粗碎，第二天再看数字。数字回来了。

到第四周，他把最后六桌也切过去，十二桌全上新汤。又观察了整整一周，退碗率不升反降。这时候他才把老配方的册子翻到最后一页，工工整整写上新方子，把旧的一页折了角，不撕掉——万一哪天需要退回来。

后来有人问周平："你就不怕头两桌的客人吃了新汤不舒服？"

周平说："怕。但只坏两桌，不坏十二桌。两桌我赔得起，十二桌我赔不起。"

又有人问："你为什么不直接问客人喜不喜欢？"

周平笑了："客人嘴上说的和碗里剩的，不是一回事。我只看碗。"

——到这儿你大概已经认出来了：这不是面馆的故事。十二张桌子是生产流量，头两桌是 *canary* 流量，新汤是新版本，老汤是旧版本，退碗率是错误率，阿贵的夜报是 *metrics*，从两桌扩到四桌再扩到全部是 *progressive rollout*，花椒太细那一天是 *automated rollback trigger*，旧配方折角不撕是保留回滚镜像。这是 **canary deployment** 的故事。

### 这是什么

*Canary deployment* 是一种渐进式发布策略：把新版本先推送到一小部分实例或一小部分用户流量上，用生产环境的**真实请求**验证它的行为，再逐步扩大比例，直到完全替换旧版本。

核心流程：

1. 部署新版本到少量实例（"canary pod"），让它承接 5-10% 的流量。
2. 持续对比 canary 和 baseline 的关键指标——延迟 p99、错误率、资源消耗。
3. 指标正常 → 提高流量比例（10% → 25% → 50% → 100%）。
4. 指标异常 → 立即回滚，把 canary 实例切回旧版本。

在 Kubernetes / OpenShift 上，*Argo Rollouts* 或 *Flagger* 通过 `AnalysisRun` 自动查询 Prometheus 指标，根据预设阈值决定升级还是回滚——人甚至不需要盯着看。Istio / Envoy 的流量权重（`VirtualService.weight`）精确控制 canary 比例。

名字来自矿井里的金丝雀：矿工下井前先放一只鸟进去，鸟没事人才下。新版本就是那只鸟。

### 为什么重要

传统发布是"大爆炸"——全部实例同时切到新版本。一旦有 bug，影响 100% 的用户。回滚本身又是一次大爆炸，同样有风险。Canary deployment 把爆炸半径从 100% 压到 5%，把回滚从"重新部署"变成"切流量权重"，毫秒级生效。

更深的价值在于：它让**发布变成了一个可观测、可量化的实验**。不是"我觉得没问题"，而是"canary 的 p99 延迟比 baseline 高了 12ms，超过阈值，自动回滚"。这和 *error budget* 是一对：error budget 决定"能不能发"，canary deployment 决定"怎么发才安全"。

在大规模系统里（Google、Netflix、LinkedIn），canary 不只看错误率——还看业务指标（点击率、转化率、播放完成率）。一个技术上没有 bug 的版本，如果让用户行为变差了，照样会被自动回滚。这就是周平为什么"只看碗"。

- 十二张桌子 —— 生产环境的全部实例 / 流量
- 靠窗的两张桌 —— canary 实例（承接少量真实流量）
- 新汤（花椒陈皮配方） —— 新版本的代码或配置
- 老汤（原始红汤） —— 当前稳定版本（baseline）
- 退碗率 —— 错误率、延迟 p99 等 SLI 指标
- 阿贵的每晚数字 —— Prometheus / Datadog 的 metrics pipeline
- 从两桌扩到四桌、六桌、十二桌 —— progressive rollout（流量权重逐步提升）
- 花椒太细导致一桌投诉 —— canary 指标异常触发告警
- 只改花椒粗细而不退回全部老配方 —— 修复后重新评估，而非盲目全量回滚
- 旧配方折角不撕 —— 保留旧版本镜像，支持快速回滚
- "只看碗" —— 用客观指标而非主观反馈做决策
</section>
<section class="en" markdown="1">
The noodle shop on Qinghe's main street was called Old Zhou's. Three generations, one recipe: a red-broth beef noodle. The stock formula was written in a grease-stained notebook that nobody was allowed to change by a single character.

Zhou Ping, the third-generation owner, was not a man who left things alone. He spent half a year in the back kitchen and concluded that halving the star anise, adding a pinch of Sichuan peppercorn and half a spoon of dried tangerine peel would make the broth clearer and the aftertaste longer. But he did not dare switch all at once — one bad day could destroy thirty years of reputation.

He came up with a method.

The shop had twelve tables. Zhou Ping picked the two by the window. Every day, only those two tables received the new broth. The other ten got the original. The two tables' customers did not know they were eating a different recipe; they only noticed that today's noodles were "a little different somehow."

He told A-Gui, the waiter, to watch those two tables. Were more bowls being sent back than usual? Were fewer people ordering seconds? Did anyone frown? Did anyone ask "What's wrong with the broth today?" Every evening A-Gui reported the numbers.

For the first three days the return rate at the two tables matched the old recipe's exactly. On the fourth day an old regular drank the broth to the last drop — a rare event at Old Zhou's. Zhou Ping opened two more tables: four on new broth, eight on old.

In the second week, the four tables' numbers held steady. He opened two more. Six and six.

In the third week there was trouble. One table complained the broth was too numbing, their lips went tingly. Zhou Ping investigated: that day the peppercorn had been ground too fine, releasing too much heat. He did not pull all six tables back to the old recipe — he only changed the peppercorn from fine grind to coarse crack and watched the next day's numbers. They came back in line.

By the fourth week he switched the last six tables over. All twelve on new broth. He watched for one more full week; the return rate didn't rise — it dropped. Only then did he turn to the last page of the notebook, write the new formula in neat strokes, and dog-ear the old page — he did not tear it out, in case he ever needed to go back.

Someone later asked Zhou Ping: "Weren't you afraid the first two tables would have a bad experience?"

"I was," he said. "But ruining two tables I can afford. Ruining twelve I cannot."

"Why didn't you just ask the customers whether they liked it?"

Zhou Ping smiled. "What people say and what they leave in the bowl are not the same thing. I only watch the bowl."

— By now you have probably recognized it: this is not a story about a noodle shop. The twelve tables are production traffic, the first two are *canary* traffic, the new broth is the new version, the old broth is the baseline, the return rate is the error rate, A-Gui's nightly report is the *metrics pipeline*, scaling from two tables to four to all twelve is a *progressive rollout*, the too-fine peppercorn day is an *automated rollback trigger*, and the dog-eared old page is the rollback image kept ready. This is the story of **canary deployment**.

### What it is

*Canary deployment* is a progressive release strategy: push the new version to a small fraction of instances or user traffic first, validate its behavior with **real production requests**, then gradually widen the fraction until the old version is fully replaced.

The core flow:

1. Deploy the new version to a small number of instances (the "canary pod"), routing 5–10% of traffic to it.
2. Continuously compare the canary's key metrics against the baseline — p99 latency, error rate, resource consumption.
3. Metrics normal → increase traffic share (10% → 25% → 50% → 100%).
4. Metrics abnormal → roll back immediately, switching canary instances back to the old version.

On Kubernetes / OpenShift, *Argo Rollouts* or *Flagger* use `AnalysisRun` to query Prometheus automatically and decide — promote or roll back — based on preset thresholds. No human needs to watch. Istio / Envoy traffic weights (`VirtualService.weight`) give precise control over the canary fraction.

The name comes from the coal-mine canary: miners sent a bird down the shaft first; if the bird was fine, the people followed. The new version is the bird.

### Why it matters

Traditional releases are "big bang" — every instance switches to the new version at once. If there is a bug, 100% of users are hit. Rolling back is another big bang, carrying its own risk. Canary deployment compresses the blast radius from 100% to 5% and turns rollback from "redeploy" into "shift traffic weight" — effective in milliseconds.

The deeper value: it makes **a release into an observable, quantifiable experiment**. Not "I think it's fine" but "the canary's p99 latency is 12ms higher than baseline, exceeding threshold, auto-rolling back." This pairs with *error budget*: the error budget decides *whether* you can ship; canary deployment decides *how* to ship safely.

At scale (Google, Netflix, LinkedIn), canaries watch more than error rates — they watch business metrics (click-through rate, conversion, playback completion). A version that is technically bug-free but degrades user behavior will still be automatically rolled back. That is why Zhou Ping "only watches the bowl."

### Metaphor mapping

- Twelve tables — all production instances / traffic
- The two window tables — canary instances (receiving a small share of real traffic)
- New broth (peppercorn-and-peel formula) — the new version of code or configuration
- Old broth (original red stock) — the current stable version (baseline)
- Bowl return rate — error rate, p99 latency, and other SLI metrics
- A-Gui's nightly numbers — the Prometheus / Datadog metrics pipeline
- Scaling from two to four to six to twelve tables — progressive rollout (traffic weight ramp)
- Too-fine peppercorn causing one table's complaint — canary metrics anomaly triggering an alert
- Changing only the peppercorn grind instead of reverting all tables — fixing and re-evaluating rather than blind full rollback
- Dog-earing the old page instead of tearing it out — keeping the old version image ready for fast rollback
- "I only watch the bowl" — making decisions on objective metrics, not subjective feedback
</section>
