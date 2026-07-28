---
layout: fable
title: "河口的渡船与那位挑船的老船头 · The River Ferries and the Dockmaster Who Packs Them Tight"
title_zh: "河口的渡船与那位挑船的老船头"
title_en: "The River Ferries and the Dockmaster Who Packs Them Tight"
concept: "bin-packing-scheduling"
tags: [kubernetes, scheduling]
illustration: /assets/art/2026-07-28-bin-packing-scheduling.jpg
---
<section class="zh" markdown="1">
清早的河口,雾还没散,码头上已经排起了大大小小的货车。有的车小巧,装一筐果子;有的车沉重,拉着整座石磨。河对岸是集市,货得靠渡船一趟趟运过去。

管调度的是位老船头。他不划船,只站在栈桥上,手里一块木牌,把一辆辆货车分派到停在岸边的几条渡船上。每条渡船吃水有限,能载的重量是死的——多压一分就要沉。船头心里门儿清:每条船此刻还剩多少_余量_,他都记着。

一辆装石磨的重车上来了。船头先不急着分,他把眼下几条船挨个过一遍:这条船已经压了大半,剩下的空当塞不下石磨,_不合适_,划掉;那条空船余量足够,行;还有一条虽有点空,但船身漏水今早不出航,也划掉。一圈筛下来,剩几条_能装得下_的船,才进入他真正犯难的那一步。

难就难在:能装的船不止一条,到底放哪条?换个新船头也许会图省事,把重车放到那条空荡荡的大船上,让每条船都松松垮垮、各装几件——看着倒也均匀。可这位老船头偏不。他专挑那条_已经装得快满_的船,把石磨往仅剩的空当里一压,严丝合缝。他给每条候选船在心里_打了个分_:越是能被这一车填满、填得越紧的船,分越高;他就把车交给分最高的那条。

旁人不解:好好的空船不用,干嘛非往满船里挤?船头呵呵一笑。你想啊——把零零碎碎的货都往几条船上压实了,腾出来的整条空船就一直空着;等晌午那辆拉着巨木、非得独占一船的大车来了,现成就有整船等着它。要是一早图均匀,把货摊得每条船都半满,巨木来时反倒没有一条容得下,只能干等。更别说:空着的船可以早早收工靠岸、不必白烧一船的柴火。当然,压得太满也有代价——万一半路又冒出急件,船上竟连一寸富余都不剩了。

至于那辆石磨若是_太重_,重到眼下没有任何一条船的余量塞得下,船头也不硬来。他让它在栈桥上_候着_,等哪条船卸空返航、腾出地方,再把它请上去。而车一旦定了船,他便当场在木牌上_记一笔、占住那个位置_,免得下一辆车也盯上同一处空当,两下相争。

——到这儿你大概已经认出来了:这位专把货往快满的船上压实、还要给每条船打分的老船头,就是 kube-_scheduler_;先划掉装不下的船那一步是 _filter_(预选),再给剩下的船打分挑最紧的那一步是 _score_(优选),而他偏爱压满而非摊匀的脾气,正是 _bin-packing_(`MostAllocated` 打分策略)。

**这是什么、为什么重要。** kube-scheduler 给每个待调度的 Pod 选节点,分两步走。先是 _filtering_:逐个节点检查可行性——余下的 CPU / memory 够不够 Pod 的 `requests`、`nodeSelector` 与 `taint/toleration` 是否匹配、亲和性约束是否满足——把装不下的节点直接淘汰。再是 _scoring_:给通过筛选的节点各打一个分,取最高分落地。打分策略决定"性格":_bin-packing_(`MostAllocated` / 早期的 `RequestedToCapacityRatio`)偏爱把 Pod 塞进已经用得较满的节点,好处是把碎片资源压实、腾出整台空节点,便于 cluster-autoscaler _缩容_省钱,也给未来的大 Pod 留出完整空间;反面是 `LeastAllocated`(默认更倾向均摊),把负载摊平,抗突发、抗单点故障更好,但更浪费、更难缩容。装不下任何节点的 Pod 会停在 `Pending`(_unschedulable_),等资源释放或 autoscaler 扩容。选定后 scheduler 会_绑定_(binding)并在缓存里占住该节点的资源,避免并发调度把同一份余量派给两个 Pod。说到底,这是一道装箱优化题(bin-packing 本身 NP-hard),scheduler 用打分做贪心近似,在"省钱压实"与"留白抗险"之间替你权衡。

**隐喻对应表:**

- 大大小小的货车:一个个待调度的 _Pod_,各有不同的 CPU / memory `requests`
- 岸边几条渡船:集群里的一个个 _Node_,各有固定的 allocatable 容量
- 站栈桥、发木牌分车的老船头:kube-_scheduler_(不亲自运货,只做分派决策)
- 每条船还剩多少余量,船头都记着:节点的 allocatable 减去已分配 requests 后的_剩余可用量_
- 先把装不下、今早不出航的船挨个划掉:_filtering / predicates_(可行性预选)
- 给剩下能装的船各打一个分:_scoring_(优选打分)
- 专挑快满的船、往空当里压实:_bin-packing_ 打分策略(`MostAllocated`,偏好高利用率节点)
- 新船头图省事把货摊得每条船半满:`LeastAllocated`(均摊/spread),抗突发但更难缩容
- 压实腾出的整条空船,留给晌午的巨木:压紧碎片、腾出整节点供 autoscaler _缩容_或容纳大 Pod
- 太重、没船装得下、只能在栈桥候着的石磨:资源不足而卡在 `Pending` 的 _unschedulable_ Pod
- 定了船就当场记一笔、占住位置:_binding_ 与缓存占位,防并发调度重复派发同一份余量
</section>
<section class="en" markdown="1">
Early morning at the river mouth, the fog not yet lifted, and the dock is already lined with carts of every size. Some are small, carrying a single basket of fruit; some are heavy, hauling a whole millstone. The market is across the water, and the goods must go over by ferry, trip after trip.

