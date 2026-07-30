---
layout: fable
title: "河上的红浮标 · The Red Buoys on the River"
title_zh: "河上的红浮标"
title_en: "The Red Buoys on the River"
concept: "Chandy–Lamport distributed snapshot algorithm"
tags: [distributed-systems]
illustration: /assets/art/2026-07-30-chandy-lamport-snapshot.jpg
---
<section class="zh" markdown="1">
沿着一条大河，散落着一串水力磨坊。每座磨坊靠运河与另外几座相连，运河都是单向的——谷子只能顺着水往一个方向漂。船夫把一麻袋一麻袋的谷子从上游磨坊运往下游磨坊，一路要漂上好几个时辰。

有一天，磨坊主们起了个念头：**此刻整条河上到底有多少袋谷子？** 不光是各家仓里堆着的，还包括正在运河上漂着、尚未靠岸的那些。麻烦在于，谁都不肯停工。水不停，船不停；你要是先数完自家、再跑去数邻居，回头一看，自家的数早就变了。而那些漂在半路上的袋子，最容易被漏掉，或者被两头都算一遍。

老磨坊主想出了一个办法。他先把自家仓里的谷袋数清、记在本子上——就记这一瞬间的数，落笔为准。然后，他往每一条从自己这儿流出去的运河里，各放下一只鲜红的浮标，让它顺水漂向下游。

规矩就这样一站一站传了下去：任何一座磨坊，一旦看见河面上漂来了它这辈子第一只红浮标，就立刻放下手里的活，先把自家仓里的谷袋数清记下。从这一刻起，它还要多做一件事——对每一条流进自己院子的运河，把陆续靠岸的谷袋一袋一袋记进账，直到那条运河上也漂来一只红浮标为止。红浮标一到，这条运河就"封账"：账上记下的，正是浮标出发前还漂在这段水面上的存货。数完，它也向自己所有下游运河各放一只红浮标，把规矩接力下去。

于是，没有一座磨坊停过工，没有一滴水被拦住。可等所有红浮标都漂到了尽头，把各家仓里记的数、加上各条运河上记的"在途"数一并汇总，得到的竟是一张严丝合缝的账：不多算一袋，也不漏算一袋。那一瞬间整条河的模样，被完完整整地"拍"了下来——尽管这张照片，是大家一边干着活一边拼出来的。

——到这儿你大概已经认出来了：这就是分布式系统里的 **Chandy–Lamport 全局快照算法**。红浮标，就是顺着 channel 传递的 _marker_；磨坊第一次见到 marker 时记下的仓储，是它的 _process state_；marker 到达前记进账的那些在途谷袋，是 _in-flight channel state_。整条河从不停机，却拼出了一张一致的全局 _snapshot_。

**这是什么、为什么重要。** Chandy–Lamport 快照算法解决的问题是：如何在一个不停机、消息还在网络里飞的分布式系统中，拍下一张_一致_的全局状态——既包括每个节点自身的状态，也包括还在 channel 里传输、尚未被接收的消息。它假设 channel 是 FIFO（先进先出）且可靠的。任何节点都可以发起：先记录自身状态，再向所有 outgoing channel 发出 marker；节点在某条 channel 上收到第一个 marker 时，记录自身状态并开始录制其余 incoming channel；某条 channel 上的 marker 一到，就停止对它的录制。为什么重要：分布式系统的死锁检测、checkpoint 与故障恢复、分布式 GC、调试，都需要一张"不撒谎"的全局截面，而你又不可能真去按下暂停键。

隐喻对应表：

