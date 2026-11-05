---
layout: fable
title: "两仓渡的过桥费 · The Toll at Two-Granary Ford"
title_zh: "两仓渡的过桥费"
title_en: "The Toll at Two-Granary Ford"
concept: "NUMA-Aware Memory Placement (First Touch, Interleave, Auto-Balancing)"
tags: [memory, performance, linux]
illustration: /assets/art/2026-11-04-numa-first-touch-interleave.jpg
youtube_id: "LgIUfxWd4vA"
---
<section class="zh" markdown="1">
两仓渡镇靠一条河活了三百年。河北岸地势高，叫东岸；河南岸地势低，叫西岸。河上只有一座石桥，桥面窄，恰好容两排独轮车错身。桥头一间小屋，收过桥费。

镇上存粮的规矩，简单到没人想过为什么：谁家的粮，存谁家这一岸。

东岸一座仓，西岸一座仓。仓有多大，得说清楚：两座仓合起来存得下全镇三年的口粮，可单看一座，只存得下一年半。这不是巧合，是当年立仓的人拿算盘打过的——一座一年半，两座正好三年。

运一趟的脚程，镇上人人都熟。本岸的粮，从家门口推到仓门口，一个时辰。要过桥，来回一趟两个时辰，还没算排队：桥只有两排车宽，谁也从桥上快不过去。过一个时辰，绕桥两个时辰，这是刻在镇子骨头上的差事。

还有一条更细的祖规，年轻人都没听过。

仓里给每户划的格子，不是零散排的，是连成一整排。一格挨一格，从仓门那头一路铺到仓尾。而且——这是关键——一户的格子只要开了头，这一户整年的粮就全进这一座仓，不能分。所以每户的粮存在哪座仓，只由一件事决定：**这一户的第一袋米，是谁从哪个门推的。**

先推者定仓。镇上的老人管这条叫"头一袋的规矩"。

头一袋是谁推的，其实从来不是一个选择。从前各家推各家，东岸的从东门进，西岸的从西门进，各归各仓，谁也不吃亏。规矩之所以三百年没出过事，是因为规矩从来没被违反过。

那一年，镇上换了个管事的。

管事姓周，力气大，脑子快，最看不惯各家推车排队。他算了一笔账：三百户各推各的，一家一个时辰，三百个时辰，路上还互相挡。不如他一个人推。他一车装得下别人三车的量，一个早上就能把三百户的粮全推完。

他住东岸，从东门进。

天亮的时候，三百户的粮全进了东仓，一袋不差。周管事擦了汗，很满意：一个早上的活，从前要三百个时辰。

秋后无事。真正的活是冬天——三百户每天要取粮做饭。

东岸的一百户，一个时辰一趟，照常。

西岸的两百户，得先过桥。来回两个时辰，桥头还排队。

第一个月，桥头小屋收的过桥费，比镇上一年收的粮税还多。第二个月，桥面有两块石板松了——不是车太重，是车太多。第三个月，西岸有个老太太，取一趟粮排到天黑才回，回来在门口坐着哭。她没有力气去骂周管事，她只是想不明白：她家的粮，三百年来都存西仓，怎么今年就得过河去取。

周管事不服。他先想到的是加桥。

镇上的石匠说，桥可以加宽——把两排改成三排。改完果然好了几天。可镇子的问题是：桥的那个宽度，本来就是全镇一年能过多少趟车的上限。加宽三成，车流多三成，可西岸要过的车是东岸的**两倍**。桥宽再多，也追不上两倍。

他站在桥头看了整整一个下午，看明白了一件事：过桥的车不是"多一点"，是每一趟都多花一倍的时间，而且排队会把所有车挤在同一个地方。桥不是慢，桥是**堵**。慢可以忍，堵会连着一起崩。

第二个月底，他去请教镇上的老仓头。

老仓头听他说完，先问了一句："你推的时候，是一个人在推吗？"

周管事说，是一个人，一个早上。

