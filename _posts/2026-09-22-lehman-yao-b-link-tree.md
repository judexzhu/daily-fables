---
layout: fable
title: "藏经阁的右侧门与封顶牌 · The Right Door and the Ceiling Plaque"
title_zh: "藏经阁的右侧门与封顶牌"
title_en: "The Right Door and the Ceiling Plaque"
concept: "Lehman-Yao B-link Tree"
tags: [databases, storage, performance]
illustration: /assets/art/2026-09-22-lehman-yao-b-link-tree.jpg
youtube_id: "vL43x-v5yI0"
---
<section class="zh" markdown="1">
京城的千卷阁是一座依山而建的九层八角木楼，里面收纳着四海赋税账册与户籍黄册。

每天破晓，来自各部衙门的文书、按察使的随从和赶考的士子蜂拥而入。要在数万卷案牍中找出一卷，全凭木楼严密的布局：顶层是天下总目，推开一扇门便能看清该往哪座副阁走；往下每一层都立着分目照壁，指引着哪间阁室藏着哪个编年；最底层则是密密麻麻的书架，卷轴整齐码在红木格子里。

文书们轻车熟路，从顶层依标寻阶而下，两柱香内便能捧出所需卷宗。

然而好景不常。天下文书日增，底层书室动不动就塞满了。旧制规定：某间书室一旦满溢，掌架司吏必须将该室闭门，将半数卷宗搬到隔壁新辟的书室中；同时，司吏必须派人沿楼梯一路飞奔回上一层，用铁锁把上层通往本室的梯口死死锁住，直到上层照壁上的路牌被重新涂改、将新书室的门牌号添上，方可开锁。

这规矩被称为"锁阶分架"。

每逢春纳或秋决，查档的人如潮水般涌来。东室满了要锁梯，西室满了也要锁梯；有时上层照壁涂改到一半，上上层的楼梯口也因为牵连改动被顺道锁死。整座千卷阁里，到处是提着灯笼在楼梯上焦急顿足的文书。有人在锁闭的梯口前高声催促，有人在昏暗的转角推搡争执。阁外排成长龙，阁内却因几把铁锁寸步难行。

那一年，总管书库的姚老教谕和巡阁使李学士立在顶楼，看着乱成一锅粥的木阶，决定废除所有铁锁。

"梯是供人走的，锁它做什么？"姚老教谕提笔写下两条极简的新令。

第一条：千卷阁内所有书室，门楣上皆悬一枚红铜打造的"封顶牌"，刻着本室内所藏卷宗的**最大编号**。

第二条：所有并列的书室之间，必须在右侧山墙凿开一扇三尺宽的"通联侧门"。无论谁在室内，推开右门便能直接迈入编号紧挨着它的下一间书室。

新规落定，满阁司吏皆感惶惑：若有人正在下楼查卷，而底层恰好分架，岂不要撞上错乱的空架？

数日后，秋粮大核，试炼如期而至。

辰时三刻，第七号书室爆满，司吏奉命分架。他不锁楼梯，也不惊动楼上，径直推开右侧预备好的七号别室，抱起编号在四百以上的半数卷宗往别室搬。随后，他将原本挂在七号室门上的"封顶八百"铜牌取下、移挂到别室门楣，又在七号室门楣挂上一块新牌——"封顶四百"，最后拔掉两室之间右侧门的门闩。

做完这一切，司吏才慢悠悠地提起茶壶，准备一会儿去楼上修改照壁。

就在此时，一名刑部缇骑飞奔下楼。他按着顶楼照壁的旧指引，一路直奔七号书室，要调取第四百五十二号卷宗。

缇骑跨进七号门槛，抬头一瞥，顿时愣住了：门楣红铜牌赫然写着"封顶四百"。他要找的卷子显然已经不在此室。

换作从前，缇骑只能退回楼梯、骂骂咧咧等待楼上更名开锁。但今日，他眼角扫到了右侧山墙上新开的侧门，门楣小标指向右侧。缇骑跨步穿过侧门，踏入七号别室。抬头再看，别室门楣铜牌正是"封顶八百"。他在架上一扫，第四百五十二号卷宗赫然在目，伸手取下，疾步离去。

整个过程中，上一层的楼梯口清风吹拂，无人驻足，甚至没人知道楼下的第七室刚刚裂成了两半。

直到半个时辰后，司吏喝完凉茶上了楼，在照壁上把七号室和别室的分野添补停当。即便碰上地动或是火烛，司吏来不及上楼修改照壁便跑出了楼，整座千卷阁的藏书依然丝毫不乱——只要顺着侧门往右走，任何卷宗都不会凭空消失。

