---
layout: fable
title: "六百个渡位 · Six Hundred Seats"
title_zh: "六百个渡位"
title_en: "Six Hundred Seats"
concept: "Kubernetes API Priority and Fairness (APF)"
tags: [kubernetes, performance]
illustration: /assets/art/2026-11-09-api-priority-and-fairness.jpg
youtube_id: "vOuhkGiqYqA"
---
<section class="zh" markdown="1">
青石渡在沉沙河西岸。对岸只有一座埠头，叫独石埠。

独石埠窄。一次只容一船靠，一船靠了要半刻钟卸完，卸完还得由官仓的账房在簿子上记一笔——**记完这一笔，才轮到下一船。** 所以这条河上真正的瓶颈从来不是水，也不是船，是那座独石埠。

渡口司在河里备着船，也备着"位"。一个过河的人占一个位，一担货占一个位，从离岸到在东岸卸完、簿子上记完，位才放开。早年规矩很省事：**四百个位给人，两百个位给货，先到先渡，候渡棚不设上限。**

一千多号人靠这条渡口过日子。更夫老陈每半个时辰要过一趟，替西岸报一声平安；商会要往东岸递行文，选一个"值年"，谁先递到谁当家；七家帮口各有各的货；官府有札子；镇上还有一堆闲着没事、一天要过三趟河看热闹的看客。各行其是，谁也不管谁，倒也太平了十几年。

出事那年秋天，渡口上新开了一家叫**顺风号**的货栈。

顺风号雇了个伙计叫周二，人勤快，就是没听懂东家的话。东家说的是"每半个时辰看一遍货单"，周二听成了"一直看"。他从早到晚往渡口跑，一天跑一万二千趟，每趟都带着一张写满"我要知道东岸仓里现在有多少箱"的货单。

候渡棚原来能站八百人。第三天站了三千，第五天站了三万。

然后独石埠开始堵。

堵不是慢慢堵，是**越堵越慢**。因为每卸一笔货，账房除了记账，还要把这张货单抄一份给所有在东岸蹲着看账的人。蹲着看账的人越多，抄一份要越久；抄得越久，一笔货占埠头的时间越长；占得越久，外海面上挤的船越多；挤得越多，账房被打断的次数越多。

**渡口一刻钟原本过两百担，那年秋天掉到六十担。** 而棚里的人从八百涨到三万。

更夫老陈那天在棚外排了四个时辰，一动没动。他要报的那句话只有两个字：**平安。**

东岸的官仓等不到平安。等不到，就按老规矩判了：西岸失联。青石渡当天从"在册"里被划掉，漕粮停了，官府还预备派人来清点人口、把流民迁走。

老陈后来跟人说，那天他不是被谁挤死的，他是**排在一条不动的队里，慢慢被"等"死的**。

**第一个办法是封顶。**

渡口司下令：水面上同时只许六百人。多的人一律不许下船。

这一条立竿见影。独石埠不再堵成一团，抄账不再被打断，渡口一刻钟又回到了两百担。渡口司松了口气。

第二天，顺风号的伙计占满了六百个位里头的五百八十个。老陈的筏子照旧在棚外，照旧过不去。

**封顶只保证了渡口不倒，它一个字都没说谁该先过。**

**第二个办法，是渡口司最后拿出来的七条。**

**第一条，分等级。** 报平安的一等，商会行文的一等，官府差事的一等，帮口日常的一等，寻常看客一等，剩下说不清的一等。最后那一等份额极小，且**不设候渡道**——来了当场回绝。另外单设一路，叫**钦差**：持钦差火牌的（整个系统的主子），不占位、不排队、直接过。理由很冷，但渡口司写在了告示上：**万一册子本身写坏了，得有人能进去改册子。**

**第二条，立名册。** 每个请求按三样东西分等级——**谁派来的**、**办什么事**、**去哪座仓**。册子上标着序号，从序号最小的开始查，**第一个对上的算数**，后面的不再看。所以序号不是排序，是**优先匹配的次序**：越要紧的规则，序号越小，先给它机会。

**第三条，分份子。** 六百个位不按人头分，按"份"分。报平安那一等份子最大——因为它是整条河的脉搏。份子是比例不是死数：哪天渡口把总位从六百改成一千二，各等按同一个比例一起涨，不用重写册子。

