---
layout: fable
title: "第七号纸 · Sheet Number Seven"
title_zh: "第七号纸"
title_en: "Sheet Number Seven"
concept: "False sharing and cache line coherence"
tags: [linux, performance]
illustration: /assets/art/2026-08-08-false-sharing-cache-coherence.jpg
---
<section class="zh" markdown="1">
海关旁边那间账房，有条规矩外人总觉得别扭。

账房里真正的账，锁在里屋的铁皮柜里。柜子归掌柜管，谁也不许在柜子上直接落笔。要记账，得先叫跑腿的小满进里屋，把那一张纸**整张**取出来，端到你桌上——整张取，整张还，从来不撕、不摘、不抄半行。你只想改一个数？也得把那一整张搬过来。

外屋两张桌子。东边阿栗，管盐；西边阿桑，管麻绳。两个人一辈子没算过对方一笔账。

还有一条规矩更要紧：同一张纸可以有好几份誊本同时摊在各人桌上——**只要谁都不动笔**。谁要动笔，就得先让小满摇一遍铃，把别人桌上那张同号的誊本统统收走撕掉；从此这张纸，只认他手上这一份。写完了，纸回柜子，别人要用，重取。

那年夏天，账房慢得出奇。

掌柜起初以为是人躲懒。他站在门口看了半晌，看见的却是两个脚不沾地的人——只不过大半的忙，是在等。阿栗写完一个数，笔还没搁稳，小满的铃就响了；阿桑桌上刚摊平的纸被抽走，他只好停笔，等小满跑一趟里屋重新取回来。取回来，他写下一个数，铃又响，这回轮到阿栗那份被收走。两个人像隔着一堵墙互相拆对方屋顶，一整天，谁也没写满半页。

掌柜挨个问：你动过他的账没有？两个都说没有。他们说的是实话。

后来他把两天的取纸记录摊开来数，才看出门道。小满那两天跑了一千多趟，其中八百多趟，取的是同一张——第七号。

第七号纸上头四行记盐，下头四行记麻绳。中间一道墨线，清清楚楚，从没串过行。

可柜子不认那道墨线。柜子只认纸。铃也只认纸。

想通之后掌柜做了两件事。头一件很笨也很管用：把麻绳那四行挪到一张全新的纸上，第七号的下半张就那么空着。伙计心疼纸，掌柜说，纸比腿便宜。第二件更省事：两人桌上各摆一块小石板，一天里的零碎先记在自己石板上，谁也不惊动谁；到关门前，一人取一次纸，把当天的总数一笔写进去，回柜。

那个月的账还是一笔没错——本来也从没错过。只是小满一天少跑了七百趟。

——到这儿你大概已经认出来了：第七号纸是一条 *cache line*，小满的铃是缓存一致性协议里的 invalidation，而阿栗和阿桑撞上的，是 *false sharing*（伪共享）。

### 这是什么

CPU 与内存之间搬运数据的最小单位，不是一个字节，也不是一个变量，而是一条 *cache line*——主流 x86_64 与 aarch64 上通常是 64 字节。每个核心有自己的 L1 缓存；同一条 cache line 可以被多个核心同时以只读方式持有，但任何核心要写它，必须先取得独占（MESI 协议里的 Exclusive / Modified），并让其余所有副本作废。

false sharing 说的就是：两个线程各自读写**互不相干**的变量，而这两个变量恰好落在同一条 cache line 上。硬件的粒度是行，不是变量，于是每一次写都会打掉对方的缓存行，双方在总线上来回争夺同一行的所有权（cache line ping-pong）。

### 为什么重要

- 它不产生任何错误结果，只吞吃吞吐量。逻辑对、测试全过，就是慢——而且核心越多越慢，看上去像是"多线程没有扩展性"。
- 典型现场：并发计数器数组 `counters[thread_id]`、结构体里紧挨着的一读一写两个热字段、按线程分片却没做对齐的统计量、锁和它保护的数据挤在一起。
- 诊断：Linux 上用 `perf c2c record` / `perf c2c report` 直接定位 HITM（缓存行被别的核心以修改态命中）；也可留意 L1d 命中率正常、IPC 却异常低的情形。
- 常见修法：热点字段按 cache line 对齐并填充（C 的 `alignas(64)`、Go 的 `_ [64]byte`、Java 的 `@Contended`）；改用 per-CPU / thread-local 累加，收尾合并一次；调整字段顺序，让"只读的"和"常写的"分居两行。
- 反面提醒：padding 是拿内存换带宽，别无差别地给所有结构体都加。先测，再补。

_隐喻对应表_

- 里屋铁皮柜里的账 → 主存
- 各人桌上的誊本 → 每个核心私有的 L1 cache
- 整张取、整张还 → cache line 是缓存与内存之间的最小传输单位（通常 64 字节）
- 第七号纸 → 一条 cache line
- 上四行记盐、下四行记麻绳 → 落在同一条 cache line 上、彼此无关的两个变量
- 谁都不动笔时可多人各持一份 → MESI 的 Shared 状态，多核并发只读
- 动笔前先摇铃收走别人的誊本 → 写操作需先取得独占并 invalidate 其他副本
- 铃响一次、对方停笔重取 → 缓存行失效带来的 stall 与重新取行
- 两人一整天没写满半页 → cache line ping-pong 导致的吞吐塌陷
- 中间那道墨线 → 变量之间的逻辑边界；硬件不认它
- 账一笔没错，只是慢 → false sharing 只损性能，不损正确性
- 小满的跑腿记录 → `perf c2c` 之类的缓存一致性剖析
- 麻绳挪去新纸、下半张空着 → cache line 对齐加 padding
- 桌上的小石板、关门前汇总一笔 → per-CPU / thread-local 累加，最后合并
</section>
<section class="en" markdown="1">
The counting house beside the customs shed had a rule that outsiders always found strange.

