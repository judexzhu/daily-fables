---
layout: fable
title: "四成的凭证 · Forty Percent of the Receipts"
title_zh: "四成的凭证"
title_en: "Forty Percent of the Receipts"
concept: "Data Skew in Distributed Joins (Hot-Key Salting)"
tags: [distributed-systems, databases, performance]
illustration: /assets/art/2026-11-03-data-skew-hot-key-salting.jpg
youtube_id: "c0lSRL6X_as"
---
<section class="zh" markdown="1">
河湾镇的秋税，靠一间分拣房和十间账房。

规矩不复杂。盐商、粮商年年从农户手里收货，货一离手就开一张凭证，凭证上只写三样：户号、数目、年份。秋后，这些凭证要挨张盖上户主姓名、村名、该户应纳的税额——这三样不在凭证上，在另外一本册子里，叫户册。三百万户，一户外一张。

凭证对户册，按户号一对，盖完章，才算完税。

一张一张翻是最笨的：三百万张户册乘上千万张凭证，一个人一辈子也翻不完。所以镇上立了十间账房，各管一片。

分法很巧妙：看户号的最后一位。尾数零进东一号房，尾数一进东二号房……尾数九进西五号房。户号是当年乱发的，尾数天然匀——每房分到三十万户户册，不多不少，一页不差。

凭证也照尾数分。往年确实匀：一户一年几十张凭证，一堆凭证流进十间房，每房手上都是薄薄一沓。

十间房同时开工，同时收工。谁先盖上最后一枚章，也得等最慢的那一间把册合上。秋税向来七天交齐。

这一年，镇上出了个陈家大宅。

陈家不是一户。账面上，它就是**一**户——一个户号，祖上分家时懒得拆，三百年的地全记在这一个号底下。可它不是几十张凭证，是四百多万张。镇上一半的粮、四成的盐，全走这一个号。

分拣房照老规矩，按尾数分。陈家户号尾数是七。四百多万张凭证，一张不差，全进了西二号房。

西二号房那夜点了四十盏灯。

其余九间房，头一夜就把自家那沓薄薄的凭证盖完了，第二天起就坐在廊下喝茶，等西二号房。

等了十一天。

老账房那晚坐在西二号房门口，想通了一件事：分得匀不匀，从来不看纸上写了多少张，只看纸上写的是谁的名字。尾数分房只保证"户"匀，不保证"凭证"匀。而十间房什么时候能收工，不看十间房加起来做了多少，只看最慢的那一间什么时候停手。

他先是想加人。西二号房从四个人加到三十个。头一天果然快了三倍。第三天，屋里挤得转不开身——账册只有一套，翻册的人只有一个，盖章的桌子只有一张。三十个人的手，全在等那一册。

人手不能加，那就换分法。

第二年秋，他添了一间房，什么也不做，只过秤。

凭证进门，先进分拣房：按户号尾数扔进十只木槽。扔完了，不急着装车，先称一称每只槽。

秤一响，事就露出来了：第七只槽，四千斤。其余九只，各八十斤。

这是老账房四十年没做过的一步——**先称，再分**。从前是先分，分错了，分完了，才回头知道分错了；那时候账已经欠了十一天。

四千斤的那只槽，得拆。

可拆不了。凭证上只有一个户号，尾数是七，扔多少遍还是进第七只槽。要让它们分开，就得在纸上加一样原本没有的东西。

老账房在陈家每一张凭证的背面，用朱笔写了一个小字：一二三四五六七八，循环着写，写满一轮再从头。加了这个小字，陈家就有了八个身份：陈七·一，陈七·二……陈七·八。

户册那边跟着办：陈家那张户册，抄八份，每份背后背一个同样的字。

于是第七只槽里的四千斤，散成八份，每份五百斤，落进八间不同的账房。八间房同时开工，八个"陈七·某"各对各的凭证。

一天半，八间房各自盖完，各自得出一个小计，送到一张小小的汇总桌上。加法一添，就是一个总数。

老账房在那张汇总桌前站了很久。他花了四十年想办法把活分匀，从来没想过——可以在同一户身上，先假装它变成了八户。

往后的年头，这件事变成了年年要改的数字。第三年盐价翻了，陈家的凭证成了八百万张，八个字不够，改成十六个字；第四年又添了两个字，写成二十四个。每年秋前，分拣房的秤一响，才知道今年该写几个字。字越多，账房越匀；可字越多，汇总桌上要添的小计也越多，八个变十六个，十六个变二十四个。

