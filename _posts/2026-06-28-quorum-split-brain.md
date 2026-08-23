---
layout: fable
title: "云顶寺的那口大钟 · The Great Bell of Cloud-Summit Temple"
title_zh: "云顶寺的那口大钟"
title_en: "The Great Bell of Cloud-Summit Temple"
concept: "quorum and split-brain"
tags: [distributed-systems]
illustration: /assets/art/2026-06-28-quorum-split-brain.jpg
---
<section class="zh" markdown="1">
云顶寺建在两座山峰之间，靠一道石桥连着。寺里有五位长老，共同掌管两件大事：敲响那口能传到整个山谷的_大钟_，和打开存着全寺过冬粮食的_粮仓_。

祖上立过一条铁规矩：任何一件大事，必须有_超过半数_的长老当面点头才能做——五个人里，至少三个。三个人按了手印，事就成；只有两个人想做，哪怕再急、再有道理，也只能等。年轻僧人不解：万一那三人糊涂、两人才是对的呢？老方丈只回一句："规矩不保证每次都对，规矩只保证——_同一时刻，绝不会有两个相反的决定同时生效_。"

那年暴雨，石桥被冲断，五位长老正好被劈成两边：山东头三位，山西头两位，谁也过不去，谁也喊不到对面。

西头两位长老急坏了：他们判断要立刻开仓放粮。可数来数去只有两个人，凑不齐三个手印，于是粮仓的门，他们怎么也开不了——只能干等。东头三位呢？三个手印齐了，他们照常敲钟、议事、开仓，一切如旧。

僧人后来才咂摩出其中的精妙：因为"过半数"这条线划在三，_任何两组"过半数"的长老，必定至少有一位是重叠的同一个人_。而同一个人，绝不会在两件相反的事上都按手印。于是无论山被劈成几块，最多只有一块能真正做事——另一块自己就把自己冻住了。假如当初定的是四位长老、两两就能成事，这场大雨就会让两边各开各的仓、各敲各的钟，整个山谷不知道该听谁的。也正因如此，要让寺院能扛住 f 位长老突然失声不语，长老总数就得是 2f+1。

——到这儿你大概已经认出来了：五位长老就是一个 _etcd_ 集群，那条"至少三个手印"的铁规矩就是 _quorum_（法定多数），被暴雨劈断的石桥是一次 _network partition_，而被规矩拦下、自己冻住自己的西头两人，正是它在替你避免的 _split-brain_（脑裂）。

---

_概念：Quorum 与 Split-brain_

分布式系统（如 _etcd_、_Raft_、_ZooKeeper_）里，多个节点必须就"谁是 leader、某条数据写没写"达成一致。做法是：任何决定都要拿到_多数派_（N 个节点里的 ⌊N/2⌋+1）同意才算数。其精妙之处在一个朴素的数学事实——_任意两个多数派必然相交_：既然每个多数派都超过一半，两个加起来就超过 N，按鸽巢原理必有重叠节点；那个重叠节点不会对两个互相冲突的决定都投票，于是同一时刻最多只有一个决定能通过。

这正是为什么集群规模通常取_奇数_（3/5/7）：偶数容易 2-2 平票，既浪费一个节点又增加风险。也是为什么要容忍 f 个节点故障、需要 2f+1 个节点。当网络分区把集群劈成两半，_只有占多数的那一半能继续工作_，少数派那一半会主动停摆——宁可不可用，也不要两个 leader 各写各的、最后数据对不上。这是 CAP 里那个经典取舍：分区发生时，选 Consistency 而非 Availability。

_故事里的隐喻对应：_

- 五位长老 = _etcd / Raft 集群_的成员节点（odd number，5 节点容忍 2 个故障）
- "至少三个手印才能成事" = _quorum_：majority = ⌊N/2⌋+1
- 敲钟 / 开仓 = 集群的_写操作 / leader 选举_等需要达成一致的决定
- 被暴雨冲断的石桥 = _network partition_（节点间通信中断）
- 西头两人想开仓却开不了、自己冻住自己 = 少数派分区_主动失去可用性_，避免 split-brain
- "任何两组过半数必有一人重叠" = _quorum intersection_，保证同一时刻至多一个有效决定
- "若当初定四人、两两成事就会两边各开各的" = 偶数节点 / 无 majority 约束导致 _split-brain_
- "扛住 f 人失声就要 2f+1 人" = 容忍 f 个故障所需的最小集群规模
- 老方丈那句"不保证每次对，只保证不会有两个相反决定" = 一致性协议保证的是 _safety_（不出错），而非_每个决定都最优_
</section>
<section class="en" markdown="1">
Cloud-Summit Temple was built between two peaks, joined by a single stone bridge. Five elders lived there, and together they governed two great things: the ringing of the _great bell_ whose toll reached the whole valley, and the opening of the _granary_ that held the temple's winter grain.

