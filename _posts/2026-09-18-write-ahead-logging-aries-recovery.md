---
layout: fable
title: "烈火残册的三遍重光术 · The Three Passes of the Fire-Scarred Ledger"
title_zh: "烈火残册的三遍重光术"
title_en: "The Three Passes of the Fire-Scarred Ledger"
concept: "Write-Ahead Logging (WAL) and the ARIES Crash Recovery Algorithm"
tags: [databases, storage, performance]
illustration: /assets/art/2026-09-18-write-ahead-logging-aries-recovery.jpg
---
<section class="zh" markdown="1">
北都户部太仓掌管天下钱粮，四海赋税与官银日夜兼程运抵于此，入库出纳络绎不绝。

早年间，太仓的账房先生们有一套顺理成章的做账习惯：银车进了院子，苦力们先把沉重的银箱抬进地窖，等封条贴好、库门落锁，账房先生才气定神闲地翻开笨重的羊皮大账，提笔把这笔收支慢慢记上去。
有一年大雪除夕，天干物燥，地道炭炉走火引发了一场冲天烈焰。
大火烧塌了账房的屋梁，半截羊皮大账被焚为灰烬，翻倒的银车与砸烂的箱笼在地窖与泥雪中混成一团。

大火扑灭后，太仓上下陷入了前所未有的死局：
地窖里滚落着几十箱无主白银，有的明明抬进了库房，账册上却空无一字；有的羊皮账上赫然写着“收讫入库”，库房里却压根找不到半个银锭——原来是苦力刚抬下车、大账刚落笔，房梁便塌了下来，箱子在混乱中被人推回了雪地里。
谁也不知道哪笔交易算完、哪笔只算了一半。朝廷发往九边的军饷被全盘冻结，户部尚书急得几欲自裁。

新任典簿官墨先生奉旨收拾残局。他站在焦黑的废墟前，当众立下一道铁律：
“自今日始，库房立‘先录后动之令’，遇灾施‘三遍理账之法’！”

第一规：**“动银之前，先落流水”（Write-Ahead Logging / 先写日志）**。
在太仓正门石壁上凿出一个防火青石暗槽，槽内安放一卷由防火石棉织就的连续流水长卷。
墨先生严令：苦力抬银箱之前，账房必须先在石槽长卷上记下一笔流水，标明自增序号（日符号数）、何人押运、哪只箱号、原由何处转入何地。
只有当墨迹吹干、流水卷滑入防火石槽后，苦力才准开箱动银。
倘若银箱在庭院里挪到一半天降横祸，地窖大账纵然烧光，石槽里的流水长卷也绝不会化作飞灰。

第二规：**“劫后重光，必行三遍”（ARIES 崩溃恢复三阶段）**。
数年后，太仓偏殿果然又遭了一次雷击骤火。当浓烟散尽、库门重开，墨先生取出完好无损的流水长卷与污损的库房底账，率众典吏不慌不忙施展“三遍理账术”：

**第一遍：审度巡查（Analysis Pass · 分析阶段）**。
墨先生从上个月太仓全量核验的那道“安澜红印”（Checkpoint）开始，顺着流水卷逐行向下通读，直至大火熄灭前的最后一行墨迹。
在这第一遍中，任何人不得挪动库中一两银子。墨先生在沙盘上推演全局：哪些银箱已被挪动但底账未及更新？大火突降的那一刹那，究竟有哪几笔交易正办到一半、尚未盖章结案？

**第二遍：重现沧桑（Redo Pass · 重做历史）**。
查明受损范围后，墨先生提笔倒退回最早那箱未落账的银号处，严格按照流水卷上的时间先后，将卷上记录的所有动作**从头到尾原原本本重做一遍**——哪怕是那些办到一半、注定要废弃的未结交易，也一律照样把银箱复位！
随从官吏大惑不解：“未结的烂账，何不直接丢弃，为何还要费力复原？”
墨先生厉声斥道：“不知彼时之真貌，焉知此刻之当损？欲除弊乱，必先教天地复归于烈火焚身的那一瞬！”