有一年账房们贪心，把字写到了六十四个。分拣房先崩了——一只槽要剖成六十四个小包，每包要贴签、要称、要装车、要派车夫。装箱的工夫，比盖章的工夫还长。那年的四十盏灯，亮在分拣房。

最难的是第五年。那年陈家旁边冒出三家新商号，各占一成半。

分拣房一称：第七只槽还是四千斤。剖开一看，里面不是一坨，是三坨，每坨一千三百斤。还有十几只小坨，各占几十斤。

老账房照旧法，给每一坨都写了八个字。三坨热户，八份小计，二十四份小计，全挤到那一张汇总桌上。

汇总桌成了新的西二号房。

他终于想明白了最后一层：**"热"不是一张固定的名单。**年年不同，有时候是一家，有时候是三家；有时候一坨重得像山，有时候十几坨各占一成。预先定死名单、定死字数，总有一年会对不上，那一年的账就烂在分拣房里。

那年冬天，他把规矩改成三步，从此年年照办。

第一步，分拣，称槽。槽子照旧按户号尾数分，分完只做一件事：量。这一步不改分法。

第二步，只剖重的。一只槽超过平均份数的两倍，才动刀。剖只在槽内剖——重的那一户拆成若干份，轻的那些户，一张不动，一个朱字不写。

第三步，合总。每个被拆开的户，自己的小计收拢到一张汇总桌上；没被拆过的户本来只有一份，直接入账，不经过汇总桌。汇总桌的大小，从此跟着热户的个数走，不跟着拆的份数走。

第六年秋，三百万张户册、八百万张凭证。十只槽里，重的散成三十份，轻的还是原样。十间房在同一个黄昏里陆续合上册子，那一年的秋税，第五天就交齐了。

老账房坐在廊下，秤还挂在分拣房的梁上。他想起四十年前师父那句话：分得匀不匀，不看纸有多少张，看纸上写的是谁。师父没往下说。往下那一句，是他自己补的：如果纸上写的那个人太集中，就在纸上，替他多起几个名字。

到了这一步，你大概已经认出它了：这就是 *data skew* 与 *hot-key salting*。

### 这是什么

分布式 *join* 有个前提：两边数据要按连接键 *shuffle* 到同一批分区里。同一个键的两边行必须落在同一个 *partition*，否则对不上。分区函数通常是 `hash(key) % N`。

这个前提默认了一件事：键的分布是匀的。一旦不匀，*hash partition* 就把大量同键数据压进同一个 *partition*，这就是 *data skew*（数据倾斜）。常见来源：一个超大客户、一个"其他"兜底值、一整批 *NULL* 键（它们会一起哈希到同一边）。

倾斜的杀伤力不在总数据量，在 *max task time*：一个 stage 的完成时间等于最慢那个 task 的时间。32 个 executor 里 31 个干了 30 秒、1 个干了 30 分钟，这个 stage 就是 30 分钟——集群的实际并行度掉到个位数。

*Hot-key salting* 分三件事：给热键的键值加随机后缀（`key#0 … key#S-1`）；把小表里对应的那几行复制 S 份，各挂一个后缀；于是热键的 N 行被拆到 S 个分区并行处理。每个分区各自算局部结果，最后再做一次小规模的二阶段聚合，把 S 份小计加回去。

关键在**只对热键做**。对全体键撒盐，等于把每个键的工作量乘 S、分区数乘 S、调度开销乘 S。

### 为什么重要

按平均数据量配资源，是分布式系统里最常见的一种自欺。倾斜把"平均"和"最慢"撕开：你按平均值买机器，按最大值等结果。

三项工程上的关键：

- **量在前**。不做统计就分片，等于闭着眼睛赌分布。现代引擎把这一步做进运行时：Spark 的 *AQE* 在 shuffle 写完之后拿到每个分区的 map statistics，再决定拆哪个分区——两个 stage 之间的一次重优化，成了常态。
- **只拆重的**。倾斜通常只出现在极少数键上。全量撒盐会让 99% 的键白付代价。阈值一般是"超过两倍平均份数"或"超过绝对字节数"。
- **合总也是瓶颈**。拆得越细，汇聚端越集中。二阶段聚合的规模必须与热键个数成正比，不与被拆的份数成正比。

代价要认：撒盐后结果集变大（小表那几行被复制 S 份）、多一个 stage、热键在两边都得能被识别出来。更好的路是**根本不做 shuffle**：如果小表能装进内存，*broadcast join*（*map-side join*）把它复制到每个 task，谁都不用重分区。但小表一旦是真的小不了，这条路就断了，只剩撒盐。

_隐喻对应表_