- 一座磨坊 → 分布式系统里的一个 _process_（节点）
- 单向运河 → 节点之间的 _channel_（假设 FIFO、可靠）
- 仓里堆着的谷袋 → 节点的本地状态 _process state_
- 漂在运河上、尚未靠岸的谷袋 → _in-flight messages_，即 channel 里还没被接收的消息
- 老磨坊主先数自己、再放浮标 → 发起者先记录自身状态，再向所有 outgoing channel 发出 _marker_
- 第一次见到红浮标就立刻数自己 → 收到首个 marker 时记录 _process state_
- 见到浮标后开始记录在途谷袋 → 开始录制其余每条 incoming channel 上到达的消息
- 某条运河的浮标一到就"封账" → 该 channel 的 marker 到达，停止录制，得到这条 _channel state_
- 汇总所有账目 → 收集各节点的局部快照，拼成一张一致的全局 _snapshot_
</section>
<section class="en" markdown="1">
Along a great river sat a string of watermills. Each mill was linked to a few others by canals, and every canal ran one way only — grain could drift in a single direction with the current. Boatmen ferried sack after sack of grain from the upstream mills to the downstream ones, and the journey took hours.

One day the millers got a thought: **right now, how many sacks of grain are there on the whole river?** Not only the ones piled in everyone's storeroom, but also the ones still adrift on the canals, not yet landed. The trouble was, no one would stop working. The water never stopped, the boats never stopped; if you counted your own store first and then ran off to count your neighbor's, by the time you looked back your own number had already changed. And the sacks in mid-journey were the easiest to miss — or to count twice, once at each end.

The old miller thought of a trick. First he counted the sacks in his own store and wrote the number in his ledger — just the number at that single instant, and once written it was final. Then, into every canal that flowed out from his mill, he dropped a bright red buoy and let it drift downstream.

The rule was passed on from mill to mill: the moment any mill spotted the first red buoy of its life come floating down, it stopped what it was doing and counted the sacks in its own store right away. From that instant on, it did one more thing — for every canal flowing _into_ its yard, it logged each arriving sack, one by one, until a red buoy came floating down that canal too. When the buoy arrived, that canal's ledger "closed": what had been logged was exactly the grain still adrift on that stretch of water at the moment the buoy set out. Then the mill dropped a red buoy into each of its own downstream canals, passing the rule along.

And so not a single mill ever stopped, not a drop of water was ever dammed. Yet once all the red buoys had drifted to the very end, the millers summed the counts from every storeroom together with the "in-transit" counts from every canal — and out came a ledger that fit together perfectly: not one sack over-counted, not one missed. The shape of the whole river at that instant had been _photographed_ whole — even though the picture was assembled by everyone while they kept right on working.

— By now you have probably recognized it: this is the **Chandy–Lamport distributed snapshot algorithm**. The red buoy is the _marker_ passed along each channel; the store a mill counts when it first sees a marker is its _process state_; the in-transit sacks it logs before the marker arrives are the _in-flight channel state_. The river never halts, yet a consistent global _snapshot_ is stitched together.

**What it is, and why it matters.** The Chandy–Lamport algorithm answers this: how do you photograph a _consistent_ global state of a distributed system that never stops and whose messages are still in flight — capturing both each node's own state and the messages still traveling through channels, not yet received? It assumes channels are FIFO and reliable. Any node can initiate: it records its own state, then sends a marker on every outgoing channel. When a node receives the first marker on some channel, it records its own state and begins recording every other incoming channel; when a marker later arrives on one of those channels, it stops recording that channel. Why it matters: distributed deadlock detection, checkpoint-and-recovery, distributed garbage collection, and debugging all need an honest global cross-section — and you can't actually press pause on a running system.

Metaphor mapping:

- A mill → a _process_ (node) in the distributed system
- A one-way canal → a _channel_ between nodes (assumed FIFO and reliable)
- Sacks piled in the store → the node's local _process state_
- Sacks adrift on a canal, not yet landed → _in-flight messages_, not yet received on the channel
- The old miller counting himself, then dropping buoys → the initiator recording its own state, then sending a _marker_ on every outgoing channel
- Counting yourself the instant you see the first red buoy → recording _process state_ on receiving the first marker
- Logging in-transit sacks after seeing the buoy → beginning to record messages arriving on the other incoming channels
- A canal's ledger "closing" when its buoy arrives → the marker arriving on that channel, ending recording and yielding that _channel state_
- Summing all the ledgers → collecting each node's local snapshot into one consistent global _snapshot_
</section>
