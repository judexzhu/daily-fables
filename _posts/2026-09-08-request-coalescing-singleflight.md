---
layout: fable
title: "龙舌井的铜环 · The Copper Ring at the Dragon Tongue Well"
title_zh: "龙舌井的铜环"
title_en: "The Copper Ring at the Dragon Tongue Well"
concept: "Request Coalescing (Singleflight)"
tags: [distributed-systems, performance, microservices]
illustration: /assets/art/2026-09-08-request-coalescing-singleflight.jpg
youtube_id: "oA6h4jiP67U"
---
<section class="zh" markdown="1">
青岩镇北的绝壁下，有一眼名传百里的深井，名唤龙舌井。

井深一百八十丈，直插幽谷地脉。井壁全由极坚硬的青冈岩凿就，逼仄如一线天，井口仅容一只朱漆大水瓮上下。这口井的水极甜极冽，全镇三十六家茶庄、十座蒸酒坊与几十户大宅，每日清晨必饮此水。

在井台守了四十年的老宋师傅，见识过龙舌井最凶险的早晨。

早年间，井台没有章法。卯时一到，各家小厮、挑夫提着木桶蜂拥而至。甲茶庄的伙计喊着水开茶等，乙酒坊的汉子吼着急待投曲。三十多条粗麻绳争先恐后往窄如刀削的井口里放。

井下一片漆黑，湿风狂卷。麻绳在半空中绞成一团死结，三只铁箍木桶在石壁上撞得粉碎，缆绳卡死在辘轳的轮槽里。折腾到巳时三刻，井下落满了断绳与木屑，井上的掌柜们捶胸顿足，整条街的茶灶硬生生冒了冷烟。

那次乱局之后，老宋师傅在井台上立了一面黑漆木架，架上钉了三十六枚黄铜活环，井口正上方换上了一只由粗麻巨缆悬吊的八斗储水大瓮。

规矩被老宋用白灰刷在山壁上：**“铜环起，诸客止。”**

第二天黎明，薄雾未散。甲茶庄的跑堂提着两个空锡壶飞奔而来：“宋伯，明前雨针要头道水，十火急！”

老宋头也不抬，伸手将架子上刻着“龙舌甘冽”的那枚铜环“啪”地推上滑槽。铜环悬起，老宋踩动脚闸，井口正中央的那只八斗大瓮呼啸着坠入百丈深井。

眨眼功夫，乙茶庄、丙酒坊、丁字客栈的二三十个伙计喘着粗气赶到，个个手里抱着空壶空桶，扯着嗓子要探绳。

老宋旱烟袋往木架上一敲：“没瞧见铜环挂着呢？大瓮已经在百丈地心汲水了，都收了你们的绳子，在石凳上坐稳当了！”

伙计们面面相觑，谁也不许抢步。二十多号人老老实实把空桶码在青石台阶上，坐在长条石凳上屏息静候。

约莫半柱香功夫，井下传来轰隆的沉闷回响，沉重的大瓮被绞盘徐徐提上井口。水汽如白雾般喷涌而出，瓮里盛着满满八斗刚出泉眼的冰肌雪水。

老宋一手扶住瓮沿，提起铜勺，如行云流水般往石凳前排好的二十多只木桶里舀去。一勺一壶，片刻不差，三十个空桶瞬间水满溢清。伙计们喜笑颜开，挑着清泉一哄而散。

等最后一只桶装满，老宋顺手摘下那枚铜环，落回原位。此时若有第三十一家赶来，铜环便重新升起，大瓮才再次下潜。

老宋坐在井沿上磕着烟灰，对新来的小徒弟吐出一口青烟：

“一口窄井，最忌同声喊渴。要是来一个人就放一根绳，井口非卡死不可。**有一人先跑了腿，后头跟着要同一碗水的人，就只管让他们在石凳上等这一趟回来分。**”

顿了顿，老宋眼睛微眯，指着那根紧绷的吊缆又补了一句：

“可你也得记着：这一瓮要是半道断了缆、或是舀上了一勺泥浆，石凳上这三十条嗓子，可得同时挨饿受渴。”

——到这儿你大概已经认出来了：这就是高并发后端与分布式系统中的 **Request Coalescing**（请求合并），在现代工程界最著名的名字叫 **Singleflight**。

