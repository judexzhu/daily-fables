---
layout: fable
title: "白马三令与绝谷封仓 · The Three Flags of the Silver Pass"
title_zh: "白马三令与绝谷封仓"
title_en: "The Three Flags of the Silver Pass"
concept: "Three-Phase Commit (3PC Protocol)"
tags: [distributed-systems]
illustration: /assets/art/2026-10-08-three-phase-commit.jpg
---
<section class="zh" markdown="1">
太行陉最险要的关隘深处，依山势筑有三座大仓：东崖的甲字粮仓、西涧的乙字草料库、南坡的丙字铁铠司。

三仓相隔十数里悬崖绝壁，各有一位粮料官把守。关外胡骑日夜窥伺，若要开闸发饷或封仓固守，必须三仓步调划一，绝不可有一仓开门而另一仓自焚。三仓正中有一座立于鹰嘴巨岩上的“平准调度台”，由关令执掌，专司向三方发令。

昔年关中用的规矩极简单：关令先派快马向三仓探问“仓廪可齐备？”；三仓若皆回报“齐备”，关令便射出响箭命令“一齐封关落锁”；若有任一仓回报“缺粮”或毫无回音，关令便鸣金收兵，三仓各自撤防。

这法子行了数年，直到有一年隆冬大雪漫山。

那一夜，关令向三方探问，三位粮料官皆查点无误，快马回报“齐备”。三仓在寒风中整装待发，死死按住锁钥。然而，就在关令握起铜弓正要射出响箭的一瞬，绝壁上方轰然崩塌，百丈冰雪将平准台连同关令整个吞没在雪海之中！

调度台骤然沉寂，鹰嘴岩上一片漆黑。

三座大仓顿时陷入无边的死寂与煎熬。三位粮料官心里清楚：自己方才都回了“齐备”，但谁也不知道另外两仓究竟回了什么；更不知道关令在遇难前的最后一刹那，到底有没有把响箭射向夜空。

若擅自开锁放行，万一另外两仓见到了暗箭早已封锁，便是军法斩首；若死守原地，可关令已死，再无第二支箭会来。三座大仓守军冻毙在风雪里，整整持戈伫立了三天三夜，寸步不敢离，所有的粮秣运转尽皆瘫痪。

这就是令历代兵家闻风色变的“绝壁死滞”。

第二年春，新任关令接管险隘，痛定思痛，在平准台前立下一条严苛无比的军令：**“前路未明，断不可一跃成决；关塞存亡，皆在白马三令。”**

新规矩分设三道令旗与三次往返：

**其一，勘定令（Can-Commit?）。**
关令若欲封关，先向三仓各发一枚朱漆探竹：“仓中可堪封锁？”
三仓各自清点库银草料，若有病卒缺粮，立回“不克”；若一切妥当，立即回报“堪备”。在此期间，任何一仓回了“不克”或迟迟未归，关令立刻鸣金弃令，三仓各自散去，全不损耗。

**其二，暂定令（Pre-Commit）。**
若三仓皆回报“堪备”，关令绝不在此时直接下令落锁，而是向三方同时打出一杆**“鹅黄半规旗”**。
此旗一出，意为：“三方探问皆已通过，全山无一人反对；尔等做好封关准备，但暂不得转动铁锁！”
三仓见到黄旗，便知全军心意已齐，再无反悔余地，各自在木账上盖下“拟定封印”，随后向关令打出“应诺”角声。

**其三，决断令（Do-Commit）。**
唯有当关令清清楚楚听齐了三处关隘传来的“应诺”角声，确认三仓皆已进入准备之态，方才从鹰嘴岩上升起大红的**“朱雀封峦旗”**！
三仓见到红旗，同时转动绞盘，万钧巨石轰然落下，铁闸锁死。

三位粮料官起初不解：“两度通报已能知晓人心，何苦多费这一番周折，非要在黄旗与红旗之间再添一道暂定之令？”

关令笑而不答。

直到深秋时节，胡骑再次犯境，风沙遮天蔽日。关令打出朱漆探竹，三仓皆回报“堪备”；关令随即升起鹅黄半规旗，三仓各自盖印领命，角声四起。

就在此时，一支冷箭从乱军中穿云而过，正中关令咽喉！调度台再次失陷，红旗未及升起，关令便已坠崖气绝！

若是往年，三仓此刻必然再次陷入无休无止的死滞。但这一回，局势截然不同。

