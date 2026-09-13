---
layout: fable
title: "双堂合议令 · The Decree of the Conjoined Halls"
title_zh: "双堂合议令"
title_en: "The Decree of the Conjoined Halls"
concept: "Raft Joint Consensus: Safe Dynamic Cluster Membership Changes"
tags: [distributed-systems, consensus, etcd]
illustration: /assets/art/2026-09-12-raft-joint-consensus.jpg
youtube_id: "4iF35r9kCO8"
---
<section class="zh" markdown="1">
乌石峡两岸有三座连环古寨，唤作白石寨、青石寨、黑石寨。三寨依山面水，共掌峡口一座巨大的平仓水闸。

三寨立有一道行之百年的盟规：水闸何日开关、何日闭闸，全凭“三老议事堂”决断。
三位寨主各执一方铜印，凡遇大事，只需**三人中有两人落印**（半数以上），便算盟令抵定，刻石行舟，谁也不得违抗。
百年间，三寨凭着“过半即行”的规矩，防备了无数天灾风浪。

直到有一年大旱，下游又有两座新寨——赤石寨与黄石寨前来投靠。
三位老寨主开怀接纳，决定将五寨结为一个大同盟，把议事堂从“三老”扩建为“五老”，五方共决。
按照大同盟的新规矩，日后大事需要**五人中有三人落印**（五取其三）。

改规矩看似是件喜事，老寨主提笔写了一道檄文：“自今日起，旧三老散堂，新五老接印。”随后差遣快马分别送往五座山寨。

然而，祸事正是从这匹快马跑出的一刻爆发的。

山道险峻，有的信使乘轻舟顺流而下，半个时辰便到了赤石寨与黄石寨；有的信使却在绝壁山道上被落石阻隔，走走停停，一天一夜也没能爬上黑石寨。

这便造成了一场可怕的错位：
青石寨与赤石寨、黄石寨早早接到了檄文，三位寨主喜气洋洋地在下堂聚齐。在他们眼里，天下已是“五老盟”，自己这三方落印，恰好占了五取其三的绝大多数！于是他们当堂敲锣打鼓，落印通过了一道“即刻开闸引水，灌溉下荒”的重令。

而在上游的白石寨与黑石寨，两位老寨主压根还没见到信使的影子。在他们的法理中，天地间依然是“三老堂”。两位老寨主碰头一商议，认为水旱未退，应当保蓄上游池水，二人大印一盖——两人对三人，同样占了过半的多数！他们同样敲锣打鼓，发出一道“封死闸门，滴水不放”的严令。

一个水闸，两道针锋相对的绝令，两队各执信物的武士在闸门前拔刀相向。
两边都指责对方篡权叛盟，而两边拿出的法令，按照各自手头的章程，全都是名正言顺的过半法定多数！

三寨联盟险些因此血流成河。

千钧一发之际，白发苍苍的总寨老令公拄杖登上闸楼，喝止了众人。
他捡起两道盖满红印的截然相反的公文，掷在石案上长叹：
“你们谁也没有背盟，错在这道改规矩的檄文！**人心有先知后觉，山道有快慢险阻。新旧两套规矩，绝不能在眨眼之间平地倒换！**”

老令公拔出佩剑，在议事大厅青石地板上划了一道深痕，立下了流传后世的**“双堂合议令”**：

“从今往后，凡有添寨除名之变，不可一纸断绝，必须设立一段‘合议过渡期’。
在此期间，既不是旧三老说了算，也不是新五老说了算，而是**设双堂，合双印**！
任何一道号令要想生效，必须同时跨过两道铁门槛：
第一道，旧三老堂中，必须有两人以上画押盖印；
第二道，新五老堂中，也必须有三人以上画押盖印。
**缺一不可，两边皆过，方可通行。**”

众人闻言，无不惊服。
按这双堂之法，倘若有人走得快、有人走得慢，哪怕一部分人以为还在过渡、一部分人以为已经改完，这世上再没有任何单独一方能够私自凑出法定人数：
要想借旧规矩作乱？缺了新堂过半的支持，令不出门；
要想借新规矩抢先？缺了老堂过半的点头，法理不容！
两重过半，像两把互相咬死的铁锁，彻底斩断了任何诞生“两个多数”的可能。