### 这是什么

在分布式系统或高并发微服务中，当一个高频访问的热点数据发生缓存失效（Cache Miss）或遇到冷启动时，海量的并发请求会在同一微秒内同时击中后端。这种现象通常被称为**缓存击穿**或**惊群效应（Thundering Herd）**。

如果系统不做管控，成百上千个并发线程或协程就会同时向底层数据库、核心存储或高延迟的外部下游发起完全相同的查询。这会导致：
- 数据库连接池在数毫秒内被彻底打满；
- 数据库 CPU 飙升至 100%，引发级联超时；
- 下游系统因瞬时突发流量雪崩。

**Request Coalescing（请求合并 / Singleflight）** 是一种极致优雅的本地并发控制模式，最知名的实现是 Go 语言官方扩展包中的 `golang.org/x/sync/singleflight`，以及 Envoy 反向代理中的 Request Collapsing：

1. **共享飞行状态（Flight Map）**：
   系统在内存中维护一个映射表，以请求的唯一标识符（Key）作为键。
2. **首发领跑（The Leader）**：
   当请求到达时，首先检查 Key。如果当前没有相同的请求在“飞行中”，该请求就成为 Leader，由它真正发起对下游昂贵资源的调用。
3. **同伴排队（Coalescing Waiters）**：
   在 Leader 执行期间，任何携带相同 Key 到达的其他并发请求（Followers），绝不发起第二次远程调用，而是挂起自身（在 Go 中为订阅一个广播 Channel），静静等待 Leader 的返回。
4. **广播分发（Broadcast Result）**：
   当 Leader 的下游调用完成并拿到结果（或错误）时，系统一次性将同一份数据复制分发给所有正在排队等待的协程。
5. **飞行结束（Cleanup）**：
   分发完毕后，从内存映射表中摘除该 Key。后续再来的新请求，将作为下一轮全新的 Leader 重新触发调用。

### 为什么重要

Singleflight 与“全局分布式锁”或“普通缓存”有着本质的区别：

- **零常驻开销**：与 Redis 缓存不同，Singleflight 不常驻数据，它只在“调用正在进行中的这个短暂并发窗口内”生效。一旦执行完毕立即释放，完全没有缓存淘汰策略（TTL/LRU）带来的内存溢出风险。
- **保护脆弱下游的护城河**：在秒杀大促、微服务容器冷启动或主从切换后缓存全空的瞬间，几十万 QPS 的相同只读流量会在第一跳网络层被直接收敛成单个真实请求，彻底瓦解惊群风暴。

**工程陷阱与避坑准则**：
- **命运连坐（Error Blast Radius）**：如老宋所言，Leader 的成败就是所有跟随者的成败。如果 Leader 遭遇超时、网络丢包或下游 Panic，这批并发等待的所有请求会**同时收到相同的失败**。
- **慢 Leader 拖垮群客（Head-of-Line Blocking）**：如果 Leader 请求因下游阻塞陷入长尾延迟，后面所有搭便车的请求都会被无限期扣押。工业级实现必须支持带超时的上下文（`DoChan` + `context.WithTimeout`），允许等待者在 Leader 挂起时提前脱离甚至发起对冲尝试。
- **只适用于幂等的只读操作**：合并只适用于读取（Query）或无副作用的计算，绝不能将带有副作用的写操作（如扣款、下单）进行合并。

_隐喻对应表_

- 窄如刀削的一百八十丈井口 → 脆弱且高延迟的底层资源（数据库/微服务瓶颈）
- 井台涌来的几十家茶庄伙计 → 瞬时突发的海量并发相同请求
- 三十条麻绳在井口绞死撞碎木桶 → 缓存击穿导致的数据库连接池耗尽与 CPU 瘫痪
- 老宋木架上的黄铜活环 → 正在飞行中的请求标记（In-flight Map Entry）
- 第一位提桶跑堂让铜环升起 → Leader 请求（发起唯一的真实底层调用）
- 其他伙计收起麻绳坐在石凳上静候 → 并发跟随者挂起并订阅等待结果（Wait Channel）
- 八斗大瓮满水出井，一勺勺分进空桶 → 单次调用结果广播复用给所有等待者
- 舀完水摘下铜环落回原位 → 调用完毕，从活跃 Map 中删除该 Key
- 大瓮断缆则三十条嗓子齐挨渴 → 错误广播放大（Shared Failure / Error Blast Radius）
- 绞盘过慢导致全员久坐受冻 → 慢 Leader 引发的队头阻塞（Head-of-Line Blocking）
</section>
<section class="en" markdown="1">
Beneath the precipitous cliffs north of Qingyan Town lay a celebrated deep well known far and wide as the Dragon Tongue Well.