**第四条，借位。** 闲着的等级可以把位借出去，但**最多只借出自己份子的三成**。这个数不是抠门，是为了能收回来：自己的货一到，得立刻把位拿回去，不能等别人卸完。每隔一小会儿重算一次——先把借出去且自己又要用的收回来，再按近来谁最挤，匀一匀。借来的位是**借**，不是**抢**：**已经上了船的人，不赶下船。** 渡口司的原话是："赶人下船，比让人等更贵。"

**第五条，分道。** 每个等级里划一百二十八条候渡道。一个帮口不排所有的道——按自己名字的号，它只落在其中**六条**道上。六条满了，它也占不了另外一百二十二条。这一条是整份告示里最巧的一格：**它不靠讲道理防人挤人，它靠说不上话的号码，让挤的人挤不到别人头上。**

**第六条，公平取号。** 每条道记一个"虚拟完工时刻"。每放一趟船，从**虚拟时刻最落后**的那条道取人，取完把那道往前推一点。于是嗓门大没有用，本钱厚也没有用，只有"这条道走得少"才占先。同一个帮口里的两个伙计，各走各的道，谁也饿不死谁。

**第七条，队满即拒。** 道满了，当场回绝，回绝时附一句"X 刻后再来"。这看着最不客气，其实最慈悲：**一个从不回绝的队，最后一定变成一个拖到超时的队**，而超时的人还占着位、还在等、还不敢重试。

告示贴出去那天，老陈的筏子在第一等里排到了第九位，一炷香没过河。

同一时刻，顺风号的周二被卡在自己的六条道里，进不去，也被回绝得明明白白。他蹲在棚外数了一夜的回绝条子，第二天早晨忽然想通了一件事：**不是渡口不给他过，是他自己一天跑一万二千趟这件事本身，才是那三万人的原因。**

他把趟数改回半个时辰一趟，六条道立刻空了五条。

那年冬天，渡口司在告示旁边又添了一行小字，写得很不客气：

**六百个位不是渡口的上限。六百个位是独石埠的上限。多放船进去，不是多运货，是多堵。**

到这儿，你大概已经认出来了：这就是 **Kubernetes 的 API 优先级与公平性**（*API Priority and Fairness*，*APF*）。

### 这是什么

先说它要挡的东西。

`kube-apiserver` 前面原本只有一把大刀：`--max-requests-inflight`（默认 400）和 `--max-mutating-requests-inflight`（默认 200）。两个数是**全体共用**的：不管来的是 kubelet 的心跳，还是某个控制器里的死循环，都从同一个池子里舀。

于是有了那个经典的正反馈：一个写坏的控制器热循环打满 apiserver → **kubelet 的心跳请求也被挤掉** → 节点被判 `NotReady` → 触发驱逐 → 新 Pod 起来 → 请求更多 → 更挤。apiserver 没倒，它只是**安静地杀掉了一部分节点**。这就是老陈那天报不出去的"平安"。

**APF** 做的事，是把那把大刀换成一整套渡口调度。

**一、两个数加起来，然后按"份"分。** APF 打开后，`--max-requests-inflight` 与 `--max-mutating-requests-inflight` 相加（默认 600），成为全服务器的**总并发**，即座位（*seat*）总数——APF 的官方用词就是"座位"，借的是火车与飞机上"一个乘客占一个座"的意象。它**不再区分读写**；要区分，就自己写两条 FlowSchema 分别匹配动词。

**二、座位不是均匀的。** `list` 请求若预估会返回大量对象，会一次占**多个**座位，数量与预估对象数成正比——这是周二那张"我要知道东岸现在有多少箱"的货单。写请求在写完**之后**还要多占一段时间、多占几个座位，因为它要给所有 `watch` 流扇出通知，扇出的量也参与估算。`watch` 请求只在**首轮存量对象的通知发完**之前占座位，之后蹲着就不占了。而 `exec`、日志 `tail` 这类长连接请求**根本不进 APF**——它们不占位、不排队。

**三、等级（PriorityLevelConfiguration）。** 每个等级有自己的并发上限。默认配置给出几个：`node-high`（节点健康上报）、`system`（`system:nodes` 组的非健康请求，即 kubelet）、`leader-election`（内置控制器抢 `leases`/`endpoints`）、`workload-high`（内置控制器的其他请求）、`workload-low`（任何其他 ServiceAccount，通常是 Pod 里的控制器）、`global-default`（其余，比如人敲的 `kubectl`）。另外两个是强制的：`catch-all`（份额极小、**不排队**）、`exempt`（完全不受限，`system:masters` 组默认落在这里）。

