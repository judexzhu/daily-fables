---
layout: fable
title: "千顷山田的照影铜镜 · The Reflected Bronze Mirrors of a Thousand Terraces"
title_zh: "千顷山田的照影铜镜"
title_en: "The Reflected Bronze Mirrors of a Thousand Terraces"
concept: "Kubernetes SharedInformer: Reflector, DeltaFIFO, Indexer, and Local Cache Sync"
tags: [kubernetes, distributed-systems, performance]
illustration: /assets/art/2026-09-19-kubernetes-sharedinformer-reflector-deltafifo.jpg
---
<section class="zh" markdown="1">
苍山万仞，山势险峻。从山巅到山脚，层层叠叠修筑了数千顷梯田与纵横交错的灌溉水渠。

在云海之上的最高峰顶，坐落着总管全山水源的“天池大水司”。水司里立着一座庞大的水闸枢纽，控制着通往各条溪涧的水流开合，并用朱砂在大石壁上记录着每一道水闸当下的蓄水寸尺。
而在山腰与山脚下，散居着三十位各司其职的水利巡山吏：有的专管稻田水渠，有的专管茶垄喷灌，有的专管鱼塘蓄水，有的专管防洪泄洪。

早些年，三十位巡山吏有一套令人苦不堪言的笨法子。
每隔半刻钟，各位巡山吏便打发脚力迅捷的小徒弟，手提灯笼沿着陡峭的五千级盘山石阶飞奔上山，气喘吁吁地狠砸天池大水司的山门，大声盘问：“敢问第四十二号水闸如今开了几寸？第八十八号水渠可曾分流？”
大水司门前整日被三十个呼哧带喘的徒弟堵得水泄不通。天池的掌簿官被吵得头昏脑涨，石壁上的墨迹还没吹干便被反复追问；
更致命的是，盘山路险，徒弟往返一趟便要耗去半日。等徒弟两腿打颤跑回山腰，天池的水位早就因山雨骤降而变动了三次。巡山吏按半天前探听来的老消息开闸，不是把下头的稻田淹成汪洋，就是让下头的茶垄干涸龟裂。

新任总水司顾先生巡视各山，目睹此景，当即召集三十位巡山吏，在山腰正中的听泉水榭立下一座“照水长轩”，颁下一套奇绝的**“长轩铜镜章程”**：

其一，**“镜照孤峰，首巡全览，浮光追变”（Reflector: List 与 Watch）**。
水榭长轩正中架起一面磨制得晶莹剔透的八尺青铜凸透长镜，正对着峰顶天池石壁上的旗语高台。
各处巡山吏再也不准派徒弟爬山。
每逢清晨初次上工，长轩的主事官只须举镜向峰顶望上一眼，将峰顶石壁上全部水闸的初始存水寸尺与当下的天池时符（ResourceVersion）一次性抄录完毕——此谓**“首巡全览”（List）**；
自此之后，峰顶天池的旗手只在水闸发生变动时，方才在崖头挥动双色令旗或拨弄铜反光镜，向山下打出一道极其迅捷的闪烁光语：“第四十二号水闸，增水一寸，时符第千零一号”——此谓**“浮光追变”（Watch）**。山下的长镜只需静止对准峰顶，有变则纳，无变则寂，再无一人需要徒步攀登五千级石阶。

其二，**“落差竹匣，顺叙纳变”（DeltaFIFO）**。
暴风雨夜，峰顶旗语如电光石火般频频闪烁。为了防止山下抄录不及或变动遗失，水镜旁设有一道带倾斜滑轨的“落差竹匣”。
每当天池闪过一道变动指令，抄信童子便迅速在竹简上刻下变动性质（添水、闭闸、改渠、覆核），顺手滑入竹匣顶端。竹简依序层叠滑落，无论山顶变动何等湍急，所有的变易皆在竹匣中按先来后到严密排队，既不跳过一毫，亦不颠倒前后。