从此千卷阁楼梯上再无锁链。无论阁中新增多少万卷黄册，上楼寻目者只管信步疾行，下楼查架者遇界便推右门。千万案牍流转如溪，而木阶终日静默通达。

——到这儿你大概已经认出来了：这就是数据库索引并发控制中的经典算法 *Lehman-Yao B-link Tree*（B-link 树）。

### 这是什么

在数据库的 *B+ Tree* 索引中，随着记录的插入，节点变满时需要分裂（*node split*）。传统 B+ 树为了防止并发读写读到半分裂的不一致状态，必须采用"锁耦合"或"锁爬行"（*lock crabbing / lock coupling*）：遍历时必须先拿到子节点的锁才能释放父节点的锁；而在发生结构修改（*Structural Modification Operation, SMO*）分裂时，甚至需要沿着回溯路径向上持有写锁（*exclusive lock*），将父节点、祖父节点一路锁死。这导致高层节点（尤其是根节点）成为极严重的并发瓶颈，极易引发死锁和吞吐量暴跌。

Philip Lehman 与 S. Bing Yao 于 1981 年提出的 *B-link Tree* 打破了这一僵局。它为树中每个节点（包括内部节点和叶子节点）增加了两个关键结构：
1. **High Key（封顶键）**：记录当前节点允许容纳的最大键值；
2. **Right Link Pointer（右兄弟指针）**：指向同一层紧邻的右侧兄弟节点。

当一个节点分裂时，算法将其拆解为原子且局部的两阶段：
- **第一阶段（节点分裂与横向链接）**：分配右兄弟节点，将右半部分键值与原 High Key 搬移至新节点，原节点的右指针指向新节点，原节点的 High Key 降为分裂键值。此时**完全不需要更新父节点**，分裂就在同层瞬间完成！
- **第二阶段（向父节点补录指针）**：作为一个独立的异步后置步骤，自底向上将新节点的指针插入父节点。

最为精妙的是**并发读完全不需要加任何读锁（甚至不需要持有父节点的锁）**：读者沿着旧的父节点指针降落到原节点后，只需对比目标键与该节点的 *High Key*。如果目标键大于 High Key，说明该节点刚刚经历了分裂，读者无需回退或重试，直接**沿着右兄弟指针向右走一步**即可找到目标数据！

### 为什么重要

*Lehman-Yao B-link Tree* 是现代高性能关系型数据库索引并发控制的奠基石。最著名的代表便是 **PostgreSQL 的核心 B 树索引引擎（`src/backend/access/nbtree/`）**——其并发读写架构完全构筑在 Lehman-Yao 算法之上（并在此基础上引入了 Page LSN 和 WAL 机制来支撑崩溃恢复）。

它的革命性意义在于：
1. **消除读操作的锁竞争**：查找遍历内部节点时完全不需要加锁（*lock-free descent*），不会对并发写入造成阻碍，彻底解决了根节点与顶层索引页面的锁争用。
2. **消除了遍历过程中的死锁（Deadlock-free）**：所有遍历只允许两种方向——自顶向下，或者在同一层自左向右横移，永远不会向上回溯或逆向查找，从几何拓扑上斩断了死锁环路。
3. **将系统崩溃与结构修改解耦**：如果在节点分裂完成、父节点尚未更新时系统突然宕机，由于右指针和 High Key 已经持久化在磁盘页面上，重启后索引依然是完整且可寻址的，后续遍历依然能顺着右指针找到数据，不会产生悬挂断链。

从 PostgreSQL 到 WiredTiger（MongoDB 的底层存储引擎），再到各类现代分布式数据库（如 CockroachDB 的存储索引层），当你在每秒数十万并发读写下看到索引依然平稳如水，其背后都在默默运行着这扇"右侧门"与这枚"封顶牌"。

_隐喻对应表_

- 依山而建的九层千卷阁 → 多层 *B+ Tree* 索引结构（根节点、内部节点、叶子节点）
- 顶层天下总目与各层照壁 → 内部节点中的路由键与子节点指针（*internal node index pages*）
- 底层书架红木格子中的卷轴 → 索引叶子节点中存储的实际键值与数据指针（*leaf node tuples*）
- 依标寻阶而下的文书 → 在索引树上执行查询的并发工作线程（*reader threads*）
- 旧制的"锁阶分架"与铁锁 → 传统 B 树并发控制中的锁爬行与写排他锁（*lock crabbing / exclusive locks*）
- 门楣上的红铜"封顶牌" → 节点中的 *High Key*（记录该节点允许的最大键值边界）
- 右侧山墙凿开的三尺"通联侧门" → 同层节点之间的 *Right Link Pointer*（横向单向链表）
- 分架后不改上层照壁、先开侧门移书 → 局部完成节点分裂与同层挂链，父节点指针后置异步插入
- 缇骑见牌超标、跨入右侧门直达目标 → 读者探测到 Key > High Key 时顺着右指针横移（*concurrent right-walk*）
- 司吏迟迟未改照壁但藏书仍不乱 → 崩溃一致性（即便父节点未更新，B-link 树拓扑依然完好可寻）
</section>
<section class="en" markdown="1">
The Grand Archive of the capital was a nine-story, octagonal pagoda perched against the hillside, housing centuries of empire-wide tax rolls, household registers, and judicial edicts.

