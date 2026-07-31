---
layout: fable
title: "两次点头的秋收宴 · The Steward's Two Rounds"
title_zh: "两次点头的秋收宴"
title_en: "The Steward's Two Rounds"
concept: "Two-Phase Commit"
tags: [distributed-systems]
illustration: /assets/art/2026-07-31-two-phase-commit.jpg
---
<section class="zh" markdown="1">
村里要办秋收宴，管家老周揽下张罗的活。宴席要成，缺一样都不行：面点铺的酥饼、渡口的船（把河对岸的客人接来）、乐班的鼓笛，还有村中大堂的场地。这四家谁也管不着谁，各忙各的。

老周没有一上来就吆喝"今晚开宴"。他知道那样太冒失——万一船夫今晚要修船，客人过不了河，酥饼白蒸一屉。于是他挨家去问，问法很讲究："今晚的宴，你能担下你那一份吗？能的话，先把它给我留住。"

面点师傅点头，当场把最好的一炉留出来，谁来买都不卖；船夫点头，把船栓在渡口，不再接别的活；乐班把今晚的档期划掉；堂倌把大堂的门闩上，只等这场宴。四家各自捏着一张"我留住了"的承诺，谁也不敢反悔——他们都悬在一种半吊子的状态里：既没开席，也不能松手。

老周揣着四个"能"回到自己屋里。这才是关键的一步：只有当四家**全都**说了能，他才会再跑第二趟，挨家丢下一句"开席！"；只要有一家摇头——哪怕船夫嘟囔"今晚起雾，不敢开船"——他就得掉头去跟另外三家说"散了，各自忙去吧"。要么四家一起成，要么一起散，绝不能面点铺开了张，客人却还在河对岸干等。

——到这儿你大概已经认出来了：这就是分布式事务里的 *two-phase commit*（两阶段提交）。老周是 *coordinator*，四家店是 *participants*。第一趟问"能不能担下"是 *prepare / vote* 阶段，点头就是投 yes 票、并把资源锁住（那炉酥饼、那条船，就是被 lock 住、进入 in-doubt 的资源）；第二趟"开席"或"散了"是 *commit / abort* 阶段。只有全票通过才 commit，一票否决就全体 abort——这就是**原子性**：所有参与者要么一起提交，要么一起回滚。

它为什么重要：跨多个独立系统改一件事时（订单扣库存、账户扣款、仓库发货，各在一台机器上），你需要它们"同生共死"，不能钱扣了货没发。2PC 给的正是这个保证。但它有个著名的软肋：假如老周收齐了"能"、正要跑第二趟时突然病倒，四家就只能一直捏着承诺干等——酥饼不敢卖，船不敢开，既不知道该开席还是该散。这就是 2PC 的 *blocking* 问题：coordinator 一旦在关键时刻失联，投了 yes 的 participant 会一直卡在 in-doubt。后来的 Raft、Paxos、三阶段提交，很大程度就是在补这个洞。

隐喻对应表：

- 管家老周 → *coordinator*（事务协调者）
- 面点铺 / 渡口 / 乐班 / 堂倌 → *participants*（各参与者、资源管理器）
- 第一趟"你能担下吗" → *prepare / vote-request* 阶段
- 点头、留住那炉饼、栓住船 → 投 yes 票 + lock 资源，进入 in-doubt / prepared 状态
- 任一家摇头 → 一票否决，触发全体 abort
- 第二趟"开席！" / "散了" → *commit / abort* 广播
- 四家一起成或一起散 → **原子性**（atomicity）
- 老周半路病倒、四家干等 → 2PC 的 *blocking* 问题（coordinator 单点失效）
</section>
<section class="en" markdown="1">
The village was to hold its harvest feast, and Old Zhou the steward took charge of arranging it. Four things had to come together, and missing any one would ruin the night: pastries from the bakery, the ferry to carry guests across the river, drums and pipes from the music troupe, and the great hall itself. The four kept their own shops and answered to no one but themselves.

Zhou did not stride out and cry "the feast is tonight!" That would be reckless — what if the ferryman had to mend his boat and no guests could cross? A whole tray of pastries wasted. So he went door to door, and his question was carefully worded: "Tonight's feast — can you carry your part? If you can, set it aside and hold it for me."

The baker nodded and pulled his finest batch from the oven, refusing to sell it to anyone else. The ferryman nodded and tied his boat at the landing, turning away other work. The troupe crossed the night off their calendar. The hall-keeper barred the great door, keeping it for the feast alone. Each now clutched a promise — "I have set mine aside" — and none dared take it back. They hung in a suspended state: the feast had not begun, yet none could let go.

Zhou carried his four "yeses" home. Here was the crucial part: only if **all four** had said yes would he make a second round, dropping a single word at each door — "Begin!" If even one had shaken their head — even the ferryman muttering "fog tonight, I dare not sail" — he would turn back and tell the other three, "It's off, go about your business." Either all four together, or none — never the bakery open for business while the guests waited stranded on the far bank.

— By now you've probably recognized it: this is *two-phase commit* in distributed transactions. Zhou is the *coordinator*, the four shops are *participants*. The first round, asking "can you carry it," is the *prepare / vote* phase; a nod is a yes-vote plus locking the resource (that batch of pastries, that boat — now locked, held in-doubt). The second round, "Begin!" or "It's off," is the *commit / abort* phase. Commit only on a unanimous yes; a single no aborts everyone — this is **atomicity**: all participants commit together, or all roll back.

Why it matters: when one change must span several independent systems (reserve inventory, charge the card, ship the order, each on its own machine), you need them to live or die together — you can't charge the money and fail to ship. 2PC gives exactly that guarantee. But it has a famous weak spot: if Zhou collects every "yes" and then collapses before his second round, the four are left clutching their promises indefinitely — the pastries unsold, the boat unsailed, no one knowing whether to begin or disband. This is the *blocking* problem: once the coordinator vanishes at the wrong moment, participants that voted yes are stuck in-doubt. Raft, Paxos, and three-phase commit are, in large part, attempts to patch this hole.

Metaphor map:

- Old Zhou the steward → *coordinator* (transaction coordinator)
- Bakery / ferry / troupe / hall-keeper → *participants* (resource managers)
- First round, "can you carry it?" → *prepare / vote-request* phase
- Nodding, holding the batch, tying up the boat → voting yes + locking the resource, entering the in-doubt / prepared state
- Any one shaking their head → a single no, triggering a global abort
- Second round, "Begin!" / "It's off" → *commit / abort* broadcast
- All four together or none → **atomicity**
- Zhou collapsing mid-way, the four left waiting → 2PC's *blocking* problem (coordinator single point of failure)
</section>