老仓头叹了口气："头一袋的规矩，不是用来分仓的，是用来分推车的人的。一户的第一袋米，谁推的，这户的格子就落在推车那个人站的那一岸。你一个人推了三百户，三百户的格子，就全落在了你站的那一岸。"

周管事在仓门口坐了半夜。他想通了三件事，后来这三件事成了镇上的新规矩。

**第一条：谁用，谁推第一袋。** 取粮的活是三百户一起干的——一百户在东岸，两百户在西岸，天天同时开工。那么推第一袋的活，也该三百户一起干：每户推自己家的头一袋，从自己这一岸的门进去。周管事再有劲，也只管住了一件事——他站的位置，不是全镇站的位置。他把推车的速度提高了三倍，代价是把一半的取粮人变成了过河的人。省下来的是开工那一天，赔进去的是往后一整年。

**第二条：摊不清主人的粮，就摊在两边。** 镇上还有些格子里存的不是谁家的粮，是公粮：桥头小屋的灯油、义学的笔墨、冬天的柴炭。公粮谁都要取，东岸要取，西岸也要取，说不清归谁。这种格子，就按东西岸交替着划：一格东、一格西、一格东、一格西。西岸的人取公粮，一半的趟数不用过桥；东岸的人取公粮，一半的趟数要过桥。谁都不快，谁都不慢，桥上的车流被铺平了。

这条规矩一开始有人反对：凭什么我家公粮要过桥？老仓头只说了一句：桥不怕多，桥怕挤。铺平了，桥一年能过完；都挤在一侧，桥三个月就塌。

**第三条：绑死的格子，要绑对。** 镇上后来添了几样东西，只给一处用：东岸渡口的存锚格子，只有摆渡的船工用；西岸磨坊的石料格子，只有磨坊的人用。这些格子，管事就明写着"只许落东仓""只许落西仓"——写明了好处很大，因为用得勤的那几个人就在那一岸。但写明了的险处也大：绑错了，那一户天天过桥，而且没人会发现。仓不报错，账也不错，只是慢。慢是不报警的。

第四年冬天，出了一桩新事。

西岸有两户人家，合用一批格子——不是什么大宅，是两口子分家没分干净：同一块地里出的粮，两家都取，一家做醋，一家做酱，天天各取两趟。按第二条规矩，这格子该摊在两边。可偏巧，那年老仓头病了，接手的年轻人信了一样新东西：县上来的记账先生。

记账先生不预先规定格子落哪座仓。他只数：每天谁从哪座仓取粮，取了几趟，取了多少斤，然后拿这个数去搬格子——西岸的人取多了，就把格子从东仓搬到西仓；东岸的人取多了，再搬回去。镇上给他这份差事起了个名，叫"看谁取，就往谁那边搬"。

这法子对大多数户极好。周管事那年夏天统计，过桥的趟数掉了六成，桥头小屋的过桥费跌得屋主都来闹。

可那两口子分了家的两户，把记账先生整疯了。

今天西岸做醋的取了四趟，记账先生把格子搬到西仓。明天东岸做酱的取了四趟，记账先生又搬回东仓。后天两个人轮流来，记账先生两天搬了六趟。搬格子本身要过桥，一趟两个时辰，一车装不了几张——搬一次，赔掉三次取粮省下的工夫。

年底结账，这两户的过桥趟数比往年还多了一倍。年轻人拿着账本去找老仓头，老仓头病刚好，看一眼就笑了："这两户不是放错了地方，是**真的在两头用**。你把它当放错的搬，它就永远是放错的。"

那年冬天，镇上把这条也写进了规矩：**记账先生只搬"确实放错了的"，不搬"确实两头都在用的"。** 分不清这两种，就别搬——搬错的代价比不搬大。

第五年春天，仓门口立了一块木牌，是老仓头让刻的。牌上只有三行字，给以后所有管事的看：

一、头一袋米，谁用谁推。你替别人推，就是把别人的格子推到你脚底下。
二、公粮摊两边，自家的归自家。
三、搬之前先数，数清楚是"放错了"还是"真共用"。