其三，**“案前水盘，随变即改”（Indexer 与本地缓存）**。
竹匣底端，由一位司局先生专司拾简。他每取出一片变动竹简，便立刻在长轩大案上的一座缩微青铜“千顷山田水盘”上，将对应的小水闸微调一格。
这座案前水盘按各条山谷（Namespace）与作物种类（Labels）精细分槽，栩栩如生刻画着整座苍山的每一处水流。
从此，三十位巡山吏若想查看自己辖区的水位，根本无需抬头看峰顶，更不必跑断腿——只要走到案前，伸手探一探面前水盘里的对应小槽，清泉寸尺立等可取，耗时不过一瞬！

其四，**“共乘长轩，按铃分派”（SharedInformer 与 Workqueue）**。
最为神妙的是，这面照水铜镜与案前水盘由三十位巡山吏**共享公用（Shared）**。
全山只需天池旗手向这一处长轩打出一道光语，三十处田庄便在水盘上同时获知最新实况。一旦某个辖区的水位需要实地调度，司局先生便顺手在对应巡山吏的竹筒里投下一枚小木签（Workqueue），巡山吏依签出门从容排查，互不干扰，亦无遗漏。

自此，天池大水司门前重归万籁俱寂，而山腰各处梯田万物丰茂、渠水如织。
苍山农家皆赞叹：
“徒步叩关，千夫竭蹶（Direct Polling）；照影流波，一鉴通神（SharedInformer）。
首巡一览定乾坤（List），流光片羽捕细尘（Watch）。
竹匣排云防错乱（DeltaFIFO），盘中清冽知万春（Indexer）。”

— — —

### 这是什么

——到这儿你大概已经认出来了：这正是 Kubernetes 核心大脑与所有自研 Operator/控制器能够以极高吞吐、极低负载稳定运行的幕后功臣：*client-go* 中的核心架构——*SharedInformer 体系（含 Reflector、DeltaFIFO、Indexer 与 Workqueue）*。

在 Kubernetes 集群中，控制器（如 ReplicaSetController、DeploymentController、Kube-Scheduler、Kubelet 以及成百上千个自定义 CRD 运行的 Operator）需要实时掌握各类资源（Pod、Node、Service 等）的最新状态以驱动调谐循环（Reconciliation Loop）。
传统的低效做法正如故事中徒弟们狂奔敲门：如果每个控制器都频繁向 API Server 发起 `GET /api/v1/pods` 轮询，底层的 etcd 数据库和 API Server 内存将在 $O(C \times N)$ 的海量网络请求下当场被打垮；即便使用单纯的 HTTP 长连接 Watch，若没有本地缓存，控制器每次需要数据仍得反复查询远程集群。

*SharedInformer* 架构通过五层环环相扣的高效管道，优雅粉碎了这一瓶颈：

1. **Reflector（铜镜映山 · List 与 Watch 的天作之合）**：
   `Reflector` 是直面 API Server 的守望哨。
   - **List（全量巡览）**：控制器启动时，Reflector 首先向 API Server 发起一次带分页的 List 请求，将指定资源在当前时间点的全量快照拉入本地，并记录返回元数据中的全局版本号——`resourceVersion`；
   - **Watch（浮光追变）**：紧接着，Reflector 借助 HTTP Chunked 长连接向 API Server 发起针对该 `resourceVersion` 的持久 Watch 监听。此后，API Server 仅在数据发生状态跃迁（`ADDED`、`MODIFIED`、`DELETED`）时，向客户端推送微小的增量 JSON 片段。即便长连接中途意外断开，Reflector 也会自动凭借最后记录的 `resourceVersion` 重新发起 Watch 实现无缝续传；若断开过久版本过期（HTTP 410 Gone），则平滑回退至全量 List 重建基线。
2. **DeltaFIFO（落差竹匣 · 增量先进先出队列）**：
   Reflector 接收到来自 Watch 的事件后，并不直接写入业务存储，而是将其封装为 `Delta` 对象（内含操作类型 `Added/Updated/Deleted/Sync` 及对象状态），推入一个有锁保护的特殊队列——`DeltaFIFO`。
   `DeltaFIFO` 保证了同一资源对象的多次连续变更按时间先后严格有序，并在突发高并发事件洪峰（如数千个 Pod 节点同时调度）时起到至关重要的削峰填谷（Buffering）缓冲作用，防止下游消费被瞬间冲垮。
