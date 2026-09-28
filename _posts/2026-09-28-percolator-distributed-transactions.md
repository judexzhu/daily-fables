---
layout: fable
title: "九仓分发与连环宗契 · The Nine Granaries and the Anchor Seal"
title_zh: "九仓分发与连环宗契"
title_en: "The Nine Granaries and the Anchor Seal"
concept: "Google Percolator (Decentralized Snapshot Isolation & Primary Lock)"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-09-28-percolator-distributed-transactions.jpg
youtube_id: "GSUDOPj5x4c"
---
<section class="zh" markdown="1">
大运河沿岸设有九座督储粮仓。南起余杭，北达通州，千艘粮舫穿梭于碧波之上，天下漕粮与商贾储谷皆汇聚于此。

运河上的大宗钱粮交割，从来不是单仓进出那般简单。一笔通商会票，往往牵涉数座甚至全部九座大仓：须在南面三仓盘出两万石稻谷，同时在北面四仓划入等值的面粉与细盐。

依照律法，这等跨仓交割必须**"同生同死，万无偏颇"**——若南仓已经扣了粮，北仓却因故未能入库，商贾便要破产跳河；若北仓平白添了货，南仓却分文未少，朝廷税司定会以亏空国帑论斩。

在早些年间，官府为了防备弊端，在运河中枢的淮安总督行署专设了一座"总簿阁"。

每逢跨仓转粮，总督行署便派出一名粮官充当"总监官"。总监官先向两端分仓飞鸽传书，命各仓关紧闸门、盘点现存，将粮食封入临时栈房；各仓回书"俱已封妥"之后，总监官方在淮安总簿阁的大红册页上郑重落笔，记下一句"天下九仓，某字第七百号交割准行"；而后再向九仓分发开仓移交的令箭。

这套规矩看似固若金汤，却埋着要命的祸患。

有一年隆冬，寒潮突降，黄河与运河交汇处冰凌塞道。一名钦命总监官刚刚收齐九仓"粮已入栈封妥"的回信，正要回总簿阁落笔，却在夜宿驿馆时暴病不醒，陷入昏迷。

九座大仓的守仓典吏在冰天雪地里苦苦等候。令箭迟迟不来，而临时栈房的闸门上锁着官印，谁也不敢私自解封。南仓的稻谷眼看要在潮气中霉变，北仓待运的军粮急等下锅却颗粒难动。典吏们派出的快马往来飞驰，却只得到总监官人事不省、总簿阁无人敢代笔的消息。

整整一个月，九座关隘的物资被死死冻结在半途，整条大运河的商贸因一人病榻而全线瘫痪。

老漕官们聚在码头长叹："不怕运河风浪急，就怕行署一官倒。这总簿阁虽能号令九仓，可它若是卡死，天下漕运便全成了无头之蛇！"

运河总督痛定思痛，张榜悬赏天下漕运奇才。不久，一位曾在户部与水陆商行行走多年的算学名士，呈上了一套彻底抛弃总簿阁的**"连环宗契法"**。

"自今而后，"名士朗声道，"运河行船交割，无需专人坐镇行署死守总簿。天地为公，时辰为纲，九仓自可互照乾坤。"

新法的精妙，全在三道环环相扣的规矩：

其一，**立天地刻数**。运河北端设有一座滴水灵台，每逢一时辰推进，便顺流送出一枚刻有绝对单调递增数码的铜筹。任何押运使要起办交割，先到灵台取一枚**"启事筹"**，定下此笔交割的起始身位。

其二，**分宗辅二锁**。押运使前往牵涉交易的各个粮仓。在每一座仓中，他并不直接修改大仓的正账，而是在侧库悄悄摆入新粮，并在库门挂上一把**"铅封悬锁"**。

然而，这几把锁大有讲究：押运使必须在交易涉及的所有粮仓中，任意选定一间作为**"宗仓"**（主仓），其余皆为**"辅仓"**（从仓）。

在宗仓门前，他挂上一把沉重的赤铜**"宗锁"**，锁牌上烙印着启事筹码，清清楚楚写明："吾乃此契根本，生死系于吾身。"

