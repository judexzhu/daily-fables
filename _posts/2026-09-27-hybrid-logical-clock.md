---
layout: fable
title: "铜壶滴漏与游标暗齿 · The Water Clock and the Vernier Cog"
title_zh: "铜壶滴漏与游标暗齿"
title_en: "The Water Clock and the Vernier Cog"
concept: "Hybrid Logical Clock (HLC)"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-09-27-hybrid-logical-clock.jpg
youtube_id: "6gHqfPwDsCw"
---
<section class="zh" markdown="1">
大雍朝的八百里加急军邮与户部转运，在万里疆域内设立了七十二座转运总台。

边防告急、粮草调发、官库银两划拨，全凭文书案卷上的"火漆时辰印"定乾坤。若前线将领申领军械在先，户部核销在后，朝廷才能批复粮饷；若逆了先后，便是欺君罔上的重罪。

每一座行台门前，都立着一座高逾丈许的青铜壶滴漏。然而，水性随天地寒暑而异——冬日关外天寒地冻，壶嘴常结暗冰，滴漏走得迟缓滞重；夏日江南湿热，水气升腾，滴漏走得飞快轻浮。即便每隔三日各行台皆设日晷校准，各台之间的滴漏，依然免不了半刻乃至一刻的微细偏差。

起初，各行台直接依本地铜壶滴漏的时辰加盖火漆印。灾祸旋踵而至。

某年深冬，镇守雁门关的守将发出一道八百里加急军报，当时关城滴漏指在"未时三刻四字"。信使快马加鞭，风驰电掣穿过峡谷，送达地处深谷、滴漏偏慢的青川府。青川府行台典吏接过信件一核对，赫然发现自己门前的铜漏才指在"未时三刻二字"！

典吏大惊失色："发信在后，收信在先？这封密折莫非是从未来飞来的鬼魅之书？"按律扣押信使，细加勘验，待到查明只是滴漏快慢之差，前线早已贻误战机。

不仅如此，倘若一处行台同一刻钟内发出数道调令，由于铜壶浮标尚未抬升一格，落下的时辰一模一样，接令的各营兵马为了争抢"谁先谁后"，险些刀兵相见。

行台提辖大怒，下令彻底弃用铜壶水漏，改用纯粹的"连号竹筹"。

每逢公文过手，便依序刻上一枚竹筹：第一、第二、第三……一直累加。

然而不出三月，户部与御史台全被逼疯了。

江南发大水，巡抚奉旨勘察堤防决口前的粮库出纳，厉声质问："昨日酉时洪水冲决前，到底发放了多少石救灾糙米？"

典吏捧着厚厚的竹筹账册冷汗直流："回大人，账上只有‘第十八万四千二百号’，至于这笔粮是昨日傍晚放的，还是三天前早晨放的……这竹筹不通日月天地，谁也算不出来！"各州各府各自数筹，谁的筹号也无法与他处的时日范围核对。

又有幕僚献策："何不让每道文书后头缀上一卷天下总图？七十二州府各设一栏，每遇公文，便将各台竹筹尽数抄录比照。"

但这更是荒唐——信使马背上挂着数十斤重的竹简，发一道军令得先花两个时辰抄写七十二个数字。战事未至，驿马早已累毙途中。

天下行台难道真要在每座穷乡僻壤的关隘，都耗费巨资打造一座由极西天朝浑天秘仪驱动、永无毫厘偏差的浑天金钟不成？

便在此时，一位兼通九章算术与天象刻漏的巧匠，献上了一方精妙绝伦的**"双规游标连环印"**。

这方印玺的机巧，藏在铜钮内部两道相互咬合的机件之中：

第一道机件，是**水刻浮标齿杆**。齿杆底部由铜壶浮子托举，但其身侧加设了一道极细密的单向倒钩棘轮——它只能随着本地水涨向上弹跳，绝不可向下倒滑半格。更神异的是，当外来急件递入行台，典吏必须先将信上所盖的水刻数读入此印。若来信的水刻数竟比本地浮标还要高，印钮内部的弹簧便"喀哒"一声，硬生生将齿杆扯起，直接咬合在那个更高的高度之上！

第二道机件，是套在浮标齿杆侧翼的**微齿步进转轮**。每当齿杆静止在某一水刻高标上时，本台若连续处置文书，微齿轮便逐次拨动一格：第一齿、第二齿、第三齿……井然有序。而一旦铜壶水涨，或是接了外地高标急信，导致浮标齿杆向上拔升至全新刻度时，微齿转轮内部的机簧便骤然脱扣，瞬息**弹跳归零**！

