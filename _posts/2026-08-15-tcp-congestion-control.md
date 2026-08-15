---
layout: fable
title: "黑水岭的骡队 · The Mule Trains of Blackwater Pass"
title_zh: "黑水岭的骡队"
title_en: "The Mule Trains of Blackwater Pass"
concept: "TCP congestion control (slow start, AIMD, bufferbloat)"
tags: [networking, distributed-systems]
illustration: /assets/art/2026-08-15-tcp-congestion-control.jpg
---
<section class="zh" markdown="1">
黑水岭上只有一道隘口，窄得两匹骡子并肩走就要擦着石壁。岭这边是产茶的六个村子，岭那边是集市。谁也没量过那道口子一天究竟能过多少骡子——山里雾大，隘口终年藏在云里，没有人站得那么高、看得那么全。

老陶是六家驮队里最年轻的一个。刚接手父亲的骡子时，他做了所有新手都会做的事：三十头骡子一次全赶上山。当晚只回来十一头，其余的堵在隘口前的乱石滩上，货散了一地，人也累垮了。

第二年他改了法子。头一趟只派一头骡子。骡子傍晚回来了，脖子上挂着集市账房给的木牌——那是"过去了、交货了"的凭证。于是第二趟派两头，两块木牌都回来，就派四头，再八头。他管这叫"探深浅"：不知道水有多深，就一脚一脚往下试，试得飞快。

试到某一趟，有骡子没回来。老陶不骂骡子，也不加派人去找——他只做一件事：下一趟的数目砍掉一半。然后从这个数目起，每趟只多加一头。一头。哪怕连着十趟都平安，也只加一头。有人笑他胆小，他说：上山快、下山慢，这是隘口教我的规矩。

奇的是，另外五家驮队后来也各自摸出了同样的规矩——加一头、砍一半，谁也没跟谁商量过。年头一长，六家在隘口的份额竟慢慢磨得差不多齐了：谁贪心多派，谁那趟就更容易折骡子，砍得也就更狠；谁保守，空出来的位置很快被别人一头一头填满。没有人主持公道，公道自己长了出来。

再后来出了桩怪事。有位乡绅嫌乱石滩太乱，出钱在隘口前修了一座极大的货场，能存下上千件货。从那天起，骡子几乎不再丢了——都能进货场排队。可老陶发现，木牌回来得越来越晚：从前当天傍晚回，后来隔一天，再后来隔三天。骡子没丢，茶却在货场里捂坏了。别家还照旧"没折骡子就加一头"，越加越堵。老陶换了个看法：他不再只盯着"丢没丢"，而是开始记"木牌要几天才回来"。一旦这个天数比最快的时候明显变长，他就先减派——哪怕一头骡子都没丢。

还有一年，守隘口的老兵学会了在人多时挂一面红旗。看见红旗的驮队主动减半，不必等到真折了骡子。那一年，六家的茶都比往年新鲜。

——到这儿你大概已经认出来了：那道看不见的隘口，是路径上的瓶颈带宽；老陶每趟派出的骡子数，是 _cwnd_（congestion window）；回来的木牌是 _ACK_；"一头变两头、两头变四头"是 _slow start_；"平安加一头、出事砍一半"是 _AIMD_；乡绅修的大货场是 _bufferbloat_；老陶改记"木牌几天回来"，是 BBR、Vegas 那一路基于时延的思路；守兵的红旗则是 _ECN_。这就是 TCP congestion control。

## 这是什么、为什么重要

congestion control 解决的问题是：发送方看不见路径中间最窄的那一段有多宽，也不知道此刻有多少人在跟自己抢。没有人会告诉它答案，它只能**用发出去的包去探**——靠 ACK 回没回、回得多快，反推路况。这跟 flow control 不是一回事：flow control 是接收方明说"我缓冲区满了"，congestion control 面对的是一个沉默的、共享的、谁也没测量过的中间瓶颈。

经典机制分两段。连接刚建立时进入 _slow start_，每收到一个 ACK 就让 cwnd 涨一个，实际效果是每个 RTT 翻倍，几个来回就能逼近可用带宽。一旦丢包，转入 _congestion avoidance_：此后每个 RTT 只加一个 MSS，再丢包就立刻减半。这种"加法增、乘法减"的不对称并非随手定的——可以证明，正是 AIMD 这一族的规则能让多条互不通信的连接同时收敛到高利用率与大致公平的份额。

_bufferbloat_ 是这套机制在今天的痛处：设备缓冲区做得太大，拥塞时包不丢，只是排队，基于丢包的算法完全感知不到，于是继续加码，把延迟推到几百毫秒甚至几秒——链路"不丢包，但慢得要死"。BBR、Vegas 改用 RTT 与实测投递速率做信号；ECN 让路由器在丢包之前就显式打标记。理解这一层，你才能解释为什么跨 region 的长肥管道吞吐上不去（cwnd 受 RTT 与丢包共同封顶）、为什么调大 buffer 反而更糟、为什么同一条链路上一个大文件传输会把交互式流量的延迟拖垮。

## 隐喻对应表