`leader-election` 单独一级不是讲究：选主失败会让控制器重启，重启的控制器回来要全量 sync informer，那笔流量比选主本身贵几个数量级。

**四、借与还。** 等级的上限写成 `nominalConcurrencyShares`（份子），不是绝对座位数，所以调总并发时所有等级按同一比例涨落。每秒重算一次动态上限：先收回自己借出去且当下又需要的座位，再按近来的需求公平分配。上下界由 `lendingPercent` / `borrowingPercent` 卡住——一边最多借出份子的百分比，另一边最多借入的百分比。**借不是抢占**：已经开始的请求不会被取消。在 apiserver 里抢占意味着半提交的写，加上客户端重试，比让人排队糟糕得多。

**五、排队、抓阄、公平。** 等级内部分成若干队列（`queues`），请求按**流**（*flow*）分配：流 = 命中的 FlowSchema 名 + 一个**区分符**（*distinguisher*，可以是请求的用户、或目标资源的 namespace，或者不分）。分配用 **shuffle sharding（洗牌分片）**：每个流从 `queues` 条队列里哈希出 `handSize` 条，再从这 `handSize` 条里挑最短的那条排。队列之间用**公平排队**（*fair queuing*）派发——按虚拟完工时刻取最落后的那条。

两个数字的取舍很具体：`handSize` 大，两个流撞在一起的概率低（谁也别想饿死谁），但少数大流量更容易吃满整个服务器；**单个流最多能排 `handSize × queueLengthLimit` 个请求**。`queues` 大，碰撞率低，内存高；`queues = 1` 等于关掉公平逻辑，只剩排队。

**六、多出来的请求怎么办。** 由等级的 `limitResponse` 决定：`Reject` 直接回 429；`Queue` 则排队，排不下或等太久，同样以 429 结束，并带 `Retry-After`。`apiserver_flowcontrol_rejected_requests_total` 的 `reason` 会把原因分开写清：`queue-full`、`concurrency-limit`、`time-out`、`cancelled`。

### 为什么重要

**一、并发不是吞吐。** 独石埠是串行的：etcd 的写要过 leader 逐条提交。超过某个拐点后，更多在途请求不会提高完成率，只会抬高每次提交的延迟，然后完成率**下降**——这是排队网络里的拥塞崩溃，不是过载的比喻，是它的定义。用 *Little's law* 说更直白：`L = λW`。位子数 `L` 是定的，你硬把 `λ` 提上去，`W` 就按比例涨；而 `W` 会反过来吃掉 `λ`。APF 的核心动作因此是**限额加排队**，不是加并发。这是它和"把 `--max-requests-inflight` 调大"最根本的分歧。

**二、一个坏客户端能拖垮一整个集群，而那件事看起来像别的问题。** 热循环的控制器打满 apiserver，症状不出现 apiserver 上，出现在**节点的 `NotReady`** 上、在驱逐上、在 Pod 起不来上。运维看到的是节点抖动，根因是某个 namespace 里一个写了 `for {}` 的 reconcile。这是 Kubernetes 事故里最常见的一类误导。APF 把这件事变得**可归因**：`apiserver_flowcontrol_*` 指标按 `flow_schema` 与 `priority_level` 打标签，谁在打满服务器，一眼看得见——前提是流的区分符选对了（`ByUser` 而不是不分）。文档里有一句毫不含糊的提醒：**控制器实例多，就该用不同的用户名认证**，否则它们全被算成一个流，公平排队对它们失效。

**三、默认配置是可改的，而且大概率该改。** 官方给的是"合理"的一组，不是"适合你"的一组。三个具体的坑：默认配置**不**给 kubelet 的本地健康检查（`/healthz`、`/livez`、`/readyz`，走安全端口但无凭据）任何优待，它们落在 `global-default` 里被人挤掉；`catch-all` 不排队，一律 429；而 `--max-requests-inflight` 在 APF 下的含义已经变了——它不再是天花板，是分母。