木牌下面还压着一本册子，是记账先生走后留下的。册子上一页一页，全是数：东仓取了多少趟，西仓取了多少趟，过桥取了多少趟，本该归这座仓却被另一座仓抱走的又有多少趟。

老仓头说，这个册子比木牌管用。木牌讲道理，册子给证据。

如今两仓渡的桥还在，还是两排车宽。桥头的过桥费也还收，只是再没涨过。镇上有句话：桥不是修宽的，是修明了的。

到这儿，你大概已经认出来了：这就是 *NUMA*（Non-Uniform Memory Access，非一致内存访问），以及 *first touch*、*interleave*、*auto-balancing* 这一整套内存放置的学问。

### 这是什么

先看硬件。多路服务器里，每颗 CPU（*socket*）带着自己那一半内存控制器和那一半 *DRAM*，这一半叫一个 *NUMA node*。CPU 读自己 node 上的内存，走本地内存控制器；读另一个 node 的内存，得穿过芯片之间的互联（Intel 的 *UPI*、AMD 的 *Infinity Fabric*）。所以"内存访问时间一致"这件事，从多路机器诞生那天起就不成立了。

数字量级：本地访问约 80–100 纳秒；跨 node 访问是它的 1.3–2 倍，而且跨 node 链路的**带宽**有限——两路机器的聚合带宽通常只有单路的 1.4–1.7 倍，不是 2 倍。

然后看 Linux 怎么分配。`malloc` 只给虚拟地址，不给物理内存。真正的物理页，是在第一次写访问触发缺页（*page fault*）时，由**当时正在跑的那个 CPU** 所在的 node 分配出来的。这就是 *first touch*（首次触碰）策略。

后果非常直接：一个线程 `malloc` 了 8 GB 并初始化，然后起 32 个线程并行处理这段内存——8 GB 全在那一个线程的 node 上，另外 16 个线程有一半的访问全是远程的。初始化那一步省下的时间，被整个运行期赔进去。这就是周管事的账。

五种放置策略：

- **Local（`MPOL_DEFAULT`）**：哪儿缺页哪儿分配。默认行为，对"谁用谁初始化"的程序已经是最优。
- **First touch by the consumer**：让要读写这块内存的线程自己去做首次触碰（并行初始化、每线程自己的 arena）。这是最有效的一条，也是最多人漏掉的一条。
- **Bind（`MPOL_BIND`）**：`numactl --membind=0` 或 `mbind(2)`，把某段内存钉死在一个 node。适合"只有固定几个线程用"的数据。绑错的代价是全程远程，而且**不报错，只是慢**。
- **Interleave（`MPOL_INTERLEAVE`）**：`numactl --interleave=all`，按页轮流铺到各 node。单个访问永远有一半是远程的，但总带宽被摊平、不会把某一个 node 的控制器打满。数据库、大扫描类负载的常见做法（PostgreSQL 没有 NUMA 感知，官方建议就是 interleave）。
- **AutoNUMA**（`/proc/sys/kernel/numa_balancing`）：内核采样缺页与调度信息，把页迁到实际访问它的 node，也把任务迁到它数据所在的 node。省事，但对**真正被多个 node 频繁写共享**的数据会误判成"放错了"，来回迁移，越搬越慢——那两口子分了家的两户。

度量工具：`numastat -p <pid>` 看四个数——`numa_hit`（分配在本 node）、`numa_miss`（本该在这、落到了别处）、`numa_foreign`（本该在别处、落在了这）、`interleave_hit`。`perf` 侧看跨 node 的 load 计数与 miss。第一眼就该看这个，而不是先怀疑代码。

### 为什么重要

因为它是那种"配置正确时性能凭空多两成、配置错误时凭空少四成、而两种情况都不报任何错"的东西。云上尤其容易踩：