The one who assigns them is an old dockmaster. He rows nothing himself; he just stands on the pier with a wooden tally in hand and sends each cart onto one of the ferries moored along the bank. Every ferry has its limit — the weight it can carry is fixed, and one pound too many will sink it. The dockmaster keeps a clear ledger in his head of exactly how much _room_ each ferry has left at this moment.

A heavy cart bearing a millstone rolls up. He doesn't assign it in a hurry. First he runs down the ferries one by one: this one is already loaded past half, and the space left won't take a millstone — _no good_, cross it off; that empty one has room to spare — fine; and that third one has space too, but its hull is leaking and it isn't sailing this morning — cross it off. After the sweep, only the ferries that _can actually fit the cart_ remain, and only then does he face the step that truly takes judgment.

The hard part is this: more than one ferry can fit it, so which gets it? A newer dockmaster might take the easy path and drop the heavy cart onto that big empty ferry, letting every boat ride loose and half-loaded — evenly spread, and pleasant enough to look at. But not this old hand. He deliberately picks the ferry that is _already nearly full_ and wedges the millstone into the last remaining gap, snug and tight. In his head he has given each candidate ferry a _score_: the more fully this one cart fills a ferry, the tighter the fit, the higher its score — and he hands the cart to the highest scorer.

Onlookers are puzzled: a perfectly good empty boat sits there, so why cram it into a full one? The dockmaster chuckles. Think it through — pack all the odds and ends tightly onto a few ferries, and a whole empty ferry stays free. When the midday cart comes hauling a giant timber that must have a boat all to itself, there's a whole empty one waiting, ready. Spread everything evenly this morning and leave every ferry half-full, and when the timber arrives not one boat can take it — it just waits. Besides, an empty ferry can tie up and knock off early, no need to burn a boatload of firewood for nothing. Of course, packing too tight has its price too — if an urgent parcel suddenly appears mid-morning, there may not be an inch of slack left aboard.

And if the millstone is simply _too heavy_ — too heavy for any ferry's remaining room right now — the dockmaster doesn't force it. He lets it _wait_ on the pier until some boat unloads and returns with space, then invites it aboard. Once a cart is assigned to a ferry, he _marks it on his tally and claims that spot_ at once, so the next cart won't eye the same gap and set off a squabble.

— By now you've probably recognized him: this old dockmaster who wedges cargo into the fullest boats and scores every ferry is the kube-_scheduler_; the step of crossing off boats that can't fit is _filtering_ (predicates), the step of scoring the survivors and picking the tightest fit is _scoring_, and his taste for packing tight rather than spreading thin is _bin-packing_ (the `MostAllocated` scoring strategy).

**What it is and why it matters.** The kube-scheduler picks a node for each pending Pod in two phases. First, _filtering_: it walks every node checking feasibility — is there enough remaining CPU / memory for the Pod's `requests`, do `nodeSelector`, `taint`/`toleration`, and affinity constraints hold — and discards the nodes that can't fit. Then, _scoring_: it gives each surviving node a score and places the Pod on the highest. The scoring strategy sets the "personality": _bin-packing_ (`MostAllocated`, or the older `RequestedToCapacityRatio`) prefers to pack Pods onto nodes that are already fairly full, which consolidates fragmented resources, frees whole empty nodes so the cluster-autoscaler can _scale down_ and save money, and keeps complete room open for a future large Pod. The flip side is `LeastAllocated` (the more common default lean), which spreads load out, giving better resilience to bursts and single-node failure, but wastes capacity and resists scale-down. A Pod that fits no node stays `Pending` (_unschedulable_) until resources free up or the autoscaler adds a node. Once chosen, the scheduler _binds_ the Pod and reserves that node's resources in its cache, so concurrent scheduling won't hand the same slack to two Pods. At bottom this is a bin-packing optimization problem (NP-hard in general), and the scheduler approximates it greedily through scoring — weighing "pack tight and save" against "leave slack and stay safe" on your behalf.

**Metaphor mapping:**

- Carts of every size: individual _Pods_ awaiting scheduling, each with its own CPU / memory `requests`
- The ferries along the bank: the _Nodes_ in the cluster, each with a fixed allocatable capacity
- The dockmaster with his tally, assigning carts: the kube-_scheduler_ (it moves no cargo itself, only decides placement)
- The room he tracks for each ferry: a node's _remaining allocatable_ — capacity minus already-assigned requests
- Crossing off boats that can't fit or aren't sailing: _filtering / predicates_ (feasibility)
- Giving each remaining ferry a score: _scoring_ (the prioritization phase)
- Wedging cargo into the fullest boat: the _bin-packing_ scoring strategy (`MostAllocated`, favoring high-utilization nodes)
- The newer hand spreading loads half-full across every boat: `LeastAllocated` (spread) — burst-resilient but harder to scale down
- The whole empty ferry freed for the midday timber: consolidating fragments to free full nodes for autoscaler _scale-down_ or a large Pod
- The millstone too heavy for any boat, left waiting on the pier: an _unschedulable_ Pod stuck `Pending` for lack of resources
- Marking the tally and claiming the spot on assignment: _binding_ and cache reservation, preventing concurrent double-assignment of the same slack
</section>
