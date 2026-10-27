---
layout: fable
title: "一声钟响，四千枚印 · Four Thousand Seals to a Single Bell"
title_zh: "一声钟响，四千枚印"
title_en: "Four Thousand Seals to a Single Bell"
concept: "Snowflake distributed ID generation"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-10-27-snowflake-distributed-id-generation.jpg
youtube_id: "mJaS_acP9to"
---
<section class="zh" markdown="1">
石印城在半山腰。城不大，却是整条山道上唯一的印局。

规矩传了九代：凡是进城存档的文书——地契、粮册、讼状、工匠的欠条——都要在右上角盖一枚编号印。编号不能重。重了，天枢阁的书吏就会把两张不同的纸塞进同一个格子；来年有人来取地契，抽出来的却是别人的欠条。

早年间这件事不难。城里有九家印坊，各记各的账：一号、二号、三号，记到第几百号就换一本。文书只在城里走，从不跨坊。九本账簿彼此不照面，也就从不打架。

后来山道通了，下游三个县并了进来，事情就变了。

一份文书可能在东坊起稿，送到西坊加印，再回南坊归档；一本粮册拆成五份，分给五家印坊同时抄。那年春天，天枢阁出了事：东坊的第一千零七号，和西坊的第一千零七号，同一天进了阁。两张纸，一个号。书吏把地契塞进了粮册的抽屉。

三个月后，城西的柳家丢了三十亩水浇地。

— —

印局的局正姓沈，叫沈砚。

他没有急着换账簿，先去看了钟楼。

钟楼在城中心。司钟人每过一个"毫刻"敲一下钟——毫刻短得听不见尾音，一昼夜八十多万下。钟楼的号是连着唱的：第几声响，就是第几个毫刻。全城只有一座钟楼，所以全城听见的"第几响"，永远是同一个数。

沈砚在钟楼底下站了一整天，想出一枚新印。

新印一按下去，纸上是三段：

第一段，钟楼此刻是第几响。
第二段，本坊的匠号。
第三段，这一响之内，本坊落下的第几印。

匠号是死的。城里的印坊都要去匠籍司挂号，拿一个零到一千零二十三之间的号，终身不变，不许重。

于是两坊同时落印:第一段一样（同一响钟），第三段也可能一样（都是这一响的第一印），但第二段不同——号就不会重。

而同坊连落两印，第三段一定不同——号也不会重。

沈砚把印样交到匠籍司那天，只说了八个字：**不必相问，各自成号。**

— —

印成了，麻烦也跟着来，一共四个。

**第一个，是四千。**

第三段只有十二位，能记零到四千零九十五。也就是说，一声钟响之内，一家印坊最多落四千零九十五印。

那年秋税，东坊门前排了长队，一声钟响里递进来六千份。匠人只能把笔提起来干等着，等钟楼敲下一声，再接着落印。那一下是硬等——不是笔坏了，是号用完了。

沈砚在墙上写：一声钟，四千印；满了，就等下一声。

**第二个，是钟倒退。**

冬至那天，司钟人的徒弟校水漏，把钟拨回去了一小格。

那一格之内，钟楼又把旧号唱了一遍。印坊不知道，照旧落印——钟号相同、匠号相同、段数相同，印出来的号，和半个时辰前一模一样。

两份文书，一个号。这一回不是账簿的错，是钟的错。

沈砚立了第二道规矩：每坊要自己记住上一枚印的第一段。钟吐出的号若不比上一个大，笔不许落——要么等它真的走过去，要么去匠籍司换一个匠号重新起头。**宁可停印，不可重号。**

**第三个，是匠号。**

匠籍司原本只有一卷名册，谁来挂号就给下一个数。后来城北失火，烧了半条街，也烧了名册。重建时两个书吏各抄了一份，各自派号。于是两家印坊，都拿到了四百一十七。

两家都很守规矩。可规矩管不到别人手里的匠号。

沈砚这才明白，那八个字里藏着一个他没写出来的前提：**匠号永不重复**。这不是印局能保证的事，得由发号的人保证——一个号，同一时刻只能有一个活着的持有人。

**第四个，是年限。**

第一段只有四十一位。按毫刻算，够用六十九年。

六十九年后，钟楼的号会顶到顶，翻不回去。这件事没有补救的余地，只能刻在墙上，让下一代人知道日子。

— —

新印用了十七年，天枢阁再没出过重号。