从此，天下行台每发一道文书，印在朱红火漆上的，皆是一对紧密咬合的双生印记：

`【水刻高标 · 游标微齿】`

这枚小巧的印玺，看似未增半分分量，却刹那间平息了天下驿道数十年的混乱：

其一，**因果永固，绝无倒流**。凡因文书往来而引发的后继之事，其水刻数要么因承袭前件而水涨船高，要么在其微齿轮上再添一刻。收信的印记永远严格大于发信的印记，世间再无"未来来信"的冤案。

其二，**日月相依，不离晨昏**。尽管水刻齿杆会被过往快件偶尔拉高，但因各州滴漏温差早有定数，它绝不会无休止地向着百年之后狂奔；它始终紧贴着本地铜壶的呼吸，牢牢束缚在大地真实的晨昏昼夜之间。御史查账，只需照着水刻大致折算，便能精准锁死"昨日申时暴雨之前"的全部底卷。

其三，**简明至极，不增毫厘**。无需七十二州府浩瀚繁杂的冗长账册，一指方圆的印面上，两个简单的数码，便抵过了千军万马的排歧定纷。

——到这儿你大概已经认出来了：这套以铜壶水刻牢拴物理时间、以游标微齿理顺瞬态因果、既抗钟偏又保单调的"双数连环印"，讲的正是分布式系统中极为优雅的经典时间机制——*Hybrid Logical Clock*（混合逻辑时钟，简称 *HLC*）。

### 这是什么

*Hybrid Logical Clock*（混合逻辑时钟，简称 *HLC*）是由 Sandeep Kulkarni、Murat Demirbas、David Madeppa、Bharath Balasubramanian 与 Phong Nguyen 于 2014 年共同提出的分布式时钟算法。它旨在完美兼顾物理时钟（*Physical Clock*，如 *NTP* 同步的墙上时钟）与逻辑时钟（*Logical Clock*，如 *Lamport Timestamp*）的各自长处，同时彻底摒弃两者的致命缺陷。

在分布式系统中，服务器节点的物理时钟受晶振温度、硬件老化及网络同步延迟影响，不可避免地存在时钟漂移（*clock skew*）。若仅依赖物理时钟为事件定序，一旦节点间出现时钟回退或微小偏差，就会彻底颠覆因果关系（*causality inversion*）；而经典的 *Lamport* 逻辑时钟或向量时钟（*Vector Clocks*），前者完全脱离了真实物理时间，无法支持按时间范围检索数据（如"读取 5 分钟前的快照"），后者的数据尺寸则随节点规模 $O(N)$ 膨胀，在大规模集群中难以承受。Google *Spanner* 虽凭借 *TrueTime* 解决了此问题，但其高度依赖机房部署的高精度原子钟与 GPS 硬件，部署门槛极高。

*HLC* 巧妙地将每个时间戳定义为一个二元组 $(l, c)$：
- $l$（物理组件）：记录该节点已观测到的**最大物理时间**（*maximum physical time seen so far*）。
- $c$（逻辑组件）：记录在同一物理时间戳下发生的事件**逻辑单调计数**（*logical counter*）。

其核心运行规则如下：
1. **本地事件（Local Event）**：
   节点读取当前物理时钟 $pt$。
   - 若 $pt > l$，说明物理时间已追赶并超越历史记录，令 $l' = pt$，$c' = 0$（计数归零）；
   - 若 $pt \le l$，说明本地物理时间尚未超过已记录的高标，令 $l' = l$，$c' = c + 1$（逻辑计数累加）。
2. **消息接收与因果推进（Message Send / Receive）**：
   节点收到附带时间戳 $(l_m, c_m)$ 的消息，并读取本地物理时钟 $pt$：
   - 新的物理组件推进为三者之极大值：$l' = \max(l, pt, l_m)$；
   - 若三者相等（$l' == l == l_m$），逻辑计数取最大值加一：$c' = \max(c, c_m) + 1$；
   - 若仅与本地 $l$ 相同，则 $c' = c + 1$；若仅与消息 $l_m$ 相同，则 $c' = c_m + 1$；
   - 若物理时钟 $pt$ 超过了 $l$ 与 $l_m$，说明物理时间全面领先，逻辑计数重置：$c' = 0$。

这一机制赋予了 *HLC* 三大数学不变量：
- **严格满足偏序因果性**：若事件 $e$ 因果先于 $f$（$e \to f$），则必然满足 $(l.e, c.e) < (l.f, c.f)$；
- **紧密贴合物理时间**：只要节点间的 *NTP* 偏差被限制在最大上限 $\epsilon$ 内，则对任意事件 $e$，恒有 $|l.e - pt.e| \le \epsilon$。这意味着时间戳绝不会无限向前飞跃，始终与物理现实保持紧密绑定；
- **存储开销恒定 $O(1)$**：通常仅用一个 64 位整数（前 48 位存毫秒物理时间，后 16 位存逻辑计数）即可完整编码，轻便精悍。