按照新规，三仓久久不见主台动静，自知关令有变。此时，按军律，若主台失联超过半个时辰，三仓之中年资最长的甲字粮仓粮料官，立即自动升为“代掌令”。

甲字粮官站在烽火台上极目四望，向乙、丙二仓吹响铜号询问彼此境况：
“我已见黄旗，盖下拟定之印；尔等何态？”

乙仓长鸣回应：“我亦见黄旗，盖下拟定之印！”
丙仓亦长鸣：“我亦领得黄旗！”

甲字粮官闻声，当机立断：**“关令虽亡，但黄旗已至三方！这说明三仓在第一轮皆报了堪备，天下绝无任一处曾报‘不克’；更无任一处曾鸣金撤防！全军皆在门槛之内，三方进退划一，立转绞盘，封山！”**

军令一出，三仓同时落锁，险隘固若金汤！

更神妙的是：假若当时关令是在第一轮探竹之后、黄旗未发之时便中箭身亡，甲字粮官向各方询问时，只要发现任一仓“未见黄旗”，甚至有人曾报过“不克”，代掌令便能确信——此前绝不可能有任何一仓擅自落锁封山！于是代掌令只需一声令下：“前令未成，全军撤防散营！”三仓便能安然退出，无一人被困在绝壁死局之中。

一道黄旗，如同一道筑在万丈悬崖与绝顶殿堂之间的**安全廊道**。它将所有人推入一个进退有据的缓冲之地：踏入此廊者，知晓天下无人退却；未入此廊者，知晓天下无人敢越雷池。

——到这儿你大概已经认出来了：这座孤峰上的三座险隘大仓，正是分布式系统里的各个数据副本与参与者（*Participants / Cohorts*）；巨岩上的关令与代掌令，正是分布式事务中的协调者与新选举协调者（*Coordinator & Backup Coordinator*）；那令军民苦等三日三夜的旧规矩，正是经典的两阶段提交（*Two-Phase Commit, 2PC*）中因协调者宕机而引发的臭名昭著的**阻塞问题（Blocking Problem）**；而这道绝不轻举妄动的白马三令，正是图灵奖得主级分布式理论家 Dale Skeen 于 1981 年提出的经典非阻塞原子提交协议——**三阶段提交（Three-Phase Commit, 3PC）**。

### 这是什么

*Three-Phase Commit*（三阶段提交，简称 *3PC*）是分布式系统与数据库理论中为了解决经典两阶段提交（*2PC*）在协调者单点故障时可能发生**无限期阻塞（Indefinite Blocking）**而提出的强一致性原子提交协议。

在传统的两阶段提交（*2PC*）中，协议仅包含两个阶段：
1. **投票阶段（Prepare / Voting Phase）**：协调者询问所有参与者能否提交，参与者写入本地日志并投出 `Yes` 或 `No`；
2. **提交阶段（Commit Phase）**：若全员 `Yes`，协调者发出 `Commit`；若有任一 `No`，协调者发出 `Abort`。

2PC 的致命缺陷在于：**状态转换存在断层**。当参与者投出 `Yes` 之后，它直接进入不确定状态（*In-Doubt State*）。此时如果协调者突然发生故障宕机，且网络发生故障，存活的参与者根本无法判断协调者在宕机前是否已经发出了 `Commit`。由于无法获知全局决议，所有持有本地排他锁的参与者只能无限期悬挂等待协调者重启恢复，系统资源被彻底锁死。

3PC 通过引入**第三个中间状态与阶段**彻底打破了这种阻塞僵局。它将原有的提交阶段一分为二，构成了三个严格的通信轮次：

1. **Can-Commit 阶段（询问投票）**：
   - 协调者向所有参与者发送 `CanCommit` 请求；
   - 参与者检查本地资源（事务日志、锁竞争、约束），回复 `Yes` 或 `No`；
   - 此时若有参与者超时或回复 `No`，协调者直接向全员发送 `Abort`。

2. **Pre-Commit 阶段（预提交过渡）**：
   - 若协调者在第一阶段收齐了所有参与者的 `Yes`，向全员发送 `PreCommit` 指令；
   - 参与者收到 `PreCommit` 后，确认事务必定不会被中止，在本地写入准备日志，进入 `Pre-Committed` 状态，并向协调者回复 `Ack`；
   - 这一阶段建立了全局核心不变量：**一旦有任何节点进入了 Pre-Committed 状态，就证明全集群在第一轮投票中没有任何节点投出 No，事务绝对不可能再发生回滚！**