老令公更是定下铁律：
只有等这道“双堂合议”的法令真真切切刻上了五座山寨每一块石碑、全票封存之后，总寨主才能再发一道“单堂就位令”，正式拆除旧堂门槛，稳稳步入五老治峡的新纪元。

自此，乌石峡添寨退寨不下十次，有时是三寨变五寨，有时是五寨退回四寨。
无论峡谷风雨如何遮蔽道路，无论信差在途中耽搁多久，大闸之前，再没有两面大旗同日凌空的惨剧。

— — —

### 这是什么

这就是分布式一致性协议（如 Raft、etcd）中最为精妙也是最复杂的安全基石——**Raft 联合共识（Joint Consensus）与集群动态成员变更**。

在分布式共识系统（如 etcd、Consul、TiKV）中，集群节点数量（如 3 节点扩容至 5 节点）必须根据业务负载进行动态调整。
然而，在没有停机维护（Zero-Downtime）的前提下，直接在集群中从旧配置 $C_{\text{old}}$ 单步切换到新配置 $C_{\text{new}}$ 是极其危险的：
由于分布式网络中存在不可控的网络延迟、分区（Partition）和消息乱序，各个节点接收并应用新配置的时机必然有先有后。

如果允许单步直接切换，极易产生**配置双裂（Split-Brain / Dual Majorities）**：
- 节点 A、B 尚未收到变更日志，仍认定集群为 3 节点配置 $C_{\text{old}} = \{A, B, C\}$。它们俩（2/3）即可组成旧法定多数，选举出 Leader 1 并提交日志；
- 节点 C、D、E 已应用了 5 节点新配置 $C_{\text{new}} = \{A, B, C, D, E\}$。它们三者（3/5）亦能组成新法定多数，选举出 Leader 2 并提交完全矛盾的日志！
此时，一个集群同时存在两个合法的多数派（Two Disjoint Majorities），系统数据发生不可逆的脑裂与分裂。

2014 年，Raft 算法发明人 Diego Ongaro 和 John Ousterhout 在其经典论文与博士论文中提出了**联合共识（Joint Consensus）**机制：

配置变更必须采用**两阶段过渡机制**，将过渡状态本身作为一条特殊的日志条目推入 Raft 复制状态机：
1. **进入联合配置阶段（Enter Joint Consensus）**：
   Leader 接收到成员变更请求后，首先向集群提交一条特殊的联合配置日志 **$C_{\text{old,new}}$**；
   在 $C_{\text{old,new}}$ 生效期间，任何日志的提交以及 Leader 的选举，都必须遵循**联合法定多数（Joint Majority）**规则：
   - 必须获得 $C_{\text{old}}$ 中任意半数以上节点的认可；
   - **同时**必须获得 $C_{\text{new}}$ 中任意半数以上节点的认可。
2. **退出并落地新配置（Finalize to $C_{\text{new}}$）**：
   一旦 $C_{\text{old,new}}$ 条目被成功提交（意味着它已被 $C_{\text{old}}$ 和 $C_{\text{new}}$ 的多数派持久化，后续任何 Leader 都必定包含此配置），Leader 方可安全发起第二条纯新配置日志 **$C_{\text{new}}$**；
   当 $C_{\text{new}}$ 被成功提交后，集群平稳过渡到纯新配置模式，老节点安全退役。

### 为什么重要

- **从数学上杜绝双脑分裂**：根据抽屉原理，任何跨越 $C_{\text{old}}$ 的多数派必有交集，任何跨越 $C_{\text{new}}$ 的多数派也必有交集。要求同时满足两者的联合多数派，使得在任意时刻**绝对不可能存在两个重叠之外的决议实体**；
- **全生命周期零停机（Zero-Downtime Reconfiguration）**：在扩缩容、故障节点替换或跨可用区机房迁移期间，集群持续对外提供高可用读写服务，无需整体停服维护；
- **单节点变更 vs 联合共识**：
  Raft 论文还提出了一种简化的“单节点成员变更（Single-Server Membership Change）”——每次只增删一个节点，在数学上也能天然避免多数派重叠分裂。
  但工程实践（如 HashiCorp Raft、Apache Ratis）发现，连续执行多次单节点变更容易出现网络并发乱序导致的幽灵配置。因此，**成熟的生产级系统更倾向于联合共识（Joint Consensus）**，以换取任意多节点并行变更时的绝对严谨与确定性。