**四、它是一个"分层失败"的设计，而不是"不失败"的设计。** 五百八十个位被一个帮口占满时，渡口没有倒，它只是让老陈过不去；`exempt` 存在的意义就是——万一告示写坏了，还有人能进去改告示。承认自己会被配错，然后为配错留一条活路，比假装配不错更可靠。

_隐喻对应表_

- 沉沙河对岸只有一座独石埠，一船一卸，记完一笔才轮到下一船 → etcd 的写由 leader 串行提交，是 apiserver 真正的瓶颈
- 一个位 = 一个从离岸到记完账的名额 → 一个 *seat* = 一个在途请求占用的一个并发单位
- 早年四百个位给人、两百个位给货，先到先渡，棚不设上限 → `--max-requests-inflight` 400 与 `--max-mutating-requests-inflight` 200，全局共享，无排队
- 顺风号的周二把"每半个时辰看一遍"听成"一直看"，一天一万二千趟 → 控制器热循环（tight reconcile / list-watch loop）打满 apiserver
- 每卸一笔要把货单抄给所有蹲守看账的人，人越多抄得越慢 → 写请求要为 `watch` 扇出通知，扇出量与在途 watcher 数成正比，也计入座位与占位时长
- 蹲守看账的人只在头一遍抄旧账时占位，之后蹲着不占 → `watch` 请求在首轮存量对象通知发完后即释放座位
- 沿河拉纤、盯着人干活这类长差事不占位不排队 → `exec`、日志 `tail` 等长连接请求不经过 APF
- 一张"东岸现在有多少箱"的货单一次要占很多位 → `list` 请求按预估对象数占用多个座位
- 越堵越慢，渡口从一刻二百担掉到六十担 → 拥塞崩溃：超过拐点后并发上升、吞吐下降（`L = λW`）
- 老陈四个时辰报不出"平安"，西岸被判失联、停漕粮 → kubelet 心跳被挤掉 → 节点 `NotReady` → 驱逐 → 更多请求的正反馈
- 封顶六百人：渡口不倒，但老陈照旧过不去 → 单一全局 max-inflight 保护了服务器，却保护不了任何具体请求
- 报平安 / 商会行文 / 官府差事 / 帮口日常 / 看客 / 说不清的 → `node-high`/`system`、`leader-election`、`workload-high`、`workload-low`、`global-default`、`catch-all`
- 最后那一等份额极小且不设候渡道，来了当场回绝 → `catch-all` 份额极小且不排队，直接 429
- 持钦差火牌者不占位不排队，理由"万一册子写坏了得有人能改册子" → `exempt` 等级，`system:masters` 默认在内，防止错误的流控配置把 apiserver 彻底锁死
- 名册按"谁派来的、办什么事、去哪座仓"分等级 → FlowSchema 的 `subjects`、`resourceRules`/`nonResourceRules`、`namespaces`/`nonResourceURLs`
- 册子标序号，从序号最小的查起，第一个对上的算数 → `matchingPrecedence` 数值最小者优先匹配，首个命中生效
- 六百个位不按人头分，按份分；份是比例不是死数 → `nominalConcurrencyShares` 按比例分配总并发，总并发变化时同比例涨落
- 报平安那一等份子最大 → `system`（kubelet）在默认配置里拿最大份之一，因为它是集群脉搏
- 闲着的等级可以借位，但最多借出三成，为的是能马上收回来 → 借出/借入按份子百分比设界，动态上限每秒重算，先收回再匀
- 已经上船的人不赶下船，赶人比让人等更贵 → 不做抢占；取消在途写会留下半提交状态，比排队更糟
- 一百二十八条候渡道，一个帮口按名字的号只落在其中六条 → `queues` 与 `handSize` 的 shuffle sharding，把单个流的破坏面钉在 `handSize` 条队列内
- 六条道满了也占不了另外一百二十二条 → 一个流最多排队 `handSize × queueLengthLimit` 个请求
- 每趟从虚拟完工时刻最落后的那条道取人 → 公平排队（fair queuing）按虚拟时间派发，声量与本钱都不占先
- 同帮口的两个伙计各走各的道，谁也饿不死谁 → 流的区分符（`ByUser` / `ByNamespace`）决定公平的粒度
- 队满了当场回绝，附一句"X 刻后再来" → 429 加 `Retry-After`；`rejected_requests_total` 的 `reason` 分 `queue-full`/`concurrency-limit`/`time-out`/`cancelled`
- 从不回绝的队最后变成拖到超时的队，而超时的人还占着位 → 拒绝优于堆积：排队超时既占座位又触发客户端重试风暴
- 周二被卡在自己的六条道里，被回绝得明明白白，第二天把趟数改回来 → 429 + `Retry-After` 让客户端能自愈；指标让责任可归因
- 告示旁的小字：六百个位是独石埠的上限，不是渡口的上限 → 总并发是瓶颈定的，不是愿望定的；调大 max-inflight 只是把拥塞挪个位置
</section>
<section class="en" markdown="1">
Bluestone Ford sits on the west bank of the Sinking Sands River. Across the water there is exactly one landing, called the Single Stone Quay.

