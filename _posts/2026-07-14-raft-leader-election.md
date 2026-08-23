---
layout: fable
title: "烽火台的换届鼓声 · The Beacon Towers' Drumroll Handover"
title_zh: "烽火台的换届鼓声"
title_en: "The Beacon Towers' Drumroll Handover"
concept: "Raft leader election and quorum"
tags: [distributed-systems]
illustration: /assets/art/2026-07-14-raft-leader-election.jpg
---
<section class="zh" markdown="1">
北境长城沿线立着七座烽火台，彼此相隔不过十里，靠鼓声和火光互相联络。每晚点火示警的暗号节奏，必须由一位"总把总"统一敲定——如果七座台各按各的心思打拍子，暗号乱作一团，友军连真假烽火都分不清。所以任何时候，全境只能认一个总把总。

总把总在位时，每隔一更就要击鼓一声，向各台确认"我还在，按我说的来"——这声鼓，各台都竖着耳朵等。哪座台连着两更没听见鼓声，它就得琢磨：总把总是不是出事了？但它不会立刻就跳出来自荐，而是先按下性子，等一段长短随机的静默期——如果这期间七台里另有人先敲响了"起意鼓"，自己就不搅局；只有等到这段随机静默过完，鼓声仍是死寂，它才会亲自敲响起意鼓，把心里记的"届数"加一，向另外六座台喊话：我要做第 X 届总把总，认是不认？

规矩很硬：一座台想真正坐上总把总的位置，必须凑齐七座台里_至少四座_（过半数）盖印回话"认"，才算数。凑不满四座，这一届起意就算白起。而且从此往后，谁记的届数更高，其余台一律以那个更高的届数为准，把自己脑子里旧的认知直接覆盖掉，不做二想。

有一年腊月大雪封山，七座台被雪墙硬生生隔成两群：西边三座，东边四座，中间断了联络整整半月。两边的把总不约而同地觉得总把总失联了，各自起意竞选。西边三座怎么求票都不够看——就算三座全投给自己，也凑不满四这个数，选来选去，谁也当不成，只能干等着，暂停发号施令。东边四座却刚好把自己这一侧四座的印全部集齐，顺顺当当选出了新总把总，continue正常打更发令，没受一点影响。等开春雪化、鼓声重新传到西边，西边的把总一听对方报出的届数比自己记的还高，二话不说立刻服气——全境重新只剩一个总把总，没有闹出两头各喊各的号、烽火互相打架的乱局。

——到这儿你大概已经认出来了：这套击鼓换届、非过半数不能就任、届数只增不减的规矩，讲的其实是 Raft 共识算法里的 _leader election_ 机制，以及它靠 _quorum_（多数派）来保证任何时候最多只有一个合法 leader 的原理——这也正是 etcd 能在网络分区时避免 _split-brain_ 的根本原因。

_概念解释_
Raft 是分布式系统里最常见的共识算法之一（etcd、Consul 底层都靠它），核心是让一群节点在任何时候都对"谁是 leader"达成一致。每个节点维护一个只增不减的 _term_；leader 靠定期 _heartbeat_（AppendEntries 心跳）证明自己还活着，follower 一旦超过 _election timeout_ 没收到心跳，就转成 _candidate_，term 加一，向其他节点发起 _RequestVote_ 拉票，只有拿到超过半数节点同意（_majority quorum_）才能真正当选。这条"必须过半"的规则从数学上保证了：任意两个不同 term 都不可能各自凑出一个互不相交的多数派——因为在同一个集群里，任意两个"多数"集合必然有交集。这正是 network partition 发生时，少数派一侧永远选不出 leader、多数派一侧能继续正常工作的根本原因，也是 etcd 集群普遍建议部署奇数节点（3/5/7）、并把"多数节点存活"当成可用性边界的原因。随机化的 election timeout 则是为了避免多个 candidate 同时起意导致选票瓜分、迟迟选不出人。

_隐喻对应表_