3. **Do-Commit 阶段（最终提交执行）**：
   - 协调者收齐所有参与者的 `Ack` 后，正式广播 `DoCommit`；
   - 参与者正式提交本地事务，释放资源锁，并回复最终完成响应。

在 3PC 状态机中，通过在“准备”与“提交”之间插入一个缓冲区（*Pre-Commit*），**去除了任何包含“可提交状态”与“可中止状态”同时并存的不确定临界区**。当协调者发生故障崩溃时，集群选举出新的备用协调者，新协调者只需向存活参与者收集当前状态：
- 若**至少有一个存活参与者处于 Pre-Committed 状态**：新协调者即可断定全集群此前全部同意，绝无节点可能已经中止，可以安全推进至 `Commit`；
- 若**所有存活参与者皆处于初始或等待状态（Uncommitted）**：新协调者可以断定绝无任何节点曾经执行过 `Commit`，可以安全地将事务全体判定为 `Abort`；
- 集群从而在无需阻塞等待原协调者复活的情况下，自主恢复并推进事务。

### 为什么重要

3PC 在分布式系统发展史上具有里程碑式的理论地位，是理解现代分布式共识与容错架构的必经桥梁：

1. **彻底攻克故障停机下的阻塞难题（Non-blocking under Fail-Stop）**：
   在纯故障停机（*Fail-Stop / Crash-Stop*，即节点只会死机、网络不会出现无法愈合的分区且具有确定性超时检测）的理想模型下，3PC 证明了原子提交协议可以做到**完全非阻塞（Non-blocking）**。参与者无论在哪个阶段遭遇协调者崩溃，都能依据自身及存活同伴的状态推导全局真理，无需悬挂锁资源。

2. **揭示了原子提交（Atomic Commit）与分布式共识（Consensus）的本质边界**：
   尽管 3PC 在理论上极为优雅，但它在工程实践中有一个致命阿喀琉斯之踵：**它无法抵御网络分区（Network Partition）与脑裂**。如果在 Pre-Commit 阶段网络发生不对称分区，分区两侧的参与者可能分别超时做出 `Commit` 与 `Abort` 两个互相矛盾的决议，从而彻底破坏事务的一致性（*Safety Violation*）。这一理论困境直接催生了后世对 **Paxos**、**Raft** 以及具备 Quorum 选举的分布式事务引擎的探索——工程师们最终认识到，在真实世界的不可靠异步网络中，单靠固定轮次的 3PC 无法同时实现可用性与安全性，唯有引入基于多数派重叠的共识协议，方能终结分区的噩梦。

3. **现代分布式两阶段与预准备模式的思想雏形**：
   3PC 提出的“先预提交通报、再最终落锁执行”的两步渐进确认哲学，深刻启发了后来的许多高可用协议：
   - **PBFT（实用拜占庭容错）**中的 `Pre-Prepare -> Prepare -> Commit` 三步确认机制；
   - **Raft** 日志复制中通过 Follower 确认半数匹配后才在下一次心跳通知状态机提交的渐进式水位机制；
   - **XA / JTA** 与现代分布式数据库中高级补偿事务的超时解挂与对账检查点。

_隐喻对应表_

- 绝壁险隘中的甲乙丙三座大仓与守将 → 分布式事务的各个参与者节点（*Participants / Cohorts*）
- 鹰嘴巨岩上的关令与平准调度台 → 分布式事务的发起者与协调者（*Transaction Coordinator*）
- 冰雪崩塌吞没调度台导致全军死守三日夜 → 经典 2PC 中协调者崩溃引发的参与者无限期持锁阻塞（*2PC Blocking Problem*）
- 第一令：关令发出的朱漆探竹与各仓回报堪备 → 3PC 的询问阶段（*Can-Commit Phase & Voting*）
- 第二令：关令升起的鹅黄半规旗与各仓盖印应诺 → 3PC 的预提交阶段（*Pre-Commit Phase & Invariant Establishment*）
- 踏入黄旗廊道知晓天下无退却的安全缓冲 → 3PC 消除不确定临界状态的核心安全不变量（*Non-blocking State Invariant*）
- 第三令：关令听齐角声后升起的朱雀封峦旗 → 3PC 的最终提交与落锁执行（*Do-Commit Phase*）
- 关令中箭后甲字粮官升任代掌令并吹角询问各方 → 备用协调者接管与终止恢复协议（*Backup Coordinator Election & Termination Protocol*）
- 见黄旗则全军同封、未见黄旗则全军撤防的决断 → 依据存活节点状态安全推进提交或回滚（*Safe State-based Resolution*）
- 迷雾风沙中两仓断联可能互生误判的隐患 → 3PC 在真实异步网络分区下可能发生脑裂一致性破坏的理论局限（*Vulnerability to Network Partitions*）
</section>
<section class="en" markdown="1">
Deep within the most perilous gorge of the Taihang Mountains stood three heavily fortified storehouses built into the living rock: the Alpha Granary clinging to the eastern precipice, the Beta Fodder Depot nestled in the western ravine, and the Gamma Armory anchored to the southern slope.

