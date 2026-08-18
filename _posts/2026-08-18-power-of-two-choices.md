---
layout: fable
title: "夜市上多看的那一眼 · One More Glance on the Night Market Street"
title_zh: "夜市上多看的那一眼"
title_en: "One More Glance on the Night Market Street"
concept: "The Power of Two Random Choices"
tags: [distributed-systems, networking]
illustration: /assets/art/2026-08-18-power-of-two-choices.jpg
---
<section class="zh" markdown="1">
入夜，长街两侧亮起三十盏灯——三十家面摊，三十口锅，三十条队。

街上的老规矩很省事：进了街，随手挑一家坐下。反正面都差不多，谁也不比谁香多少。

于是每晚都会出现同一幅景象。街东头有三口锅闲着，老板抱着胳膊看人来人往；街西头某一摊前排了十一个人，末尾那位站到锅里的汤都换了两回。没有人做错什么——每个人都只是"随手挑了一家"。可三十家摊子被随手挑上几百次，总有一家倒霉地被挑中特别多次。**平均每摊只该排两三个人，但最长的那条队，从来不是平均的那条。**而客人抱怨的、掉头就走的，永远是最长的那条。

管市的老陈想过一个办法：在街口摆一块牌子，派个伙计每隔一炷香跑一趟全街，把各摊现在排几个人抄上去。进街的人看一眼牌子，直奔人最少的那家。

试了一晚，更糟。牌子上写着"第七摊空着"，接下来一炷香里进街的每一个人都奔第七摊——等他们走到，那儿排了二十个，而牌子上还端端正正写着"空"。**牌子从写下的那一刻起就在过时**，而过时的消息把所有人一起送进同一个坑里。老陈还想过让伙计跑得勤些，可街太长，伙计的腿只有两条。

后来是煮面的阿婆随口说了句闲话：

"你从街口走进来，不用问谁，也不用看牌子。随便瞄两家，坐人少的那家，就完了。"

老陈觉得这太潦草了。三十家里只看两家，剩下二十八家一概不知，这跟闭着眼睛挑有什么两样？

他还是让人试了一晚。

结果吓了他一跳。最长的那条队，从原先的十一二个，掉到了三四个。不是好一点点，是塌下去了一大截。

道理其实不难。原先随手一挑，一家摊子倒霉纯粹是运气叠着运气；现在每来一位客人，两家摊子要当场比一次，**队越长的那家，赢下这一比的机会就越小**。长队于是自己把自己挡在门外。人越多，这个差别越大——街上摊子从三十家扩到三百家，"随手挑"的最长队会跟着变长，而"瞄两家"的最长队几乎原地不动。

老陈又贪心起来，问：那瞄三家呢？瞄五家呢？

试过。三家确实比两家好，但只好了一丁点，远不及从"一家"跳到"两家"的那一大截。而每多瞄一眼，就得多走几步、多耽误一会儿。到第五家，腿脚的功夫已经把那点便宜吃干净了。

而且阿婆这法子还有个安静的好处：**街口那块牌子可以拆了。**没有伙计跑腿，没有过时的账本，没有一窝蜂。每位客人只问两家，问的还是此刻眼前的实况。

——到这儿你大概已经认出来了：这就是 *the power of two random choices*。

### 这是什么

把 n 个球随机扔进 n 个桶，最满的那个桶大约会有 Θ(log n / log log n) 个球——n 取一万时是十来个，而平均只有 1 个。这是纯随机（*random* 负载均衡）的宿命：均值很好看，尾巴很难看。

Azar、Broder、Karlin、Upfal 在 1994 年证明了一件反直觉的事：每次**随机抽 d 个桶，放进其中较空的那个**，最满的桶会降到 ln ln n / ln d + Θ(1)。注意 d 从 1 变到 2，这是从 log n / log log n 掉到 log log n——**指数级的改善**；而 d 从 2 变到 3，只是把分母的 ln d 从 ln 2 变成 ln 3，常数因子而已。所以这个结论常被总结成一句话：**第二次采样带来几乎全部收益，第三次开始基本白给。**

为什么"多看一眼"能有这么大威力？因为它把独立的坏运气变成了有反馈的竞争：一个 backend 要排到很长的队，必须连续多次在两两对比中战胜对手，而它每长一点，胜出的概率就低一点。