- 黑水岭的隘口 —— 路径上最窄的瓶颈链路，容量未知且被多方共享
- 老陶每趟派出的骡子数 —— cwnd（拥塞窗口）
- 骡子带回的木牌 —— ACK
- 一头、两头、四头、八头 —— slow start，指数式探测
- 有骡子没回来 —— 丢包，被当作拥塞信号
- 平安一趟只加一头 —— congestion avoidance，每 RTT 加性增长
- 出事就砍一半 —— multiplicative decrease
- 六家驮队自发磨平的份额 —— AIMD 的公平性收敛，无需协调
- 乡绅修的大货场 —— 过大的设备缓冲区，bufferbloat
- 骡子不丢、茶却捂坏 —— 无丢包但排队时延暴涨
- 老陶改记"木牌几天回来" —— RTT 信号，Vegas / BBR 一类基于时延的算法
- 守兵的红旗 —— ECN，路由器在丢包前显式标记拥塞
- 从没有人量过隘口的宽度 —— 端到端原则：网络不告知容量，只能自行推断
</section>
<section class="en" markdown="1">
There is only one pass through Blackwater Ridge, so narrow that two mules walking abreast scrape the rock on either side. On this side lie six tea-growing villages; on the far side, the market. Nobody has ever measured how many mules the gap can swallow in a day — the mist sits low, the pass hides in cloud all year, and no one stands high enough to see the whole of it.

Old Tao was the youngest of the six caravan masters. When he took over his father's mules, he did what every beginner does: he drove all thirty of them up at once. Eleven came home that night. The rest were jammed on the scree below the gap, cargo scattered, men spent.

The next year he changed his method. On the first trip he sent one mule. It came back at dusk with a wooden tally on its neck — the market clerk's proof that it had crossed and delivered. So he sent two. Both tallies came home, so he sent four, then eight. He called it sounding the water: you don't know how deep it is, so you feel your way down, one step at a time — but quickly.

On some trip, a mule failed to return. Old Tao neither cursed the mules nor sent men out to search. He did exactly one thing: he halved the size of the next trip. And from that number onward he added only one mule per trip. One. Even after ten safe trips in a row, still only one. People called him timid. He said: quick going up, slow coming down — the pass taught me that rule.

The strange part is that the other five caravans each arrived at the same rule on their own — add one, halve on loss — without ever discussing it. And over the years their shares of the pass ground themselves roughly even. Whoever got greedy lost mules sooner and was cut back harder; whoever stayed timid found his empty space filled, one mule at a time, by someone else. No magistrate enforced fairness. Fairness simply grew.

Then something odd happened. A local gentleman, offended by the mess on the scree, paid to build an enormous holding yard below the gap, big enough for a thousand loads. From that day, mules almost never went missing — there was always room to queue. But Old Tao noticed the tallies coming home later and later: first the same evening, then a day later, then three. No mules lost, yet the tea spoiled in the yard. The other caravans, still adding one mule for every trip without a loss, pushed harder and harder into the jam. Old Tao changed what he watched. Instead of only asking whether mules came back, he began recording how many days the tallies took. When that number crept well above its best, he cut back — even with not a single mule lost.

And one year the old soldier who kept watch above the gap began raising a red flag when the crowd grew thick. Caravans that saw the flag halved on their own, without waiting to lose anything. That year everyone's tea arrived fresher than usual.

— By now you've probably recognized it: the unmeasured gap is the bottleneck bandwidth on a path; the number of mules Old Tao sends per trip is the _cwnd_ (congestion window); the returning tallies are _ACKs_; one-two-four-eight is _slow start_; add-one-on-success, halve-on-loss is _AIMD_; the gentleman's enormous yard is _bufferbloat_; Old Tao's switch to timing the tallies is the delay-based family (Vegas, _BBR_); and the watchman's red flag is _ECN_. This is TCP congestion control.

## What it is, and why it matters

Congestion control answers a hard question: the sender cannot see how wide the narrowest link on the path is, nor how many others are competing for it right now. Nobody tells it. It can only **probe with the packets it sends**, inferring conditions from whether ACKs return and how fast. This is not flow control — flow control is the receiver explicitly saying "my buffer is full." Congestion control faces a silent, shared, unmeasured bottleneck in the middle.

The classic mechanism has two phases. A new connection enters _slow start_, growing cwnd by one segment per ACK — effectively doubling every RTT — so it approaches the available bandwidth within a few round trips. On loss it shifts to _congestion avoidance_: from then on, one MSS per RTT, and an immediate halving on the next loss. That asymmetry between additive increase and multiplicative decrease is not arbitrary. It can be shown that AIMD-style rules are what let many independent, non-communicating flows converge simultaneously toward high utilization and a roughly fair split.

_Bufferbloat_ is where this breaks in modern networks. Device buffers grew so large that congestion no longer drops packets — it just queues them. A loss-based algorithm senses nothing, keeps pushing, and latency climbs to hundreds of milliseconds or seconds: a link that "loses nothing but is agonizingly slow." BBR and Vegas instead use RTT and measured delivery rate as signals; ECN lets routers mark congestion explicitly before dropping anything. Once you see this layer, you can explain why a long fat cross-region pipe won't fill (cwnd is capped by RTT and loss together), why enlarging a buffer can make things worse, and why one bulk transfer can wreck the latency of interactive traffic sharing the same link.

## Metaphor mapping

- The pass at Blackwater Ridge — the narrowest bottleneck link on the path, capacity unknown and shared
- Mules sent per trip — cwnd, the congestion window
- Wooden tallies brought home — ACKs
- One, two, four, eight — slow start, exponential probing
- A mule that doesn't return — packet loss, read as a congestion signal
- Adding just one mule after a safe trip — congestion avoidance, additive increase per RTT
- Halving after a loss — multiplicative decrease
- Six caravans' shares grinding themselves even — AIMD's convergence to fairness without coordination
- The gentleman's enormous holding yard — oversized device buffers, bufferbloat
- No mules lost, but the tea spoils — no loss, yet queueing delay explodes
- Timing how long tallies take to return — RTT as signal; Vegas / BBR-style delay-based control
- The watchman's red flag — ECN, explicit congestion marking before drops
- Nobody ever measured the gap — the end-to-end principle: the network won't tell you its capacity, you must infer it
</section>