- 一户一张的户册 → 小表（dimension table），被加宽的一侧
- 一年几十张的收货凭证 → 大表（fact table）
- 按户号尾数进十间账房 → `hash(join_key) % N` 的 shuffle 分区
- 尾数天然匀、每房三十万户 → 小表侧天然均匀：倾斜几乎从不来自小表
- 陈家大宅：一个户号，四百多万张凭证 → 热键：单个键承载了实际数据的一大截
- 尾数只保证"户"匀，不保证"凭证"匀 → 哈希保证键空间均匀，不保证数据量均匀
- 十间房同开工同收工，等最慢那间合册 → stage 完成时间 = *max task time*，并行度被最长 task 吃掉
- 西二号房从四人加到三十人 → 纵向扩 executor / 加 core，买到的是线性收益的上限
- 账册只有一套、翻册只有一人、盖章只有一张桌 → 单 task 内部的串行瓶颈，加资源解决不了
- 凭证先进分拣房、扔进十只木槽 → shuffle write
- 称槽，发现第七只四千斤、其余各八十斤 → 运行时拿到每个分区的字节数，识别倾斜分区
- 超过平均两倍才动刀 → *AQE* 的倾斜阈值（`skewedPartitionFactor`、`skewedPartitionThresholdInBytes`）
- 轻的槽一张不动、一个朱字不写 → skew join 只处理超阈值分区，对其余分区零成本
- 凭证背后写一二三四五六七八 → 给热键值加随机后缀 `key#0 … key#S-1`
- 陈家户册抄八份，各背一个字 → 小表侧热键行复制 S 份，与后缀一一对齐
- 八个"陈七·某"落进八间房同时盖 → 热键数据被拆到 S 个分区并行处理
- 八个小计送到一张汇总桌 → 二阶段聚合：partial 结果再 final 合并
- 字越多越匀，字越多汇总越挤 → 盐度 S 的取舍：均衡增益递减，复制与汇聚开销线性上升
- 撒到六十四字，分拣房先崩 → 过细的盐把 shuffle 与调度开销变成新瓶颈
- 三坨各一千三，二十四份小计挤一张桌 → 多个热键时，汇聚端重新变成单点
- 汇总桌的大小跟着热户个数走 → 聚合规模与热键数成正比，与拆的份数无关
- "热"不是一张固定名单 → 倾斜随数据分布漂移，写死的盐表总有对不上的一年
- 陈家的地全记在一个号下 → 现实中热键的来源：单一大客户、默认值、兜底值、NULL 键
</section>
<section class="en" markdown="1">
The autumn tax of Riverbend depended on one sorting room and ten counting houses.

The rule was not complicated. Salt and grain merchants bought from farmers every year, and the moment goods left a hand, a receipt was written with exactly three things on it: household number, quantity, year. After the harvest, every one of those receipts had to be stamped with the householder's name, the village, and the tax owed — none of which appeared on the receipt. They lived in another register called the household roll. Three million households, one sheet each.

Receipt against roll, matched on household number, stamped, and only then was the tax settled.

Turning pages one at a time is the stupidest method available: three million roll sheets against ten million receipts is more than one lifetime of page-turning. So the town built ten counting houses, each responsible for a slice.

The slicing was clever: look at the last digit of the household number. A trailing zero went to East One, a trailing one to East Two, and a trailing nine to West Five. Household numbers had been issued haphazardly over the years, so the trailing digit was naturally even — each house received three hundred thousand roll sheets, no more, no less, not one page off.

Receipts were divided by the same digit. And in past years it truly was even: a household generates a few dozen receipts a year, a pile of receipts flowed into ten rooms, and every room held a thin stack.

All ten rooms started together and finished together. Whichever room stamped its last seal first still had to wait for the slowest room to close its ledger. The autumn tax was always settled in seven days.

Then came the year of the Chen Compound.

The Chen family was not a household. On paper it was **one** household — a single number, because three hundred years ago nobody had bothered to split the family registry, and three centuries of land sat under that one number. But it did not produce a few dozen receipts. It produced more than four million. Half the town's grain and forty percent of its salt moved under that single number.

The sorting room followed the old rule and went by the trailing digit. The Chen number ended in seven. All four million receipts, not one misplaced, went into West Two.

That night West Two lit forty lamps.

The other nine rooms cleared their thin stacks on the first night, and from the second morning on they sat in the corridor drinking tea, waiting for West Two.

They waited eleven days.

Sitting in West Two's doorway that night, the old accountant understood something. Evenness was never about how many sheets of paper there were. It was about whose names were written on them. The trailing digit guaranteed that *households* were even. It guaranteed nothing about *receipts*. And when ten rooms could close had nothing to do with how much the ten rooms did together — only with when the slowest one stopped.