### 隐喻对应表

| 故事元素 | 计算机概念 | 技术细节与工程映射 |
| :--- | :--- | :--- |
| **平仓水闸与乌石峡水道** | 分布式核心数据与关键资源 | etcd 中的分布式键值状态机与外部业务读写 |
| **三老各自掌管的铜印** | Raft 集群节点拥有的投票权（Voter Node） | 每个节点持有的有效投票与日志确认权重 |
| **三人中有两人落印即开闸** | 法定多数原则（Quorum / Majority） | 奇数节点集群保证仲裁决议无歧义（$N/2 + 1$） |
| **快马送檄文因山道险阻有先有后** | 异步网络延迟与乱序交付（Network Delay） | 各节点接收并应用配置日志条目存在时差 |
| **下堂三寨以为五取三合法开闸** | 基于新配置 $C_{\text{new}}$ 形成的独立法定多数 | 部分节点抢先切到新配置，擅自提交矛盾决议 |
| **上游二老依旧规二人即过半闭闸** | 基于旧配置 $C_{\text{old}}$ 形成的独立法定多数 | 滞后节点仍依从旧配置，造成集群双主脑裂 |
| **老令公设立的“双堂合议令”** | 联合配置（Joint Consensus $C_{\text{old,new}}$） | 引入包含新旧集合的过渡配置元数据条目 |
| **新堂过半且老堂亦过半方可落印** | 联合法定多数判定（Joint Majority Rule） | 任何提交必须同时获得 $C_{\text{old}}$ 和 $C_{\text{new}}$ 多数确认 |
| **刻石铭记方发单堂就位令** | 提交并应用纯新配置 $C_{\text{new}}$ 条目 | 确认 $C_{\text{old,new}}$ 提交持久化后，方可切换至 $C_{\text{new}}$ |
| **十次添寨退寨再无双旗凌空** | 零停机动态成员变更（Zero-Downtime） | 任意节点的加入与驱逐在数学证明下始终安全 |
</section>

<section class="en" markdown="1">
Across the rugged cliffs of Black Stone Gorge stood three ancient sister fortresses: White Rock, Green Rock, and Black Rock. Perched between sheer precipices and rushing rapids, the three strongholds guarded a colossal watergate controlling the flow of the entire river basin.

For a hundred years, the confederacy was ruled by an unbreakable covenant: the opening and closing of the watergate was determined solely by the *Council of the Three Elders*.
Each fortress chieftain bore a bronze seal. In all matters of state, **so long as two of the three pressed their seals** (a simple majority), the decree was etched into stone, and the river pilots obeyed without question.
For generations, this rule of simple majority protected the basin through flood and drought.

Until one year of brutal drought, two downstream strongholds—Red Rock and Yellow Rock—sought to join the league.
The three elder chieftains welcomed them with open arms, agreeing to expand the council from the Three Elders to the *Council of the Five Sages*.
Under the charter of the new grand alliance, all future decrees would require **three seals out of five** (a three-fifths majority).

Amending the covenant seemed a cause for celebration. The grand chieftain penned a proclamation: *"From this day forth, the Council of Three is dissolved; the Council of Five assumes the seals."* Fast couriers were dispatched to deliver the edict to all five mountain fortresses.

Yet the catastrophe began the moment the couriers galloped into the wilderness.

The mountain trails were treacherous. Couriers traveling by light skiff downstream reached Red Rock and Yellow Rock in half an hour; others, navigating sheer cliffside ledges, were halted by rockslides, taking a full day and night to scale the heights of Black Rock.