A dozen leagues of sheer cliffs and bottomless chasms separated the three bastions, each governed by its own supply warden. Beyond the mountain passes, nomadic horsemen patrolled day and night. Whenever rations had to be issued or the mountain passes sealed against siege, all three garrisons had to move in absolute unison; if one gate was barred while another was breached or torched, the entire bastion would crumble. Perched atop the Eagle Beak Crag midway between them sat the Grand Dispatch Tower, where the Pass Commandant commanded the gorge by sending dispatches in all directions.

In earlier years, the protocol was deceptively simple: the Commandant would dispatch swift couriers to each garrison asking, "Are the provisions secured?" If all three replied "Secured," the Commandant would release a whistling signal arrow, ordering them to drop the iron portcullises simultaneously. If even a single garrison replied "Deficient" or sent no word at all, the gong would sound for a total retreat, and all three would stand down.

The rule held for years—until the winter of the great blizzard.

That night, the Commandant dispatched his queries, and all three wardens inspected their reserves and sent word: "Secured." At the gates, the soldiers braced against the howling wind, keys poised in heavy padlocks. But at the very moment the Commandant drew his brass bow to shoot the signal arrow, a massive avalanche sheared the cliff above. A hundred fathoms of snow and shattered rock swept the Grand Dispatch Tower and the Commandant into the abyss.

Darkness fell over Eagle Beak Crag. Absolute silence enveloped the gorge.

The three storehouses were plunged into an agonizing limbo. Each warden knew his own garrison had reported "Secured." Yet none knew what the other two had reported, nor whether the Commandant, in his dying breath, had managed to release the signal arrow into the storm.

To unlock the gates and disperse might mean treason if the others had seen an arrow and sealed their doors; to drop the portcullis unilaterally might leave comrades trapped outside. And with the Commandant dead, no second arrow would ever come. For three days and three nights, soldiers stood frozen in their armor, spears gripped in numbed hands, paralyzed in place while transport across the mountain completely ground to a halt.

This was the legendary "Precipice Deadlock."

The following spring, a newly appointed Commandant arrived at the Eagle Beak Crag. Determined to break the curse, he carved a stark military decree onto stone: **"No decision shall leap into finality before the threshold is made clear; the survival of the pass rests upon the Three Flags of the White Steed."**

The new protocol mandated three distinct flags and three consecutive round trips:

**First: The Inquiry Flag (Can-Commit?).**
Whenever the gates were to be barred, the Commandant sent a cinnabar tally to each storehouse: "Can your fortress endure the seal?"
Each warden inspected his stores. If soldiers were ill or supplies lacking, they returned "Deficient"; if fully prepared, they returned "Prepared." If any garrison replied "Deficient" or timed out, the Commandant sounded the bronze gong to abort, and all three stood down without incurring cost.

**Second: The Interim Flag (Pre-Commit).**
If all three garrisons returned "Prepared," the Commandant strictly refrained from dropping the portcullises. Instead, he unfurled a **"Half-Crescent Saffron Banner."**
This flag signaled: "The inquiry has cleared across all quarters; not a single bastion objected. Prepare for the seal, but do not yet turn the iron key!"
Upon seeing the yellow banner, each garrison knew that unanimity was reached and no retreat was possible anywhere in the pass. Each stamped "Preliminary Seal" into its ledger and sounded a brass horn back toward Eagle Beak Crag in acknowledgment.

**Third: The Final Decree (Do-Commit).**
Only when the Commandant had clearly heard the horn blasts from all three garrisons—confirming that every bastion was poised at the threshold—did he raise the magnificent **"Vermillion Phoenix Banner"** high above Eagle Beak Crag!
At the sight of the red banner, winches groaned across the three bastions, massive counterweights dropped, and the great portcullises slammed shut in unison.