His first thought was more hands. West Two went from four people to thirty. For one day it was genuinely three times faster. By the third day the room was too crowded to turn around in: there was one set of ledgers, one person who could turn its pages, one table with room for one seal. Thirty pairs of hands were all waiting on that one register.

You cannot add hands. So change how the work is divided.

The next autumn he added a room that did nothing but weigh.

Receipts came in and went first to the sorting room: thrown into ten wooden bins by trailing digit. Then, before loading the carts, weigh each bin.

The scale spoke, and the truth was plain. Bin seven: four thousand catties. The other nine: eighty each.

This was the step the old accountant had never taken in forty years — **weigh before you divide**. Before, he divided, got it wrong, finished dividing, and only then learned it had gone wrong. By then the tax was eleven days in arrears.

The four-thousand-catty bin had to be split.

But it could not be split. A receipt carries one household number, the number ends in seven, and no matter how many times you throw it, it lands in bin seven. To separate those receipts you must add something to the paper that was never there.

On the back of every Chen receipt the old accountant wrote one small character in vermilion: one, two, three, four, five, six, seven, eight — cycling, then cycling again. With that character, the Chen household acquired eight identities: Chen-seven-one, Chen-seven-two, and so on through Chen-seven-eight.

The roll side followed suit: the single Chen sheet was copied eight times, and each copy carried one of the same characters on its back.

So the four thousand catties in bin seven became eight parts of five hundred, landing in eight different counting houses. Eight rooms started at once; eight "Chen-seven-something" entries each matched against their own receipts.

A day and a half later the eight rooms had each closed their portion and produced a subtotal, which went to one small reconciliation table. One addition, and the total existed.

The old accountant stood at that table for a long time. Forty years he had tried to divide the work evenly. It had never occurred to him that you could take one household and, for a while, pretend it was eight.

In later years, that number became a figure he had to revise annually. In the third year the salt price doubled and the Chen receipts reached eight million; eight characters were not enough, so he used sixteen. A few years on he wrote twenty-four. Every autumn, before the harvest, the sorting room's scale spoke and told him how many characters this year required. More characters meant more even rooms; more characters also meant more subtotals on the reconciliation table — eight becoming sixteen, sixteen becoming twenty-four.

One year the houses got greedy and wrote sixty-four characters. The sorting room collapsed first: one bin had to be carved into sixty-four parcels, and every parcel had to be tagged, weighed, loaded, and carted. The packing took longer than the stamping. That year the forty lamps burned in the sorting room.

The hardest year was the fifth. Three new trading houses opened beside the Chens, each taking fifteen percent.

The sorting room weighed: bin seven was still four thousand catties. Opened up, it was not one lump but three — thirteen hundred each — plus a dozen small lumps of a few dozen catties apiece.

The old accountant applied the old method and gave each lump eight characters. Three hot households, eight subtotals each, twenty-four subtotals, all crowded onto one reconciliation table.

The reconciliation table became the new West Two.

And he finally understood the last layer: **"hot" is not a fixed roster.** It differs every year. Sometimes one household, sometimes three. Sometimes one lump as heavy as a mountain, sometimes a dozen lumps at ten percent apiece. Fix the roster in advance, fix the number of characters in advance, and one year it will not match — and that year's accounts rot in the sorting room.

That winter he rewrote the rule as three steps, and kept to them thereafter.

First: sort, then weigh. The bins were still divided by trailing digit; the only addition was measurement. This step changes no division.

Second: carve only what is heavy. A bin more than twice the average share gets the knife. And the carving stays inside the bin — the heavy household is split into parts, while the light households are untouched, not one character written.

Third: reconcile. Every split household gathers its own subtotals onto one table; households that were never split have exactly one part and enter the ledger directly, never touching the table. From then on the table's size tracked the number of hot households, not the number of parts.

In the sixth autumn: three million roll sheets, eight million receipts. In the ten bins, the heavy ones scattered into thirty parts and the light ones stayed as they were. The ten rooms closed their ledgers one after another in a single evening, and that year's autumn tax was settled on the fifth day.

The old accountant sat in the corridor with the scale still hanging from the sorting-room beam. He remembered his master's words from forty years before: whether work divides evenly is not about how many sheets there are — it is about whose names are on them. His master had not gone on. The next sentence was his own: if the name on the paper is too concentrated, then for that paper, give it a few more names.

By now you have probably recognized it: this is *data skew* and *hot-key salting*.

### What it is