而在其余辅仓门前，他挂上的锁牌则刻着一道清晰的指引："吾为随行辅锁，欲知此仓存亡，请速往某州某号宗仓验看宗锁！"

其三，**决死生于一瞬**。当所有牵涉的宗仓与辅仓皆已稳妥备好粮草、挂妥悬锁之后，押运使再度向滴水灵台申领一枚最新的**"决事筹"**。

随后，押运使**只需亲临宗仓一人一处**！

他当着宗仓典吏的面，咔哒一声开启赤铜宗锁，并在宗仓的永固青石账碑上，以朱砂重重写下一行大字：

`【启事筹某某 · 决事筹某某 · 宗契已成】`

就在宗仓石碑落笔、宗锁摘下的这一弹指之间——哪怕其余八座辅仓的门前依然挂着锁，哪怕押运使刚走出宗仓大门便跌下运河被激流冲走——**这笔横跨九座大仓的浩大交割，便已经在天地之间尘埃落定，永不可逆！**

随从典吏疑惑不解："押运使若被水冲走了，剩下的辅仓还锁着，岂非又要变成昔日的死局？"

名士抚须大笑："天差地别！昔日之所以死锁，是因为生死之机锁在行署一人的喉咙里；如今的生死之机，已经清清楚楚刻在宗仓的青石碑上！"

次日清晨，北面某处辅仓门前，急于提粮的客商发现侧库依然挂着悬锁，而押运使杳无音信。

守仓典吏毫不慌乱。他走上前看了一眼锁牌，只见上面写着："详验扬州第二号宗仓"。典吏立刻放出快船信鸽，去探看扬州宗仓的石碑：

若鸽信飞回，报称扬州宗仓已凿刻"宗契已成"的朱砂大字，北仓典吏便当着客商的面，手起斧落斩断辅锁，顺手在北仓账簿上补齐记录，直接放粮（*roll forward*）；

若鸽信查明扬州宗仓空空如也，连宗锁都因年久失修自行锈烂脱落，北仓典吏同样手起斧落斩断辅锁，将侧库粮食推回原仓，宣布交易作废（*roll back*）。

每一个路过的客商、每一个看守的库吏，甚至每一艘运河上的货船，人人皆可代为查验，人人皆可顺手了结！

从这一天起，八百里大运河上再无滞碍。再浩大繁复的跨仓迁转，既无需中央总簿的繁琐批复，也无惧押运官吏的半途风霜。运河千帆竞发，碧波万顷，天下仓储在九府山水之间吞吐自如。

——到这儿你大概已经认出来了：这套以天象筹数划定视界、以宗仓一契定夺全盘乾坤、以辅仓反向指引实现路人皆可异步化解锁扣的"连环宗契法"，讲的正是分布式数据库与大规模增量计算中名震天下的经典协议——Google 于 2010 年提出的 *Percolator* 分布式事务模型（亦即今日 *TiKV* 等现代 NewSQL 引擎核心的无中心化两阶段提交与主锁消解机制）。

### 这是什么

*Google Percolator* 是由 Daniel Peng 与 Frank Dabek 于 2010 年在 OSDI 发表的论文《Large-scale Incremental Processing Using Distributed Transactions and Notifications》中提出的分布式事务模型。其初衷是为 Google 的海量网页倒排索引构建提供一套跨行、跨表、具备完全 ACID 保障与快照隔离（*Snapshot Isolation*）特性的分布式事务层。它完全构建在原本只支持单行单列原子操作的非事务型键值存储系统（*Bigtable*）之上。

传统的分布式两阶段提交（*2PC*）严重依赖一个中央事务协调器（*Coordinator*）。协调器必须把全局事务状态记录在集中的持久化日志中。一旦协调器在 `Prepare` 与 `Commit` 之间崩溃，所有参与节点（*Cohort*）就会陷入漫长的事务阻塞状态，无法自行决定提交还是中止。

*Percolator* 提出了一种极为精妙的无中心化解法：**将事务的全局状态，去中心化地保存在数据单元本身之中，并通过单点主锁（Primary Lock）的原子落盘来宣告整个事务的生效**。

