---
layout: fable
title: "各记各的账 · Everyone Keeps Their Own Ledger"
title_zh: "各记各的账"
title_en: "Everyone Keeps Their Own Ledger"
concept: "Vector clocks and causal ordering"
tags: [distributed-systems, networking]
illustration: /assets/art/2026-09-01-vector-clock-causality.jpg
---
<section class="zh" markdown="1">
青瓦镇有三家药铺——东街的仁和堂、南巷的济世坊、西坡的同春号。三家各有各的药柜，各有各的客人，但有些方子需要三家合开：仁和堂出白术，济世坊出茯苓，同春号出甘草，凑齐了才能抓一副完整的药。

麻烦出在记账上。

三家掌柜原先共用一本大账，放在镇口茶馆里。谁做了一笔买卖，走去茶馆写一行。可三家一天加起来做上百笔生意，茶馆的账本不够翻，掌柜的腿也不够跑。更要命的是，有时两个伙计同时到茶馆，争一支笔，写串了行，一笔陈皮的进货记到了茯苓下面，查账时谁也说不清先后。

镇长说："不如让衙门统一排号。每笔生意来衙门领一个号，按号排序就知道先后。"

试了三天。衙门只有一个号簿，三家排队领号，队排到街尾。领号比做生意还慢——一笔三文钱的甘草，要花半个时辰排队。

后来老掌柜陆先生想了个法子。他说：

"不用去茶馆，不用去衙门。**各记各的账。**"

规矩是这样的。每家药铺挂三块小木牌，分别写着"仁和"、"济世"、"同春"。每做一笔自己的生意，翻自己那块牌上的数字加一。三块牌加在一起就是一组数——比如仁和堂此刻牌上写着"仁和 7，济世 3，同春 5"，意思是：仁和堂自己做了 7 笔，它知道的济世坊做到了第 3 笔，知道的同春号做到了第 5 笔。

关键在"传话"。

三家有时会互相送药材。仁和堂送白术给济世坊时，把自己三块牌的数字抄在送货单上。济世坊收到后，逐块比对：对方写的仁和 7、自己记的仁和才 5——说明仁和堂又做了两笔自己不知道的，更新为 7。对方写的同春 5、自己记的同春也是 5——没有新消息，不动。然后济世坊自己的牌加一，变成"仁和 7，济世 4，同春 5"。

最精妙的是判断先后。

年底查账，镇长问："仁和堂那笔八角的进货，和同春号那笔甘草的出货，到底谁先？"

陆先生说："看两笔账的牌号。仁和堂那笔记的是（8, 3, 5），同春号那笔记的是（7, 3, 6）。仁和的 8 大于 7，但同春的 5 小于 6——**不是每一个数都大，也不是每一个数都小**。这说明两笔账之间没有因果关系，它们是同时发生的，谁也不在谁之前。"

镇长又问："那济世坊收了仁和堂白术之后抓的那副药呢？"

陆先生说："仁和堂送白术时记的是（7, 3, 5），济世坊抓药时记的是（7, 4, 5）。济世坊每一个数都大于或等于仁和堂的——**这说明抓药一定发生在送白术之后**。因果关系看得一清二楚。"

镇长恍然大悟："所以你这三块牌不是在记时间，是在记**谁知道了什么**。"

陆先生点头："时间会骗人——三家的沙漏快慢不同。但谁知道了什么，骗不了。"

——到这儿你大概已经认出来了：三块牌就是 *vector clock*，每块牌上的数字是对应节点的逻辑计数器，送货单上抄的数字是 *piggyback* 在消息上的向量，逐块取大是 *merge*，自己加一是 *tick*，"每一个数都大于等于"是 *happens-before*（因果关系），"既不全大也不全小"是 *concurrent*（并发）。这是 **vector clocks and causal ordering** 的故事。

### 这是什么

*Vector clock* 是分布式系统中追踪事件因果关系的一种逻辑时钟。每个节点维护一个向量（长度等于节点数），向量中的第 i 个分量记录该节点"知道的"节点 i 已经发生了多少事件。

核心操作只有三个：

1. **Tick**：本地事件发生时，自己的分量 +1。
2. **Send**：发送消息时，把当前向量附在消息上。
3. **Merge**：收到消息时，逐分量取 max(自己的, 消息里的)，然后自己的分量 +1。

