---
layout: fable
title: "巡天神炉与金鉴令 · The Flying Crucible and the Gilded Tally"
title_zh: "巡天神炉与金鉴令"
title_en: "The Flying Crucible and the Gilded Tally"
concept: "Suzuki-Kasami Algorithm"
tags: [distributed-systems]
illustration: /assets/art/2026-10-11-suzuki-kasami-token-broadcast.jpg
youtube_id: "WnB7LW8TyuI"
---
<section class="zh" markdown="1">
昆仑群峰之巅，云海苍茫，散布着九座世代炼丹的玄真剑阁。

九阁弟子同修天地玄气，然而炼制渡劫天丹所需的“巡天神炉”只有一座。此神炉悬于九阁中央的虚空剑阵之中，炉内翻滚着极阳的三昧真火。神炉天规森严：天地之间，任何时刻**只准一阁弟子启炉开炼**；若有两阁同时催火注气，阴阳异气互冲，神炉必定当场炸裂，整座山脉万劫不复。

早年间，各阁为了借用神炉，常向其他八阁尽发飞剑。每次炼丹，求借者不仅要等齐全部八阁的飞剑回批，还得在剑书上密密麻麻比对刻印的水运干支。剑影在群山间遮天蔽日，动辄数十柄飞剑来回激射。倘若两阁同刻发剑，甚至还需在山谷间反复换剑相让。长此以往，飞剑消耗如流水，往往神炉闲置了半日，八方的回信还在风雪中打转。

后来，终南剑仙游历至此，在虚空中铸下了一枚非金非木的宝牒——**金鉴令**。

剑仙立下两道新规：

第一，**见令如见炉**。天下的神炉只有一座，而虚空中的金鉴令亦只有一枚。不论哪一阁弟子欲启炉炼丹，只要手里紧握着这枚金鉴令，便拥有执掌神炉的至高特权，无需向任何同门多言半句，径直开炉即可。丹成之后，若无旁人求借，金鉴令便安稳留于该阁之内，下一季若想再炼，信手拈来便可再度启炉，连半柄飞剑都无需耗费。

第二，**传书唤令，鉴内藏队列**。倘若某阁弟子欲炼丹，而金鉴令并不在阁中，该阁无需向天下打探令在何方，只需向其余八阁各发一柄刻有本阁印记与求令次序的“巡天灵箭”。

各阁门前皆立有一座石碑，默默记录着天下一至九阁历来送达的最高灵箭序号。当某阁收到同门灵箭，若见箭上的序号高于石碑旧刻，便拂尘拭去旧痕，刻上最新序号。

那握有金鉴令的阁主，更是职责非凡。金鉴令的反面生有两重玄妙阵法：一重是九方玉格，刻录着金鉴令**真正准允各阁出入神炉的履历总计**；另一重则是一道流动不息的云纹暗格，化作一条**待炼的仙籍队列**。

某年大雪封山，第三阁炉火渐熄，欲启神炉续命，但金鉴令此刻正静卧于第一阁的青石台上。

第三阁首席弟子即刻取来八柄云纹飞箭，将本阁求丹的累计筹策添为第七次，随手掷向八方峰峦。飞箭掠过重峦，一瞬即达。第二、第四直至第九阁见箭，自知无令在手，仅是将门前石碑上第三阁的计数由六刻改为七，便阖门静修。

飞箭落入第一阁时，第一阁的丹火恰好圆满出炉。

阁主手握金鉴令，正欲将令收纳，瞥见第三阁的飞箭悬停庭前，上书“第七度启丹”。阁主反转金鉴令，垂目端详：金鉴令背后的九方玉格中，第三阁上一回被准允炼丹的记录，尚停留在第六度。

“七大于六，”阁主颔首，“三阁求令并非旧梦残影，而是确有新火待燃。”

阁主随即将金鉴令的灵符一展，见令内云纹队列中已无先至之人，便信手将第三阁的玄铁佩牌录入金鉴令的待炼队列。既然自身丹火已熄，阁主毫不恋栈，轻诵真言，那枚悬着第三阁名牌的金鉴令立时化作一道破空金虹，直贯云霄，瞬息穿越数十里风雪，端端正正落入第三阁的案台之上。

自始至终，第一阁从未耗费半柄回音飞剑；天下其余七阁，亦不曾多吐一字。

群峰间飞剑漫天的乱象，就此烟消云散。

---

读到这里，聪明的你或许已经认出：这便是分布式计算中经典的互斥算法——**铃木-笠见广播算法（Suzuki-Kasami Algorithm）**。

### 这是什么