书吏们还发现一件顺手的好处：把印号当成一个字来念，念出来的大小，差不多就是文书落印的先后。归档时不必翻到中间去找位置，新纸接着末尾放就行——一整柜的卷宗，从头到尾是顺的。

这是那枚印的第四样好处，沈砚自己也没想到。

同一年，城南有个老书吏提出了另一种印：不写钟、不写匠号，闭着眼从一袋乱数里抓一个数，抓出来就当号。那印确实也不容易重——抓两次抓出同一个数的机会，小到可以不管。可它长得太长，要一百二十八个记号；而且完全没有先后，归档时只能一页页翻着比对。

还有一个更省事的办法：全城只留一间总号房，谁要号，就去窗口领下一个。号一定不重，顺序也漂亮。可全城的纸都得排在一间屋子前面；那间屋子若熄了灯，整座城的文书就都停在窗口。

沈砚选了最难的那条：不设总窗口，让每个匠人自己发号——只要先把匠号分好，剩下的交给钟。

这时候你应该已经认出来了：这就是 *Snowflake*，分布式 ID 生成里被抄得最多的那一枚。

### 这是什么

*Snowflake* 是 Twitter 在 2010 年开源的 64 位 ID 方案。一枚 ID 就是一个 `int64`，按位切成几段：

- **1 位符号位**：留空，保证在 Java 的 `long` 里恒为正。
- **41 位毫秒时间戳**：从一个自定义纪元（*epoch*，Twitter 用 2010-11-04）起算，够用约 69 年。
- **10 位机器号**（*worker id*）：可拆成 5 位机房 + 5 位机器，最多 1024 个节点。
- **12 位序列号**（*sequence*）：同一毫秒内的自增计数，0–4095，也就是每个节点每毫秒最多 4096 个 ID，约 409 万个/秒。

生成一个 ID 的流程很短：取当前毫秒；若与上一毫秒相同，序列号加一，加满 4096 就**自旋等到下一毫秒**（"一声钟，四千印"）；若时钟比上一毫秒还小，说明发生了**时钟回拨**（*clock rollback*），要么拒绝出号，要么等时钟追上来，绝不能在旧毫秒上继续发。

关键在于它**不需要为每个 ID 做协调**。机器号是预先分配的，一旦分好，1024 个节点各发各的，互不通话，也不会撞号。整套方案里唯一需要协调的东西，就是机器号本身——通常由 ZooKeeper、etcd 的租约（*lease*）、Kubernetes 的 StatefulSet 序号，或者云厂商的实例元数据来保证"一个号同一时刻只有一个活着的持有者"。

### 为什么重要

自增主键（`AUTO_INCREMENT` / `SERIAL`）把发号权收在一个地方。它简单、有序、短，代价是：写入串行化、跨库分表要另想办法、主从切换时容易出重号或跳号，而且 ID 会泄露业务量——订单号连号，竞争对手就知道你一天卖多少。

UUIDv4 反过来：本地生成、永不冲突，但 128 位、完全随机。作为 InnoDB 的主键，随机插入会让 B+ 树的页分裂和缓存命中率一起变差；同样的行数，随机主键的索引体积和写入延迟都明显高于有序主键。

Snowflake 站在两者中间：**64 位、无需协调、按时间近似有序**。近似有序这一点在工程上价值极大——它让"按 ID 排序"约等于"按创建时间排序"，分页、增量同步、按时间归档都能直接借力，同时不必为每个 ID 付一次全局往返。

它的坑也全在位数里，而且每一个都真实发生过：

- **时钟回拨**。NTP 校时、虚拟机热迁移、宿主机的时钟漂移，都会让时钟往回跳，同毫秒的 ID 随之重复。实现必须在时钟倒退时拒绝服务，而不是"尽力而为"。
- **机器号冲突**。这是最常见的线上事故：两个 Pod 拿到同一个 `worker id`（滚动更新期间新旧副本短暂共存，或按 IP 末位派号造成碰撞），两个进程在同一毫秒发出同一个 ID。机器号必须由强一致的分配器发放，带租约与存活检测。
- **突发超过 4096/毫秒**。序列号耗尽，进程进入等待，吞吐被硬顶住。这是设计上限，不是 bug。
- **69 年年限**。41 位毫秒用完之后只能换纪元或加位，届时旧 ID 的排序会断裂。
- **跨节点的"有序"只是近似**。它保证同节点内单调，不保证全局全序；不同机器的时钟有几毫秒偏差，ID 的顺序就和真实发生的顺序有几毫秒错位。拿它当"严格的事件顺序"用，是常见的误读。