Every dawn, clerks from the ministries, retinues of inspecting censors, and scholars preparing for imperial examinations poured through the lower gates. Navigating through tens of thousands of scrolls was made possible only by the tower's meticulous geometry: the top floor displayed the imperial master index, directing visitors to the right wing; descending floors featured carved cedar screens showing which archive hall held which historical eras; and the ground floors were packed with floor-to-ceiling rosewood cubbies holding the scrolls themselves.

Experienced couriers could descend from the pinnacle along the stairs and emerge clutching the requested docket in less than two incense sticks of time.

Yet prosperity brought chaos. As imperial records multiplied, ground-floor chambers overflowed with increasing frequency. By ancient statute, whenever a chamber exceeded capacity, the archival bailiff was required to seal its doors, partition half the scrolls into an adjacent newly constructed hall, and dispatch runners to sprint up the stairs to padlock the staircase landing above. No one was permitted to descend into that quadrant until the directional signs on the landing above had been planed, repainted, and reinscribed with the new room numbers.

This procedure was known as "staircase latching."

During spring tax audits or autumn reviews, visitors flooded the archive like a tidal surge. An east room split locked an east staircase; a west room split locked a west staircase. Occasionally, as signs on the third landing were being rewritten, the stairs leading to the fourth landing had to be locked in sympathy to prevent inconsistent descents. Across the Grand Archive, runners stood stranded on stair landings, lantern flames flickering as tempers flared. Angry shouts echoed down the stairwells while the courtyard outside grew hopelessly congested—all because of iron padlocks on wooden steps.

That year, Master Yao, the venerable keeper of archives, and Academician Lehman stood on the top balcony, gazing down at the paralyzed stairs, and decided to banish every padlock from the building.

"Stairs are made for walking," Master Yao remarked. "Why cage them?" He dipped his brush and issued two concise decrees.

First: every archive room must hang a polished bronze plaque above its lintel, inscribed with the **maximum catalog number** permitted within that room. This was called the "Ceiling Plaque."

Second: through the right stone wall of every room, masons must cut a three-foot arched passageway leading directly into the adjacent room on its right. The "Right Door."

The scribes and junior archivists were terrified: "If a runner descends the stairs while a room below is being divided, won't they barge into empty shelves and fall into confusion?"

Days later, the autumn grain reckoning arrived, bringing the ultimate test.

At mid-morning, Room Seven filled to bursting. The bailiff stepped in to perform a partition. He did not touch the stairs above, nor did he alert the floor above. He simply unlatched the freshly prepped Room Seven-B on the right, moved all scrolls numbered 401 through 800 into the new room, transferred the old plaque reading "Ceiling 800" to Seven-B's lintel, hung a new plaque reading "Ceiling 400" over Room Seven, and drew the bolt on the connecting right door.

Having finished this, the bailiff poured himself a bowl of tea, planning to amble upstairs later when the corridors quieted to update the landing directory.

At that exact instant, an imperial bailiff rushed down the stairs, following the landing sign that still directed scroll #452 to Room Seven.

He dashed through Room Seven's doorway, glanced up, and froze: the bronze plaque above the door now read "Ceiling 400." The document he sought was clearly no longer in this room.

Under the old rules, he would have had to retreat in frustration to the landing and wait for painters to unlock the gate. But catching sight of the newly carved archway in the right wall, he stepped straight through the side door into Room Seven-B. Looking up, he saw the plaque reading "Ceiling 800." Within seconds, his fingers brushed against scroll #452. He pulled it from the shelf and departed at a run.

Throughout the entire episode, the stairs on the floor above remained open to the morning breeze. No runner had paused. Not a single person upstairs even realized that Room Seven had just divided in two.

An hour later, the bailiff finished his tea, climbed the stairs, and repainted the directional board at his leisure. Even if an earthquake had struck and the bailiff had fled the building before updating the landing sign, not a single scroll would have been lost or hidden—readers venturing into Room Seven would simply step through the right doorway and find their prize.

From that day forward, iron padlocks were never again clamped upon the stairs of the Grand Archive. No matter how many thousands of scrolls poured into the repository, searchers raced downward unimpeded, stepping through the right doors whenever they crossed a boundary. The river of records flowed without pause, upon staircases that remained forever quiet, unlocked, and free.