因果判断规则：
- 向量 A 的**每一个**分量都 ≤ 向量 B 的对应分量，且至少有一个严格小 → A *happens-before* B（A 是 B 的因）。
- 既不全 ≤ 也不全 ≥ → A 和 B *concurrent*（并发，无因果关系）。

Lamport 时钟只有一个标量计数器，能判断 happens-before 的一个方向，但不能区分"没有因果"和"有因果"——两个不相关的事件可能拿到相同的 Lamport 时间戳。Vector clock 用 N 个分量解决了这个问题：它能精确告诉你两个事件是"有因果"还是"真正并发"。

Amazon DynamoDB 早期用 vector clock 检测写冲突；Riak 数据库用它做 *sibling* 冲突检测。在实践中，当节点数很大时向量会膨胀，因此出现了 *dotted version vector*、*interval tree clock* 等压缩变体。

### 为什么重要

分布式系统没有全局时钟。物理时钟有漂移——两台机器的"现在"可以差几十毫秒，这在微服务链路里足以颠倒因果。NTP 能纠偏但不保证单调，Spanner 用原子钟把误差压到微秒级但代价极高。

Vector clock 绕开了物理时间，直接追踪"**谁知道了什么**"——这才是因果的本质。它回答的不是"几点钟发生的"，而是"这件事有没有可能影响到那件事"。

这个区分在冲突解决中至关重要。两个写操作如果是 happens-before，后者覆盖前者就好（*last-writer-wins* 是安全的）。如果是 concurrent，系统必须保留两个版本让应用层决定——强行挑一个就是丢数据。Vector clock 是唯一能告诉你"该覆盖还是该保留"的机制。

_隐喻对应表_

- 三家药铺 —— 分布式系统中的三个节点
- 各挂三块小木牌 —— vector clock（每个节点维护的向量）
- 翻自己那块牌 +1 —— tick（本地事件计数 +1）
- 送货单上抄三块牌的数字 —— piggyback（消息附带向量时间戳）
- 收货时逐块取大 —— merge（取分量最大值）
- "每一个数都大于等于" —— happens-before（因果关系成立）
- "既不全大也不全小" —— concurrent（并发，无因果）
- 共用大账本 —— 单点集中式排序（不可扩展）
- 衙门统一排号 —— Lamport 时钟（标量，不能区分并发）
- "时间会骗人，但谁知道了什么骗不了" —— 逻辑时钟 vs 物理时钟
</section>
<section class="en" markdown="1">
Qingwa town had three apothecary shops — Renhe Hall on East Street, Jishi Workshop in South Lane, and Tongchun House on West Slope. Each had its own herb cabinet and its own customers, but some prescriptions required all three: Renhe supplied atractylodes, Jishi supplied poria, Tongchun supplied licorice root. Only with all three could you fill a complete prescription.

The trouble was in the bookkeeping.

The three shopkeepers originally shared one large ledger, kept at the teahouse in the town square. Whoever completed a transaction walked over and wrote a line. But the three shops together did over a hundred transactions a day — the teahouse ledger couldn't keep up, and neither could the shopkeepers' legs. Worse, sometimes two clerks arrived at the teahouse at the same time, fought over the brush, and wrote on the wrong line — a purchase of tangerine peel ended up under poria, and at audit time nobody could tell what came first.

The town elder suggested: "Let the magistrate's office assign serial numbers. Come to the office for a number before each transaction; sort by number and you know the order."

They tried for three days. The office had one numbering book. All three shops queued for numbers, and the line stretched to the end of the street. Getting a number took longer than the transaction itself — a three-coin sale of licorice root cost half an hour of queuing.

Then old shopkeeper Master Lu thought of a different approach. He said:

"No teahouse, no magistrate. **Everyone keeps their own ledger.**"

The rules were these. Each shop hung three small wooden tablets, labeled "Renhe," "Jishi," and "Tongchun." Every time a shop completed one of its own transactions, it incremented the number on its own tablet by one. The three tablets together formed a tuple — for instance, Renhe Hall's tablets might read "Renhe 7, Jishi 3, Tongchun 5," meaning: Renhe has done 7 of its own transactions, it knows Jishi has done at least 3, and it knows Tongchun has done at least 5.

The key was in the "passing of word."

The three shops sometimes sent herbs to each other. When Renhe sent atractylodes to Jishi, it copied its three tablet numbers onto the delivery slip. When Jishi received the delivery, it compared tablet by tablet: the slip says Renhe 7 but Jishi's own record says Renhe 5 — so Renhe did two more transactions that Jishi didn't know about; update to 7. The slip says Tongchun 5 and Jishi's record also says 5 — no new information, leave it. Then Jishi incremented its own tablet by one, making "Renhe 7, Jishi 4, Tongchun 5."