后来的方案大多是在这几段里做取舍：ULID 用 48 位毫秒 + 80 位随机数，换取"不必分配机器号"；Sonyflake 用 39 位 10 毫秒为单位 + 8 位序列 + 16 位机器号，把年限拉长、把每毫秒上限压低；Instagram 则把"分段 + 存储过程"搬进 Postgres，用数据库保证唯一、靠分片拿吞吐。

_隐喻对应表_

- 天枢阁的编号印 → 全局唯一 ID
- 九家印坊各记各的账 → 多节点本地生成，不为每个 ID 做协调
- 两份文书盖了同一个号 → ID 冲突（*collision*）
- 钟楼每"毫刻"一响 → 毫秒级时间戳（41 位）
- 全城只有一座钟楼 → 共享的单调时间源
- 第一段：钟响到第几声 → 时间戳位段
- 第二段：匠籍司发的匠号 → 机器号（*worker id*，10 位）
- 零到一千零二十三 → 1024 个节点上限
- 第三段：这一响内的第几印 → 序列号（*sequence*，12 位）
- "不必相问，各自成号" → 免协调的本地生成
- 一声钟四千零九十五印，满了等下一声 → 序列号耗尽后自旋到下一毫秒
- 钟被拨回去一小格 → 时钟回拨（*clock rollback*）
- 记住上一枚印的第一段，不增不放笔 → 时钟倒退时拒绝发号，保住单调
- 宁可停印，不可重号 → 唯一性优先于可用性
- 名册烧了，两个书吏各派一次号 → 机器号重复分配
- 两家印坊都拿到四百一十七 → worker id 冲突：同毫秒同序列必然撞号
- 一个号同一时刻只能有一个活着的持有人 → 租约（*lease*）保证机器号唯一
- 四十一位够用六十九年 → 时间戳位段耗尽（*epoch exhaustion*）
- 印号念出来大小近似落印先后 → 时间有序 ID，利于 B+ 树顺序插入与范围扫描
- 闭眼从乱数袋里抓号 → UUIDv4：本地唯一但无序、128 位
- 乱数印要一百二十八个记号 → UUID 做主键时索引膨胀
- 全城只留一间总号房 → 中心化自增序列，单点瓶颈
- 总号房熄灯，全城停在窗口 → 发号服务成为单点故障
- 新纸接着末尾放，整柜卷宗是顺的 → 有序主键的顺序写与范围查询
</section>
<section class="en" markdown="1">
The city of Stone Seal sat halfway up the mountain. It was small, but it held the only seal bureau on the whole trade road.

The rule was nine generations old: every document filed in the city — deeds, grain ledgers, pleadings, a craftsman's IOU — had to carry a numbered seal stamped in its upper right corner. The numbers could not repeat. If they did, the clerks at the Heaven's Pivot Archive would slide two different sheets into the same pigeonhole, and a year later a man asking for his land deed would pull out a stranger's debt.

For a long time this was not hard. Nine seal workshops kept nine separate ledgers: one, two, three, up to a few hundred, then a new book. Documents circulated inside the city and never crossed between workshops. Nine ledgers that never met never quarrelled.

Then the mountain road opened, three downstream counties were absorbed, and everything changed.

A single document might be drafted in the east shop, sealed again in the west, and filed back in the south. A grain ledger was split into five parts and copied simultaneously by five shops. That spring the archive had its incident: the east shop's number one thousand seven and the west shop's number one thousand seven entered the archive on the same day. Two sheets, one number. The clerk filed a land deed into the grain drawer.

Three months later the Liu family lost thirty irrigated acres.

— —

The head of the seal bureau was named Shen Yan.

He did not rush to replace the ledgers. He went to look at the bell tower.

The tower stood at the city's centre. Once every *hao-ke* — a hair-flick of time, too short for the ear to catch its echo, more than eight hundred thousand of them in a day — the bell-keeper struck the bell. And the tower's count was continuous: the number of the strike was the number of the hao-ke. There was only one tower in the city, so the count everyone heard was always the same count.

Shen Yan stood beneath that tower for a full day, and came away with a new seal.

Pressed once, the new seal left three segments on the page:

The first: which strike of the bell the tower had just rung.
The second: the workshop's guild number.
The third: how many seals this workshop had already pressed within that one strike.

The guild number was fixed. Every workshop registered with the guild registry and drew a number between zero and one thousand twenty-three, held for life, never repeated.