**第三遍：倒溯平账（Undo Pass · 回滚未结事务）**。
当整座太仓的银箱完完全全恢复到大火降临前的那个绝对瞬间，墨先生方才调转锋芒，在沙盘上锁定那几笔未结的交易，自尾向头倒卷而回！
他将半路搁置的银箱逐一搬回原主车上，退还原籍。
更为神妙的是，墨先生每撤销一步，必在流水卷上立刻添写一张朱砂“退还红帖”（补偿日志记录 / CLR）。
他向众吏告诫：“大灾之后余烬未冷，倘若此时再起一次大火，后人接着理账，见此红帖便知此银已退，绝不会重复退还，更不会困于死局！”

三遍走完，太仓百万库银分毫不差，账目澄澈如镜。
满朝文武叹为观止：
“先落流水后动金，风雷过隙骨长存。
巡查审度明虚实，重现沧桑辨伪真。
回锋倒卷清余欠，朱帖长留绝后纷。”

— — —

### 这是什么

——到这儿你大概已经认出来了：这正是现代关系型数据库与存储引擎（如 PostgreSQL、MySQL InnoDB、Oracle、SQLite）能够保证 ACID 事务持久性（Durability）与原子性（Atomicity）的不朽基石：*预写式日志（Write-Ahead Logging / WAL）* 与数据库崩溃恢复理论的开山丰碑——*ARIES 算法（Algorithm for Recovery and Isolation Exploiting Semantics）*。

在数据库运行过程中，为了追求极致的磁盘 I/O 性能，数据页（Data Pages）是在内存的缓冲池（Buffer Pool）中被修改的，成为“脏页”（Dirty Pages）。如果每次事务提交都要把分散在磁盘各处的脏数据页随机写入磁盘（Random I/O），数据库吞吐量将极其低下；但如果直接异步刷盘，一旦机器断电宕机，内存中的脏页瞬间丢失，已提交的事务会凭空消失，未提交的事务则会留下半生不熟的残缺数据，彻底摧毁数据一致性。

WAL 与 ARIES 算法以精妙的架构彻底解开了这一死结：

1. **WAL 预写协议（Write-Ahead Logging Protocol）**：
   在任何脏数据页被写回持久化存储介质之前，记录该修改的日志记录（Log Record）**必须先一步顺序刷入持久化的磁盘日志文件**中；在事务提交时，只需保证该事务的提交日志记录写入磁盘（Sequential I/O），事务即可宣布成功提交。
   每条日志都被赋予一个全局单调递增的序号——*LSN（Log Sequence Number）*，数据页上也记录着最后修改它的 `pageLSN`，构建起严密的因果时序链条。
2. **ARIES 崩溃恢复三阶段（The Three Passes of ARIES）**：
   当数据库发生意外断电崩溃并重新启动时，恢复子系统执行经典的“三遍扫描”：
   - **分析阶段（Analysis Pass）**：从最近一次检查点（Checkpoint）开始**向前正向扫描**日志。重构出崩溃发生那一瞬间内存中的两张核心状态表：脏页表（Dirty Page Table / DPT，记录哪些页脏了以及最早弄脏它的 `recLSN`）与活跃事务表（Transaction Table / TT，记录崩溃时尚未提交的事务）；
   - **重做阶段（Redo Pass · Repeating History）**：从 DPT 中所有脏页里最小的 `recLSN` 开始，**向前正向扫描**日志直至崩溃前的最后一条记录。ARIES 坚决践行“重现历史”（Repeating History）哲学：无论是已提交还是未提交的事务，一律将修改重新应用到数据页中（若 `pageLSN >= logLSN` 则跳过，保证幂等）。将数据库物理状态不偏不倚地恢复到**宕机发生前一微秒的绝对真实状态**；
   - **回滚阶段（Undo Pass）**：从崩溃时的活跃事务表出发，沿着事务的前驱日志指针，**向后逆向扫描**日志，逐一撤销所有未提交事务对数据库的修改。在撤销每一步操作时，系统写入一条特殊的**补偿日志记录（Compensation Log Record / CLR）**。CLR 包含一个跳跃指针指向被回滚操作的前一个日志，确保如果在恢复过程中数据库**再次发生二次宕机**，系统绝不会对同一个撤销操作进行重复回滚，彻底杜绝无限回滚死循环。