### 为什么重要

*Hybrid Logical Clock* 是现代分布式 NewSQL 数据库与全球分布式系统的基石之一：

1. **低成本实现分布式快照隔离（Snapshot Isolation）**：传统的全局强一致性数据库往往需要依赖集中式授时服务器（如 TiDB 的 Placement Driver / TSO），容易形成单点瓶颈与跨地域延迟；或者依赖 Google Spanner 那样昂贵罕见的原子钟硬件。*HLC* 使得像 **CockroachDB** 与 **YugabyteDB** 这样的开源云原生数据库，能够在普通商用机器与公有云网络（依赖普通 NTP 同步）上，去中心化地为多副本事务生成全局单调的时间戳，实现无锁快照读与可串行化事务。
2. **原生支持历史版本多版本并发控制（MVCC）与时间旅行（Time-Travel Query）**：因为 $l$ 分量受物理时间严格约束，系统既可以用它做 MVCC 的精确垃圾回收（*compaction*），又可以直接将用户输入的自然时间（如 "SELECT * AS OF SYSTEM TIME '2026-09-27 10:00:00'"）映射为对应的 HLC 区间，轻松达成历史回溯。
3. **工业级大规模工程验证**：除了 CockroachDB，**MongoDB** 自 3.6 版本起引入的 `ClusterTime`（因果一致性会话与复制集同步协议），其底层原理同样完全构建在 *HLC* 之上。它被公认为近十年来分布式时钟理论走向大规模工业实践的最杰出范例之一。

_隐喻对应表_

- 大雍朝七十二座转运总台 → 分布式网络中的各个节点（*Distributed Nodes*）
- 各行台门前受寒暑影响的铜壶滴漏 → 存在时钟漂移与网络延迟的物理时钟（*Physical Clock / NTP*）
- 雁门关与青川府的信件时间倒流案 → 纯物理时钟在网络延迟与时钟偏斜下的因果倒置（*Causality Inversion*）
- 无法考据时日晨昏的纯连号竹筹 → 脱离物理时间的 Lamport 逻辑时钟（*Logical Timestamp*）
- 记录天下七十二府筹号的沉重竹简 → 存储开销随节点数线性膨胀的向量时钟（*Vector Clocks, $O(N)$*）
- 极西天朝昂贵罕见的浑天金钟 → 依赖专用 GPS 与原子钟硬件的 Google Spanner *TrueTime*
- 双规游标连环印 → 混合逻辑时钟（*Hybrid Logical Clock, HLC*）
- 单向卡齿的水刻浮标尺（$l$） → 记录已见最大物理时间的物理组件（*Physical Component $l$*）
- 随新水刻归零的游标微齿转轮（$c$） → 同一物理时间内的逻辑单调计数器（*Logical Counter $c$*）
- `【水刻高标 · 游标微齿】`双数火漆印 → 紧凑恒定开销的 HLC 复合时间戳（*Composite Timestamp $(l, c)$*）
</section>
<section class="en" markdown="1">
Across the sprawling territory of the Great Empire, the Imperial Courier Service maintained seventy-two major transit relay stations.

Emergency military dispatches, granary transfers, and treasury drafts all depended on a single mark for their authority: the wax seal bearing the official time of dispatch. If a frontier general requisitioned weapons before the Ministry of Revenue authorized the issue, grain could be released; if the sequence appeared inverted, it was deemed falsification and treason.

Outside the gate of every relay station stood a massive bronze water clock—a dripping clepsydra over ten feet tall. Yet water answers to the seasons. In the bitter winters beyond the northern passes, ice formed in the narrow spouts, slowing the drips to a sluggish creep. In the muggy southern summers, heat thinned the flow and evaporation quickened the pace. Even though astronomers calibrated the sundials every three days, the water clocks across the realm inevitably drifted apart by several minutes.

At first, the stations stamped documents using their local clepsydra readings. Catastrophe soon followed.

One harsh winter, the commander at Yanmen Pass dispatched an urgent courier marked "third quarter of the Sheep hour, fourth tick." The courier spurred his steed through the mountain gorges and reached Qingchuan Prefecture in record time. But Qingchuan lay in a shaded valley where the clepsydra ran slow. When the duty clerk inspected the wax seal, his own clock pointed merely to "third quarter of the Sheep hour, second tick."