**铃木-笠见算法**（Suzuki-Kasami Algorithm，由 Ichiro Suzuki 与 Toshimi Kasami 于 1985 年提出）是一种基于**权杖广播（Token-based Broadcast）**的分布式互斥（Distributed Mutual Exclusion）协议。

在无中心主节点的分布式系统中，多节点竞争进入临界区（Critical Section）通常分为两派哲学：
1. **基于许可协商（Permission-based）**：如 Ricart-Agrawala 算法或 Maekawa 算法，每个节点进入临界区前必须向其他节点发送请求并收集许可回执（$O(N)$ 或 $O(\sqrt{N})$ 消息量）。
2. **基于权杖（Token-based）**：系统中存在且仅存在一枚逻辑“权杖（Token）”，唯有持有权杖的节点方可进入临界区。

铃木-笠见算法是权杖阵营的集大成者。它通过以下机制运行：
- **请求广播**：非持有节点若需进入临界区，将其本地单调递增的请求计数器加一，向系统中所有其他节点广播一条带有自身序号的 `REQUEST(node_id, seq)` 消息（消息量为 $N-1$）。
- **本地请求矩阵**：每个节点维护一个大小为 $N$ 的数组 $RN[1..N]$，记录已知其他节点发送过的最高序列号。收到请求时更新：$RN[j] = \max(RN[j], seq)$。
- **权杖内部状态**：权杖不仅是一个信号量，它内部封装了两个关键数据结构：
  1. $LN[1..N]$：记录权杖历史上**最近一次准予各节点访问临界区的请求序列号**。
  2. $Q$：一个等待获取临界区的节点先进先出队列（FIFO Queue）。
- **特权转移与判定**：权杖持有者退出临界区时，将其本地 $LN[i]$ 置为本次请求号。接着扫描全局：若发现某个节点 $j$ 满足 $RN[j] = LN[j] + 1$，且 $j$ 不在队列 $Q$ 中，便将 $j$ 推入队列。最后，将权杖连同内部的 $LN$ 和 $Q$ 直接点对点（Point-to-Point）发送给队列头部的节点。

### 为什么重要

1. **从 $2(N-1)$ 到 $N$ 的消息优化极限**：
   相比 Ricart-Agrawala 等基于许可的算法每次进入临界区需耗费 $2(N-1)$ 条消息（全员请求 + 全员回复），铃木-笠见算法将消息量降至 $N$ 条（$N-1$ 条广播请求 + $1$ 条点对点权杖转移）。如果节点在退出临界区后再次发起请求且权杖尚未移出，消息量直接降为 **0 条**。

2. **区分“新请求”与“陈旧网络重放”**：
   在异步网络中，网络延迟可能导致过期的请求消息姗姗来迟。铃木-笠见算法利用权杖内置的 $LN[j]$ 数组与本地 $RN[j]$ 进行严格的代数比较（$RN[j] == LN[j] + 1$）。只要 $LN[j] \ge RN[j]$，说明该请求早已执行完毕，节点便能优雅且安全地丢弃旧消息，杜绝了幽灵请求引发的死锁与重复执行。

3. **现代分布式调度的状态随行范式**：
   传统的权杖环（Token Ring）算法中，空闲权杖会在无请求的节点间盲目空转消耗带宽。而铃木-笠见算法将等待队列 $Q$ 与执行状态 $LN$ 直接封装进 Token 随路移动，实现了**按需驱动的精准点对点交付**。这种“状态内嵌于特权凭证（Carrying state in the capability token）”的思想，直接启发了现代分布式队列、集群租约迁移以及分布式无锁资源调度器的设计。

_隐喻对应表_

- 昆仑九座玄真剑阁 → 分布式系统中的各个独立节点 (*Distributed Nodes*)
- 虚空中央的巡天神炉 → 需要互斥访问的共享资源 / 临界区 (*Critical Section*)
- 严禁两阁同时催火的天规 → 互斥安全不变性 (*Mutual Exclusion / Safety Invariant*)
- 虚空宝牒“金鉴令” → 分布式互斥特权权杖 (*Privilege Token*)
- 各阁门前刻录求令次序的石碑 → 节点本地维护的最高请求序列号数组 (*RN Array*)
- 欲求神炉时散发的八柄巡天灵箭 → 向集群其余节点广播的请求消息 (*Broadcast Request messages, $N-1$*)
- 金鉴令背后的九方玉格 → 权杖内部记录各节点已服务序列号的数组 (*LN Array*)
- 金鉴令内流动不息的云纹暗格 → 权杖内部维护的等待节点先进先出队列 (*FIFO Token Queue $Q$*)
- 辨识“七大于六”方才准予借令 → 通过比对 $RN[j] == LN[j] + 1$ 判定请求的真实新鲜度
- 灵符化作金虹直落第三阁 → 点对点单播交付权杖 (*Point-to-Point Token Transfer*)
- 丹火圆满后留令自用无须发箭 → 连续访问临界区的零消息开销特性 (*Zero-message cost for repeated entries*)
</section>
<section class="en" markdown="1">
High above the misty peaks of the Kunlun Mountains, nine Daoist sword pavilions stood nestled among the precipices, their disciples cultivating celestial arts across generations.