The wardens initially questioned the extra labor: "Two rounds of dispatches already make intentions clear. Why waste time introducing this intermediate yellow flag between query and action?"

The Commandant merely smiled.

The answer arrived late that autumn, when horsemen assaulted the pass beneath blinding dust storms. The Commandant dispatched the cinnabar tallies, and all three garrisons answered "Prepared." He then raised the Half-Crescent Saffron Banner; all three stamped their ledgers and sounded their horns.

At that exact moment, a stray iron-tipped arrow whistled through the dust and pierced the Commandant's throat. The dispatch tower fell silent once more. The Commandant tumbled into the gorge before the red banner could be unfurled!

In previous years, this would have triggered the same unending deadlock. But under the new decree, the outcome was entirely different.

When half an hour passed without word from the tower, the warden of Alpha Granary—as the senior officer designated by decree—stepped forward as the "Acting Commandant."

Standing upon his watchtower, the Alpha warden sounded his horn across the ravine to interrogate his peers:
"I hold the Saffron Banner and have stamped the Preliminary Seal! What is your status?"

Beta Depot answered with two horn blasts: "I hold the Saffron Banner and have stamped the Preliminary Seal!"
Gamma Armory echoed: "The Saffron Banner is in hand!"

The Alpha warden hesitated not a second: **"The Commandant has fallen, but the Saffron Banner reached every gate! This proves that every bastion voted 'Prepared' in the first round; not a single post reported 'Deficient,' and no retreat was ever sounded. We all stand across the threshold together. Drop the portcullises and seal the pass!"**

Winch cables snapped taut, iron gates crashed down, and the pass stood impregnable.

Even more remarkably: had the Commandant fallen earlier—after the cinnabar tallies but before the saffron banner was sent—the Alpha warden would have found upon questioning that none had received the yellow flag. He could then conclude with mathematical certainty that no garrison could have possibly dropped its portcullis. A single order to abort would release all three garrisons safely, without a single soldier trapped in deadlock.

The saffron banner acted as a **secure corridor** built between the abyss and the fortress. Entering that corridor proved that no one behind had retreated; remaining outside proved that no one ahead had leaped.

— By now you have likely recognized it: the three mountain bastions are the participating nodes (*Participants / Cohorts*); the Commandant and his designated successor atop Eagle Beak Crag are the transaction coordinator and backup coordinator (*Coordinator & Backup Coordinator*); the three-day paralysis that froze the garrison is the infamous **blocking problem** of the classic Two-Phase Commit (*2PC*); and the three deliberate flags of the White Steed are the non-blocking atomic commitment protocol formulated by Turing Award-level theorist Dale Skeen in 1981: the **Three-Phase Commit Protocol (3PC)**.

### What it is

The *Three-Phase Commit Protocol* (3PC) is a distributed transaction algorithm designed to eliminate the **indefinite blocking** vulnerability inherent in the classical Two-Phase Commit (2PC) protocol when a coordinator crashes during execution.

In classical Two-Phase Commit (2PC), execution proceeds in two phases:
1. **Prepare / Voting Phase**: The coordinator asks participants if they can commit. Participants log their intent and vote `Yes` or `No`.
2. **Commit Phase**: If all vote `Yes`, the coordinator broadcasts `Commit`; if any vote `No`, the coordinator broadcasts `Abort`.

The fatal flaw of 2PC lies in its **abrupt state boundary**. Once a participant votes `Yes`, it enters an uncertain state (*In-Doubt*). If the coordinator crashes while holding the decision and communication fails, surviving participants cannot deduce whether the coordinator issued a `Commit` before dying. Lacking global consensus, all participants holding local locks must block indefinitely until the failed coordinator recovers and replays its log.

3PC breaks this impasse by introducing a **third, intermediate state and phase**. It splits the commitment step into two distinct communication rounds, creating a three-stage pipeline:

1. **Can-Commit Phase (Inquiry & Vote)**:
   - The coordinator sends a `CanCommit` message to all participants;
   - Participants evaluate local resource availability (locks, disk buffers, constraints) and reply `Yes` or `No`;
   - If any participant votes `No` or times out, the coordinator broadcasts an immediate `Abort`.