- **实例规格本身就是 NUMA 边界。** 大规格虚拟机横跨多个 node，小规格通常只有一个。同一份代码在 4 vCPU 上好好的，搬到 64 vCPU 上就出现"CPU 只用了六成，吞吐却上不去"——那不是 CPU 瓶颈，是内存带宽瓶颈。
- **初始化路径和运行路径经常不是同一批线程。** 主线程建索引、加载快照、预热 *heap*，工作线程池之后才开始干活。这一条几乎必然导致全量远程访问，而它藏在启动阶段，profiler 抓不到。
- **透明大页会放大倾斜。** 缺页时直接分 2 MB 巨页，一个巨页只能落在一个 node 上——倾斜的粒度从 4 KB 放大到 2 MB，而且 `khugepaged` 后台合并出来的巨页会更不本地。
- **虚拟机要如实告知拓扑。** 客户机里如果看不到 *vNUMA* 拓扑，guest 的调度器就以为内存是均匀的，它的所有放置决策都是基于一个错误的世界模型。

三条实践，按收益排序：

1. **让消费者自己第一次触碰。** 并行初始化、每线程独立 arena、线程绑核再分配。这一条通常就吃掉大部分损失。
2. **共享只读/大扫描数据用 interleave。** 摊平带宽，换掉峰值本地性——这是笔划算的买卖。
3. **不要对真共享的热数据开 auto-balancing。** 先 `numastat` 看 `numa_miss` 与 `numa_foreign` 的比例，确认是"放错了"再搬。

最后是那个常常被忽略的取舍：**本地性不等于吞吐。** 把所有东西都钉在一个 node 上，本地命中率能到 100%，但那个 node 的内存带宽上限也就成了系统的上限。两仓渡的桥不是修宽的，是修明了的——你要的不是"都不用过桥"，你要的是"没有一座仓被用爆，也没有一座仓被闲着"。

_隐喻对应表_

- 东岸、西岸 → 两个 *NUMA node*（每颗 socket 各自的本地 DRAM）
- 河与那座石桥 → 芯片间互联（*UPI* / *Infinity Fabric*），跨 node 的通路
- 桥只有两排车宽 → 跨 node 链路的带宽上限
- 本岸一个时辰、过桥两个时辰 → 本地访问 ~80–100ns，远程是它的 1.3–2 倍
- 桥面石板松动、西岸老太太排队到天黑 → 远程访问造成的带宽饱和与排队延迟，不是单次慢，是整体堵
- 仓只有一年半的容量，两座合起来三年 → 每个 node 的本地内存容量与带宽都是有限的一半
- 每户的格子连成一整排 → 一页内存必须是连续的物理页，落在同一个 node
- "先推者定仓" → *first touch*：物理页在第一次写缺页时，分配到触发缺页的那个 CPU 所在 node
- 周管事一个人推完三百户 → 单线程 `malloc` 并初始化，之后多线程使用
- 他住东岸，从东门进 → 初始化线程恰好在一个 node 上
- 三百户的粮全进东仓 → 全量页落在同一个 node，一半线程从此变远程
- 加宽桥只好了几天 → 加带宽治不了需要跨 node 的流量比例；互联带宽不是单路的 2 倍
- 老仓头一句"头一袋是谁推的" → 诊断入口：不是代码逻辑，是初始化路径
- 第一条"谁用谁推第一袋" → 消费者自己做首次触碰（并行初始化、每线程 arena）
- 第二条"公粮摊在两边" → `MPOL_INTERLEAVE`，按页轮转铺到各 node，摊平带宽
- 第三条"绑死的格子要绑对，绑错不报错只是慢" → `MPOL_BIND` / `numactl --membind`，绑错全程远程且无告警
- 记账先生"看谁取，就往谁那边搬" → *AutoNUMA* 的页与任务迁移
- 记账先生对大多数户极有效 → auto-balancing 对"确实放错了"的负载收益明显
- 分了家的两口子天天轮流取 → 真正被多个 node 频繁写共享的数据
- 记账先生两天搬了六趟，反而更慢 → 误判共享为错放，迁移开销吃掉收益，来回抖动
- "只搬确实放错了的，不搬确实两头都在用的" → 先看 `numa_miss` / `numa_foreign` 比例再决定是否迁移
- 册子上那本数：本岸多少趟、过桥多少趟、被抱走多少趟 → `numastat -p` 的 `numa_hit` / `numa_miss` / `numa_foreign` / `interleave_hit`
- 巨页让一格变成一整排 → 透明大页把倾斜粒度从 4 KB 放大到 2 MB
- 客栈包下的两间房跨了楼两侧，旅客却不知道 → 客户机看不到 *vNUMA* 拓扑，guest 调度器基于错误的世界模型做决策
- "桥不是修宽的，是修明了的" → 目标不是零远程访问，是没有 node 被打爆、也没有 node 被闲置
</section>
<section class="en" markdown="1">
Two-Granary Ford lived off one river for three hundred years. The north bank rose, and people called it the East Side; the south bank fell away, and they called it the West. One stone bridge crossed the water, narrow enough that exactly two carts could squeeze past each other. At the bridgehead stood a small hut that collected a toll.