### 为什么重要

WAL 与 ARIES 是构建一切可靠数据基础设施的技术元典：

1. **将随机写转化为顺序写，吞吐量提升数量级**：
   WAL 允许数据库在内存中肆意合并修改，只需将极小的日志变更以顺序 I/O（Append-Only）落盘即可确认事务。这正是现代数据库能够支撑成千上万并发写事务的根本动力。
2. **Repeating History 带来的极致简洁与鲁棒性**：
   ARIES 算法在 Redo 阶段不加区分地重放一切操作，彻底解除了并发事务锁与物理存储布局之间的复杂耦合，使得并发事务控制（如行级锁、多粒度锁）与故障恢复机制得以完全正交解耦。
3. **CLR 保证崩溃恢复的幂等性与容灾容错极限**：
   补偿日志（CLR）的设计堪称计算机工程的鬼斧神工——它证明了一个系统即使在“正在恢复崩溃”的过程中遭遇连续十次停电断电，重启后依然能够依靠 CLR 的指针跳跃稳定恢复，绝不损坏半个字节。

_隐喻对应表_

- 太仓二十万两流动官银与地窖银箱 → 数据库驻留在内存中的脏页与磁盘中的物理数据页
- 苦力先抬银进窖后记账引发的火后混乱 → 未遵守 WAL 协议导致崩溃后无法厘清一致状态
- 防火青石暗槽与石棉流水长卷 → 顺序追加写（Append-Only）的预写式日志文件（WAL Log）
- 墨迹吹干入槽方准动银 → WAL 核心协议：数据页落盘前日志必须先强制落盘（WAL Flush）
- 卷上自增序号（日符号数）与箱上记号 → 全局单调递增的日志序列号（LSN）与数据页号（pageLSN）
- 第一遍审度巡查（Analysis Pass） → 正向扫描日志以重建脏页表（DPT）与活跃事务表（TT）
- 第二遍重现沧桑（Redo Pass） → 从最小 recLSN 坚决重放历史，还原宕机前的绝对物理状态
- 第三遍倒溯平账（Undo Pass） → 逆向扫描日志，逐一回滚所有未提交的活跃事务
- 撤销一步必填一张朱砂退还红帖 → 写入补偿日志记录（CLR），防止恢复期二次宕机引发死循环
</section>

<section class="en" markdown="1">
The Grand Imperial Treasury of the Northern Capital governed the wealth of the entire empire. Tributary silks and silver ingots from thirty provinces arrived at its water gates day and night, an unbroken torrent of commercial accounting.

In earlier reigns, the treasury scribes followed a comfortable, intuitive habit: when a caravan of bullion wagons rolled into the courtyard, porters unloaded the heavy silver chests into deep underground vaults. Only after the brass locks clicked shut and the vault wax hardened did the scribe draw forth his leather master ledger, dipping his brush in ink to record the transaction.
Then came the bitter winter of the Great Fire.
Sparks from a subterranean brazier ignited dry timber, engulfing the counting house in a roaring inferno. The cedar roof collapsed, reducing half of the master ledgers to soot, while shattered silver chests and overturned wagons scattered across the burning snow.

When the embers cooled, the treasury found itself paralyzed in an irrecoverable swamp of contradictions:
Dozens of unlabeled silver chests lay in the muddy courtyard. Some had been stored safely in the vaults, yet the soot-stained ledgers held no mention of them; other ledger pages recorded large tributes as "Safely Sealed," yet not a single ingot could be found in the vaults—the porters had dropped the boxes on the snow, the scribes had inked the entry, and the roof had collapsed before the chests ever touched the cellar floor.
No man alive could discern which transactions had finalized and which were half-done. Imperial payrolls and border military rations were frozen solid, and the Grand Minister contemplated taking his own life.