So two shops sealing at the same instant shared the first segment (same strike) and might share the third (both pressing their first seal of that strike), but never the second — and thus never the whole number.

And one shop pressing twice in a row produced a different third segment — also never the whole number.

The day he handed the sample to the registry, Shen Yan said eight words: **Ask no one. Each makes its own.**

— —

The seal worked, and the trouble came anyway, in four shapes.

**The first was four thousand.**

The third segment held twelve places, zero through four thousand ninety-five. Within a single bell strike, one workshop could press at most four thousand ninety-five seals.

That autumn's tax season, the line outside the east shop delivered six thousand sheets inside one strike. The sealers had to lift their brushes and simply wait for the next strike before pressing again. It was a hard wait — not a broken brush, but an exhausted number.

Shen Yan wrote on the wall: one strike, four thousand seals; when it fills, wait for the next.

**The second was a backward bell.**

At the winter solstice, the bell-keeper's apprentice calibrated the water clock and turned the bell back by a single notch.

Inside that notch the tower sang its old count again. The workshops did not know, and pressed as usual — same bell, same guild number, same ordinal. The seal that came out was identical to one pressed half an hour earlier.

Two documents, one number. This time the fault was not the ledger. It was the bell.

Shen Yan set a second rule: each workshop must remember the first segment of its last seal. If the tower's count was not greater than the previous one, the brush must not fall — either wait until the count truly passes, or go to the registry and take a new guild number to begin again. **Stop the press rather than repeat a number.**

**The third was the guild number itself.**

The registry had kept a single roll: whoever came to register received the next number. Then fire took half the north district, and the roll with it. Two clerks rebuilt it, each copying a version, each dispensing numbers. And so two workshops both held four hundred seventeen.

Both were perfectly obedient. But no rule reaches into another man's guild number.

Only then did Shen Yan understand the premise hidden inside his eight words: **guild numbers never repeat.** That was not something a seal bureau could guarantee. It had to be guaranteed by whoever dispensed them — one number, at any moment, with exactly one living holder.

**The fourth was the horizon.**

The first segment had only forty-one places. Counted in hao-ke, that was sixty-nine years.

After sixty-nine years the tower's count would reach its ceiling and could not roll over. There was no remedy for this. It could only be carved into the wall, so the next generation would know the date.

— —

The new seal served for seventeen years and the archive never saw a repeat.

The clerks discovered a convenience along the way: read the whole seal as a single number, and its size roughly followed the order in which the documents had been pressed. Filing no longer required opening a cabinet in the middle to find a slot — new sheets went onto the end. A whole cabinet of volumes ran straight through, front to back.

That was the fourth benefit of the seal, and Shen Yan had not designed it.

The same year, an old clerk in the south district proposed a different seal: no bell, no guild number — shut your eyes, draw a number from a sack of random digits, and use whatever comes out. That seal would not easily repeat either; the chance of drawing the same number twice was small enough to ignore. But it was long — one hundred twenty-eight marks — and it carried no order at all, so filing meant leafing through page after page to compare.

There was an even simpler scheme: keep one central number office in the city. Whoever wants a number queues at the window and receives the next one. No repeats, and the order would be beautiful. But every sheet in the city would stand in line before a single room; and if that room's lamp went out, every document in the city stopped at the window.

Shen Yan chose the hardest road: no central window, every sealer issues their own — as long as the guild numbers are settled first, the bell does the rest.

By now you have probably recognized it: this is *Snowflake*, the most copied design in distributed ID generation.

### What it is

*Snowflake* is a 64-bit ID scheme that Twitter open-sourced in 2010. One ID is one `int64`, cut into bit fields:

- **1 sign bit**: unused, so the value stays positive in a Java `long`.
- **41 bits of millisecond timestamp**: measured from a custom *epoch* (Twitter used 2010-11-04), enough for roughly 69 years.
- **10 bits of machine number** (the *worker id*): splittable into 5 bits of datacenter and 5 of machine, so at most 1024 nodes.
- **12 bits of sequence**: a per-millisecond counter, 0–4095. That is 4096 IDs per node per millisecond, about 4.09 million per second.

Generating one is short: read the current millisecond. If it equals the previous one, increment the sequence; when the sequence fills at 4096, **spin until the next millisecond** ("one strike, four thousand seals"). If the clock is *behind* the previous millisecond, a **clock rollback** has occurred — either refuse to issue, or wait for the clock to catch up. Never keep issuing on a stale millisecond.