Though each pavilion practiced its own sword discipline, there existed only a single **Flying Crucible** in the mortal realm capable of refining heavenly elixirs. Suspended in a void formation at the mountain center, the crucible surged with primordial samadhi fire. The laws governing it were absolute: **only one pavilion could ignite the crucible at any single moment**. Should two pavilions inject their spiritual qi simultaneously, the conflicting astral energies would instantly shatter the furnace, triggering a catastrophic avalanche that would obliterate the mountain range.

In early eras, whenever a pavilion wished to borrow the crucible, its disciples were forced to dispatch messenger flying swords to every other pavilion. Before igniting the flame, the petitioner had to wait for unanimous written permits from all eight peaks, comparing intricate water-clock timestamps carved into parchment slips. Swords blotted out the sun as dozens crisscrossed the ravines. If two peaks happened to petition at the identical moment, they had to exchange further letters to establish precedence. Oftentimes, the crucible sat cold and idle for hours while missives spun aimlessly in blinding mountain blizzards.

Witnessing this exhaustion, a wandering sword immortal cast an ethereal talisman from celestial ore and suspended it above the peaks: **the Gilded Tally**.

The immortal decreed two foundational laws:

First, **possession of the tally grants command of the flame**. The world held but one crucible, and the void held but one Gilded Tally. Whichever pavilion held the tally possessed undisputed sovereign right to enter the crucible without uttering a single word to its brethren. When their batch was completed, if no other peak had requested the tally, it remained quietly on their stone dais. Should that same peak wish to refine another batch the following morning, they simply began anew—costing not a single flying sword across the skies.

Second, **broadcast the call, but let the tally carry the scroll**. If a peak required the crucible while the tally was elsewhere, they had no need to wander the valleys inquiring where it lay. They simply carved their pavilion crest and their cumulative petition count onto eight swift spiritual arrows, launching them simultaneously to every other peak.

Outside each pavilion stood a stone stele, quietly recording the highest arrow sequence number ever received from each fellow peak. Whenever a new arrow arrived, if its sequence exceeded the stele's old carving, the stone was swept clean and engraved with the fresh number.

The current keeper of the Gilded Tally bore a special burden. Upon the reverse side of the golden talisman lay two mystical formations: a nine-grid jade matrix recording the **exact count of completed elixir sessions granted to each peak throughout history**, and a swirling mist groove acting as a **living FIFO scroll of awaiting petitioners**.

During a harsh winter solstice, the spiritual fires of the Third Pavilion waned, urgently requiring the crucible's flame. But the Gilded Tally rested peacefully upon the quiet desk of the First Pavilion.

The head disciple of the Third Pavilion drew eight cloud-etched arrows, inscribed them with their seventh cumulative petition count, and loosed them into the storm. In a single breath, the arrows struck the remaining eight peaks. The Second, Fourth, through Ninth Pavilions noted the arrow, incremented their stone steles from six to seven for the Third Pavilion, and shut their gates, possessing no tally to bestow.

When the arrow reached the First Pavilion, its master had just retrieved their glowing celestial pill from the furnace.

Holding the Gilded Tally, the master prepared to set it down when the hovering arrow caught his eye, bearing the inscription: *The Third Pavilion petitions for its seventh batch.*

The master flipped the talisman and inspected its reverse jade matrix. There, the historical tally for the Third Pavilion remained at six.

"Seven is strictly greater than six," murmured the master. "This is no stale echo of an old petition, but a fresh prayer for the flame."

Inspecting the mist groove, the master saw no earlier petitioners waiting in queue, and promptly inscribed the crest of the Third Pavilion into the tally's internal scroll. Having finished his own refinement, he uttered an incantation. The Gilded Tally uncoiled into a golden ray of light, streaking dozens of miles across the snowy chasm, settling directly upon the desk of the Third Pavilion.

Not a single return sword was dispatched by the First Pavilion; not a single extraneous word was spoken by the other seven peaks.

The sky remained quiet, serene, and unbroken.

---