在工程上这就是 *P2C*（power of two choices）负载均衡：Envoy 的 `LEAST_REQUEST` 策略默认 `choice_count: 2`——不是扫描所有 endpoint 找最闲的，而是随机挑两个比一比；NGINX 的 `random two least_conn`、HAProxy 的 `balance random(2)` 都是同一招。

它真正的对手其实不是 *random*，而是"全局最优"的 *join-shortest-queue*：扫描全部 n 个 backend 挑最闲的那个。JSQ 在单个 dispatcher、信息实时的理想情况下确实更优，但现实里有两个杀手——**采样成本是 O(n)**，以及**负载视图必然是陈旧的**。多个 LB 副本共享一份几秒前的 metrics，会同时判定同一个 backend "最闲"，然后一起扑上去，把它打垮，下一轮再一起扑向下一个。Mitzenmacher 的经典结论是：**信息一旦陈旧，JSQ 会比纯随机还差**，而 P2C 因为只用两个即时采样、且不追求最优，反而对陈旧和抖动格外皮实。

这也是它值得记住的地方：一个 O(1) 成本、无中心状态、无协调的局部决策，换来了接近全局最优的尾部表现。你在调 Envoy、写 client-side LB、或者给一个自研 dispatcher 挑策略时，默认答案通常就是"随机抽两个，选负载低的那个"。

_隐喻对应表_

- 三十家面摊 → n 个 backend / endpoint
- 每摊门前的队 → 各 backend 的 queue depth（未完成请求数）
- 进街的食客 → 到达的 request
- "随手挑一家" → 纯 *random* 负载均衡，d = 1
- 最长的那条队 → *max load*，也就是 p99 尾延迟的来源
- 平均两三个人 vs 最长十一个 → 均值很好而尾部很差
- 街口那块牌子 → 中心化的全局负载视图（周期上报的 metrics）
- "牌子从写下就在过时" → *staleness*，负载信息的固有延迟
- 所有人一起奔第七摊 → *herd behavior*，陈旧信息导致的羊群效应
- 伙计跑遍全街 → *join-shortest-queue*，O(n) 的采样成本
- "随便瞄两家，坐人少的" → *power of two choices*，d = 2
- 长队越长越难在两两对比中胜出 → 负反馈，负载自我均衡
- 摊子从三十家扩到三百家、最长队几乎不动 → max load 只随 log log n 增长
- 瞄三家只好一丁点 → d 只出现在 ln d 里，收益迅速递减
- 多瞄一眼要多走几步 → *probe cost*
- 牌子可以拆了 → 无中心状态、无协调，纯局部决策
</section>
<section class="en" markdown="1">
At nightfall, thirty lanterns come on down the long street — thirty noodle stalls, thirty pots, thirty lines.

The old custom on this street was easy enough: walk in, pick a stall, sit down. The noodles are much the same everywhere; no one's broth is meaningfully better than anyone else's.

And so the same scene played out every evening. At the east end, three pots sat idle while their owners stood with folded arms watching the crowd drift by. At the west end, one stall had eleven people waiting, and the last of them stood there long enough for the broth in the pot to be changed twice. Nobody did anything wrong — everyone simply "picked a stall." But when thirty stalls get picked at random a few hundred times, one of them always gets unlucky and gets picked far too often. **On average each stall should have two or three people waiting. But the longest line is never the average line.** And the longest line is the one people complain about, and the one people turn around and walk away from.

Old Chen, who managed the street, tried a fix. He put a board up at the entrance and sent a boy to walk the whole street every so often, counting the lines and chalking the numbers up. Anyone walking in could glance at the board and head straight for the emptiest stall.

One night of that was worse. The board said "stall seven is empty," and every single person who entered the street for the next stretch of time walked to stall seven — and by the time they got there, twenty people were waiting, while the board still read, neatly, *empty*. **The board was out of date from the moment it was written**, and stale news walked everyone into the same hole together. Chen thought about sending the boy around more often, but the street was long and the boy had only two legs.

It was the old woman boiling noodles who eventually said, off-handedly:

"When you come in off the street, don't ask anyone, don't look at the board. Just glance at two stalls, sit at the one with fewer people. That's it."

Chen thought this was hopelessly sloppy. Two stalls out of thirty, and total ignorance about the other twenty-eight — how was that any better than picking blind?

