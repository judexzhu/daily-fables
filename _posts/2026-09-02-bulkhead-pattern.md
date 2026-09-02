---
layout: fable
title: "隔舱的规矩 · The Rule of Compartments"
title_zh: "隔舱的规矩"
title_en: "The Rule of Compartments"
concept: "Bulkhead pattern"
tags: [distributed-systems, sre]
illustration: /assets/art/2026-09-02-bulkhead-pattern.jpg
youtube_id: "1pTi0DdPN2U"
---
<section class="zh" markdown="1">
南河上跑着一种老式的木货船，船身宽，吃水深，一趟能装几千斤米粮。

早年的船都是通舱——船底就是一整个大肚子，米从船头堆到船尾，中间什么都没有。装卸方便，跑了十几年也没出过事。

直到那年秋天，一艘通舱船在夜里擦了暗礁。

破口不大，拳头那么一个洞，在船尾偏左的位置。水涌进来，先淹了船尾的米。船老大叫人堵，拿棉絮塞、拿木板钉，忙了一炷香。水还是进。

问题不在堵不堵得住。问题在于：**水进了船尾之后，顺着通舱的底板一路往船头漫。** 通舱没有任何东西拦它。一炷香之后，整条船的底板上都是水，米全泡了。船身越来越沉，水线越来越高，最后连破口的位置都没入了水面以下——堵都没法堵了。

三千斤米，全丢了。船也沉了。

而隔壁码头有一条差不多大的船，同一年也擦过礁。那条船的船老大是个老师傅，造船时就在船底每隔五尺钉了一道木板墙，把通舱隔成了六个独立的小舱。每个舱之间严丝合缝，水过不去。

那次擦礁，破口在第四舱。水涌进第四舱，半个时辰就灌满了。但第三舱是干的，第五舱是干的，船头船尾全是干的。船老大让人把第四舱的货搬出来扔掉——五百斤米没了，但船还浮着，剩下的两千五百斤安安全全运到了对岸。

后来有人问老师傅："你那六道隔板，平时不嫌碍事吗？装货得一个舱一个舱地塞，卸货也得一个舱一个舱地掏，比通舱慢多了。"

老师傅说："慢是慢。但通舱的船，一个洞沉一条船。隔舱的船，一个洞只沉一格。**你是要快着装、快着沉，还是慢着装、慢着到？**"

又有人问："那干脆隔成三十个舱，不是更安全？"

老师傅摇头："舱太多，每格装不了几袋米，伙计钻进去转不了身，装卸比走路还慢。而且隔板本身也占地方——三十道板，船肚子小了一成。**隔几格是门手艺，不是越多越好。**"

——到这儿你大概已经认出来了：这就是 *bulkhead pattern*。

### 这是什么

*Bulkhead*（舱壁）这个名字直接来自造船术语：货船底部用横向隔板分成若干独立水密舱，一个舱进水不影响其他舱，船不会因为一个破口而整体沉没。

在软件工程中，*bulkhead pattern* 是一种**资源隔离**策略：把系统的资源（线程池、连接池、内存、CPU 配额）分成独立的隔间，每个隔间服务于不同的调用方或功能。当一个隔间里的工作出了问题——某个下游服务超时、某类请求突然暴增——它只能耗尽自己那个隔间里的资源，不会拖垮其他隔间。

最常见的实现是**独立线程池**。比如一个 API 服务同时调用支付、库存、推荐三个下游。如果三个调用共用一个线程池，推荐服务一旦变慢，所有线程都会阻塞在推荐的调用上，支付和库存也跟着不可用——一个非关键服务拖死了整个系统。Bulkhead 的做法是给每个下游分配独立的线程池：推荐池 10 个线程、支付池 20 个、库存池 15 个。推荐再慢，最多占满自己那 10 个线程，支付和库存完全不受影响。