It is narrow. One boat at a time. A boat takes a quarter hour to unload, and then the granary's clerk has to enter the shipment in the ledger — **the entry must be finished before the next boat ties up.** So the true bottleneck on this river was never the water and never the boats. It was that one quay.

The ford office kept boats in the water, and it kept **seats**. A person crossing takes a seat; a load of goods takes a seat; the seat is released when the goods are unloaded on the east bank and the ledger entry is made. The old rule was simple: **four hundred seats for people, two hundred for goods, first come first served, and the waiting shed has no limit.**

Over a thousand people lived off that ford. The night watchman, Old Chen, crossed every half hour to report that the west bank was well; the merchants' guild sent its documents east to elect a factor for the year, whoever arrived first holding the post; seven trade houses moved their goods; the magistrate sent pouches; and a good number of idle townsfolk crossed three times a day just to look. Each went about its own business, nobody governed anybody, and for a dozen years it held.

The autumn it broke, a freight house called **Fairwind** opened at the ford.

Fairwind hired a hand named Zhou the Second — industrious, but he had not understood his employer. The instruction was "look at the manifest once every half hour." Zhou heard "keep looking." He ran to the ford from dawn to dark, twelve thousand trips a day, each carrying a slip reading: *I need to know how many crates are in the east granary right now.*

The waiting shed held eight hundred. On the third day it held three thousand. On the fifth, thirty thousand.

Then the Single Stone Quay began to jam.

Not gradually. **The more it jammed, the slower it got.** Every time a shipment was unloaded, the clerk not only entered it — he had to copy the slip for every person squatting on the east bank watching the ledger. The more watchers, the longer each copy took; the longer the copy, the longer each boat held the quay; the longer the quay was held, the more boats pressed in; the more boats pressed in, the more often the clerk was interrupted.

**The ford had moved two hundred loads a quarter hour. That autumn it moved sixty.** And the shed went from eight hundred to thirty thousand.

Old Chen stood outside that shed for four hours without moving. The message he was carrying was two characters long: **all is well.**

The east granary never received it. Never receiving it, it applied the old rule: the west bank is out of contact. Bluestone Ford was struck off the register that same day, the grain shipments stopped, and the magistrate began preparing to send men to count the population and relocate the refugees.

Old Chen later told people he had not been crushed by anyone. He had **stood in a queue that did not move, and been slowly waited to death.**

**The first remedy was a cap.**

The ford office ordered: never more than six hundred people on the water at once. Nobody else may board.

It worked immediately. The quay stopped balling up, the copying stopped being interrupted, and the ford returned to two hundred loads a quarter hour. The office breathed out.

The next day, Fairwind's hands occupied five hundred and eighty of the six hundred seats. Old Chen's raft sat outside the shed, exactly as before, unable to cross.

**A cap guarantees the ford will not collapse. It says not one word about who should go first.**

**The second remedy was seven articles, and it was the last one the ford office produced.**

**One: rank them.** A grade for the peace report, a grade for guild documents, a grade for magistrate business, a grade for the houses' daily goods, a grade for onlookers, and a grade for everything unclassifiable. That last grade has a very small share and **no waiting lanes at all** — it is refused on the spot. And one more lane is set apart, called **by imperial warrant**: whoever holds the warrant — the master of the whole system — takes no seat, joins no queue, and crosses at once. The reason is cold, and the office wrote it on the notice: **if the register itself is ever written wrong, somebody has to be able to get in and fix the register.**