在 *Percolator* 底层，每一行数据的每一个单元格（*Cell*）都被拆分为三个主要列：
- `data:start_ts`：保存真实数据内容，以事务开始时间戳 `start_ts` 为版本号。
- `lock:start_ts`：保存未决事务的锁信息。若存在此列，表明当前单元正被某一事务占用。
- `write:commit_ts`：保存事务最终提交的记录，以提交时间戳 `commit_ts` 为版本号，内容记录对应的 `start_ts`，指示该版本数据已对外部读事务可见。

事务的执行分为两大阶段：

1. **预写阶段（Prewrite Phase）**：
   - 事务客户端从单调递增的授时中心（*Timestamp Oracle*, 简称 *TSO*）获取一个开始时间戳 `start_ts`。
   - 检查冲突：对于所有待写入的键，检查是否存在 $commit\_ts > start\_ts$ 的 `write` 列（写冲突），或者任何时间戳的 `lock` 列（读写冲突）。若有冲突，事务回滚重试。
   - **选定主键（Primary Key）**：客户端在所有待写键中，任意挑出一个作为 **Primary**，其余皆为 **Secondary**。
   - 写入 Primary：在 Primary 键的底层行原子地写入 `data:start_ts` 与 `lock:start_ts`（此锁标记其自身为 Primary）。
   - 写入 Secondaries：为其余所有键写入 `data:start_ts` 与指向 Primary 键的 `lock:start_ts`（锁内携带指针 `primary_key`）。

2. **提交阶段（Commit Phase）**：
   - 客户端从 TSO 再次申请一个提交时间戳 `commit_ts`（必有 $commit\_ts > start\_ts$）。
   - **原子提交 Primary**：客户端在 Primary 键上原子地写入 `write:commit_ts` 并移除 `lock:start_ts`。
   - **至关重要的转折点**：**一旦 Primary 的 `write` 写入成功，整个事务即告正式、不可逆地提交（Committed）！**
   - 异步提交 Secondaries：客户端随后向所有 Secondary 键写入 `write:commit_ts` 并移除锁。即便客户端此时彻底崩溃崩溃未能完成该步，事务的提交状态也已固若金汤。

3. **冲突消解与崩溃自愈（Resolve Locks & Lazy Roll-Forward）**：
   - 任何并发的读写事务如果碰上了遗留在 Secondary 键上的残留锁，无需等待或呼叫中央协调器。
   - 读事务只需顺着锁牌里的指针读取 Primary 键的状态：
     - 若 Primary 上已存在对应的 `write:commit_ts`，说明该事务早已提交成功！当前事务顺手帮其将该 Secondary 键推进为已提交（*Roll Forward*）；
     - 若 Primary 上的锁依然存在但客户端租约未超期，则说明事务仍在正常进行，主动稍作避让；
     - 若 Primary 上无 `write` 记录且其锁已超期（或 Primary 上的锁已被回滚清理），说明发起者早已死于半途，当前事务可安全地清除该残留锁并清退悬挂数据（*Roll Back*）。

### 为什么重要

*Percolator* 在现代分布式计算与分布式数据库演进史上具有里程碑式的意义：

1. **摆脱集中式协调器的单点脆弱性与吞吐瓶颈**：传统分布式事务要么受制于高可用协调器集群的吞吐天花板，要么在节点网络分区与故障时遭遇严重级联阻塞。*Percolator* 把事务协调状态化整为零，直接嵌入分布式存储的单行多版本单元格中，使分布式事务在具备跨行强一致性的同时，继承了分布式底层存储的高横向扩展能力。
2. **读写无锁阻塞（Lock-Free Reads under Snapshot Isolation）**：在 *Percolator* 提供的快照隔离级别下，只读事务读取时间戳为 $T$ 的数据时，只需寻找 $commit\_ts \le T$ 的最新 `write` 列，读操作不会施加任何排他锁，也绝不会阻塞并发写事务的提交，极大地保证了分析型与检索型读取的高并发吞吐。
3. **现代分布式 NewSQL 架构的基石**：以 **TiDB**（底层分布式存储引擎 *TiKV*）为代表的现代国产与开源 NewSQL 数据库，核心分布式事务引擎就是对 *Percolator* 算法的工程化实现与重度优化（结合 Raft 共识保证每个分片的高可用，通过 1PC 优化消除单分片事务开销，并通过异步提交 *Async Commit* 进一步缩减网络往返）。掌握了 *Percolator* 的主锁机制，便真正掌握了现代分布式数据库事务核芯的运作密码。