A distributed *join* rests on a precondition: both sides must be *shuffled* into the same set of partitions by the join key. Rows sharing a key must land in the same *partition*, or they cannot meet. The partitioning function is usually `hash(key) % N`.

That precondition assumes one thing: keys are evenly distributed. When they are not, *hash partition* crushes a mass of same-key data into a single *partition* — *data skew*. Common sources: one enormous customer, a catch-all "other" value, and a whole batch of *NULL* keys (which all hash to the same side).

Skew's damage is not in the total data volume but in *max task time*: a stage finishes when its slowest task finishes. If 31 of 32 executors take 30 seconds and one takes 30 minutes, the stage takes 30 minutes — effective parallelism collapses to single digits.

*Hot-key salting* is three moves: append a random suffix to the hot key's value (`key#0 … key#S-1`); copy the matching few rows of the small side S times, each carrying one suffix; and the hot key's N rows now spread across S partitions to be processed in parallel. Each partition computes a partial result, and a small two-stage aggregation adds the S subtotals back together.

The essential part is **doing it only for hot keys**. Salt every key and you multiply every key's work by S, the partition count by S, and the scheduling overhead by S.

### Why it matters

Sizing resources by average data volume is the most common self-deception in distributed systems. Skew tears "average" away from "slowest": you buy machines by the mean and wait for the maximum.

Three things matter in practice:

- **Measure first.** Partitioning without statistics is gambling blind on a distribution. Modern engines move this into the runtime: Spark's *AQE* collects map statistics per partition after the shuffle write and then decides which partitions to split — a re-optimization between two stages, treated as normal.
- **Split only the heavy ones.** Skew lives in a tiny number of keys. Salting everything makes 99% of keys pay for nothing. The threshold is usually "more than twice the average share" or an absolute byte limit.
- **Reconciliation is a bottleneck too.** The finer you split, the more concentrated the merge. Two-stage aggregation must scale with the number of hot keys, not with the number of parts.

Know the costs: the result set grows (the small side's rows are replicated S times), an extra stage appears, and the hot keys must be identifiable on both sides. The better route is **not to shuffle at all**: if the small side fits in memory, a *broadcast join* (*map-side join*) replicates it to every task and nobody repartitions. But once the small side genuinely is not small, that road is closed and salting is all that is left.

_Metaphor mapping_

- One roll sheet per household → the small side (dimension table), the side that gets widened
- A few dozen receipts a year → the large side (fact table)
- Ten counting houses by trailing digit → `hash(join_key) % N` shuffle partitioning
- Naturally even trailing digits, three hundred thousand sheets each → the small side is naturally uniform; skew almost never comes from it
- The Chen Compound: one household number, four million receipts → a hot key: one key carrying a large slice of the actual data
- The digit guarantees even *households*, not even *receipts* → hashing evens the key space, not the data volume
- Ten rooms start and finish together, waiting on the slowest to close → stage time = *max task time*; parallelism eaten by the longest task
- West Two going from four people to thirty → vertical scaling (more executors/cores), which has a ceiling
- One set of ledgers, one page-turner, one sealing table → serial bottleneck inside a single task; more resources do not help
- Receipts entering the sorting room and its ten bins → shuffle write
- Weighing the bins: seven at four thousand catties, the rest at eighty → runtime per-partition byte sizes reveal the skewed partitions
- Carving only bins over twice the average → *AQE* skew thresholds (`skewedPartitionFactor`, `skewedPartitionThresholdInBytes`)
- Light bins untouched, not one vermilion character → skew join processes only over-threshold partitions; zero cost for the rest
- Writing one through eight on the receipt backs → appending a random suffix `key#0 … key#S-1` to the hot key value
- Copying the Chen roll sheet eight times, each with one character → replicating the small side's hot-key row S times, aligned to the suffixes
- Eight "Chen-seven-something" entries sealed in eight rooms at once → the hot key's data split across S partitions in parallel
- Eight subtotals delivered to one reconciliation table → two-stage aggregation: partial results, then a final merge
- More characters mean even rooms and a more crowded table → the salt-degree S trade-off: diminishing balance gains, linearly rising replication and merge costs
- Sixty-four characters and the sorting room collapses → over-salting turns shuffle and scheduling overhead into the new bottleneck
- Three lumps of thirteen hundred, twenty-four subtotals on one table → with multiple hot keys, the merge side becomes a single point again
- Table size tracking the number of hot households → aggregation scales with hot-key count, not part count
- "Hot" is not a fixed roster → skew drifts with the data; a hardcoded salt table will fail in some year
- All the Chen land under one number → the real-world sources of hot keys: a single large customer, a default value, a catch-all, a NULL key
</section>