**Two: keep a register.** Every request is graded on three things — **who sent it**, **what business it is**, **which granary it targets**. The register is numbered, and it is read from the lowest number upward, **first match wins**, nothing after it is consulted. The number is therefore not a ranking but an **order of precedence**: the rule that matters most gets the smallest number, and first claim.

**Three: allocate by share.** The six hundred seats are not divided per head but per **share**. The peace-report grade holds the largest share, because it is the pulse of the whole river. A share is a proportion, not a number: raise the ford from six hundred seats to twelve hundred and every grade grows by the same fraction, with no rewriting of the register.

**Four: lend seats.** An idle grade may lend its seats out, but **never more than three tenths of its own share.** That number is not stinginess; it is what makes recall possible — when its own goods arrive it must take the seats back at once, not wait for someone else's unload to finish. The office recomputes every little while: first reclaim what it lent and now needs, then even out the rest by who has been crowded lately. A borrowed seat is **borrowed**, not **seized**: **nobody already aboard is thrown off.** The office's wording: "Throwing a man off the boat costs more than making him wait."

**Five: split the lanes.** Inside each grade, a hundred and twenty-eight waiting lanes are marked out. A trade house does not queue in all of them — by the number of its own name it lands in just **six**. When those six are full it still cannot take a single one of the other hundred and twenty-two. This is the cleverest cell in the whole notice: **it does not argue people out of crowding each other. It uses a number nobody can talk to, so that a crowd cannot crowd anyone else.**

**Six: call the numbers fairly.** Each lane keeps a *virtual completion time*. Each departure is taken from the lane **furthest behind** on that clock, and then that lane's clock is pushed forward. Loudness buys nothing. Capital buys nothing. Only "this lane has moved less" gets you ahead. Two hands of the same house walk their own lanes, and neither can starve the other.

**Seven: refuse a full lane at once.** When a lane is full, refuse on the spot, and refuse with a note: *come back in so many minutes.* This looks like the rudest article and is in fact the kindest: **a queue that never refuses always becomes a queue that times out** — and the ones timing out are still holding seats, still waiting, and afraid to retry.

The day the notice went up, Old Chen's raft stood ninth in the first grade and was across inside the time it takes to burn a stick of incense.

At that same hour, Zhou the Second was stuck in his own six lanes, refused with perfect clarity. He squatted outside the shed all night counting refusal slips, and by morning something had landed: **the ford had not been refusing to let him cross. His twelve thousand trips a day were themselves the reason for the thirty thousand people.**

He put the trips back to one every half hour. Five of his six lanes emptied immediately.

That winter the ford office added a line beside the notice, and it was not polite:

**Six hundred seats are not the ford's limit. Six hundred seats are the quay's limit. Putting more boats on the water does not move more goods. It jams more.**

By now you have probably recognized it: this is Kubernetes' **API Priority and Fairness** (*APF*).

### What it is

Start with what it has to hold back.

In front of `kube-apiserver` there used to be a single blunt instrument: `--max-requests-inflight` (default 400) and `--max-mutating-requests-inflight` (default 200). Both numbers are **shared by everyone**: whether the request is a kubelet heartbeat or a hot loop inside some controller, it scoops from the same pool.

Hence the classic positive feedback: a broken controller hot-loops the apiserver → **the kubelet's heartbeat is squeezed out too** → the node is judged `NotReady` → eviction fires → new pods start → more requests → more pressure. The apiserver does not go down. It **quietly kills some of your nodes.** That is Old Chen's undeliverable peace report.

**APF** replaces the blunt instrument with an entire ferry schedule.

**One: add the two numbers, then divide by share.** With APF enabled, the two flags are summed (600 by default) into the server's total concurrency — its total number of seats (*seat* is literally the APF term, borrowed from a passenger taking one of a fixed supply of seats on a train). It **no longer distinguishes reads from writes**; to distinguish them, write two FlowSchemas matching different verbs.

**Two: seats are not uniform.** A `list` request estimated to return many objects occupies **several** seats, proportional to the estimated object count — that is Zhou's slip asking how many crates are in the granary. A write request keeps occupying seats, and more of them, for some time **after** the write completes, because it must fan notifications out to every `watch` stream, and the volume of that fan-out participates in the estimate. A `watch` request occupies its seat only until **the initial burst of notifications for pre-existing objects** is delivered, then sits without a seat. And long-running requests — `exec`, log `tail` — **never enter APF at all**: no seat, no queue.