3. **Indexer 与 ThreadSafeStore（案前水盘 · 本地高性能线程安全缓存）**：
   一个独立的后台协程持续从 `DeltaFIFO` 中弹出（Pop）变更项，一方面将对象最新的完整状态写入本地内存存储——`Indexer`（底层为 `ThreadSafeStore`）。
   `Indexer` 拥有强大的多维索引能力，可以依据自定义的索引函数（Indexers，如按命名空间、按 Node 节点、按 Label 标签）在本地内存中建立快速哈希索引。
   **这意味着控制器执行的所有读操作（`lister.List()` 或 `lister.Get()`），100% 全部命中本地内存缓存，耗时仅需数微秒，对 API Server 与 etcd 产生零网络调用与零压力！**
4. **SharedInformer 与 SharedProcessor（共乘长轩 · 共享监听与单点分发）**：
   同一个集群中往往有几十个不同的控制器对同一种资源（如 Pod）感兴趣。如果每个控制器各起一个 Informer，依然会造成重复网络连接。`SharedInformer` 让所有同类监听器共享底层的**单一 Reflector 长连接**与**单一 Indexer 本地缓存**，并通过内部的 `SharedProcessor` 将变更事件广播分发给注册在该 Informer 上的各个业务处理钩子（ResourceEventHandler：`OnAdd`、`OnUpdate`、`OnDelete`）。
5. **Workqueue（竹筒下签 · 解耦与限流重试工作队列）**：
   当 `OnAdd` 或 `OnUpdate` 触发时，事件处理器绝不在回调函数里直接执行耗时的业务逻辑，而是仅仅把发生变化的资源标识符——“命名空间/名称”（如 `default/nginx-pod` 这个 Key）压入一个线程安全的**限流工作队列（RateLimitingQueue）**中。
   调谐协程（Worker）从工作队列中取出 Key，再通过本地 Indexer 查出最新状态进行核对。若调谐失败，Workqueue 自动提供指数退避（Exponential Backoff）重试机制，彻底实现了“事件接收”与“业务调谐”的松耦合与高可用。

### 为什么重要

*SharedInformer* 是 Kubernetes 能够从管理几百个容器扩张到统帅百万容器超大规模集群的命脉所在：

1. **保护 API Server 与 etcd 的终极避雷针**：
   将全部集群只读请求（占据集群整体流量的 90% 以上）全量卸载到本地内存 Indexer 中。数十个控制器在本地肆意遍历千万级对象，API Server 与 etcd 却依然闲庭信步。
2. **事件驱动与状态调谐的完美平衡（Level-Triggered Resilience）**：
   结合 Workqueue 只传递 Key 的设计，无论中途丢失多少次短暂网络连接，控制器调谐时永远依赖本地 Indexer 中的最新完整状态。即使网络抖动或错失了中间某个瞬态事件，最终也能自动收敛至期望终态。
3. **高并发缓存安全与可预测内存模型**：
   通过 `cache.MutationDetector` 和只读克隆保障本地缓存不被业务逻辑篡改，并通过 SharedInformer 避免进程内重复复制对象，最大化节省 Operator 的内存足迹。

_隐喻对应表_

- 峰顶天池大水司与万仞石壁 → Kubernetes 控制平面中心（API Server 与 etcd 状态存储）
- 徒弟提灯屡屡奔山敲门问水位 → 控制器盲目轮询 API Server 引发的 etcd 性能崩溃与网络阻塞
- 听泉水榭架设的八尺照影铜镜 → 客户端负责拉取与监听事件的核心组件（Reflector）
- 晨起举镜一次抄录全量寸尺与时符 → Reflector 启动时的初次全量快照拉取（List）与记录 resourceVersion
- 峰顶挥动令旗与铜反光镜打出闪烁光语 → 依托 HTTP Chunked 长连接的增量事件流推送（Watch）
- 带倾斜滑轨排队的落差竹匣 → 按时序安全缓冲与去重的增量队列（DeltaFIFO）
- 长轩大案上的微缩千顷山田水盘 → 本地线程安全的内存快照与多维索引存储（Indexer / ThreadSafeStore）
- 巡山吏只需探案前水盘知晓全山水位 → 控制器读请求 100% 命中本地 Indexer 内存，零网络开销
- 三十位巡山吏共用一处长轩铜镜与水盘 → 单一资源多控制器共享底层连接与缓存（SharedInformer）
- 司局先生向巡山吏竹筒中投下调粮木签 → 事件回调仅向限流工作队列投入对象键名（Workqueue Key）
</section>