The newly appointed Chief Scribe, Master Mo, stood before the blackened ruins and proclaimed two eternal statutes:
"From this dawn forward, the Treasury operates under the **Rule of Write-Ahead**, and we shall resurrect the ashes through the **Three Passes of Restoration**!"

Rule One: **"Ink Before Ingot" (The Write-Ahead Logging Protocol)**.
Upon the granite masonry of the treasury gateway, Mo chiseled an aperture leading to an iron fireproof chute. Inside lay a continuous roll of asbestos-woven fireproof parchment.
Master Mo declared: Before a single porter touches a chest, the scribe must write an entry upon the fireproof scroll, stamped with a strictly monotonic sequence number, the merchant's name, the chest's unique number, and the before-and-after disposition of silver.
Only after the ink dried and the parchment rolled into the fireproof chute were porters permitted to budge the chest.
Even if a carriage broke down midway in the muddy courtyard, though the ledger on the table burned to cinders, the record in the fireproof chute remained eternal.

Rule Two: **"Three Passes of Resurrection" (The ARIES Recovery Phases)**.
Years later, lightning struck the west wing of the treasury, unleashing another sudden fire. When the smoke cleared, Master Mo retrieved the intact fireproof scroll and the soot-darkened vault ledgers, guiding his scribes through the three rigorous passes:

**The First Pass: The Analysis Pass**.
Starting from the previous month's certified seal of balance—the *Audit Checkpoint*—Master Mo scanned the scroll forward to the very last stroke of ink made before the flames.
In this pass, not a single chest of silver was moved. Mo charted the wreckage upon a sand table: Which silver chests had been modified in the vaults without ledger confirmation? And at the precise heartbeat when the roof fell, which transactions remained active and incomplete?

**The Second Pass: The Redo Pass (Repeating History)**.
Having mapped the unsettled chests, Master Mo turned back to the earliest recorded modification that had not yet been sealed in stone. Following the scroll's chronology, he commanded his men to **replay every single logged action forward** right up to the spark of the fire—even replaying the actions of merchants whose business had been cut short!
His juniors protested: "Why waste sweat restoring transactions that were never completed?"
Master Mo chided them: "Without knowing the exact truth of what transpired, how can one judge what is broken? To purge chaos, one must first resurrect the world exactly as it stood at the instant of destruction!"

**The Third Pass: The Undo Pass (Rolling Back Unfinished Transactions)**.
With the treasury restored to the exact physical state it occupied the microsecond before the disaster, Master Mo reversed his course. Focusing on the transactions flagged as incomplete, he scanned **backward** from the end of the scroll, rolling back uncommitted steps and returning silver chests to their rightful owners.
Crucially, for every step reversed, Master Mo inked a distinctive vermillion slip into the scroll—a *Compensation Slip* (Compensation Log Record / CLR).
He instructed his scribes: "In the turbulence of calamity, a secondary fire may strike while we rebuild. Should the roof collapse again today, the next scribe will see this red slip, skip the reversed work, and never undo the same chest twice!"

When the three passes concluded, twenty million taels of imperial bullion balanced to the grain of silver.
The Imperial Court sang in enduring admiration:
"Write the scroll before moving gold; through fire and storm, truth holds its mold.
Analyze the ruins to know the dead; repeat all history where the living tread.
Roll back the severed in reverse design; with scarlet tallies, keep the future fine."

— — —

### What it is

By now the architecture is unmistakable: this is the bedrock of database durability (Durability) and atomicity (Atomicity) across modern relational and transactional systems (PostgreSQL, MySQL InnoDB, SQLite, Oracle): *Write-Ahead Logging (WAL)* and the crowning masterpiece of recovery theory—the *ARIES (Algorithm for Recovery and Isolation Exploiting Semantics)* algorithm.