Its shaft reached one hundred and eighty fathoms into the bowels of the mountain. Carved entirely through bedrock of adamantine blue granite, the shaft was narrower than a slit in a fortress wall—its mouth permitting only a single large vermillion-lacquered cask to pass up and down. Yet its water was so sweet and piercingly crisp that thirty-six tea houses, ten distilleries, and dozens of great estates throughout the valley depended on it for their first morning brew.

Old Master Song, who had guarded the wellhead for forty years, had lived through the Dragon Tongue Well's most perilous dawns.

In the old days, there was no order at the wellhead. The moment the dawn bell tolled, runners and porters from every quarter descended upon the stone terrace in a shouting swarm. One tea master's boy screamed that his kettles were dry; a brewer hollered that his fermenting vats would spoil. Thirty coarse hemp ropes were hurled into the dark, razor-narrow shaft all at once.

Down in the black throat of the gorge, wild winds howled. The ropes tangled into a hopeless knot midair; three iron-banded buckets shattered against the rocks; and the snarled lines jammed the wooden windlass tight. By mid-morning, the deep shaft was choked with severed twine and splintered stave-wood. The town's hearths stood cold, and the tea ovens blew bitter grey smoke.

After that disastrous morning, Master Song erected a black-lacquered rack beside the windlass, studded with thirty-six sliding brass rings. Directly above the well shaft, he suspended a single eight-bushel cask hanging from an enormous braided hawser.

He painted the new law on the cliff face in lime wash: **"When the brass ring hangs, all ropes must hang back."**

The following morning before dawn, mist still clung to the gully. A frantic boy from the first teahouse arrived clutching two empty pewter kettles: "Master Song! Our early-spring tea needs the first drawing! A matter of life and death!"

Without glancing up, Master Song reached over to the wooden rack and slid the brass ring engraved with *Dragon Tongue Spring* straight up into its lock. With the ring raised, Song kicked the windlass catch. The great eight-bushel cask plunged into the dark chasm with a hollow roar.

Within breaths, thirty runners from neighboring wineries and inns arrived gasping for breath, hugging their empty jars and shouting to cast down their ropes.

Master Song rapped his long pipe against the frame: "Can't you see the brass ring hanging? The great cask is already drawing from the vein a thousand cubits below. Coiling your ropes, and sit yourselves down on the stone benches!"

The runners traded glances, but none dared break the rule. Two dozen apprentices lined up their empty vessels along the flags and sat down in an orderly row upon the long stone benches, waiting in silence.

After half the burning of an incense stick, a low rumble echoed from the deep. The heavy windlass groaned as the brimming cask crested the lip of the well. Vapor billowed out into the morning chill like white clouds, and inside sat eight full bushels of snow-clear water.

Resting his arm against the cask rim, Master Song raised a long-handled copper ladle. In one fluid rhythm, he scooped the sparkling water into each of the twenty-odd waiting barrels. One ladle per vessel, without a drop wasted. Within minutes, every barrel stood brimming. The boys hoisted their carrying poles, bowed in gratitude, and scattered down the mountain trails.

Only when the last vessel was filled did Master Song slide the brass ring back to its resting place. Had a thirty-first courier arrived that instant, the ring would have risen anew, and the cask would have begun another descent.

Sitting upon the stone rim and tapping out his pipe embers, Master Song puffed a blue cloud toward his young apprentice:

"At a narrow well, the worst folly is for thirty throats to holler at once. If every runner drops his own rope, the shaft will jam till midday. **Let the first man run the errand; as for all who seek the very same draft, seat them on the benches and let them share the single haul when it returns.**"

Pausing, the old well-keeper squinted toward the taut hawser, adding quietly:

"Yet heed this well: if that hawser snaps halfway down, or the cask scoops up mud, all thirty boys on those benches will go home empty-handed together."

— By now, you have probably recognized the pattern: this is **Request Coalescing**, known throughout modern backend engineering as **Singleflight**.

### What it is

In distributed architectures and high-concurrency microservices, when a popular cached key expires (Cache Miss) or a cold cache is rebooted, an avalanche of concurrent requests often strikes the backend within the exact same millisecond. This phenomenon is known as a **Cache Stampede** or the **Thundering Herd Problem**.

Without safeguards, hundreds or thousands of concurrent threads or coroutines will simultaneously dispatch the exact same query downstream to the primary database or an expensive remote service. This triggers:
- Exhaustion of database connection pools within milliseconds;
- Sudden spikes to 100% database CPU, provoking cascading timeouts;
- Cascading brownouts across downstream dependencies.

**Request Coalescing (Singleflight)** is an exceptionally clean concurrency pattern—most famously implemented in Go's standard extended library `golang.org/x/sync/singleflight` and Envoy's HTTP Request Collapsing:

1. **The In-Flight Map**:
   The proxy or application maintains an in-memory map keyed by the unique identifier of the requested query or resource.
2. **The Leader**:
   When a request arrives, it looks up its key. If no identical request is currently "in flight", this request becomes the Leader and dispatches the genuine call to the expensive downstream resource.
3. **Coalesced Waiters (Followers)**:
   While the Leader's call is outstanding, any subsequent requests arriving with the identical key refrain from calling the downstream service. Instead, they suspend execution (in Go, by listening on a shared completion channel) and wait for the Leader's return.
4. **Broadcast Dispatch**:
   The instant the Leader completes its downstream execution and receives the result (or an error), the system copies and delivers that single outcome to every waiting follower concurrently.
5. **Flight Cleanup**:
   Once delivery is complete, the key is evicted from the in-flight map. Any new request arriving thereafter will launch a brand-new Leader cycle.

### Why it matters

Singleflight differs fundamentally from traditional caching or distributed locking:

- **Zero Persistent Memory Overhead**: Unlike a Redis cache, Singleflight retains zero data long-term. It operates purely within the transient concurrency window while a call is active. Once resolved, memory is immediately reclaimed, completely sidestepping LRU/TTL cache-sizing concerns.
- **A Fortress for Fragile Backends**: During sudden viral traffic bursts, container cold-starts, or cache invalidation events, thousands of redundant read operations per second are collapsed at the edge into a single outbound query, defusing thundering herds before they reach storage.

**Engineering Pitfalls & Tradeoffs**:
- **Shared Fate (Error Blast Radius)**: As Master Song warned, the Leader's fate is shared by all followers. If the Leader encounters a timeout, network drop, or panic, **all waiting requests fail simultaneously with the exact same error**.
- **Head-of-Line Blocking**: If a slow Leader stalls in downstream I/O, every piggybacking follower is held hostage. Industrial implementations must provide context-cancellation hooks (`DoChan` + `context.WithTimeout`), allowing followers to abandon the wait or dispatch a hedged fallback if the Leader takes too long.
- **Strictly for Idempotent Reads**: Request coalescing applies exclusively to safe, idempotent queries. State-mutating operations (such as payments, account debits, or inventory decrements) must never be coalesced.

_Metaphor mapping_

- The narrow, 180-fathom well shaft → The shared, high-latency downstream bottleneck (database / external API)
- Swarms of teahouse apprentices clamoring for water → Concurrent identical requests during a cache stampede
- Thirty tangled ropes jamming the shaft → Exhausted database connection pools and collapsed throughput
- Master Song's sliding brass rings on the rack → The in-memory in-flight map tracking active calls
- The first runner raising the brass ring → The Leader request executing the underlying remote call
- Subsequent runners sitting quietly on the stone benches → Followers blocked on a shared completion channel / future
- The single 8-bushel cask ladling water into every vessel → A single query result broadcast to all waiting callers
- Sliding the brass ring down once the cask is emptied → Evicting the completed key from the flight map
- A broken hawser leaving all thirty boys thirsty → Shared fate / error blast radius propagating across all waiters
- A sluggish windlass delaying all seated runners → Head-of-line blocking by an unoptimized or slow Leader
</section>