The clerk recoiled in horror: "Dispatched later than it was received? Does this parchment arrive from the future?" The courier was thrown into irons. By the time investigators confirmed it was merely clock drift, the fortress had fallen.

Worse still, whenever a station issued several orders within the span of a single water drop, the float had no time to rise. All orders bore identical timestamps, and rival garrisons nearly drew swords arguing over which regiment held precedence.

The Chief Warden was furious. He banned water clocks entirely and introduced sequential bamboo tally sticks.

Every time a dispatch passed through a clerk's hands, he carved the next number on a bamboo stick: one, two, three, and onward.

Within three months, both the Treasury and the Imperial Censors were driven to despair.

When a summer flood breached the southern dikes, an inspecting censor demanded of the granary master: "Before the torrent broke at dusk yesterday, exactly how many bushels of relief grain were released?"

The clerk broke into a cold sweat over his ledgers: "My Lord, the tally simply reads Number 184,200. Whether that grain left yesterday evening or three mornings ago... these tallies know nothing of sun or stars. No man alive can tell." Each province counted its own tallies, and no number could be correlated with a calendar date.

An advisor then suggested attaching a comprehensive realm scroll to every letter: seventy-two columns, one for each province, updating the latest known tally from every corner of the empire. But this was madness. Couriers carried thirty pounds of bamboo slats on their saddles, spending hours copying seventy-two columns before mounting their horses. The relays collapsed under their own weight.

Must the empire construct in every frontier outpost an astronomical celestial clock powered by imperial hydraulics and atomic precision, like the legendary contraptions of the Western dynasties?

It was then that a master artisan, versed in both hydraulics and ancient mathematics, presented the **Dual-Track Vernier Clepsydra Seal**.

The secret of this stamp lay in two interlocking bronze mechanisms housed within its handle:

The first mechanism was the **Water-Level Floated Rack**. Supported by the float in the local clepsydra, the rack carried fine, unidirectional ratchet teeth along its spine. It could click upward as the water rose, but could never slip backward. Furthermore, when an incoming dispatch arrived, the clerk set the seal against the mark on the letter. If the dispatch bore a water level higher than the local float, a spring clicked sharply, pulling the rack upward to latch onto that higher mark instantly.

The second mechanism was the **Vernier Stepping Wheel**, mounted on the shoulder of the rack. Whenever the rack rested at a fixed water mark, any successive document stamped by the station advanced the vernier wheel by one tooth: tooth one, tooth two, tooth three. But the moment the water level rose—or an incoming dispatch pulled the rack up to a new height—the vernier wheel tripped its release catch and **snapped back to zero**.

From that day forward, every dispatch stamped across the empire carried a paired inscription pressed into the vermillion wax:

`[Water Level · Vernier Notch]`

This modest seal, requiring no more bronze than an ordinary stamp, resolved decades of courier chaos overnight:

First, **causality was preserved without inversion**. Every event triggered by a prior message inherited either the higher water level or a stepped vernier tooth. Receipt was guaranteed to be strictly greater than dispatch. Never again did a message appear to arrive from tomorrow.

Second, **time remained anchored to the sun**. Though an incoming letter might pull the rack slightly ahead of the local water level, the seasonal temperature limits kept the drift bounded. The rack never raced into the distant future; it remained tethered to the real rhythm of day and night. When censors audited accounts, they could convert the water level directly to find every ledger entry "before yesterday's dusk."

Third, **the cost remained constant and light**. No heavy scrolls bearing seventy-two tallies were needed. Two compact numbers on a thumb-sized seal accomplished what thousands of clerks could not.

—By now you've probably recognized it: this paired seal, anchoring physical time with a water rack while tracking causality with a vernier cog, is the classic distributed timekeeping mechanism known as the *Hybrid Logical Clock* (HLC).

### What it is

The *Hybrid Logical Clock* (HLC) was introduced in 2014 by Sandeep Kulkarni, Murat Demirbas, David Madeppa, Bharath Balasubramanian, and Phong Nguyen. It combines the strengths of physical clocks (such as NTP-synchronized wall clocks) and logical clocks (such as Lamport timestamps) while avoiding their respective pitfalls.

In distributed systems, physical clocks inevitably drift due to quartz oscillator imperfections, temperature swings, and network jitter (*clock skew*). Relying purely on physical timestamps leads to causality inversions—where an effect appears to precede its cause because of clock drift or backward jumps. Conversely, pure logical clocks (like Lamport timestamps) decouple completely from real physical time, making time-range queries (e.g., "read the snapshot from 5 minutes ago") impossible. Vector clocks track full causality, but their size grows linearly with cluster size $O(N)$, creating unacceptable storage and network overhead. Google *Spanner* solved this with *TrueTime*, but requires specialized datacenter hardware—GPS receivers and atomic clocks—that is costly and unavailable in standard cloud environments.