The town's rule for storing grain was so simple nobody had ever asked why: every household's grain goes in the granary on its own bank.

One granary east, one west. The sizes matter, so here they are: together the two held three years of the town's flour, but a single granary held a year and a half. That was not an accident. Whoever built them had worked it out on an abacus — one and a half apiece, three between them.

Everyone knew the cart times by heart. Grain from a household on the same bank: one hour, door to granary door. Across the bridge: two hours round trip, before you counted the queue. The bridge was two carts wide and nothing on it ever moved faster. One hour on your own side, two hours over the water — that schedule was carved into the town's bones.

There was also a finer rule of the founders, one the young had never heard.

The bins inside a granary were not scattered. They ran in one continuous row, bin against bin, from the door to the far wall. And — this is the part that matters — once a household's row was begun, that household's entire year of grain went into that one granary. It could not be split. Which granary held a household's grain was therefore decided by exactly one event: **who pushed that household's first sack of rice through which door.**

First pusher fixes the granary. The old people called it the first-sack rule.

Who pushed the first sack had never really been a choice. Every household pushed its own, east siders through the east door, west siders through the west door, and nobody lost anything. The rule had held for three centuries because the rule had never been broken.

That year, the town changed stewards.

The new steward was named Zhou. Strong, quick, and unable to bear the sight of households queueing with carts. He did the arithmetic: three hundred households pushing their own, an hour each, three hundred hours, and they blocked each other on the road besides. Better that he push it all himself. His cart held three times what anyone else's did, and he could move three hundred households' grain in a single morning.

He lived on the east bank. He went in through the east door.

By dawn, all three hundred households' grain sat in the east granary, not one sack missing. Zhou wiped his forehead, well pleased: one morning's work, where three hundred hours had gone before.

The autumn passed quietly. The real work was winter — three hundred households drawing grain every day to cook.

The hundred east-side households: one hour each, as always.

The two hundred west-side households had to cross the bridge first. Two hours round trip, plus the queue at the bridgehead.

In the first month, the toll hut collected more than the town's grain tax for a year. In the second month, two flagstones on the bridge worked loose — not from weight but from traffic. In the third month, an old woman from the west bank queued for a load of grain and got home after dark, sat down in her doorway, and cried. She had no strength to curse Steward Zhou. She only could not understand it: her household's grain had lived in the west granary for three hundred years, and this year she had to cross a river to fetch it.

Zhou refused to accept it. His first thought was to widen the bridge.

The mason said it could be done — two carts abreast to three. For a few days it helped. But the town's problem was this: the bridge's width *was* the town's annual ceiling on cart crossings. Widen it by a third and traffic grows by a third, while the west bank's traffic is **twice** the east's. No width catches up to twice.

He stood at the bridgehead a whole afternoon and saw one thing clearly: the crossing carts were not "a bit more." Every single trip cost double, and the queue jammed all of them into one place. A slow bridge you can endure. A jammed bridge brings everything down together.