_隐喻对应表_

- 大运河沿岸九座储粮大仓 → 分布式存储系统中的分片与节点 (*Distributed Storage Nodes / Tablets*)
- 淮安总督行署中央总簿阁 → 传统两阶段提交中的集中式事务协调器 (*Centralized 2PC Transaction Coordinator*)
- 总监官重病导致九仓无限期死锁 → 传统两阶段提交在协调器崩溃时的阻塞缺陷 (*2PC Blocking Problem on Coordinator Crash*)
- 运河滴水灵台依序发放的数码铜筹 → 单调递增授时中心 (*Timestamp Oracle / TSO*)
- 启事筹（取起始时辰） → 事务开始时间戳 (*start_ts*)
- 决事筹（取交割终刻） → 事务提交时间戳 (*commit_ts*)
- 侧库中悄悄摆放的待纳粮草 → 预写阶段存入的数据列 (*data:start_ts*)
- 库门上悬挂的防乱动铅封锁 → 预写阶段施加的行级锁 (*lock:start_ts*)
- 任意挑选的赤铜宗仓与宗锁 → 事务中选定的核心主键及主锁 (*Primary Key / Primary Lock*)
- 辅仓锁牌上指向宗仓的文字标记 → 辅锁携带的反向指针 (*Secondary Lock pointing to primary_key*)
- 宗仓青石碑凿刻朱砂大字并摘锁 → 提交阶段原子写入 `write:commit_ts` 并移除主锁 (*Atomic Primary Commit*)
- 宗锁落定瞬间全盘已成，押运使落水亦无妨 → 主锁一旦提交，整个分布式事务立即不可逆转生效 (*Commit Boundary at Primary*)
- 探访客商与典吏查宗仓石碑顺手斩断辅锁放粮 → 并发事务遇到遗留辅锁时自主执行向前滚动推进 (*Resolve Locks / Lazy Roll-Forward*)
- 查明宗仓空虚且锁锈脱落时清退侧库废粮 → 遇死锁与故障发起者时自主执行回滚清理 (*Roll-Back on Aborted Primary*)
</section>
<section class="en" markdown="1">
Along the Grand Canal stood nine imperial granaries. From Yuhang in the south to Tongzhou in the north, thousands of grain barges traversed the emerald waters, marshaling the realm's grain taxes and commercial harvests.

Large transactions along the canal were never as simple as grain entering or leaving a single warehouse. A major merchant draft typically entangled several, or even all nine, storehouses: twenty thousand piculs of rice had to be debited across three southern granaries, while equivalent stores of wheat flour and fine salt were simultaneously credited across four northern depots.

By imperial decree, such cross-granary transfers had to **"succeed as one or perish together"**—if the southern silos surrendered their grain while the northern granaries failed to log delivery, merchants faced bankruptcy; if the northern granaries gained inventory without corresponding deductions from the south, tax magistrates sentenced the clerks for defrauding the imperial treasury.

In earlier years, the imperial court had attempted to prevent corruption by establishing a "Grand Register Pavilion" at the central headquarters in Huai'an.

Whenever a multi-granary transfer took place, the governor-general dispatched a chief commissary. This officer first sent carrier pigeons to the involved depots, ordering them to lock their sluices, tally current stock, and move the grain into temporary staging bays. Only after every warehouse replied that their holdings were securely sealed did the commissary dip his brush into vermillion ink and write upon the grand register in Huai'an: *"Imperial Nine-Depot Transfer, Draft No. 700: Approved."* Only then did he dispatch riders bearing tokens to command the physical release.

This procedure appeared ironclad, yet it concealed a catastrophic hazard.