Netflix 的 Hystrix（现已停维但思想仍在）最早把这个模式叫做 *bulkhead*。Resilience4j 的 `Bulkhead` 和 `ThreadPoolBulkhead` 是当前 Java 生态的标准实现。在 Kubernetes 里，*resource requests/limits* 本质上就是 Pod 级别的 bulkhead：每个 Pod 有自己的 CPU 和内存配额，一个 Pod 内存泄漏只会被 OOMKill 自己，不会吃掉 node 上其他 Pod 的内存。Istio 的 `DestinationRule` 里的 `connectionPool` 配置同样是 bulkhead 思想——限制到每个上游服务的最大连接数和并发请求数。

### 为什么重要

没有 bulkhead 的系统是通舱船：**任何一个组件的故障都可以耗尽全局共享资源，进而拖垮所有组件。** 这就是级联故障（*cascading failure*）的经典路径——不是因为所有组件同时坏了，而是一个组件变慢，占满了共享资源，让其他健康的组件也拿不到资源。

Bulkhead 和 *circuit breaker* 经常搭配使用，但解决的是不同维度的问题。Circuit breaker 回答的是"要不要继续调这个下游"（快速失败），bulkhead 回答的是"这个下游最多能用多少资源"（资源上限）。一个是流量开关，一个是容量围栏。两者一起才构成完整的故障隔离。

隔间的粒度是关键的设计决策。太粗（整个服务一个池）等于没隔；太细（每个 API endpoint 一个池）管理成本爆炸，而且小池容易在正常流量下就饱和。实践中通常按**故障域**划分：同一个下游服务、同一个数据库、同一类风险的操作归为一个隔间。

_隐喻对应表_

- 通舱货船 —— 所有调用共享一个线程池/连接池的系统
- 一个拳头大的破口 —— 单个下游服务变慢或故障
- 水顺着通舱底板漫到全船 —— 故障耗尽共享资源，级联到所有功能
- 三千斤米全丢、船沉了 —— 整个系统不可用
- 每隔五尺一道隔板 —— bulkhead：按故障域隔离资源
- 六个独立水密舱 —— 独立的线程池/连接池/资源配额
- 第四舱进水，其余五舱干燥 —— 故障被限制在一个隔间内
- 五百斤米丢了，两千五百斤安全到岸 —— 部分降级，核心功能存活
- "装卸比通舱慢" —— 隔离的运维成本：配置、监控、调优每个池
- "三十个舱太多，转不了身" —— 过细的隔离粒度：资源碎片化、管理负担
- "隔几格是门手艺" —— 隔间粒度设计：按故障域而非按 API 划分
</section>
<section class="en" markdown="1">
On the South River there used to run a type of old wooden cargo ship — broad-beamed, deep-drafted, carrying several thousand catties of rice in a single trip.

In the early days every ship was open-hold: the hull was one big belly, rice stacked from bow to stern with nothing in between. Loading was easy, and for more than a decade nothing went wrong.

Until one autumn night, an open-hold ship scraped a submerged reef.

The breach was small — a fist-sized hole on the port side near the stern. Water rushed in and began soaking the rice at the back. The captain ordered his crew to plug it: cotton wadding, wooden boards, hammers swinging in the dark. They worked for half an hour. The water kept coming.

But the real problem was not whether they could plug the hole. The real problem was this: **once water entered the stern, it flowed freely along the open floor toward the bow.** An open hold has nothing to stop it. Half an hour later, the entire bottom of the ship was underwater, and every sack of rice was ruined. The hull sank lower, the waterline crept higher, and eventually the breach itself was submerged — now they could not even reach it to plug.

Three thousand catties of rice, lost. The ship, sunk.

At the next dock over, a ship of roughly the same size had also scraped a reef that same year. Its captain was an old master who, when the ship was built, had insisted on nailing a wooden partition wall across the hull every five feet, dividing the open hold into six independent compartments. Each wall was sealed tight — water could not pass.

When that ship struck the reef, the breach was in compartment four. Water flooded compartment four and filled it in an hour. But compartment three was dry. Compartment five was dry. The bow and stern were bone dry. The captain had his crew haul the cargo out of compartment four and dump it overboard — five hundred catties of rice gone, but the ship stayed afloat, and the remaining twenty-five hundred catties arrived safely at the far bank.

Later someone asked the old master: "Don't those six partition walls get in the way? You have to load one compartment at a time and unload one at a time — much slower than an open hold."