The ancestors had laid down one iron rule: any great act required _more than half_ of the elders to nod in person — at least three of the five. Three handprints, and the deed was done; if only two wished it, no matter how urgent or how right, they could only wait. A young monk objected: what if those three were fools and the two were right? The old abbot answered only, "The rule does not promise we are right every time. The rule promises one thing only — that _at no single moment can two opposing decisions both take effect_."

That year a storm washed out the stone bridge, and the five elders happened to be split: three on the east peak, two on the west. None could cross, none could even call across to the other side.

The two on the west were beside themselves: they judged the granary must be opened at once. But count as they might, there were only two of them — they could not gather three handprints, and so the granary door, try as they might, would not open. They could only wait. And the three on the east? Their three handprints were complete, so they rang the bell, deliberated, and opened the granary as always. Everything went on.

Only later did the monk taste the subtlety of it: because the line of "more than half" is drawn at three, _any two groups that each form a majority must share at least one elder in common_. And that one shared elder will never press his handprint to two opposing deeds. So no matter into how many pieces the mountain is cut, at most one piece can truly act — the other freezes itself. Had the ancestors instead set four elders, with any two able to act, that same storm would have let each side open its own granary and ring its own bell, and the whole valley would not know whom to obey. And for the same reason, for the temple to survive f elders suddenly falling silent, the total number of elders must be 2f+1.

— By now you've probably recognized it: the five elders are an _etcd_ cluster, the iron rule of "at least three handprints" is _quorum_, the storm-broken bridge is a _network partition_, and the two western elders, held back by the rule and freezing themselves, are exactly the _split-brain_ it is sparing you.

---

_Concept: Quorum and Split-brain_

In distributed systems (_etcd_, _Raft_, _ZooKeeper_), several nodes must agree on things like "who is leader" or "was this write committed." The method: every decision needs a _majority_ (⌊N/2⌋+1 of N nodes) to agree before it counts. The beauty rests on one humble mathematical fact — _any two majorities must intersect_: since each majority is more than half, the two added together exceed N, so by the pigeonhole principle they must overlap on some node; that overlapping node will not vote for two conflicting decisions, and so at most one decision can pass at any moment.

This is precisely why cluster sizes are usually _odd_ (3/5/7): even numbers invite 2-2 ties, wasting a node while adding risk. It's also why tolerating f node failures needs 2f+1 nodes. When a partition splits the cluster in two, _only the half holding a majority keeps working_; the minority half deliberately stops — better to be unavailable than to have two leaders each writing their own version, leaving the data irreconcilable. This is the classic CAP trade-off: when a partition strikes, choose Consistency over Availability.

_The metaphor mapping:_

- The five elders = the member nodes of an _etcd / Raft cluster_ (odd number; 5 nodes tolerate 2 failures)
- "At least three handprints to act" = _quorum_: majority = ⌊N/2⌋+1
- Ringing the bell / opening the granary = the cluster's _writes / leader election_ — decisions requiring agreement
- The storm-broken bridge = a _network partition_ (loss of communication between nodes)
- The two westerners wanting to open the granary but unable, freezing themselves = the minority partition _deliberately giving up availability_ to avoid split-brain
- "Any two majorities must share one elder" = _quorum intersection_, guaranteeing at most one valid decision at a time
- "Had it been four, with any two able to act, each side would open its own granary" = even node counts / no majority constraint leading to _split-brain_
- "To survive f silent elders you need 2f+1" = the minimum cluster size to tolerate f failures
- The abbot's line "not right every time, only never two opposing decisions" = a consensus protocol guarantees _safety_ (no contradiction), not that every decision is _optimal_
</section>