HLC represents every timestamp as a compact pair $(l, c)$:
- $l$ (Physical component): tracks the **maximum physical time seen so far** across the node and all messages it has observed.
- $c$ (Logical component): tracks a **monotonically increasing logical counter** for events that share the exact same physical component $l$.

The update rules are straightforward:
1. **Local event**:
   The node samples its local physical clock $pt$.
   - If $pt > l$, physical time has advanced past prior records: set $l' = pt$ and reset $c' = 0$.
   - If $pt \le l$, physical time has not overtaken the highest recorded mark: keep $l' = l$ and increment $c' = c + 1$.
2. **Message send / receive**:
   Upon receiving a message with timestamp $(l_m, c_m)$, the node samples its local physical clock $pt$:
   - Advance the physical component to the maximum of all three values: $l' = \max(l, pt, l_m)$.
   - If all three match ($l' == l == l_m$), advance the counter past both: $c' = \max(c, c_m) + 1$.
   - If $l' == l$, increment $c' = c + 1$; if $l' == l_m$, set $c' = c_m + 1$.
   - If $pt$ is strictly greater than both $l$ and $l_m$, physical time has pulled ahead: reset $c' = 0$.

These rules guarantee three crucial properties:
- **Strict causality preservation**: If event $e$ causally precedes $f$ ($e \to f$), then $(l.e, c.e) < (l.f, c.f)$.
- **Bounded drift from physical time**: As long as NTP keeps physical clock drift across nodes within a maximum bound $\epsilon$, then for every event $e$, $|l.e - pt.e| \le \epsilon$. HLC timestamps never drift unboundedly away from true wall-clock time.
- **Constant storage overhead $O(1)$**: An HLC timestamp typically packs into a single 64-bit integer (e.g., 48 bits for millisecond physical time and 16 bits for the logical counter), keeping network packets and storage indices small.

### Why it matters

Hybrid Logical Clocks have become foundational infrastructure in modern distributed databases:

1. **Decentralized Snapshot Isolation without exotic hardware**: Traditional distributed databases with serializable or snapshot isolation either relied on centralized timestamp authorities (like TiDB's Placement Driver / TSO), which become cross-region latency bottlenecks, or required specialized atomic clocks like Spanner. HLC allows distributed databases like **CockroachDB** and **YugabyteDB** to run on commodity cloud instances with standard NTP, generating globally consistent transaction timestamps without a central coordinator.
2. **Native support for MVCC and Time-Travel Queries**: Because the physical component $l$ is strictly bounded by wall-clock time, storage engines can safely garbage-collect older MVCC versions based on physical retention policies. Furthermore, user-facing queries like `AS OF SYSTEM TIME '2026-09-27 10:00:00'` map directly to HLC bounds, enabling seamless point-in-time recovery and historical snapshots.
3. **Broad industrial adoption**: Beyond NewSQL engines, **MongoDB** built its causal consistency sessions and replication synchronization (`ClusterTime`) on top of HLC starting in version 3.6. HLC stands as one of the most practical and influential distributed systems breakthroughs of the past decade.

_Metaphor mapping_

- Seventy-two imperial courier relay stations → Distributed nodes across the cluster (*Distributed Nodes*)
- Clepsydra water clocks sensitive to heat and frost → Physical wall clocks subject to drift and NTP skew (*Physical Clock / NTP*)
- Inverted timestamps between Yanmen Pass and Qingchuan → Causality inversion caused by clock skew (*Causality Inversion*)
- Pure sequential bamboo tallies divorced from day and night → Lamport logical clocks lacking physical correlation (*Logical Timestamp*)
- Bulky realm scrolls tracking seventy-two provincial tallies → Vector clocks scaling linearly with node count (*Vector Clocks, $O(N)$*)
- Imperial celestial clock with astronomical hydraulics → Google Spanner TrueTime with dedicated GPS and atomic clocks
- Dual-Track Vernier Clepsydra Seal → Hybrid Logical Clock (*HLC*)
- Unidirectional ratchet rack tracking water level ($l$) → Physical component tracking maximum observed physical time (*$l$ component*)
- Vernier stepping wheel resetting on new water level ($c$) → Logical counter resetting when physical time advances (*$c$ component*)
- Paired `[Water Level · Vernier Notch]` wax imprint → Compact $O(1)$ composite timestamp (*Composite Timestamp $(l, c)$*)
</section>