The most elegant part was determining order.

At year-end audit, the town elder asked: "Renhe's star anise purchase and Tongchun's licorice sale — which came first?"

Master Lu said: "Look at the tablet numbers for each. Renhe's transaction reads (8, 3, 5) and Tongchun's reads (7, 3, 6). Renhe's 8 is greater than 7, but Tongchun's 5 is less than 6 — **not every number is greater, and not every number is smaller.** This means the two transactions have no causal relationship. They happened concurrently; neither came before the other."

The elder asked again: "What about the prescription Jishi filled after receiving Renhe's atractylodes?"

Master Lu said: "Renhe's delivery was recorded as (7, 3, 5). Jishi's prescription was recorded as (7, 4, 5). Every number in Jishi's tuple is greater than or equal to Renhe's — **this means the prescription definitely happened after the delivery.** The causal relationship is crystal clear."

The elder's eyes lit up: "So your three tablets are not recording time — they are recording **who knows what.**"

Master Lu nodded: "Time can lie — each shop's hourglass runs at a different speed. But who knows what — that cannot be faked."

— By now you have probably recognized it: the three tablets are a *vector clock*, each tablet number is a node's logical counter, the numbers copied onto the delivery slip are the vector *piggybacked* on a message, taking the larger value tablet by tablet is *merge*, incrementing your own is *tick*, "every number greater or equal" is *happens-before* (causal ordering), and "neither all greater nor all smaller" is *concurrent*. This is the story of **vector clocks and causal ordering**.

### What it is

A *vector clock* is a logical clock used in distributed systems to track causal relationships between events. Each node maintains a vector (of length equal to the number of nodes), where the i-th component records how many events that node "knows about" from node i.

There are only three core operations:

1. **Tick**: when a local event occurs, increment your own component by 1.
2. **Send**: when sending a message, attach the current vector.
3. **Merge**: when receiving a message, take the component-wise max of your vector and the message's vector, then increment your own component by 1.

Causal ordering rules:
- If **every** component of vector A is ≤ the corresponding component of B, and at least one is strictly less → A *happens-before* B (A causally precedes B).
- If neither A ≤ B nor B ≤ A → A and B are *concurrent* (no causal relationship).

A Lamport clock uses only a single scalar counter. It can establish one direction of happens-before, but cannot distinguish "no causal relationship" from "causal" — two unrelated events may receive the same Lamport timestamp. A vector clock uses N components to solve this: it tells you precisely whether two events are causally related or truly concurrent.

Amazon DynamoDB's early design used vector clocks to detect write conflicts; Riak used them for *sibling* conflict detection. In practice, vectors grow linearly with node count, which led to compressed variants like *dotted version vectors* and *interval tree clocks*.

### Why it matters

Distributed systems have no global clock. Physical clocks drift — two machines' "now" can differ by tens of milliseconds, enough to reverse causality in a microservice call chain. NTP corrects drift but does not guarantee monotonicity; Spanner uses atomic clocks to compress uncertainty to microseconds, but at extreme cost.

Vector clocks sidestep physical time entirely and track "**who knows what**" — which is the true essence of causality. They answer not "what time did this happen" but "could this event possibly have influenced that one."

This distinction is critical for conflict resolution. If two writes are happens-before, the later one can safely overwrite the earlier (*last-writer-wins* is safe). If they are concurrent, the system must preserve both versions and let the application decide — picking one arbitrarily means losing data. Vector clocks are the only mechanism that can tell you "should I overwrite or should I keep both."

### Metaphor mapping

- Three apothecary shops — three nodes in a distributed system
- Three wooden tablets per shop — vector clock (each node's vector)
- Incrementing your own tablet — tick (local event counter +1)
- Copying tablet numbers onto delivery slips — piggybacking the vector on messages
- Taking the larger value tablet by tablet on receipt — merge (component-wise max)
- "Every number greater or equal" — happens-before (causal relationship holds)
- "Neither all greater nor all smaller" — concurrent (no causal relationship)
- Shared ledger at the teahouse — single-point centralized ordering (unscalable)
- Magistrate's serial numbers — Lamport clock (scalar; cannot distinguish concurrency)
- "Time can lie but who-knows-what cannot" — logical clocks vs. physical clocks
</section>