One harsh winter, a ferocious blizzard choked the confluence of the Yellow River and the Canal with jagged ice floes. A chief commissary, having just received confirmations that all nine depots had safely locked their staging bays, was struck down by a sudden violent fever at an overnight posthouse, falling unconscious.

In the biting cold, the warehouse keepers across nine garrisons waited. The release tokens never came. Because the staging bay gates were sealed with imperial wax, no keeper dared break them open. In the south, damp air threatened to rot mountains of rice; in the north, garrison troops faced starvation with locked storehouses in plain view. Fast couriers raced back and forth, only to report that the commissary lay in a coma and no subordinate had the statutory authority to touch the Grand Register.

For an entire month, essential stores remained frozen midway across nine garrisons. The commerce of the entire Grand Canal ground to a halt because a single official fell sick.

Old boatmen sighed along the docks: *"We fear neither canal gale nor river surge; we fear only the one magistrate at headquarters collapsing. That Grand Pavilion commands nine warehouses, but when its desk jams, the whole realm turns into a headless snake."*

The governor-general posted an open bounty for a remedy. Soon, a veteran mathematician and logistics master who had served in both the Ministry of Revenue and merchant guilds stepped forward with the **"Linked Anchor-Seal Method,"** completely discarding the Grand Register Pavilion.

"From this day forth," the master declared, "transfers along the canal require no resident mandarin guarding a central ledger. Let heaven and water keep the hour; let the nine granaries verify each other across the provinces."

The brilliance of this new method rested on three interlocking rules:

First, **The Celestial Sequence**. At the northern terminus of the canal, a water-clock platform issued bronze tallies stamped with monotonically increasing sequence numbers. Any commissary initiating a transfer first obtained a **"Commencement Tally"** (*start_ts*) to establish the transaction's baseline viewpoint.

Second, **The Anchor and the Linked Auxiliaries**. The commissary traveled to the participating depots. Rather than editing the primary ledger directly, he staged the grain inside a side bay and clamped a **"Lead-Sealed Padlock"** upon its latch.

Crucially, he divided the padlocks into two categories: among all the participating storehouses, he arbitrarily designated **one storehouse as the Anchor** (*Primary*), while treating all remaining depots as **Auxiliaries** (*Secondaries*).

At the Anchor granary, he hung a massive bronze **"Anchor Lock,"** stamped with the Commencement Tally and inscribed: *"I am the primary anchor of this contract. All life and death hinge upon me."*

At each Auxiliary granary, however, the padlock bore a directive pointing outward: *"I am an auxiliary lock. To discover my fate, make haste to the Anchor Granary at Garrison A and inspect the Master Seal!"*

Third, **Deciding Fate in a Single Stroke**. Once all participating storehouses—both Anchor and Auxiliaries—had safely staged their goods and clamped their locks, the commissary requested one final token from the water-clock platform: the **"Resolution Tally"** (*commit_ts*).

The commissary was then required to visit **only the Anchor Granary alone**!

Before the eyes of the Anchor keeper, the commissary clicked open the bronze Anchor Lock, took up a vermillion brush, and carved deeply into the Anchor's eternal stone ledger:

`[Commencement No. X · Resolution No. Y · Anchor Contract Fulfilled]`

The instant that inscription dried upon the stone and the Anchor Lock fell open—even though the eight auxiliary depots remained locked behind iron gates, even if the commissary slipped into the roaring canal and drowned the moment he walked out the door—**the multi-granary transfer across all nine storehouses was irrevocably and eternally committed!**

A junior clerk asked in dismay: "If the commissary drowns and the auxiliary depots remain locked, will we not plunge back into our old nightmare?"

The master laughed heartily: "It is night and day! The old nightmare occurred because the decision was imprisoned inside the living throat of one mandarin at headquarters. Today, the verdict is carved openly upon the granite of the Anchor Granary for the whole world to read!"

The following dawn, at a northern auxiliary depot, a merchant arrived to collect his grain, only to find the auxiliary padlock still hanging upon the gate and the commissary nowhere to be found.