In high-performance database engines, modified data pages cannot be written to disk synchronously on every transaction commit without strangling throughput in random disk I/O bottlenecks. Instead, modifications occur in an in-memory buffer pool as "dirty pages." If the system crashes, unwritten dirty pages evaporate from RAM. Without a deterministic recovery protocol, committed data would vanish, and uncommitted transactions would leave corrupted, partial writes on disk.

WAL and ARIES resolve this dilemma through mathematical elegance:

1. **The WAL Protocol**:
   Before any dirty in-memory data page can be written back to durable disk storage, the corresponding log record describing the modification **must be flushed to append-only disk storage first**. An update transaction is declared "committed" the moment its small commit log record is flushed sequentially to disk, decoupling commit latency from large random page writes.
   Every log record carries a globally monotonic *Log Sequence Number (LSN)*. Each disk page also records the `pageLSN` of the latest update applied to it, creating a verifiable chronological anchor.
2. **The Three Passes of ARIES Recovery**:
   When a database reboots after an unexpected crash or power failure, it executes ARIES's canonical three-phase recovery:
   - **The Analysis Pass**: Scans the log **forward** starting from the most recent Checkpoint. It reconstructs the in-memory state as of the crash: the Dirty Page Table (DPT, tracking which pages were dirty and their earliest unwritten `recLSN`) and the Transaction Table (TT, identifying active, uncommitted transactions);
   - **The Redo Pass (Repeating History)**: Scans **forward** from the smallest `recLSN` across all dirty pages up to the end of the log. ARIES adheres strictly to the philosophy of *Repeating History*: it reapplies all logged modifications—for both committed and uncommitted transactions—bringing the database to the exact physical state it occupied the microsecond before the crash. If a page already has `pageLSN >= logLSN`, the redo operation is skipped, guaranteeing idempotency;
   - **The Undo Pass**: Scans **backward** through the log, rolling back the actions of all active, uncommitted transactions identified in the Transaction Table. For every undone action, ARIES writes a **Compensation Log Record (CLR)**. The CLR contains an `UndoNextLSN` pointer bypassing the reversed action. If the database crashes *again during recovery*, subsequent restarts inspect the CLR, skip already-undone actions, and prevent catastrophic infinite recovery loops.

### Why it matters

WAL and ARIES constitute the foundational grammar of dependable computing:

1. **Transforming Random I/O into Blazing Sequential Throughput**:
   WAL allows database engines to buffer gigabytes of dirty random writes in RAM while guaranteeing ACID durability through lightweight, sequential append-only logging.
2. **Repeating History Decouples Recovery from Concurrency**:
   By restoring the exact physical state of the database before attempting logical rollbacks, ARIES decouples crash recovery from complex application-level lock models, enabling granular row-level locking without risking recovery corruption.
3. **Provable Idempotency under Cascading Crashes**:
   The genius of Compensation Log Records (CLRs) ensures that crash recovery itself is fully crash-resilient. A database can lose power ten times consecutively midway through recovery, yet resume and finish without human intervention or data corruption.

_Metaphor mapping_

- Twenty million taels of silver in courtyard wagons vs. underground vaults → In-memory dirty pages in the buffer pool vs. persistent disk data pages
- Moving chests before logging leading to post-fire paralysis → Violating WAL causing irrecoverable database corruption upon sudden crash
- Fireproof stone chute and continuous asbestos scroll → Append-only Write-Ahead Log (WAL file) on durable storage
- Ink drying in chute before lifting chests → The WAL protocol: log records must flush to disk before dirty data pages can be written
- Ascending brush serial numbers on slips and chest tags → Globally monotonic Log Sequence Numbers (LSN) and pageLSNs
- First Pass: Surveying the ruins without moving gold → The Analysis Pass reconstructing the Dirty Page Table and Transaction Table
- Second Pass: Replaying every action up to the blaze → The Redo Pass (Repeating History) bringing the database to the exact pre-crash state
- Third Pass: Rolling back incomplete transactions backward → The Undo Pass rolling back uncommitted transactions
- Inking a vermillion compensation slip upon each reversal → Writing Compensation Log Records (CLRs) to prevent infinite loops during recovery crashes
</section>