At the end of the second month he went to the old granary keeper.

The old man heard him out, then asked one question: "When you pushed, was it one man pushing?"

One man, Zhou said. One morning.

The old keeper sighed. "The first-sack rule was never about which granary. It was about which *pushers*. Whoever pushes a household's first sack — that household's row lands on the bank where the pusher stands. You pushed for three hundred households alone. Three hundred rows landed on the bank where you stood."

Zhou sat in the granary doorway half the night. He worked out three things, and those three became the town's new rules.

**One: whoever uses it pushes the first sack.** Drawing grain is work three hundred households do together — a hundred east, two hundred west, starting at the same hour every day. So pushing the first sack should be work three hundred households do together too: each household pushes its own first sack through its own bank's door. All Zhou's strength bought him exactly one thing — his own position, not the town's. He tripled the speed of pushing and turned half the grain-drawers into river-crossers. He saved the first morning and paid for it all year.

**Two: grain with no clear owner gets spread across both.** Some bins held no household's grain but the public store — lamp oil for the toll hut, ink and brushes for the school, firewood for winter. Everyone draws from it, east and west alike; there is no owner to name. Those bins get laid out alternately: one east, one west, one east, one west. West-siders drawing public grain skip the bridge half the time; east-siders cross half the time. Nobody is fast, nobody is slow, and the bridge traffic is flattened.

There was objection at first: why should my household's public grain cross the river? The old keeper answered with one line: the bridge does not fear volume, it fears concentration. Flattened, the bridge lasts a year. Piled on one side, it collapses in three months.

**Three: a pinned bin has to be pinned right.** In later years the town added a few things used in only one place: anchor storage at the east ferry, used only by the ferrymen; millstone stock on the west bank, used only by the mill. Those bins the steward marks plainly — "east granary only," "west granary only." Marking them is a great gain, because the few people who use them constantly are standing on that bank. Marking them is also a great risk: pin it wrong and that household crosses the river every day, and nobody ever finds out. The granary raises no error. The ledger balances. It is simply slow. Slowness does not raise alarms.

In the fourth winter, something new happened.

Two west-bank households shared a set of bins — not a great estate, just a couple who had divided a household without dividing it cleanly: grain from the same field, drawn by both, one making vinegar and one making sauce, two trips each every day. By rule two, those bins should be spread across both banks. But the old keeper had fallen ill that year, and the young man who took over trusted something new: the county's bookkeeper.

The bookkeeper prescribed nothing in advance. He only counted: who drew grain from which granary each day, how many trips, how many catties — and then moved bins according to that count. West-siders drawing more, so move bins from east to west. East-siders drawing more, move them back. The town gave his office a name: watch who draws, move toward them.

For most households the method was excellent. That summer Zhou tallied a sixty percent drop in bridge crossings, and the toll hut's revenue fell so far the hut's owner came to complain.

But the couple who had divided their household without dividing it drove the bookkeeper mad.

Today the west-bank vinegar maker drew four times, and the bookkeeper moved the bins west. Tomorrow the east-bank sauce maker drew four times, and he moved them back east. The day after, both came in turn, and he moved them six times in two days. Moving a bin means crossing the bridge, two hours a trip, a cart holding only so many bins — one move cost three times what the saved trips were worth.

At year's end, those two households had crossed the bridge twice as often as in any previous year. The young man brought the ledger to the old keeper, now recovered, who glanced at it and laughed. "Those two are not in the wrong place. They are genuinely used from both ends. Treat them as misplaced and they will be misplaced forever."

That winter the town wrote the fourth rule: **the bookkeeper moves what is truly misplaced, never what is truly shared.** If you cannot tell the two apart, do not move — a wrong move costs more than no move.

In the fifth spring, a wooden board went up over the granary door, carved at the old keeper's order. Three lines, for every steward to come:

One: the first sack, the user pushes. Push for someone else and you push their row under your own feet.
Two: public grain spread across both banks; a household's own grain stays its own.
Three: count before you move. Count whether it is misplaced, or truly shared.

Beneath the board lay a notebook the bookkeeper had left behind. Page after page of numbers: how many trips drawn from the east granary, how many from the west, how many crossed the bridge, and how many trips took grain that belonged to this granary but had been carried off by the other.

The old keeper said the notebook was more use than the board. The board gives principles. The notebook gives evidence.

The bridge at Two-Granary Ford still stands, still two carts wide. The toll hut still collects, and the toll has not risen since. The town has a saying: you do not widen the bridge, you make it legible.

By now you have probably recognized it: this is *NUMA* — Non-Uniform Memory Access — together with *first touch*, *interleave*, and *auto-balancing*, the whole discipline of memory placement.

### What it is

Start with the hardware. On a multi-socket server, each CPU (*socket*) brings its own memory controllers and its own half of the *DRAM*, and that half is a *NUMA node*. Read memory on your own node and you go through a local memory controller. Read the other node's memory and you cross the link between chips — Intel's *UPI*, AMD's *Infinity Fabric*. The idea that memory access takes uniform time has been false since the day multi-socket machines were born.

Order of magnitude: local access is roughly 80–100 ns; cross-node access is 1.3–2× that, and the cross-node link's **bandwidth** is limited — aggregate bandwidth on a two-socket machine is typically 1.4–1.7× a single socket's, not 2×.

Now the software side. `malloc` hands out virtual addresses, not physical memory. The physical page is allocated on the first write that faults it in, and it is allocated **on the node of the CPU that took that fault**. That is *first touch*.

The consequence is immediate: one thread `malloc`s 8 GB and initializes it, then spawns 32 threads to process it — all 8 GB sits on that one thread's node, and half of the other threads' accesses are remote. The time saved during initialization is paid back for the entire run. That is Steward Zhou's ledger.

Five placement policies:

- **Local (`MPOL_DEFAULT`)**: allocate wherever the fault happens. The default, and already optimal for a program where the consumer initializes.
- **First touch by the consumer**: let the threads that will read and write the memory perform the first touch (parallel init, per-thread arenas). The most effective measure, and the most commonly missed.
- **Bind (`MPOL_BIND`)**: `numactl --membind=0` or `mbind(2)`, nailing a region to one node. Right for data only a few fixed threads touch. Bind it wrong and the entire access path is remote — with **no error, just slowness**.
- **Interleave (`MPOL_INTERLEAVE`)**: `numactl --interleave=all`, striping pages round-robin across nodes. Individual accesses are always half remote, but total bandwidth is flattened and no single node's controller saturates. The usual choice for databases and large scans (PostgreSQL has no NUMA awareness at all; the documented advice is interleave).
- **AutoNUMA** (`/proc/sys/kernel/numa_balancing`): the kernel samples faults and scheduling to migrate pages toward the node that accesses them, and tasks toward the node holding their data. Convenient — but for data genuinely written from several nodes it misreads "shared" as "misplaced" and migrates back and forth, slower every round. That is the couple who divided their household.

Measurement: `numastat -p <pid>` gives four numbers — `numa_hit` (allocated on this node), `numa_miss` (should have been here, landed elsewhere), `numa_foreign` (should have been elsewhere, landed here), `interleave_hit`. On the `perf` side, cross-node load counters and misses. That is the first thing to look at, not the code.

### Why it matters

Because this is the category of thing that silently adds twenty percent when configured right and silently removes forty percent when configured wrong, and neither case raises an error. In the cloud it is especially easy to hit:

- **The instance size is itself a NUMA boundary.** Large VMs span nodes; small ones usually do not. The same code behaves on 4 vCPUs and then, on 64, shows "CPU at sixty percent but throughput will not rise" — which is not a CPU bottleneck but a memory-bandwidth one.
- **Initialization paths and run paths are usually different threads.** The main thread builds indices, loads snapshots, warms the *heap*; the worker pool starts afterwards. This practically guarantees fully remote access, and it hides in startup where profilers do not look.
- **Transparent huge pages amplify the skew.** A fault may hand out 2 MB at once, and a huge page can live on only one node — the granularity of skew goes from 4 KB to 2 MB, and huge pages collapsed by `khugepaged` in the background are even less local.
- **Virtual machines must report the topology honestly.** If the guest cannot see its *vNUMA* topology, the guest scheduler assumes memory is uniform — and every placement decision it makes is based on a wrong model of the world.

Three practices, ordered by payoff:

1. **Let the consumer do the first touch.** Parallel initialization, per-thread arenas, bind the thread before you allocate. This alone usually captures most of the loss.
2. **Interleave shared read-mostly and large-scan data.** Trade peak locality for flattened bandwidth — a good trade.
3. **Do not turn auto-balancing loose on genuinely shared hot data.** Check `numastat` for the ratio of `numa_miss` to `numa_foreign` first, and confirm it is misplaced before you move it.

And the trade-off that gets missed: **locality is not throughput.** Pin everything to one node and local hit rate reaches 100 percent — and that node's memory bandwidth ceiling becomes the system's ceiling. You do not widen the bridge at Two-Granary Ford, you make it legible. What you want is not "nobody crosses the river." It is "no granary is saturated and no granary is idle."

_Metaphor mapping_

- East bank / west bank → the two *NUMA nodes* (each socket's local DRAM)
- The river and its stone bridge → the chip-to-chip interconnect (*UPI* / *Infinity Fabric*), the cross-node path
- The bridge being two carts wide → the cross-node link's bandwidth ceiling
- One hour on your own bank, two hours across → local access ~80–100 ns, remote 1.3–2×
- Loose flagstones, the old woman queueing until dark → bandwidth saturation and queueing delay, not a single slow access but a systemic jam
- A granary holding a year and a half, both holding three → each node's local capacity and bandwidth are a finite half
- Each household's bins laid in one continuous row → a page is contiguous physical memory living on one node
- "First pusher fixes the granary" → *first touch*: the physical page is allocated on the node of the CPU that takes the write fault
- Steward Zhou pushing for three hundred households → one thread `malloc`ing and initializing, then many threads consuming
- He lives east, enters by the east door → the initializing thread happens to run on one node
- All three hundred rows land in the east granary → every page lands on one node; half the threads now run remote
- Widening the bridge helps for days → more bandwidth does not fix the *fraction* of traffic that must cross; interconnect bandwidth is not 2× single-socket
- The keeper's question, "who pushed the first sack?" → the diagnostic entry point is not the code path but the initialization path
- Rule one: the user pushes the first sack → consumer-side first touch (parallel init, per-thread arenas)
- Rule two: public grain spread across both banks → `MPOL_INTERLEAVE`, pages striped across nodes to flatten bandwidth
- Rule three: pin plainly, but a wrong pin is slow and silent → `MPOL_BIND` / `numactl --membind`, wrong pins are remote with no warning
- The bookkeeper's "watch who draws, move toward them" → *AutoNUMA* page and task migration
- It works well for most households → auto-balancing pays off on genuinely misplaced data
- The divided couple drawing in turn every day → data genuinely written from several nodes
- Six moves in two days, slower than before → mistaking shared for misplaced; migration cost eats the gain, then thrash
- "Move what is misplaced, never what is shared" → check `numa_miss` / `numa_foreign` before migrating
- The notebook's counts: own-bank trips, crossings, stolen loads → `numastat -p` counters `numa_hit` / `numa_miss` / `numa_foreign` / `interleave_hit`
- Huge pages turning one bin into a whole row → THP coarsening the skew granularity from 4 KB to 2 MB
- The inn's two rooms on opposite sides of the building, guests unaware → the guest cannot see *vNUMA*, so its scheduler decides on a wrong world model
- "You do not widen the bridge, you make it legible" → the goal is not zero remote access; it is no node saturated and none idle
</section>