2. **Pre-Commit Phase (Transition & Agreement)**:
   - If all participants voted `Yes`, the coordinator broadcasts `PreCommit`;
   - Upon receiving `PreCommit`, participants write an acknowledgment to their write-ahead log, enter the `Pre-Committed` state, and reply with an `Ack`. They know with certainty that the transaction will not abort;
   - This phase establishes the core safety invariant: **If any node has entered the Pre-Committed state, every node in the cluster must have voted Yes in the initial inquiry, meaning an Abort is globally impossible.**

3. **Do-Commit Phase (Final Execution & Release)**:
   - Upon gathering `Ack` messages from all participants, the coordinator broadcasts `DoCommit`;
   - Participants commit changes locally, release transaction locks, and return final completion acknowledgments.

By wedging the `Pre-Commit` state between preparation and final commitment, 3PC **eliminates any protocol state where both a commit decision and an abort decision are simultaneously plausible**. If the coordinator crashes, a newly elected backup coordinator queries surviving participants:
- If **at least one surviving participant is in the Pre-Committed state**: The backup coordinator knows that all participants previously agreed to commit and that none could have aborted; it safely proceeds to `Commit`.
- If **all surviving participants are in the initial or uncommitted state**: The backup coordinator knows that no participant could have possibly executed a `Commit`; it safely aborts the transaction.
- The cluster resumes operation and completes the transaction without blocking on the recovery of the original coordinator.

### Why it matters

The Three-Phase Commit protocol occupies an essential place in distributed systems theory, bridging classic database transactions and modern consensus protocols:

1. **Provably Non-Blocking under Fail-Stop Models**:
   Under a pure fail-stop failure model (where nodes fail only by crashing and network timeouts provide reliable crash detection without partitions), 3PC proves that atomic commitment can be made completely non-blocking. Surviving nodes can always deduce the global state and reach a safe decision autonomously.

2. **Illuminating the Boundaries of Atomic Commit and Consensus**:
   While theoretically elegant, 3PC harbors an engineering Achilles' heel: **it is vulnerable to network partitions**. If an asymmetric network partition occurs during the Pre-Commit phase, nodes on opposite sides of the partition may timeout and independently reach contradictory decisions (`Commit` on one side, `Abort` on the other), resulting in catastrophic split-brain state corruption. This realization spurred the development of quorum-based consensus engines such as **Paxos** and **Raft**—proving that in asynchronous, partitionable networks, fixed-round atomic commitment must yield to quorum intersection.

3. **Architectural Archetype for Progressive Consensus**:
   The multi-stage progression introduced by 3PC (querying intent, securing agreement, executing commit) serves as the architectural blueprint for modern high-availability protocols:
   - **Practical Byzantine Fault Tolerance (PBFT)**: Utilizes the `Pre-Prepare -> Prepare -> Commit` three-stage pipeline to establish consensus among distrusted nodes;
   - **Raft State Machine Commitment**: Requires followers to replicate an entry before advancing the commit index on subsequent leader heartbeats;
   - **Modern Distributed Database Engines**: Informs timeout recovery and non-blocking state reconstruction in transactional engines.

_Metaphor mapping_

- The three mountain bastions (Alpha, Beta, Gamma) and their wardens → Participating nodes in a distributed transaction (*Participants / Cohorts*)
- The Pass Commandant atop the Eagle Beak Crag → The transaction coordinator (*Transaction Coordinator*)
- The avalanche burying the dispatch tower and freezing soldiers for three days → The classic 2PC coordinator crash causing indefinite blocking (*2PC Blocking Problem*)
- The First Flag: Cinnabar tallies and the wardens' "Prepared" responses → The inquiry round of 3PC (*Can-Commit Phase & Voting*)
- The Second Flag: The Half-Crescent Saffron Banner and preliminary seal stamping → The pre-commit round establishing unanimity (*Pre-Commit Phase & Invariant Establishment*)
- The secure corridor of the saffron banner where no one retreats → The 3PC safety invariant eliminating uncertainty (*Non-blocking State Invariant*)
- The Third Flag: The Vermillion Phoenix Banner dropped upon hearing all horns → The final commit and lock release (*Do-Commit Phase*)
- The Alpha warden assuming command and questioning peers after the Commandant's death → Backup coordinator election and termination protocol (*Backup Coordinator Election & Termination Protocol*)
- Committing if the yellow banner is held or aborting if absent → Safe state-based resolution by surviving nodes (*Safe State-based Resolution*)
- Dust storms causing miscommunication across ravines → 3PC's fatal vulnerability to network partitions and split-brain decisions (*Vulnerability to Network Partitions*)
</section>