**Three: priority levels (`PriorityLevelConfiguration`).** Each level has its own concurrency ceiling. The default configuration ships several: `node-high` (node health updates), `system` (non-health requests from the `system:nodes` group — kubelets), `leader-election` (built-in controllers competing for `leases`/`endpoints`), `workload-high` (other built-in controller requests), `workload-low` (any other ServiceAccount, typically controllers inside Pods), `global-default` (everything else, such as a human typing `kubectl`). Two more are mandatory: `catch-all` (a very small share, and it **does not queue**) and `exempt` (entirely unlimited; the `system:masters` group lands here by default).

Giving `leader-election` its own level is not fussiness: a failed election restarts the controller, and a restarted controller comes back and re-syncs its informers, which costs orders of magnitude more traffic than the election itself.

**Four: lending and reclaiming.** A level's ceiling is written as `nominalConcurrencyShares`, not as an absolute seat count, so raising total concurrency raises every level by the same proportion. Dynamic limits are recomputed periodically: each level first reclaims seats it lent and now needs, then the rest is apportioned by recent demand. Bounds are set by lending and borrowing percentages of the nominal limit. **Borrowing is not preemption**: a request already executing is never cancelled. In an apiserver, preemption means half-committed writes plus client retries, which is far worse than a wait.

**Five: queueing, shuffling, fairness.** Within a level, requests are distributed across queues (`queues`) by **flow** — flow = the matching FlowSchema's name plus a **distinguisher** (the requesting user, the target resource's namespace, or nothing). Distribution uses **shuffle sharding**: each flow hashes out `handSize` queues from the `queues` available and joins the shortest of those. Between queues, dispatch is **fair queuing** — take the queue furthest behind in virtual finish time.

The two numbers trade off concretely: a larger `handSize` makes two flows less likely to collide (so neither can starve the other) but makes it likelier that a few flows can dominate the server; **a single flow can queue at most `handSize × queueLengthLimit` requests**. A larger `queues` lowers collision rate at the cost of memory; `queues = 1` effectively disables fair queuing and leaves only queueing.

**Six: what happens to the excess.** The level's `limitResponse` decides: `Reject` returns 429 immediately; `Queue` queues, and if the queue is full or the wait too long, also ends in 429 with `Retry-After`. The `reason` label on `apiserver_flowcontrol_rejected_requests_total` separates the causes: `queue-full`, `concurrency-limit`, `time-out`, `cancelled`.

### Why it matters

**One, concurrency is not throughput.** The quay is serial: an etcd write goes through the leader one commit at a time. Past a knee, more in-flight requests do not raise the completion rate; they raise per-commit latency, and then the completion rate **falls**. That is congestion collapse in a queueing network — not a metaphor for overload, its definition. *Little's law* puts it plainly: `L = λW`. The seat count `L` is fixed; force `λ` up and `W` rises proportionally, and `W` then eats `λ`. APF's core move is therefore **limit plus queue**, not more concurrency. That is its fundamental disagreement with "just raise `--max-requests-inflight`."

**Two, one bad client can take down a whole cluster, and it will look like something else.** A hot-looping controller saturates the apiserver, and the symptom appears not on the apiserver but on **nodes going `NotReady`**, on evictions, on pods failing to start. Operations sees node flapping; the root cause is a `for {}` in somebody's reconcile loop. This is among the most common misdirections in Kubernetes incidents. APF makes it **attributable**: `apiserver_flowcontrol_*` metrics carry `flow_schema` and `priority_level` labels, so who is saturating the server is visible at a glance — provided the distinguisher is chosen well (`ByUser`, not nothing). The documentation is blunt about it: **controllers with many instances should authenticate with distinct usernames**, or they are all counted as one flow and fair queuing does nothing for them.

**Three, the default configuration is editable, and probably should be edited.** What ships is a *reasonable* set, not a set that fits you. Three specific traps: the defaults give kubelet local health checks (`/healthz`, `/livez`, `/readyz`, on the secure port but unauthenticated) no special treatment, so they land in `global-default` and get crowded out; `catch-all` does not queue, so it is 429 by default; and under APF `--max-requests-inflight` no longer means a ceiling — it means a numerator.