The depot keeper did not panic. He leaned close to read the tag upon the lock: *"Refer to Anchor Granary No. 2 at Yangzhou."* The keeper dispatched a carrier pigeon to check the stone stele at Yangzhou.

When the pigeon returned bearing word that Yangzhou's stele was already carved with *"Anchor Contract Fulfilled,"* the northern keeper swung an iron axe, shattered the auxiliary padlock, transcribed the resolution tally into his own local ledger, and released the grain (*roll forward*).

Had the pigeon reported that Yangzhou's stele was barren and the Anchor Lock had rusted away into dust, the keeper would have shattered the padlock just as readily, shoveled the staged grain back into general stock, and declared the transfer void (*roll back*).

Every passing merchant, every gatekeeper, every barge captain could verify the Anchor and complete the resolution with their own hands!

From that day forward, congestion vanished from the eight hundred miles of the Grand Canal. No matter how vast or intricate the transfer, it neither demanded approval from a central bureau nor feared the fragile mortality of traveling officials. The canal flowed unburdened, and nine provinces moved their wealth in effortless harmony.

—By now you've probably recognized it: this method of bounding visibility through celestial sequence numbers, anchoring global transactional fate within a single primary key, and enabling concurrent observers to lazily resolve secondary locks, is the celebrated distributed transaction protocol known as **Google Percolator**—introduced in 2010 and serving today as the architectural foundation of decentralized Two-Phase Commit (*2PC*) and Snapshot Isolation (*SI*) in modern NewSQL engines such as *TiKV*.

### What it is

*Google Percolator* was introduced in 2010 by Daniel Peng and Frank Dabek in their OSDI paper, *"Large-scale Incremental Processing Using Distributed Transactions and Notifications."* Designed to power Google's web-search indexing pipeline, Percolator provided cross-row, cross-table ACID transactions with Snapshot Isolation (*SI*) over a non-transactional distributed key-value store (*Bigtable*), which natively supported only single-row atomic mutations.

Classic distributed Two-Phase Commit (*2PC*) relies on a centralized transaction coordinator. The coordinator must record global state transitions in persistent logs. If the coordinator crashes between the `Prepare` and `Commit` phases, participating nodes (*cohorts*) remain indefinitely blocked, unable to autonomously decide whether to commit or abort.

*Percolator* eliminates the centralized coordinator bottleneck through an elegant architectural inversion: **it decentralizes transaction state into the data cells themselves and uses the atomic commitment of a single "Primary Lock" as the definitive commit point for the entire distributed transaction.**

Under the hood, Percolator decomposes each cell into three distinct column families:
- `data:start_ts`: Stores the actual payload value, versioned by the transaction's start timestamp `start_ts`.
- `lock:start_ts`: Holds active lock metadata. If present, it indicates an in-flight, uncommitted transaction is modifying the key.
- `write:commit_ts`: Records the committed transaction record, versioned by `commit_ts`. It stores a pointer back to `start_ts`, signaling to readers that the data version is officially visible.

The protocol executes in two phases:

1. **Prewrite Phase**:
   - The transaction client fetches a monotonically increasing `start_ts` from a centralized Timestamp Oracle (*TSO*).
   - Conflict check: For every key to be written, the client inspects the row. If any `write` record exists with $commit\_ts > start\_ts$ (write-write conflict) or if any `lock` exists at any timestamp (read-write conflict), the transaction aborts and retries.
   - **Primary Key Designation**: The client designates one arbitrary key from its write set as the **Primary**, treating all other keys as **Secondaries**.
   - Prewrite Primary: The client atomically writes `data:start_ts` and `lock:start_ts` into the Primary key (the lock designates itself as primary).
   - Prewrite Secondaries: The client writes `data:start_ts` and `lock:start_ts` for all remaining keys, embedding a pointer to the Primary key inside each secondary lock (`primary_key`).

2. **Commit Phase**:
   - The client fetches a commit timestamp `commit_ts` from the TSO (guaranteed $commit\_ts > start\_ts$).
   - **Atomic Primary Commit**: The client atomically writes `write:commit_ts` and removes `lock:start_ts` on the Primary key.
   - **The Irrevocable Commit Boundary**: **The exact instant the Primary's `write` record lands, the entire transaction is permanently and irreversibly committed!**
   - Asynchronous Secondary Rollout: The client proceeds to write `write:commit_ts` and clear locks on all Secondary keys. If the client crashes at this instant, the transaction remains fully committed.