By now you've probably recognized it: this is the classic distributed mutual exclusion protocol—the **Suzuki-Kasami Algorithm**.

### What it is

The **Suzuki-Kasami Algorithm** (introduced by Ichiro Suzuki and Toshimi Kasami in 1985) is a seminal **token-based broadcast algorithm** for distributed mutual exclusion.

In decentralized systems without a coordinator, algorithms for mutual exclusion typically fall into two fundamental paradigms:
1. **Permission-based**: Such as Ricart-Agrawala or Maekawa's algorithm, where a candidate node must gather explicit permissions from peer nodes or quorum intersections before entering a critical section ($O(N)$ or $O(\sqrt{N})$ network overhead).
2. **Token-based**: A single unique logical token circulates through the system. Only the node holding the token is permitted to execute inside the critical section.

Suzuki-Kasami is the gold standard of token-based protocols on broadcast networks:
- **Broadcast Request**: When a non-holder node wishes to enter the critical section, it increments its local monotonic request counter and broadcasts a `REQUEST(node_id, seq)` message to all other $N-1$ nodes.
- **Local Request Array ($RN$)**: Every node maintains an array $RN[1..N]$, tracking the highest sequence number observed from every other node. Upon receipt of a request, it updates: $RN[j] = \max(RN[j], seq)$.
- **Token Internal State**: The token is not an opaque boolean flag; it carries two critical data structures:
  1. $LN[1..N]$: An array recording the request sequence number of node $j$'s **most recently executed critical section entry**.
  2. $Q$: A FIFO queue containing IDs of nodes waiting for the token.
- **Token Transfer Logic**: Upon leaving the critical section, the token holder sets $LN[i] = RN[i]$. It then iterates over all nodes $j$: if $RN[j] == LN[j] + 1$ and $j \notin Q$, node $j$ is appended to $Q$. Finally, if $Q$ is non-empty, the token (along with $LN$ and $Q$) is transmitted point-to-point directly to the node at the head of $Q$.

### Why it matters

1. **Optimal $N$ Message Bound**:
   Compared to permission-based algorithms like Ricart-Agrawala which require $2(N-1)$ messages per critical section entry (requests to all peers plus replies from all peers), Suzuki-Kasami slashes this to at most $N$ messages ($N-1$ broadcast requests plus $1$ point-to-point token transfer). Furthermore, if a node makes multiple consecutive requests while still retaining the token, the synchronization message cost is **0**.

2. **Differentiating Fresh Petitions from Stale Network Delays**:
   In asynchronous networks, old messages can be delayed arbitrarily. Suzuki-Kasami solves message replay vulnerability through algebraic comparison between the token's $LN$ array and the node's local $RN$ array ($RN[j] == LN[j] + 1$). If $LN[j] \ge RN[j]$, the holder provably knows the request has already been satisfied and safely ignores it, preventing duplicate executions and deadlocks.

3. **State-Carrying Capability Model**:
   Unlike naïve Token Ring protocols where an idle token cycles continuously through nodes wasting network bandwidth, Suzuki-Kasami moves the token strictly on-demand. By embedding the waiting queue $Q$ and execution log $LN$ directly inside the capability token, it pioneered the pattern of *state-carrying distributed tokens*, which heavily influences modern lock migration, distributed lease handoffs, and concurrent task scheduling systems.

_Metaphor mapping_

- Nine Daoist sword pavilions in Kunlun → Autonomous nodes in a distributed system (*Distributed Nodes*)
- Flying Crucible in the central void → Shared exclusive resource / critical section (*Critical Section*)
- Catastrophic explosion rule if two pavilions ignite → Mutual exclusion safety invariant (*Mutual Exclusion / Safety Invariant*)
- Ethereal talisman "Gilded Tally" → Exclusive privilege token (*Privilege Token*)
- Stone steles outside pavilions recording arrow numbers → Local request array tracking highest seen sequence numbers (*$RN$ Array*)
- Eight spiritual arrows broadcast to all peaks → Broadcast request messages sent to all other nodes (*$N-1$ Request Broadcast*)
- Nine-grid jade matrix on the reverse of the tally → Array within token recording sequence numbers of served requests (*$LN$ Array*)
- Swirling mist groove inside the tally → FIFO queue of waiting nodes maintained within the token (*Token Queue $Q$*)
- Checking "seven is strictly greater than six" → Evaluating $RN[j] == LN[j] + 1$ to verify true request freshness
- Golden ray flying directly to the Third Pavilion → Unicast point-to-point transfer of the token (*Point-to-Point Token Transfer*)
- Reusing the tally the next morning with zero arrows → Zero message overhead for consecutive entries by the same node
</section>