**Four, it is designed to fail in layers, not to not fail.** When one trade house held five hundred and eighty of six hundred seats, the ford did not collapse — it merely stopped Old Chen. `exempt` exists for exactly one reason: if the notice itself is written wrong, somebody must still be able to get in and rewrite it. Admitting you can be misconfigured, and leaving a way through for the misconfiguration, is more reliable than pretending you cannot be.

_Metaphor mapping_

- One quay on the far bank, one boat at a time, ledger entry before the next boat → etcd writes are committed serially by the leader; that is the apiserver's real bottleneck
- A seat = one place from cast-off to ledger entry → a *seat* = one unit of concurrency held by one in-flight request
- The old rule: four hundred seats for people, two hundred for goods, first come first served, no shed limit → `--max-requests-inflight` 400 and `--max-mutating-requests-inflight` 200, globally shared, no queueing
- Zhou hears "once every half hour" as "keep looking", twelve thousand trips a day → a hot-looping controller (tight reconcile / list-watch loop) saturating the apiserver
- Every unload copies the slip for every watcher, so more watchers means slower unloads → write requests pay seat-time for `watch` fan-out, and fan-out volume feeds the seat estimate
- Watchers hold a seat only while the old ledger is copied the first time → `watch` releases its seat once the initial object burst is delivered
- Towing, watching a man work: no seat, no queue → `exec` and log `tail` bypass APF entirely
- One slip asking how many crates there are occupies many seats → `list` requests occupy seats proportional to estimated object count
- The more it jams the slower it gets; 200 loads a quarter hour becomes 60 → congestion collapse: past the knee, concurrency up and throughput down (`L = λW`)
- Old Chen cannot report for four hours; the bank is judged lost and grain stops → kubelet heartbeat starved → node `NotReady` → eviction → more requests
- A cap of six hundred: the ford stands, Old Chen still cannot cross → a single global max-inflight protects the server and no particular request
- Peace report / guild documents / magistrate business / daily goods / onlookers / unclassifiable → `node-high`/`system`, `leader-election`, `workload-high`, `workload-low`, `global-default`, `catch-all`
- The last grade holds a tiny share and gets no lanes, refused on the spot → `catch-all` has a very small share and does not queue, returning 429
- The imperial warrant: no seat, no queue, because someone must be able to fix a broken register → the `exempt` level, `system:masters` included, so a bad flow-control config cannot lock the apiserver out
- The register grades by who sent it, what business, which granary → FlowSchema `subjects`, `resourceRules`/`nonResourceRules`, `namespaces`/`nonResourceURLs`
- Numbered register, read from the lowest number, first match wins → `matchingPrecedence`: lowest number first, first match applies
- Six hundred seats divided by share, shares being proportions not counts → `nominalConcurrencyShares` apportion total concurrency and scale with it
- The peace-report grade holds the largest share → `system` (kubelets) gets one of the largest defaults, because it is the cluster's pulse
- Idle grades lend but never more than three tenths, so they can take seats back at once → lending and borrowing bounds as percentages of the nominal limit, recomputed periodically, reclaim before reallocate
- Nobody already aboard is thrown off; throwing a man off costs more than a wait → no preemption: cancelling an in-flight write leaves half-committed state, worse than queueing
- One hundred and twenty-eight lanes; a house lands in six by the number of its name → `queues` and `handSize` shuffle sharding, bounding one flow's blast radius to `handSize` queues
- Six lanes full still leaves the other hundred and twenty-two untouched → one flow queues at most `handSize × queueLengthLimit` requests
- Each departure taken from the lane furthest behind on virtual time → fair queuing dispatches by virtual finish time; neither loudness nor capital gets ahead
- Two hands of one house walk separate lanes and neither starves → the distinguisher (`ByUser` / `ByNamespace`) sets the granularity of fairness
- A full lane is refused with "come back in so many minutes" → 429 with `Retry-After`; `rejected_requests_total` splits `queue-full`/`concurrency-limit`/`time-out`/`cancelled`
- A queue that never refuses becomes a queue that times out, and the timed-out still hold seats → refusal beats pile-up: queue timeouts occupy seats and trigger retry storms
- Zhou stuck in his own six lanes, refused clearly, halves his trips by morning → 429 plus `Retry-After` lets clients self-correct; metrics make blame attributable
- The line beside the notice: six hundred seats are the quay's limit, not the ford's → total concurrency is set by the bottleneck, not by ambition; raising max-inflight only relocates the jam
</section>