The real books lived in an iron cabinet in the back room. The cabinet belonged to the master, and no one was allowed to write on anything while it sat there. To record a figure, you sent Xiaoman, the runner, into the back room to fetch the sheet — **the whole sheet**, carried out and carried back whole. Never torn, never a line copied out on its own. You only wanted to change one number? You still moved the entire sheet.

Two desks in the front room. Li on the east side kept the salt accounts. Sang on the west side kept the rope accounts. In all their years neither had ever added up a single figure belonging to the other.

And one more rule, the important one: several copies of the same sheet could lie open on several desks at once — **as long as nobody picked up a pen**. The moment someone wanted to write, Xiaoman rang his little bell and went round collecting every other copy of that sheet and tearing it up. From then on, that sheet existed only in the writer's hands. When the writing was done the sheet went back to the cabinet, and anyone else who wanted it had to send for it again.

That summer the counting house slowed to a crawl.

The master assumed someone was idling. He stood in the doorway and watched, and what he saw was two people who never sat still — except that most of their motion was waiting. Li would set down a figure, and before his pen was fully at rest the bell rang; the sheet Sang had just smoothed flat was pulled off his desk, and he had to stop and wait while Xiaoman ran to the back room and fetched it again. It came back. Sang wrote one number. The bell rang again, and this time it was Li's copy that went. The two of them were like men on opposite sides of a wall, each pulling the roof off the other's house. By closing, neither had filled half a page.

The master asked them each in turn: have you been touching his accounts? Both said no. Both were telling the truth.

Then he laid out two days of fetch records and counted. Xiaoman had made over a thousand trips in those two days, and more than eight hundred of them had been for the same sheet — number seven.

Sheet seven carried salt on its top four lines and rope on its bottom four. An ink rule ran between them, perfectly clear, never once crossed.

But the cabinet did not recognize that ink rule. The cabinet only recognized sheets. And so did the bell.

Having seen it, the master did two things. The first was crude and worked: he moved the four rope lines onto a fresh sheet and left the bottom half of sheet seven blank forever. The clerks winced at the waste of paper. Paper, said the master, is cheaper than legs. The second was easier still: he put a small slate on each desk. All day the odd figures went onto your own slate, disturbing nobody; at closing, each clerk sent for his sheet once, wrote the day's total in a single stroke, and sent it back.

That month's books were, as always, correct to the last figure. They had never been anything else. It was only that Xiaoman now ran seven hundred fewer errands a day.

— By now you have probably recognized it: sheet seven is a *cache line*, Xiaoman's bell is invalidation in a cache coherence protocol, and what Li and Sang ran into is *false sharing*.

### What it is

The smallest unit of data moved between CPU and memory is not a byte and not a variable — it is a *cache line*, typically 64 bytes on mainstream x86_64 and aarch64. Each core has its own L1 cache. A given cache line may be held simultaneously by many cores for reading, but any core that wants to write it must first acquire exclusive ownership (Exclusive / Modified in the MESI protocol) and invalidate every other copy.

False sharing is what happens when two threads read and write **completely unrelated** variables that happen to land on the same cache line. The hardware's granularity is the line, not the variable, so every write knocks out the other side's copy and the two cores bounce ownership of one line back and forth across the interconnect — cache line ping-pong.

### Why it matters

- It produces no wrong answers, only lost throughput. The logic is right, the tests pass, it is simply slow — and it gets slower on machines with more cores, so it masquerades as "our threading just doesn't scale".
- Classic sites: a `counters[thread_id]` array, two hot fields sitting adjacent in a struct where one is read-mostly and one is write-heavy, per-thread statistics sharded but not aligned, a lock packed next to the data it protects.
- Diagnosis: on Linux, `perf c2c record` / `perf c2c report` points straight at HITM events (a line hit in modified state in another core's cache). Also suspect it when L1d hit rates look fine but IPC is inexplicably low.
- Common fixes: align and pad hot fields to a cache line (`alignas(64)` in C, a `_ [64]byte` filler in Go, `@Contended` in Java); accumulate into per-CPU or thread-local counters and merge once at the end; reorder struct fields so read-mostly and write-heavy data live on separate lines.
- The counterweight: padding trades memory for bandwidth. Do not sprinkle it over every struct. Measure first, then pad.

_Metaphor mapping_

- The books in the iron cabinet → main memory
- The copy on each clerk's desk → each core's private L1 cache
- Fetched whole, returned whole → the cache line is the minimum transfer unit between cache and memory (usually 64 bytes)
- Sheet number seven → a single cache line
- Salt on the top lines, rope on the bottom → two unrelated variables that happen to share one cache line
- Many copies allowed while nobody writes → the Shared state in MESI; concurrent read-only access across cores
- Ringing the bell to collect every other copy before writing → a write must acquire exclusivity and invalidate all other copies
- Each ring forcing the other to stop and re-fetch → stalls and line refills caused by invalidation
- Neither filling half a page all day → throughput collapse from cache line ping-pong
- The ink rule down the middle of the sheet → the logical boundary between variables, which the hardware does not honor
- Books correct but slow → false sharing costs performance, never correctness
- Xiaoman's fetch records → cache coherence profiling, e.g. `perf c2c`
- Rope moved to a fresh sheet, half a sheet left blank → cache line alignment plus padding
- The slates on the desks, totalled once at closing → per-CPU / thread-local accumulation, merged at the end
</section>