<section class="en" markdown="1">
Mount Cangshan soared into the clouds with ten thousand precipitous cliffs. From its mist-wreathed crags down to the mountain base stretched thousands of acres of terraced paddy fields, interconnected by a sprawling, dizzying labyrinth of stone irrigation canals.

Perched upon the highest summit above the sea of clouds sat the Heavenly Reservoir Office, which governed the primary water gates of the entire mountain. The reservoir masters commanded colossal iron sluices controlling the downstream torrents, recording the active water levels of every weir in cinnabar upon a monolithic stone cliff.
Scattered across the ridges and foothills lived thirty mountain water wardens, each entrusted with a specific agrarian domain: the Rice Terrace Warden, the Tea Grove Warden, the Fish Pond Warden, the Flood Diversion Warden, and many more.

In earlier reigns, the thirty wardens suffered under an agonizing, backward ritual.
Every quarter-hour, each warden dispatched a fleet-footed young apprentice with a paper lantern, sprinted up five thousand sheer stone steps in the burning sun, and hammered frantically on the heavy timber gates of the Heavenly Reservoir, shouting: *"How many inches is Sluice 42 open? Has Sluice 88 begun diverting water?"*
The gatehouse of the Heavenly Reservoir was perpetually choked with thirty panting apprentices shouting at once. The reservoir scribes were driven to madness, their ink repeatedly questioned before it could even dry upon the stone cliff.
Worse still, running up and down five thousand precipitous steps consumed half a day. By the time an exhausted apprentice staggered back to the foothills, mountain squalls had altered the reservoir sluices three times over. Wardens opened irrigation canals based on six-hour-old intelligence, either drowning tender rice seedlings in flash floods or leaving tea terraces parched to dust.

The newly appointed Chief Water Master, Gu, surveyed the mountainside and beheld the misery. He summoned the thirty wardens to the Listening Spring Pavilion at the mountain midpoint, erecting the grand Waterside Gallery and instituting the revolutionary **Statute of the Reflected Bronze Mirrors**:

First, **"Mirror Facing the Peak: The Initial Full Survey and Following the Lights" (Reflector: List and Watch)**.
In the center of the Waterside Gallery, Gu mounted an eight-foot polished bronze convex mirror, aligned directly with the semaphore beacon atop the Heavenly Reservoir.
No apprentice was ever permitted to climb the mountain again.
At the beginning of each day, the gallery scribe adjusted the bronze mirror once, scanning the grand chalk-board atop the peak through a telescope to transcribe the full opening positions of all sluices and the active imperial day-mark (*resourceVersion*)—this was the **Initial Full Survey (List)**.
From that heartbeat onward, the summit signallers waved dual-colored flags and manipulated polished bronze flashers only when a sluice actually moved, flashing a single, instantaneous optical code across the sky: *"Sluice 42, raised one inch, Day-mark 1001"*—this was **Following the Lights (Watch)**. The bronze mirror at the foothills remained perfectly still, reflecting changes when they occurred and resting silent when water flowed undisturbed. Never again did a human soul climb five thousand steps for news.

Second, **"The Flowing Bamboo Chute: Sequencing the Deltas" (DeltaFIFO)**.
On storm-swept nights, summit flash-codes flickered across the sky in furious succession. To prevent downstream scribes from dropping flashes or scrambling sequences, an inclined bamboo chute (*DeltaFIFO*) was suspended beneath the mirror.
Whenever a flash-code beamed down from the peak, a lookout engraved a wooden slip with the nature of the change (Added, Altered, Closed, Verified) and dropped it into the bamboo chute. The slips tumbled down the smooth rail in strict chronological order, guaranteeing that no matter how violently the storm raged, all transitions remained safely queued, never skipping a single delta or scrambling the order of events.