This gave birth to a terrifying temporal dislocation:
Green Rock, Red Rock, and Yellow Rock received the edict early. Their three chieftains assembled joyfully in the lower gorge hall. In their eyes, the world was now governed by the *Council of Five*, and their three seals formed an undeniable three-fifths majority! With beating drums, they stamped a decree: *"Raise the watergate immediately to irrigate the downstream plains."*

Upstream, however, the chieftains of White Rock and Black Rock had seen neither courier nor proclamation. In their universe, the *Council of Three* remained supreme law. The two elders conferred over the drought, resolved to hold back the waters to protect the highland reservoirs, and pressed their bronze seals—two against one, a perfect two-thirds majority! With sounding gongs, they issued a contradictory decree: *"Bar the gates shut; release not a drop."*

One single watergate; two completely opposite, irreconcilable decrees. Armed guards under rival banners drew steel at the control winches.
Each accused the other of treason, yet both brandished decrees stamped by legitimate, lawful majorities under the charters in their possession!

The alliance stood on the razor's edge of civil war.

At the final moment, the white-haired elder grand marshal ascended the gatehouse with his staff, roaring at the factions to halt.
He picked up the two contradictory, seal-covered scrolls from the stone table and sighed heavily:
"Neither of you has betrayed the oath. The fault lies in the proclamation itself! **Minds awaken at different hours; mountain paths run with different speeds. Two governing covenants cannot be swapped in the blink of an eye!**"

The old marshal drew his sword, carving a deep furrow across the bluestone floor of the council chamber, declaring the law that would govern the gorge for centuries: **The Decree of the Conjoined Halls**.

"Henceforth, whenever our league admits or expels a fortress, it shall not be accomplished by a single stroke. There must be an intermediate **Conjoined Council Period**.
During this passage, neither the Old Three nor the New Five rule alone. **Two halls sit together, and two sets of seals must unite!**
For any decree to take effect, it must cross two iron thresholds simultaneously:
First, a majority of the Old Council of Three must set their seals;
Second, a majority of the New Council of Five must also set their seals.
**Neither may be missing; both thresholds must be satisfied.**"

The commanders looked on in stunned realization.
Under this conjoined rule, even if messages flew fast or lagged behind, even if some thought the league was transitioning while others thought it complete, no single splinter faction could ever assemble a quorum alone:
Attempt to hijack the league under the old charter? Without majority support from the new hall, the decree dies in the chamber;
Attempt to rush ahead under the new charter? Without majority approval from the elder hall, the law has no standing!
The two overlapping majorities locked together like intertwined iron teeth, mathematically exterminating any possibility of two rival majorities coexisting.

The grand marshal laid down the final decree:
Only after this Conjoined Council decree was engraved on the stone stelae of all five fortresses and affirmed across the river could the master issue the final *Charter of the Single Hall*, smoothly decommissioning the old seals and inaugurating the era of the Five Sages.

Across the centuries that followed, the league expanded and contracted many times—from three to five, and from five back to four.
Yet no matter how storms choked the canyon trails, no matter how long couriers were delayed in the drifts, never again did two rival banners clash above the watergate.

— — —

### What it is

This is the most subtle and mathematically rigorous safeguard in distributed consensus protocols (such as Raft and etcd): **Raft Joint Consensus and Dynamic Cluster Membership Changes**.

In distributed consensus engines (such as etcd, Consul, and TiKV), cluster membership (e.g., expanding from 3 nodes to 5 nodes) must be adjusted dynamically as workloads grow.
However, performing a direct, single-step cutover from old configuration $C_{\text{old}}$ to new configuration $C_{\text{new}}$ without downtime is hazardous:
Because distributed networks inevitably suffer asynchronous latency, network partitions, and message reordering, individual nodes receive and apply configuration updates at different times.

Allowing a single-step cutover leads directly to **Split-Brain (Dual Disjoint Majorities)**:
- Nodes A and B have not yet received the reconfiguration log entry and still believe the cluster is the 3-node set $C_{\text{old}} = \{A, B, C\}$. Together (2/3), they form a valid majority under $C_{\text{old}}$, electing Leader 1 and committing logs;
- Nodes C, D, and E have already applied the 5-node configuration $C_{\text{new}} = \{A, B, C, D, E\}$. Together (3/5), they form a valid majority under $C_{\text{new}}$, electing Leader 2 and committing conflicting logs!
At that instant, two independent majorities exist simultaneously within the same cluster, causing fatal split-brain and permanent data divergence.