The point is that it **needs no coordination per ID**. Worker ids are assigned in advance; once assigned, 1024 nodes issue independently, never speaking to each other, never colliding. The only thing in the whole design that needs coordination is the worker id itself — usually guaranteed by ZooKeeper, an etcd *lease*, a Kubernetes StatefulSet ordinal, or cloud instance metadata, so that one number has exactly one living holder at a time.

### Why it matters

An auto-increment primary key (`AUTO_INCREMENT` / `SERIAL`) concentrates the right to issue numbers in one place. It is simple, ordered, and short — at the cost of serialized writes, awkward cross-shard schemes, duplicate or skipped numbers across failovers, and IDs that leak business volume: consecutive order numbers tell a competitor exactly how much you sold today.

UUIDv4 is the opposite: generated locally, never colliding, but 128 bits and fully random. As an InnoDB primary key, random insertion hurts page splits and cache hit rate together; for the same row count, a random key's index is measurably larger and its write latency measurably worse than an ordered key's.

Snowflake sits between them: **64 bits, coordination-free, approximately time-ordered.** That approximate ordering is worth a great deal in practice — it makes "sort by ID" nearly equal to "sort by creation time", so pagination, incremental sync, and time-based archiving all ride on it, without paying a global round trip per ID.

Its failure modes are all in the bit layout, and every one of them has happened in production:

- **Clock rollback.** NTP correction, live VM migration, host clock drift — all can move the clock backwards, and IDs in the same millisecond repeat. An implementation must refuse service when the clock regresses, not do its best.
- **Worker id collision.** The most common real incident: two pods receive the same `worker id` (old and new replicas briefly coexisting during a rolling update, or ids derived from the last octet of an IP), and both issue the same ID in the same millisecond. Worker ids must come from a strongly consistent allocator with leases and liveness checks.
- **Bursts above 4096 per millisecond.** The sequence exhausts, the process waits, and throughput hits a hard ceiling. That is a design limit, not a bug.
- **The 69-year horizon.** Once 41 bits of milliseconds are spent, only a new epoch or more bits will do — and old and new IDs no longer sort together.
- **"Ordered" across nodes is only approximate.** Monotonicity is per node, not global. A few milliseconds of clock skew between machines means a few milliseconds of mismatch with real causal order; treating these IDs as strict event order is a common misreading.

Later designs mostly re-cut the same fields. ULID uses 48 bits of milliseconds plus 80 random bits, trading away worker-id allocation. Sonyflake uses 39 bits of 10-millisecond units, 8 bits of sequence, and 16 bits of machine, stretching the horizon and lowering the per-millisecond ceiling. Instagram moved "segments plus stored procedures" into Postgres, letting the database guarantee uniqueness while sharding supplies throughput.

_Metaphor mapping_

- The archive's numbered seal → a globally unique ID
- Nine shops keeping nine separate ledgers → many nodes generating locally, no coordination per ID
- Two documents bearing the same number → an ID collision
- One bell strike per hao-ke → the millisecond timestamp (41 bits)
- A single bell tower in the city → a shared monotonic time source
- Segment one: which strike → the timestamp field
- Segment two: the registry's guild number → the worker id (10 bits)
- Zero through one thousand twenty-three → the 1024-node ceiling
- Segment three: the ordinal within one strike → the sequence (12 bits)
- "Ask no one. Each makes its own." → coordination-free local generation
- Four thousand ninety-five to a strike, then wait → sequence exhaustion and spinning to the next millisecond
- The bell turned back one notch → clock rollback
- Remembering the last first segment; no increase, no press → refuse to issue on a regressed clock, preserve monotonicity
- Stop the press rather than repeat a number → uniqueness over availability
- The roll burns; two clerks each dispense numbers → duplicate worker-id assignment
- Two shops both holding four hundred seventeen → worker-id collision: same millisecond, same sequence, same ID
- One number, one living holder at a time → a lease that keeps worker ids unique
- Forty-one places, sixty-nine years → timestamp field exhaustion
- Reading the seal as one number ≈ press order → time-ordered IDs, sequential B+ tree inserts and range scans
- Drawing blind from a sack of random digits → UUIDv4: locally unique, unordered, 128 bits
- A random seal needing one hundred twenty-eight marks → the index bloat of UUID primary keys
- One central number office for the city → a centralized auto-increment sequence, a single bottleneck
- The lamp goes out and the whole city queues → the ID service becomes a single point of failure
- New sheets onto the end; the cabinet runs straight through → ordered keys, sequential writes, range queries
</section>