Third, **"The Sandbox Model: Immediate Local Reflection" (Indexer and Local Cache)**.
At the bottom of the bamboo chute sat a dedicated keeper. As each wooden slip tumbled into his basket, he adjusted a small bronze lever on an intricate, water-filled physical sandbox model of all thousand terraces resting on the pavilion table (*Indexer*).
This sandbox model was partitioned into labeled grooves by mountain valley (*namespace*) and crop variety (*labels*), creating an exact, live, microsecond replica of every stream on Mount Cangshan.
Henceforth, whenever the Rice Warden or Lotus Warden needed to verify water levels, he did not look at the summit, nor did he run up the peaks—he walked two steps to the table, dipped his fingers into the sandbox groove, and read the water level instantly. A microsecond local inspection, with zero mountain journeys and zero burden on the summit!

Fourth, **"The Shared Gallery and the Signal Tally" (SharedInformer and Workqueue)**.
Most ingenious of all, this grand bronze mirror and tabletop sandbox were **shared communally (Shared)** among all thirty wardens.
The summit signalman beamed only a single flash-code down the mountain, yet thirty wardens beheld the updated reality in the sandbox simultaneously. When a specific terrace required physical ditch-clearing, the keeper dropped a small numbered wooden token (*Workqueue Key*) into that warden's private wicker tray. The warden collected his token and tended his fields at his own pace, unhurried, independent, and free of panic.

Ever after, silence reigned outside the Heavenly Reservoir on the peaks, while the thousands of acres across the valleys flourished in emerald splendor.
The mountain farmers sang across the terraced heights:
"To storm the peak for tidings was a thousand souls undone (Direct Polling);
To catch the gleaming water-lights is mastery begun (SharedInformer).
The first broad scan reveals the realm (List); the dancing flashes tell the tale (Watch).
The bamboo chute preserves the stream (DeltaFIFO); the sandbox mirrors hill and vale (Indexer)."

— — —

### What it is

By now the architecture is unmistakable: this is the master engine behind the entire Kubernetes control plane and every production-grade Custom Controller and Operator: the *SharedInformer* architecture in *client-go* (encompassing *Reflector*, *DeltaFIFO*, *Indexer*, and the *Workqueue*).

In a Kubernetes cluster, controllers (such as the ReplicaSetController, DeploymentController, Kube-Scheduler, Kubelet, and hundreds of custom CRD Operators) must continuously track the latest desired and actual states of resources (Pods, Nodes, Services) to drive their Reconciliation Loops.
The naive approach—represented by the apprentices sprinting up the mountain—would be polling: if every controller repeatedly issued `GET /api/v1/pods` against the API Server, etcd and the control plane would collapse under $O(C \times N)$ network serialization overhead. Even with long-lived streaming Watches, absent a localized caching layer, controllers would still saturate the network fetching object details.

The *SharedInformer* framework resolves this through a sophisticated five-stage pipeline:

1. **Reflector (The Mirror Facing the Peak · List and Watch)**:
   The `Reflector` is the front-line sentinel communicating with the API Server.
   - **List (Full Survey)**: Upon initialization, the Reflector issues an initial HTTP List request to retrieve a full snapshot of all existing target resources, capturing the snapshot's global sequence marker: the `resourceVersion`.
   - **Watch (Following the Lights)**: Immediately following the List, the Reflector establishes a persistent HTTP Chunked streaming Watch connection anchored to that `resourceVersion`. The API Server sends only lightweight, incremental event envelopes (`ADDED`, `MODIFIED`, `DELETED`) when state transitions occur. If the network drops, the Reflector automatically resumes the Watch from its last observed `resourceVersion`; if the version has expired from etcd history (HTTP 410 Gone), it gracefully falls back to a fresh List to re-establish the baseline.
2. **DeltaFIFO (The Flowing Bamboo Chute · Buffered Sequencing)**:
   Events captured by the Reflector are not shoved directly into business logic. Instead, they are wrapped as `Delta` objects (carrying the action type `Added/Updated/Deleted/Sync` and the object payload) and queued into a thread-safe, buffered queue called `DeltaFIFO`.
   `DeltaFIFO` ensures that rapid, bursting updates to the same resource are delivered in strict chronological sequence, absorbing sudden event avalanches (e.g., thousands of Pods terminating simultaneously) without dropping state or overwhelming downstream consumers.