3. **Conflict Resolution & Lazy Roll-Forward (Resolve Locks)**:
   - When a concurrent reader or writer encounters a lingering secondary lock, it never blocks waiting for an external coordinator.
   - The reader follows the embedded pointer directly to the Primary key:
     - If the Primary key already contains a valid `write:commit_ts`, the transaction was committed. The reader proactively rolls the secondary lock forward (*Roll Forward*) by writing `write:commit_ts` and clearing the lock.
     - If the Primary key still holds an unexpired lock, the transaction is actively progressing, and the reader briefly backs off.
     - If the Primary key has no `write` record and its lock has expired or been cleaned up, the originator crashed prior to committing. The reader safely rolls back (*Roll Back*) the secondary key by erasing the dangling data and lock.

### Why it matters

Percolator is a foundational milestone in distributed systems and modern database engineering:

1. **Elimination of Coordinator Vulnerability**: Traditional 2PC suffers from coordinator failover complexities, catastrophic split-brain hazards, and persistent blocking states. Percolator stores transaction metadata within the distributed storage layer itself, enabling distributed transactions to inherit the massive horizontal scalability and fault-tolerance of the underlying storage engine.
2. **Lock-Free Reads under Snapshot Isolation**: For read transactions querying at snapshot timestamp $T$, the reader only scans for `write` records where $commit\_ts \le T$. Readers do not take shared locks and never block concurrent writers, providing exceptional throughput for high-concurrency read-heavy workloads.
3. **The Blueprint for Modern NewSQL Engines**: Modern distributed NewSQL databases such as **TiDB** (specifically its distributed storage layer, *TiKV*) adopted and refined Percolator as their foundational distributed transaction protocol. TiKV pairs Percolator with Raft consensus groups for local partition high availability and introduces optimizations such as 1PC (single-region fast path) and Async Commit to eliminate extra network round-trips. Understanding Percolator's primary lock mechanism unlocks the core mechanics of modern cloud-native transactional databases.

_Metaphor mapping_

- Nine imperial granaries along the Grand Canal → Distributed storage nodes and partitions (*Distributed Storage Nodes / Tablets*)
- Grand Register Pavilion at Huai'an headquarters → Centralized transaction coordinator in traditional 2PC (*Centralized 2PC Coordinator*)
- Commissary falling into a coma freezing all nine depots → The blocking problem of 2PC when coordinator crashes (*2PC Blocking Problem*)
- Water-clock platform issuing sequential tallies → Monotonically increasing Timestamp Oracle (*TSO*)
- Commencement Tally (*start_ts*) → Transaction start timestamp (*start_ts*)
- Resolution Tally (*commit_ts*) → Transaction commit timestamp (*commit_ts*)
- Staging grain inside side bays during prewrite → Writing uncommitted payload into data column (*data:start_ts*)
- Clamping lead-sealed padlocks onto staging bays → Acquiring row-level locks during prewrite (*lock:start_ts*)
- Designating one Anchor Granary with a bronze Anchor Lock → Selecting an arbitrary key as the Primary Lock (*Primary Key / Primary Lock*)
- Auxiliary locks pointing back to the Anchor Granary → Secondary locks carrying pointers to the primary key (*Secondary Lock with primary_key pointer*)
- Carving vermillion inscription on Anchor stele and releasing Anchor Lock → Atomically writing `write:commit_ts` and clearing primary lock (*Atomic Primary Commit*)
- Transaction committed instantly even if commissary drowns → Irrevocable commit boundary anchored solely at the primary key (*Primary Commit Boundary*)
- Visiting merchants and keepers checking Anchor stele to cut auxiliary locks → Concurrent readers performing lazy roll-forward (*Resolve Locks / Roll Forward*)
- Clearing staged grain when Anchor stele is empty and lock rusted → Autonomous rollback of abandoned transactions (*Roll Back*)
</section>