— By now you've probably recognized it: this is the celebrated *Lehman-Yao B-link Tree* concurrency algorithm for database index management.

### What it is

In database *B+ Tree* indexing, insertions eventually cause storage nodes to overflow, necessitating a *node split*. In classical B+ tree concurrency control, avoiding inconsistent reads during structural modification operations (*SMOs*) required *lock crabbing* (or *lock coupling*): a traversing query held an exclusive or shared lock on the parent node until it successfully acquired a lock on the child node. During node splits, writers had to propagate exclusive locks upward toward the root, locking entire ancestor paths. This created catastrophic bottlenecks at root and top-level pages, causing extreme lock contention and deadlock risks under heavy concurrent workloads.

Philip Lehman and S. Bing Yao revolutionized B-tree concurrency in 1981 with the *B-link Tree*. They augmented every node in the tree (both internal routing nodes and leaf pages) with two minimal structural elements:
1. **High Key**: The upper bound of the key range stored in that node's subtree.
2. **Right Link Pointer**: A horizontal pointer connecting the node to its immediate right sibling at the same level.

When a node overflows and splits, Lehman-Yao decouples the structural modification into two atomic, independent phases:
- **Phase One (Half-Split & Right Link)**: A new right sibling page is allocated. The upper half of the keys and the original High Key are moved into the new page. The original node points its right link pointer to the new sibling, and lowers its High Key to the split boundary. Crucially, **the parent node is not touched or locked yet**. The split is complete and safe at the current tree level.
- **Phase Two (Parent Insertion)**: Inserting the new sibling's downlink into the parent node is performed as a separate, asynchronous bottom-up step.

Most brilliantly, **readers descend through the tree without acquiring any locks on internal nodes**. If a reader descends into a node using an outdated routing entry and finds that the search key exceeds the node's *High Key*, it recognizes that a concurrent split occurred. Instead of aborting, backtracking, or waiting for parent locks, the reader simply **follows the right link pointer horizontally** to the sibling page, locating the desired key transparently!

### Why it matters

The *Lehman-Yao B-link Tree* is the bedrock of concurrency in modern relational database storage engines. Its most famous real-world implementation is **PostgreSQL's core B-tree indexing engine (`src/backend/access/nbtree/`)**, which implements Lehman-Yao B-link trees augmented with write-ahead logging (*WAL*) and page log sequence numbers (*LSNs*) for crash recovery.

Its core architectural advantages are profound:
1. **Lock-Free Index Traversal**: Readers traverse internal index nodes without acquiring shared locks (*latch-free descent*), completely removing read-write contention on root and higher-level internal pages.
2. **Deadlock Immunity**: Traversal strictly follows two directions: top-to-bottom descending, or left-to-right horizontal walking. It never backtracks upward or walks leftward, topologically precluding circular wait conditions and guaranteeing deadlock freedom.
3. **Resilience to System Crashes**: If the database crashes after Phase One (node split completed) but before Phase Two (parent downlink inserted), the index on disk remains structurally sound and fully searchable. The right link pointer ensures that subsequent queries can always discover the split data without corrupting query correctness.

From PostgreSQL to WiredTiger (the default storage engine in MongoDB) to CockroachDB's underlying indexing structures, whenever an enterprise database handles hundreds of thousands of concurrent index lookups and inserts with sub-millisecond latencies, it is the quiet elegance of Lehman and Yao's "Right Door" and "Ceiling Plaque" keeping the path open.

_Metaphor mapping_

- Nine-story Grand Archive on the hillside → Multi-level *B+ Tree* index hierarchy (root, internal nodes, leaves)
- Top master index and landing directional screens → Routing keys and downlink pointers in *internal node index pages*
- Rosewood cubbies packed with scrolls on lower floors → Data records and index tuples in *leaf pages*
- Clerks and couriers descending the stairs → Concurrent query reader threads (*readers*) traversing the index tree
- "Staircase latching" and iron padlocks → Traditional *lock crabbing / lock coupling* and exclusive structural modification locks
- Bronze "Ceiling Plaque" above each doorway → Node *High Key* (the strict upper bound of keys in that subtree)
- Three-foot arched "Right Door" in the side wall → Horizontal *Right Link Pointer* connecting sibling nodes at the same level
- Splitting room and unlatching right door before updating upstairs signs → Two-phase split (completing local sibling link before updating parent)
- Courier seeing High Key exceeded and walking through right door → Concurrent reader detecting Key > High Key and executing a *right-walk*
- Master bailiff delaying upstairs sign updates without losing scrolls → Crash consistency (decoupling parent update from node accessibility)
</section>