He tried it for a night anyway.

The result startled him. The longest line dropped from eleven or twelve people to three or four. Not a little better. It collapsed.

The reason isn't hard. Under the old custom, a stall got crowded by bad luck stacking on bad luck. Now, every arriving customer forced two stalls into a head-to-head comparison, and **the longer a stall's line, the smaller its chance of winning that comparison**. Long lines kept themselves out. And the bigger the street, the wider the gap: grow from thirty stalls to three hundred, and the worst line under "pick one at random" grows with it, while the worst line under "glance at two" barely moves.

Chen, greedy now, asked: what about three stalls? Five?

They tried. Three was better than two — but only slightly, nothing like the leap from one to two. And every extra glance meant a few more steps and a little more delay. By the fifth stall, the walking had eaten the entire advantage.

The old woman's rule had one more quiet virtue: **the board at the entrance could come down.** No boy running errands, no stale ledger, no stampede. Each customer asked two stalls, and asked about right now.

— By this point you've probably recognized it: this is *the power of two random choices*.

### What this is

Throw n balls into n bins uniformly at random, and the fullest bin ends up with roughly Θ(log n / log log n) balls — about a dozen when n is ten thousand, against an average of one. That is the fate of pure *random* load balancing: a beautiful mean and an ugly tail.

In 1994 Azar, Broder, Karlin and Upfal proved something counterintuitive: if each ball instead **samples d bins at random and goes into the less loaded one**, the fullest bin drops to ln ln n / ln d + Θ(1). Look closely at what going from d = 1 to d = 2 does — log n / log log n collapses to log log n, an **exponential** improvement. Going from d = 2 to d = 3 only changes ln d from ln 2 to ln 3, a constant factor. Hence the usual one-line summary: **the second sample buys almost everything; the third buys almost nothing.**

Why does "one more glance" carry so much weight? Because it converts independent bad luck into competition with feedback. For a backend to accumulate a long queue, it has to keep winning pairwise comparisons — and every extra request it holds makes winning the next one less likely.

In practice this is *P2C* (power of two choices) load balancing. Envoy's `LEAST_REQUEST` policy defaults to `choice_count: 2` — it does not scan every endpoint looking for the idlest one, it picks two at random and compares. NGINX's `random two least_conn` and HAProxy's `balance random(2)` are the same trick.

Its real rival isn't *random* but the "globally optimal" *join-shortest-queue*: scan all n backends and take the idlest. With a single dispatcher and perfectly fresh information, JSQ genuinely wins. Reality supplies two killers — **O(n) sampling cost**, and **a load view that is inevitably stale**. Several load balancer replicas sharing a metrics snapshot from a few seconds ago will all decide the same backend is idlest, all pile onto it, flatten it, and then pile onto the next one together. Mitzenmacher's classic result: **once information is stale, JSQ performs worse than pure random**, while P2C — using only two fresh samples and never chasing the optimum — is remarkably robust to staleness and jitter.

That is what makes it worth remembering: an O(1), stateless, uncoordinated local decision that buys you near-optimal tail behaviour. When you're tuning Envoy, writing a client-side balancer, or picking a policy for a homegrown dispatcher, the default answer is usually "sample two at random, take the less loaded one."

_Metaphor mapping_

- The thirty noodle stalls → n backends / endpoints
- The line in front of each stall → each backend's queue depth (outstanding requests)
- Customers walking onto the street → arriving requests
- "Just pick a stall" → pure *random* load balancing, d = 1
- The longest line → *max load*, the source of p99 tail latency
- Two or three on average vs eleven at the worst stall → fine mean, terrible tail
- The board at the entrance → a centralized global load view (periodically reported metrics)
- "Out of date from the moment it was written" → *staleness*, the inherent lag in load information
- Everyone rushing stall seven at once → *herd behavior* caused by stale information
- The boy walking the whole street → *join-shortest-queue*, O(n) sampling cost
- "Glance at two, sit at the shorter one" → *power of two choices*, d = 2
- Longer lines being less likely to win a comparison → negative feedback, self-balancing load
- Thirty stalls to three hundred, worst line barely moving → max load grows only as log log n
- Three stalls being only slightly better → d appears only inside ln d, sharply diminishing returns
- Extra glances costing extra steps → *probe cost*
- The board coming down → no central state, no coordination, purely local decisions
</section>
