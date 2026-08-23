---
layout: fable
title: "渔村补网的规矩 · The Fishing Village's Repair Rule"
title_zh: "渔村补网的规矩"
title_en: "The Fishing Village's Repair Rule"
concept: "PodDisruptionBudget"
tags: [kubernetes]
illustration: /assets/art/2026-07-06-pod-disruption-budget.jpg
---
<section class="zh" markdown="1">
云溪湾是个小渔村,六条渔船每天轮流出海,村里的鱼摊全靠它们撑着。渔网泡了海水,用久了会朽,补网是免不了的事。可补网得把船拉上岸,一拉就是大半天,船不出海,那天的鱼就少一份。

村里老船长早年定过一条规矩:谁家想把船拉上岸补网,得先去找港口的老周报备。老周手里记着一本账——今天已经有几条船在岸上修,还剩几条在海上。规矩很简单:不管排班顺序如何,岸上补网的船不能超过两条,海上至少得留四条,不然鱼摊就得关张,渔行的信用也就砸了。

有一回,东头老陈的船破得厉害,顶着要修;可那天西头两家已经报了修网,岸上正好两条。老陈找老周说"我这网再不换就要漏水了",老周翻账本一看,摇头:"再等等,等西头哪条先补完下水,你再上岸。"老陈虽然着急,也只能先凑合着开出去晚点回来,等着轮到自己。

第二天西头一条补完网重新下海,老周这才在账本上划掉一笔,回头跟老陈说:"现在可以了,你上岸吧。"老陈的船一上岸,账本上"岸上"那栏又添了一笔,还是没超过两条。就这样,不管哪条船想修,老周永远先看账,永远不让海上剩的船跌破那条线——哪怕全村都嚷嚷着要修网,也得排队,一条一条来。

村里因此从没断过鱼,渔行的招牌也没砸过,虽然偶尔有人要多等一两天才能补上网。

——到这儿你大概已经认出来了:老周手里那本"最少留几条船在海上"的账,讲的其实是 Kubernetes 里的 _PodDisruptionBudget（PDB）_。

_概念解释:_
PodDisruptionBudget 是 Kubernetes 用来约束"主动驱逐"（_voluntary disruption_,比如节点排空、集群升级、维护)的一种资源。它规定一个应用的多个副本中,任何时候"可被同时赶走"的数量上限(_maxUnavailable_)或"必须留活"的下限(_minAvailable_)。当 kubectl drain、节点升级、autoscaler 缩容等操作想要驱逐 Pod 时,必须先调用 _Eviction API_,API Server 会检查 PDB:如果驱逐会让存活副本数跌破约定的下限,请求就会被拒绝(通常返回 429),操作方只能等下一个窗口再试。这样,滚动维护、节点升级这些"主动"的中断就不会意外把一个服务打到不可用——哪怕维护再急,也得一个一个来,应用的最低可用性始终有保障。它管不了_非自愿_中断(比如节点直接宕机),但对可控的维护操作是硬约束。

_隐喻对应表:_

- 六条渔船 → 一个 Deployment/StatefulSet 的多个 Pod 副本
- "岸上最多两条、海上至少四条"的规矩 → PDB 里的 maxUnavailable / minAvailable
- 港口老周和他的账本 → Kubernetes 的 Eviction API + PDB controller
- 想拉船上岸补网 → 主动驱逐请求(节点排空、集群升级、autoscaler 缩容等 voluntary disruption)
- 老周说"再等等" → Eviction API 因违反 PDB 而拒绝请求(429),需等待重试
- 补完网重新下海 → 一个 Pod 完成滚动更新/重启后重新 Ready,释放了配额
- 鱼摊不断货 → 服务在维护期间始终保持最低可用性(availability SLA)
</section>
<section class="en" markdown="1">
Yunxi Bay was a small fishing village with six boats that rotated out to sea every day. The whole village's fish stall depended on them. Nets soak in seawater and eventually rot, so re-netting was unavoidable. But re-netting meant hauling a boat ashore, and once ashore it stayed half a day at least — and every boat out of the water was one less share of that day's catch.

Years back, the old captain had set a rule: anyone wanting to haul their boat ashore for repair had to check in first with Old Zhou at the harbor. Old Zhou kept a ledger — how many boats were already ashore under repair, how many remained at sea. The rule was simple: no matter the queue, no more than two boats could be ashore for repair at once, and at least four had to stay out at sea — otherwise the fish stall would have to close, and the fishing guild's word would be worthless.

One day, old Chen's boat from the east end was badly worn and desperately needed mending. But that same day, two households from the west end had already checked in for repairs — the shore was already at two. Chen went to Old Zhou: "My net's about to give out, it'll be taking on water any day now." Old Zhou flipped through his ledger, shook his head: "Wait a bit. Once one of the west-end boats finishes and goes back out, then you can come ashore." Reluctantly, Chen patched things together and kept sailing a little longer, waiting his turn.

The next day, one of the west-end boats finished its repair and returned to sea. Only then did Old Zhou cross off a line in his ledger and tell Chen: "Now you can come in." The moment Chen's boat came ashore, the "ashore" column gained an entry again — still no more than two. And so it went: whoever wanted repairs, Old Zhou always checked the ledger first, and never let the boats remaining at sea drop below that line — even when the whole village was clamoring for repairs at once, they queued, one at a time.

Because of this, the village never ran short of fish, and the guild's reputation never took a hit — though every so often, someone had to wait an extra day or two before their net got fixed.

By now you've probably recognized it: Old Zhou's ledger of "how many boats must stay at sea" is really describing Kubernetes' _PodDisruptionBudget (PDB)_.

_Concept explanation:_
A PodDisruptionBudget is a Kubernetes resource that constrains _voluntary disruption_ — things like node drains, cluster upgrades, and planned maintenance. It sets, across an application's replicas, either an upper bound on how many can be taken down at once (_maxUnavailable_) or a lower bound on how many must stay alive (_minAvailable_). When operations like `kubectl drain`, node upgrades, or autoscaler scale-downs want to evict a pod, they must first call the _Eviction API_. The API server checks the PDB: if the eviction would push the number of healthy replicas below the agreed threshold, the request is rejected (typically with a 429), and the caller has to retry on the next window. This way, voluntary disruptions during rolling maintenance or upgrades never accidentally push a service below its minimum availability — no matter how urgent the maintenance, it happens one at a time. A PDB has no power over _involuntary_ disruptions (like a node crashing outright), but for controllable maintenance operations, it's a hard constraint.

_Metaphor mapping:_

- Six fishing boats → the multiple Pod replicas of a Deployment/StatefulSet
- "At most two ashore, at least four at sea" rule → maxUnavailable / minAvailable in the PDB
- Old Zhou and his ledger → Kubernetes' Eviction API + PDB controller
- Wanting to haul a boat ashore for repair → a voluntary disruption request (node drain, cluster upgrade, autoscaler scale-down, etc.)
- Old Zhou saying "wait a bit" → the Eviction API rejecting the request (429) because it would violate the PDB, forcing a retry
- A boat finishing repairs and returning to sea → a Pod completing its rolling update/restart and becoming Ready again, freeing up budget
- The fish stall never running dry → the service maintaining minimum availability throughout maintenance (an availability SLA)
</section>