- 总把总 → leader
- 届数 → term
- 每更一声鼓确认"我还在" → heartbeat（AppendEntries）
- 连续两更没听到鼓声 → election timeout
- 随机长短的静默期 → randomized election timeout（防止选票瓜分）
- 敲起意鼓、向六台喊话 → candidate 发起 RequestVote
- 七座里至少四座盖印认可 → majority quorum
- 腊月大雪把七台分成三/四两群 → network partition
- 西边三座怎么也凑不满四票 → 少数派一侧无法达到 quorum，选不出 leader
- 东边四座顺利集齐自己一侧的四票 → 多数派一侧正常选出新 leader
- 届数更高者为准，旧认知直接作废 → 更高 term 覆盖旧 term，保证全局收敛，避免 split-brain
</section>
<section class="en" markdown="1">
Along the northern border wall stood seven beacon towers, each barely ten li from the next, linked only by drumbeats and firelight. Every night's warning-fire signal had to follow one unified rhythm, set by a single _chief signalman_ — if all seven towers kept their own beat, the signals would turn to noise, and friendly troops couldn't tell a real warning from a false one. So at any given moment, the whole stretch of wall could recognize exactly one chief signalman.

While in office, the chief signalman struck a drum once every watch to tell the towers "I'm still here, follow my lead" — and every tower kept an ear out for that beat. If a tower went two watches straight without hearing it, its keeper would start to wonder whether the chief had met with trouble. But he wouldn't jump in right away — he'd first wait out a stretch of silence of random length. If, during that wait, some other tower struck its own "I intend to lead" drum first, he'd stand down and let it be. Only if the random silence ran out with the drums still dead would he strike that drum himself, bump the _term_ number he kept in his head up by one, and call out to the other six towers: I mean to be chief for term X — do you recognize me or not?

The rule was unbending: to actually take the seat, a tower had to gather seals of approval from _at least four of the seven_ — a strict majority. Falling short meant the whole attempt counted for nothing. And from that point on, whoever held the higher term number won automatically — every other tower simply overwrote its old memory and fell in line, no second-guessing.

One year, a brutal winter snowstorm sealed the mountain pass and split the seven towers into two isolated clusters: three in the west, four in the east, cut off from each other for half a month. Keepers on both sides independently concluded the chief had gone silent, and both sides tried to elect a new one. The western three could never gather enough votes — even if all three voted for the same candidate, that only made three, one short of the four needed — so no matter how many rounds they tried, no one was ever confirmed, and the west simply sat idle, signaling suspended. The eastern four, on the other hand, managed to gather all four of their own seals cleanly, elected a new chief without a hitch, and kept signaling normally the whole time. When spring thaw restored contact and the drumbeat finally reached the west, the western keepers heard a term number higher than the one they remembered and yielded immediately, no argument — the whole wall was back down to a single chief signalman, with no chaos of two sides shouting conflicting signals into the night.

——By now you've probably recognized it: this rule of drumroll handovers, majority-only confirmation, and ever-increasing term numbers is describing the _leader election_ mechanism in the Raft consensus algorithm, and how it relies on a _quorum_ (majority) to guarantee that at most one legitimate leader ever exists at any given moment — which is exactly how etcd avoids _split-brain_ during a network partition.

_Concept explanation_
Raft is one of the most widely used consensus algorithms in distributed systems (it underpins etcd and Consul), and its core job is getting a cluster of nodes to agree on "who is the leader" at all times. Every node maintains a monotonically increasing _term_. A leader proves it's alive through periodic _heartbeats_ (AppendEntries with no log entries); once a follower goes past its _election timeout_ without hearing one, it becomes a _candidate_, increments its term, and sends out _RequestVote_ calls to the rest of the cluster — but it only becomes leader if it wins votes from a strict _majority quorum_ of nodes. That "must be a majority" rule is what mathematically guarantees no two disjoint majorities can ever exist at the same time within one cluster, since any two majority sets of a group must overlap. This is exactly why, during a network partition, the minority side can never elect a leader while the majority side keeps functioning normally — and why etcd clusters are conventionally deployed with an odd number of nodes (3, 5, 7), with "majority of nodes alive" as the hard boundary of availability. The randomized election timeout exists specifically to prevent multiple candidates from starting elections simultaneously and splitting the vote so badly that no one ever wins.

_Metaphor mapping_

- Chief signalman → leader
- Term number → term
- A drumbeat each watch confirming "I'm still here" → heartbeat (AppendEntries)
- Two watches with no drumbeat → election timeout
- The random stretch of silence before acting → randomized election timeout (prevents split votes)
- Striking the "I intend to lead" drum, calling the other six → a candidate issuing RequestVote
- At least four of seven seals of approval → majority quorum
- The winter storm splitting seven towers into groups of three and four → network partition
- The western three can never scrape together four votes → the minority side can't reach quorum, no leader elected
- The eastern four cleanly gather their own four votes → the majority side elects a new leader normally
- Higher term wins, old memory simply overwritten → a higher term overrides a stale one, guaranteeing convergence and no split-brain
</section>
