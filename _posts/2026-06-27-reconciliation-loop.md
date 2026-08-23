---
layout: fable
title: "园丁老周的那幅画 · Old Zhou the Gardener and His Painting"
title_zh: "园丁老周的那幅画"
title_en: "Old Zhou the Gardener and His Painting"
concept: "reconciliation loop (level-triggered control)"
tags: [kubernetes]
illustration: /assets/art/2026-06-27-reconciliation-loop.jpg
---
<section class="zh" markdown="1">
村东头有个园丁，叫老周。他守着一座很大的花园，主人常年不在，只在工具棚里留下一幅画。画上是花园"应该有的样子"：三十棵月季、一池睡莲、东墙爬满藤、草坪修到脚踝高。

别的园丁干活靠记账。隔壁老李有个本子，写满了"昨夜大风吹倒两盆""前天兔子啌了三株"，他每天翻本子，照着一条条补救。可本子总有漏记的时候——有一夜下了冰隹，他睡得沉，没记上，第二天那片花就荒了，因为"账上没这一条"。

老周不记账。他每天早上只做一件事：站在花园中央，把眼前的花园和那幅画，一寸一寸地比。少一棵月季，补一棵；藤爬过了线，剪回去；睡莲多了，捞走几片。他从不问"昨晚发生了什么"，他只问"现在和画差在哪"。

有人笑他笨：同一棵月季，今天补、明天若还缺还补，重复劳动。老周说，正因为我不在乎"发生过什么"，所以无论是风、是兔子、是冰隹、还是我自己昨天补错了——我今早看一眼，照样能补回来。哪怕我中暑晕过去三天，醒来也不必翻三天的账，我只要再看一眼画，接着比就行。

那年闹大旱，半个花园枯了。老李对着账本崩溃——漏记太多，理不清。老周照旧站到园子中央，看画，比，补。一季之后，他的花园又和画一模一样了。

——到这儿你大概已经认出来了：老周就是 Kubernetes 里的 _controller_，他守的那幅画是 _desired state_，眼前的花园是 _actual / observed state_，他每天"看一眼、比一比、补一补"的动作，就是那个永不停歇的 _reconciliation loop_。

---

_概念：Reconciliation Loop（协调循环）与 level-triggered 控制_

Kubernetes 控制器的核心不是"对事件做出反应"，而是持续地把 _观测到的实际状态_ 和 _声明的期望状态_ 做对比，然后采取动作让二者收敛。这叫 _level-triggered_（基于"现在是什么状态"），区别于 _edge-triggered_（基于"刚刚发生了什么事件"）。这正是它健壮、能自愈的根本原因。

_故事里的隐喻对应：_

- 老周 = _controller / operator_
- 那幅画 = _desired state_（spec，比如 Deployment 的 replicas: 30）
- 眼前的花园 = _actual state_（status，通过 API 观测到的真实情况）
- "看一眼 → 比一比 → 补一补" = _reconcile_：observe → diff → act，循环不止
- 老李的记账本 = _edge-triggered_：依赖事件流，漏一个事件就永久偏差，无法自愈
- "同一棵月季今天补明天还补" = _idempotent_（幂等）：reconcile 可反复执行而无副作用
- "中暑晕三天，醒来再看一眼画" = controller 重启后无需 replay 历史事件，从当前状态接着收敛
- "大旱半园枯了照样补回来" = 自愈：无论偏差从何而来、有多大，loop 总会把系统拉回期望状态
</section>
<section class="en" markdown="1">
At the east end of the village lived a gardener named Old Zhou. He tended a very large garden whose owner was away year-round, leaving behind only a painting in the tool shed. The painting showed how the garden was _supposed_ to look: thirty rose bushes, a pond of water lilies, the east wall covered in climbing vines, the lawn trimmed to ankle height.

Other gardeners worked by keeping ledgers. Old Li next door had a notebook filled with entries — "last night's gale knocked over two pots," "rabbits gnawed three plants the day before" — and each day he'd flip through it and fix things one entry at a time. But a ledger always has gaps: one night a hailstorm came while he slept too deeply to write it down, and the next day that patch of flowers went to ruin, because "it wasn't in the book."

Old Zhou kept no ledger. Every morning he did just one thing: he stood in the center of the garden and compared what was before his eyes with the painting, inch by inch. A rose missing? Plant one. Vines crossed the line? Trim them back. Too many water lilies? Scoop a few out. He never asked "what happened last night" — he only asked "where does it differ from the painting right now."

People mocked him as a fool: the same rose bush, replanted today, replanted again tomorrow if it's still missing — wasted, repeated labor. Old Zhou said: precisely because I don't care about "what happened," whether it was the wind, the rabbits, the hail, or even my own mistaken fix from yesterday — I take one look this morning and I can set it right all the same. Even if I collapsed from heatstroke and lay out for three days, when I wake I needn't replay three days of records; I only need one more look at the painting, and carry on comparing.

The year of the great drought, half the garden withered. Old Li broke down over his ledger — too many missed entries, impossible to untangle. Old Zhou stood in the center of his garden as always, looked at the painting, compared, restored. One season later, his garden matched the painting exactly once more.

— By now you've probably recognized it: Old Zhou is the _controller_ in Kubernetes, the painting he guards is the _desired state_, the garden before his eyes is the _actual / observed state_, and his daily act of "take a look, compare, restore" is that never-resting _reconciliation loop_.

---

_Concept: The Reconciliation Loop and Level-triggered Control_

The heart of a Kubernetes controller is not "reacting to events," but continuously comparing the _observed actual state_ against the _declared desired state_, then taking action to make the two converge. This is _level-triggered_ (based on "what is the state right now"), as opposed to _edge-triggered_ (based on "what event just happened"). This is exactly why it is robust and self-healing.

_The metaphor mapping:_

- Old Zhou = the _controller / operator_
- The painting = the _desired state_ (spec, e.g. a Deployment's replicas: 30)
- The garden before his eyes = the _actual state_ (status, the real situation observed via the API)
- "look → compare → restore" = _reconcile_: observe → diff → act, looping endlessly
- Old Li's ledger = _edge-triggered_: dependent on the event stream; miss one event and you drift permanently, unable to self-heal
- "replant the same rose today and again tomorrow" = _idempotent_: reconcile can run over and over with no side effects
- "heatstroke for three days, then one look at the painting" = after a controller restarts it needn't replay historical events; it converges from the current state
- "half the garden withered in the drought, restored all the same" = self-healing: no matter where the drift came from or how large, the loop always pulls the system back to the desired state
</section>