In 2014, Diego Ongaro and John Ousterhout introduced the **Joint Consensus** mechanism in their seminal Raft thesis:

Cluster reconfiguration must traverse a **two-phase transition**, where the transitional state is treated as a first-class replicated log entry in the state machine:
1. **Enter Joint Consensus ($C_{\text{old,new}}$)**:
   Upon receiving a membership change request, the Leader appends a special joint configuration entry, **$C_{\text{old,new}}$**, to its log;
   While $C_{\text{old,new}}$ is active, all log commit decisions and Leader elections must obey the **Joint Majority Rule**:
   - Must secure agreement from a majority of nodes in $C_{\text{old}}$;
   - **AND** must secure agreement from a majority of nodes in $C_{\text{new}}$.
2. **Finalize to $C_{\text{new}}$**:
   Once the $C_{\text{old,new}}$ entry is committed (meaning it has been durably stored across majorities of both $C_{\text{old}}$ and $C_{\text{new}}$, guaranteeing that any future leader must possess this entry), the Leader safely proposes a second entry: the pure **$C_{\text{new}}$** configuration;
   Once $C_{\text{new}}$ is committed, the cluster fully enters the new topology, and decommissioned nodes can be powered down safely.

### Why it matters

- **Mathematical Prevention of Split-Brain**: By the pigeonhole principle, any two majorities of $C_{\text{old}}$ overlap, and any two majorities of $C_{\text{new}}$ overlap. Requiring a joint majority across both configurations guarantees that **no two disjoint quorums can ever form at any point in time**;
- **True Zero-Downtime Operations**: Expansions, contractions, and cross-availability-zone migrations occur seamlessly while the cluster continues servicing high-throughput client traffic;
- **Single-Server Changes vs. Joint Consensus**:
  Raft also supports an alternative: "Single-Server Membership Changes" (adding or removing one server at a time, where majorities naturally overlap without a joint state).
  However, production implementations (such as HashiCorp Raft and Apache Ratis) recognize that concurrent network reorderings during back-to-back single-server changes can create subtle corner cases. Therefore, **mature enterprise engines embrace Joint Consensus** for its uncompromising determinism when modifying arbitrary subsets of nodes.

### Metaphor Mapping

| Story Element | Computing Concept | Technical Details & Architecture Mapping |
| :--- | :--- | :--- |
| **Watergate at Black Stone Gorge** | Replicated state machine / Shared resource | The core data and operations managed by the consensus cluster |
| **Chieftains' bronze seals** | Voting member nodes in Raft cluster | Nodes with active voting rights (`Voter` status) |
| **Two seals out of three needed to open** | Quorum / Majority rule | Simple majority required to commit logs or elect leaders ($N/2 + 1$) |
| **Couriers delayed along cliff trails** | Asynchronous network delay & partition | Unpredictable network timing causing nodes to receive configs at different moments |
| **Three lower fortresses claiming 3-of-5 majority** | Disjoint quorum formed under $C_{\text{new}}$ | Updated nodes forming a majority under new config and acting independently |
| **Two upper fortresses claiming 2-of-3 majority** | Disjoint quorum formed under $C_{\text{old}}$ | Stale nodes forming a majority under old config, causing dual leaders |
| **The Decree of the Conjoined Halls** | Joint Consensus configuration ($C_{\text{old,new}}$) | A transitional log entry binding both old and new memberships together |
| **Must have both old and new majorities** | Joint Majority rule | Commits require separate majority approval from both $C_{\text{old}}$ and $C_{\text{new}}$ |
| **Carving stone stelae before single-hall charter** | Committing $C_{\text{old,new}}$ before appending $C_{\text{new}}$ | Ensuring transition is immutable across cluster before shedding old configuration |
| **Decades of additions/removals without split banners** | Zero-downtime dynamic membership change | Provably safe online reconfiguration without stopping production services |
</section>