3. **Indexer and ThreadSafeStore (The Tabletop Sandbox · In-Memory Cache)**:
   A background consumer loop pops deltas from `DeltaFIFO` and updates an in-memory, thread-safe cache called the `Indexer` (backed by `ThreadSafeStore`).
   The `Indexer` computes multi-dimensional indexes using custom indexing functions (e.g., indexing Pods by Namespace, NodeName, or Label selectors).
   **Crucially, all subsequent read queries from controllers (`lister.List()` or `lister.Get()`) are served 100% out of local RAM in microseconds, generating zero network traffic and zero load against the API Server and etcd!**
4. **SharedInformer and SharedProcessor (The Shared Gallery · Shared Watch and Fanout)**:
   Multiple controllers within a process often monitor the same resource type (e.g., both Kube-Scheduler and NodeController watch Nodes). Rather than instantiating separate watch connections and duplicate in-memory caches, a `SharedInformer` multiplexes a **single Reflector connection** and a **single shared Indexer cache**. Its internal `SharedProcessor` fans out change events to all registered handlers (`OnAdd`, `OnUpdate`, `OnDelete`).
5. **Workqueue (The Signal Tally · Decoupling and Rate-Limited Retries)**:
   When an event handler fires, it does not execute complex reconciliation logic inline. Instead, it extracts only the resource's coordinate—the lightweight string Key (`namespace/name`, such as `default/nginx-pod`)—and pushes it into a `RateLimitingQueue` (*Workqueue*).
   Worker goroutines dequeue keys and pull the freshest state from the local Indexer to reconcile. If reconciliation fails, the Workqueue applies exponential backoff retries, elegantly decoupling asynchronous event notification from state convergence.

### Why it matters

*SharedInformer* is the technical foundation that allowed Kubernetes to scale from hundreds of containers to fleets of millions of workloads:

1. **Impervious Shield for API Server and etcd**:
   By absorbing more than 90% of cluster queries into local in-memory Indexers, thousands of Operator loops can query resources endlessly without causing control plane latency degradation or etcd lock starvation.
2. **Level-Triggered State Convergence**:
   By queuing only the resource Key into the Workqueue rather than the full delta payload, the controller's reconciliation worker always inspects the freshest, live state currently sitting in the local Indexer. If network packets or intermediate updates were dropped during transient disconnections, the controller naturally converges on the ultimate desired state regardless.
3. **Memory Footprint Optimization via Shared Caches**:
   Consolidating multiple controller watches into a single SharedInformer eliminates duplicated in-memory object allocations, keeping Kubernetes controllers lean and cache-coherent.

_Metaphor mapping_

- The Heavenly Reservoir on the mountain summit and stone wall → The Kubernetes Control Plane (API Server and etcd state store)
- Apprentices repeatedly climbing five thousand stone steps to ask water levels → Controllers blindly polling the API Server, crippling etcd under linear load
- Eight-foot polished bronze convex mirror in the gallery → The client component watching remote state (Reflector)
- Morning telescope survey transcribing full initial levels and day-mark → Reflector's initial full baseline fetch (`List`) and capturing `resourceVersion`
- Flashing semaphore flags and bronze mirrors signaling only changes → HTTP Chunked streaming deltas (`Watch`)
- Inclined bamboo chute buffering slips in sequence → Thread-safe buffered queue absorbing bursts (`DeltaFIFO`)
- Tabletop miniature sandbox model reflecting all mountain streams → Local in-memory indexed cache (`Indexer` / `ThreadSafeStore`)
- Wardens dipping fingers into sandbox to read levels in an instant → Controllers executing `lister.Get()` / `List()` against local RAM with zero network calls
- Thirty wardens sharing a single bronze mirror and sandbox → Multiple controllers sharing one connection and cache (`SharedInformer`)
- Dropping numbered wooden tokens into the warden's private wicker tray → Event handlers enqueuing lightweight `namespace/name` keys into the Workqueue
</section>