The old master said: "Slower, yes. But an open-hold ship, one hole sinks the whole vessel. A partitioned ship, one hole sinks one compartment. **Would you rather load fast and sink fast, or load slow and arrive?**"

Someone else asked: "Then why not thirty compartments — wouldn't that be even safer?"

The old master shook his head. "Too many compartments and each one barely fits a few sacks. The crew can't turn around inside. Loading takes longer than walking. And the partition walls themselves take up space — thirty walls, and you've lost a tenth of your cargo hold. **How many compartments to build is a craft, not a contest for the most.**"

— By now you have probably recognized it: this is the *bulkhead pattern*.

### What it is

The name *bulkhead* comes directly from shipbuilding: transverse partition walls divide a cargo hull into independent watertight compartments. One compartment flooding does not affect the others, and the ship does not sink from a single breach.

In software engineering, the *bulkhead pattern* is a **resource isolation** strategy: partition a system's resources (thread pools, connection pools, memory, CPU quotas) into independent compartments, each serving a different caller or function. When work in one compartment goes wrong — a downstream service times out, a class of requests suddenly spikes — it can only exhaust the resources inside its own compartment. It cannot drag down the others.

The most common implementation is **separate thread pools**. Suppose an API service calls three downstreams: payments, inventory, and recommendations. If all three share one thread pool and the recommendation service slows down, every thread blocks on recommendation calls, and payments and inventory become unavailable too — a non-critical service has killed the entire system. The bulkhead approach gives each downstream its own pool: 10 threads for recommendations, 20 for payments, 15 for inventory. If recommendations stall, at most 10 threads are consumed; payments and inventory are completely unaffected.

Netflix's Hystrix (now in maintenance mode, but the ideas live on) was the first to name this pattern *bulkhead*. Resilience4j's `Bulkhead` and `ThreadPoolBulkhead` are the current standard in the Java ecosystem. In Kubernetes, *resource requests and limits* are essentially Pod-level bulkheads: each Pod has its own CPU and memory quota; a Pod with a memory leak gets OOMKilled on its own and does not eat memory from other Pods on the node. Istio's `DestinationRule` with `connectionPool` settings is the same idea — capping the maximum connections and concurrent requests to each upstream service.

### Why it matters

A system without bulkheads is an open-hold ship: **any single component's failure can exhaust globally shared resources and drag down every other component.** This is the classic path of *cascading failure* — not because everything breaks at once, but because one component slows down, saturates the shared pool, and starves every healthy component of resources.

Bulkhead and *circuit breaker* are often used together but solve different dimensions of the problem. A circuit breaker answers "should I keep calling this downstream" (fail fast). A bulkhead answers "how much resource can this downstream consume at most" (resource ceiling). One is a traffic switch; the other is a capacity fence. Together they form complete fault isolation.

Compartment granularity is the key design decision. Too coarse (one pool for the entire service) is the same as no bulkhead. Too fine (one pool per API endpoint) explodes management cost, and small pools saturate under normal traffic. In practice, partitioning follows **failure domains**: calls to the same downstream, queries to the same database, operations with the same risk profile go into one compartment.

_Metaphor mapping_

- The open-hold cargo ship — a system where all calls share one thread pool / connection pool
- A fist-sized breach — a single downstream service slowing down or failing
- Water flowing freely across the open floor — failure exhausting shared resources, cascading to all functions
- Three thousand catties of rice lost, ship sunk — total system unavailability
- A partition wall every five feet — bulkhead: isolating resources by failure domain
- Six independent watertight compartments — separate thread pools / connection pools / resource quotas
- Compartment four floods, the other five stay dry — failure contained within one compartment
- Five hundred catties lost, twenty-five hundred arrive safely — partial degradation, core functions survive
- "Loading is slower than an open hold" — the operational cost of isolation: configuring, monitoring, tuning each pool
- "Thirty compartments is too many, crew can't turn around" — over-fine granularity: resource fragmentation, management burden
- "How many compartments to build is a craft" — compartment granularity design: partition by failure domain, not by API
</section>
