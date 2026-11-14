---
layout: fable
title: "守潮人的簿子 · The Tide Keeper's Ledger"
title_zh: "守潮人的簿子"
title_en: "The Tide Keeper's Ledger"
concept: "Gorilla Time-Series Compression"
tags: [databases, storage, observability]
illustration: /assets/art/2026-11-13-gorilla-tsdb-compression.jpg
youtube_id: "_qQ-mJrak68"
---
<section class="zh" markdown="1">
望潮塔立在海崖最东头，塔顶那口铜钟是给渔船报潮的。

守塔的人叫阿潮。每隔十分钟，他要爬上塔顶，看一眼石壁上的水尺，把时辰和水位一并记进簿子里。水位要记到小数点后六位——这是京城海务司定的规矩，因为潮汐表就是照着这些簿子算出来的，差一分，渔船的锚就下错了地方。

阿潮记得一丝不苟。一年下来，簿子堆满了整间屋子。

麻烦在每年冬末。所有簿子要装船，绕过半个海疆送进京城。船就那么大，而簿子年年增厚。有一年装了三条船；再往后，海务司的司徒大人来信说：明年只能派一条船。信末还添了一句冰冷的话——**一个字都不许省**。

阿潮试过省。他试过把六位小数砍成三位，收到回信挨了骂；他试过只记"大概水位"，海务司说那份潮汐表让三条渔船搁浅了。他要的不是"少记一点"，而是**把同样的意思写得更短**。

于是他坐在灯下，从头翻自己的簿子。翻到第七本，他先看出了一件小事。

每一行的时辰，从早到晚，写的都是同一个间隔：十分钟、十分钟、十分钟。他这本簿子里"十分钟"这三个字，一年写了一万遍。他把它改成**只记间隔**——第一行写"十分钟"，后面不必再写。

再翻几本，他看出了第二件小事：**间隔自己也很少变。** 只有当他在塔顶耽搁了、或者换班的人晚到了一会儿，间隔才会变一点。于是他不再写间隔，只写**间隔变了多少**——而它几乎永远是零。

他在簿子上添了新的记法：

- 一个点 `·` —— 间隔一点没变（绝大多数情况，一个符号）
- 一画 `—` 加个小数 —— 间隔变了一点点
- 两画 `——` 加个中数 —— 变得多些
- 三画 `———` 加个大数 —— 变了半刻钟以上
- 四画 `————` —— 干脆把完整的间隔重新报一遍

一万行时辰，压成了大半页点。

接着他盯上了水位那一列。这一列才占簿子的九成。他把相邻两次水位都写成二进制，一行一行对齐着看，忽然发现：**上下两行，左边的开头一模一样，右边的末尾也常常一模一样**，真正不同的只是中间那一小截。

理由其实很朴素：海水不会上一刻一米二、下一刻三百米。它总是在同一个量级里慢慢挪，所以前面那些位长期不动；而水尺刻到六位小数，末几位又常常是零。两行一比，中间那截短的只有几位。

于是他改了水位的写法：先记**左边从第几位起开始相同**、**右边到第几位为止相同**，再把中间那截不同的一小截抄下来。一行水位，从六十多个符号缩成了十来个。风平浪静的夜里，前后两次水位一模一样，他连中间那截都不用写——只落一个点：`·`，跟上次一样。

半年下来，他省掉了九成。

真正的坎在九月。

一场台风过境，海水在两个时辰里涨了两丈。水位从一米三跳到三丈一——量级变了，左边相同的位数从四十多位缩成了几位，右边也全乱了。他之前想了个偷懒的办法：把那两个"相同到第几位"的数字写在簿子第一页，之后一直沿用。台风一来，这个写法要么记不下真实的水位（那就等于偷偷省字，犯了司徒大人的忌讳），要么他得把幅度留得极宽，宽到平时每一个点都要多写十几个符号。

他坐在灯下，看着被风刮歪的窗，明白了一件事：**"两端相同到哪儿"，并不该是整本簿子的规矩，而该是每一行自己的决定。**

他重新定了格式。每一行水位开头，先落一个小记号：

- `··` —— 两个点：这一行沿用上一行的那对"相同到哪里"的数，只抄中间那截（平日的绝大多数）
- `·|` —— 一个点一竖：这一行我自己重新报那对数，然后再抄中间那截（台风、换季、量级跳变时才用）

一个字也没有多花。台风那几页，簿子确实厚了一些——但只厚了那几页。

那年冬天，阿潮把十年的簿子装进一只布袋，交给了船。司徒大人拆开看了三天，把每一行都还原了出来，一分不差。他问阿潮用了什么法术。

阿潮说：我什么都没省，我只是**不再重复写那些从来没变过的东西**。

——到这儿你大概已经认出来了：这就是 *Gorilla 压缩*。阿潮只写"间隔变了多少"，是 *delta-of-delta* 时间戳编码；他把相邻两次水位异或、只抄中间那一小截，是 *XOR* 浮点编码；那个"沿用上一行的窗口还是重新报一个窗口"的两点记号，就是 *Gorilla* 里的两位控制位。

### 这是什么

*Gorilla* 是 Facebook 在 2015 年提出的内存时序数据库压缩方案（VLDB 论文《Gorilla: A Fast, Scalable, In-Memory Time Series Database》）。它要解决的是监控数据的存储问题：一条指标每 10 秒一个点，一年 300 多万个点，每个点若按"8 字节时间戳 + 8 字节 float64"存，就是 16 字节、约 50MB。而指标数据绝大多数**可预测**——采样间隔固定，数值缓慢变化。Gorilla 就吃这两份可预测性，做到平均 **1.37 字节一个点**，约 12 倍压缩，且**完全无损**。

时间戳：先存起始时间，之后每个点存"间隔"（*delta*），再之后每个点存"间隔的变化量"（*delta-of-delta*）。定时采集时 dod 恒为 0，只需 **1 个 bit**。其余情况按大小分桶：`10` + 7 bit（±63）、`110` + 9 bit、`1110` + 12 bit、`1111` + 32 bit。时间戳几乎白送。

数值：把当前 float64 与上一个做 *XOR*。IEEE 754 的位布局让相邻且相近的数值共享指数位和高位尾数，异或结果前面一大片 0、后面也一大片 0。于是：

- XOR 为 0（值完全没变）→ 一个 bit `0`
- XOR 非 0 → `1`，再看有效区间（*meaningful bits*）：若它的前导零数和尾随零数都落在上一行记下的窗口内，就用 `0` + 直接抄中间那截（**复用窗口**）；否则用 `1` + 5 bit 前导零数 + 6 bit 长度 + 中间那截（**重报窗口**）

两个细节最见功夫：一是**窗口按每个值决定**，不做全局假设——所以数据剧烈跳变时只是那一段变胖，不会牺牲正确性，也不会拖累平时；二是**解码只需要上一个值**，状态是 O(1)，不用回看整块数据，所以它适合流式写入、也适合在读取时边解边算。

### 为什么重要

因为你每天用的监控系统几乎都在这条路上。Prometheus 的本地 TSDB 用 *delta-of-delta* 时间戳加 *XOR* 变体，InfluxDB 的 TSM、VictoriaMetrics、Gorilla 本身、AWS Timestream——同一套思路的不同变奏。一个集群的指标量动辄上亿条时间线，压缩比的差别不是"省点硬盘"，而是**决定了单机能不能扛住、查询要不要扫过量数据、账单是一位数还是两位数**。

更深的一层是选型哲学。通用压缩（gzip、zstd）按块编码，压缩比可能更高，但**解压必须从头来**：想取第 100 万个点，得先把前面全解开，随机访问没了；CPU 代价也高。Gorilla 放弃了"榨干熵"，换取三件在存储引擎里更贵的东西：

- **无损**——监控数据被有损压缩后，`p99` 就不对了，而 `p99` 正是你要看的东西
- **流式与 O(1) 状态**——一个点只依赖前一个点，写入和解码都能边走边算
- **最坏情况有界**——数据再乱，一个点也只多出几位，不会出现"这段数据压不动"的悬崖

它教的是一种普适手法：**要压缩的不是数据本身，而是数据里"没有变化"的那部分。** 恒定间隔、缓慢漂移、稳定前缀——任何一条管道里长期重复的东西，都值得被识别出来、只写一次。

_隐喻对应表_

- **阿潮的簿子** —— 时序数据库里的一块时间线数据（time series）
- **"一个字都不许省"** —— 无损压缩的硬约束，监控值不允许有损近似
- **每十分钟记一次** —— 固定采样间隔（scrape interval）
- **先写"十分钟"，后面只记间隔** —— *delta* 时间戳编码
- **只记"间隔变了多少"，几乎总是零** —— *delta-of-delta* 编码，dod=0 时只花 1 bit
- **一个点 `·`** —— dod 为 0 的单比特标记
- **一画/两画/三画/四画加数** —— 按 dod 大小分桶：`10`+7bit / `110`+9bit / `1110`+12bit / `1111`+32bit
- **相邻两行水位左边开头、右边末尾都一样** —— IEEE 754 下相近数值共享高位，且定精度小数尾部位常为零
- **只抄中间那一小截不同的** —— 对前值做 *XOR* 后只写 meaningful bits
- **水位一模一样时只落一个点** —— XOR 为 0，编码成单个 `0` bit
- **记下"左边相同到第几位、右边到第几位"** —— 前导零数（leading zeros）与尾随零数（trailing zeros）构成的窗口
- **把这对数写在第一页、之后一直沿用** —— 全局固定窗口的错解：跳变时要么丢数据，要么给平时留出巨大冗余
- **"两端相同到哪儿该由每一行自己决定"** —— 窗口按每个值判断，这是 Gorilla 的关键设计
- **`··` 沿用上一行的窗口** —— 控制位 `10`：复用窗口，只写中间那截
- **`·|` 这一行重新报窗口** —— 控制位 `11`：5 bit 前导零数 + 6 bit 长度，再写中间那截
- **台风那几页确实变厚，但只厚了那几页** —— 最坏情况只有局部膨胀，不牺牲正确性、不拖累常态
- **十年簿子装进一只布袋** —— 约 12 倍压缩，1.37 字节/点
- **司徒大人逐行还原，一分不差** —— 无损、可流式解码，且只需前一个值作为状态
</section>
<section class="en" markdown="1">
The Tide-Watching Tower stands at the far eastern edge of the sea cliff. The bronze bell at its top tells the fishing boats what the water is doing.

The keeper is called A-Chao. Every ten minutes he climbs to the top, reads the water gauge cut into the stone wall, and writes the hour and the water level into his ledger. The level is recorded to six decimal places — a rule set by the Maritime Office in the capital, because the tide tables are computed from these ledgers. Get a digit wrong and a fishing boat drops anchor in the wrong place.

A-Chao is meticulous. In a year, the ledgers fill a whole room.

The trouble comes at the end of every winter. All the ledgers must be loaded onto ships and carried halfway around the coast to the capital. The ships are only so large, and the ledgers grow thicker every year. One year it took three ships; after that, Minister Situ wrote from the Maritime Office to say that next year there would be one. At the end of the letter was a cold sentence: **not a single character may be omitted.**

A-Chao tried omitting. He tried cutting six decimals to three and got a scolding by return mail. He tried writing "about this much" and the Maritime Office wrote back that three fishing boats had run aground on that tide table. What he needed was not to record less. He needed to **write the same thing in fewer characters**.

So he sat down under the lamp and read his own ledgers from the beginning. In the seventh volume he noticed something small.

Every row's hour read the same interval: ten minutes, ten minutes, ten minutes. The words "ten minutes" appeared ten thousand times in one year. He changed the format to record **only the interval** — write it once at the top, then never again.

A few volumes later he noticed a second small thing: **the interval itself hardly ever changed.** Only when he was late on the stairs, or a relief keeper arrived late, did it shift. So he stopped writing the interval and wrote **how much the interval had changed** — which was almost always zero.

He added new marks to the ledger:

- A single dot `·` — the interval did not change at all (the overwhelming majority; one mark)
- One stroke `—` plus a small number — it shifted a little
- Two strokes `——` plus a middling number — more than a little
- Three strokes `———` plus a large number — more than a quarter hour
- Four strokes `————` — write the whole interval out again

Ten thousand rows of hours collapsed into half a page of dots.

Then he turned to the water-level column. That column was nine tenths of the book. He wrote consecutive levels in binary, lined them up row against row, and saw it at once: **the left end of two rows was identical, and the right end was usually identical too.** What actually differed was a short stretch in the middle.

The reason was plain enough. The sea does not sit at 1.2 metres one moment and 300 metres the next. It drifts slowly inside the same order of magnitude, so the leading bits stay put for a long time; and since the gauge is cut to six decimals, the trailing bits are often zeros. Compare two rows and the middle is only a few bits wide.

So he changed how he wrote the level: first, **from which bit on the left the two rows agree** and **up to which bit on the right they agree**, then copy out only the differing middle. A row of water level shrank from sixty-odd marks to a dozen. On a still night, when two readings were identical, he didn't even need the middle — just a single dot: `·`, same as last time.

In half a year he had saved nine tenths of the work.

The real wall came in September.

A typhoon crossed, and the sea rose six metres in four hours. The level jumped from 1.3 metres to 31 — the order of magnitude changed, the shared leading run collapsed from forty-odd bits to a handful, and the trailing run was a mess. He had taken a shortcut: write those two "where do they agree" numbers on the first page of the ledger and use them forever after. When the typhoon came, that scheme either could not hold the true level (which meant quietly omitting characters, exactly what the Minister forbade) or he had to leave the margin absurdly wide, costing a dozen extra marks on every ordinary point.

He sat under the lamp watching the wind bend the window and understood one thing: **"where the two ends agree" should not be a rule for the whole book. It should be each row's own decision.**

He rewrote the format. Every water-level row now opens with a small mark:

- `··` — two dots: this row reuses the previous row's pair of "agrees up to here" numbers and copies only the middle (nearly every ordinary row)
- `·|` — a dot and a bar: this row announces the pair itself, then copies the middle (only for typhoons, season changes, jumps in magnitude)

Not one character was spent unnecessarily. The typhoon pages really were thicker — but only those pages.

That winter A-Chao packed ten years of ledgers into a single cloth sack and handed it to the boat. Minister Situ unpacked it, read for three days, and reconstructed every single row, to the last digit. He asked A-Chao what sorcery he had used.

A-Chao said: I omitted nothing. I simply **stopped writing down the things that never changed.**

— By now you've probably recognised it: this is *Gorilla compression*. A-Chao writing only "how much the interval changed" is *delta-of-delta* timestamp encoding. Comparing two readings by XOR and copying only the differing middle is *XOR* floating-point encoding. And the two-dot mark that chooses between "reuse the last window" and "announce a new one" is the two control bits that carry Gorilla's value encoding.

### What it is

*Gorilla* is the in-memory time-series compression scheme Facebook published in 2015 ("Gorilla: A Fast, Scalable, In-Memory Time Series Database", VLDB). It attacks the storage problem of monitoring data: one metric sampled every 10 seconds is over 3 million points a year, and at "8-byte timestamp + 8-byte float64" per point that is 16 bytes and roughly 50 MB. Metric data is overwhelmingly **predictable** — fixed sampling intervals, slowly drifting values — and Gorilla eats exactly that predictability, landing at an average of **1.37 bytes per point**, about 12x, and **completely losslessly**.

Timestamps: store the start time, then store the interval (*delta*) for each point, then store the change in the interval (*delta-of-delta*). On regular scraping dod is always 0 and costs **one bit**. Otherwise it buckets by magnitude: `10` + 7 bits (±63), `110` + 9 bits, `1110` + 12 bits, `1111` + 32 bits. Timestamps become nearly free.

Values: *XOR* the current float64 with the previous one. IEEE 754's bit layout means nearby values share exponent bits and high mantissa bits, so the XOR result is a long run of leading zeros and a long run of trailing zeros. Hence:

- XOR is 0 (the value did not change) → a single `0` bit
- XOR is non-zero → `1`, then look at the *meaningful bits*: if both the leading-zero count and the trailing-zero count fall inside the window recorded for the previous value, emit `0` and copy just the middle (**reuse the window**); otherwise emit `1` + 5 bits of leading zeros + 6 bits of length + the middle (**announce a new window**)

Two details carry the design. First, **the window is decided per value**, not assumed globally — so a violent jump thickens only that stretch, never sacrificing correctness and never taxing the ordinary case. Second, **decoding needs only the previous value** — O(1) state, no rewinding to the start of a block — which is what makes it suitable for streaming writes and for decoding as you scan.

### Why it matters

Because almost every monitoring system you use is on this path. Prometheus's local TSDB uses *delta-of-delta* timestamps plus a *XOR* variant; InfluxDB's TSM, VictoriaMetrics, Gorilla itself, AWS Timestream — the same idea in different keys. With hundreds of millions of active series in a cluster, the compression ratio is not "saving some disk." It decides whether a single node can hold the data, whether a query scans excessive bytes, and whether the bill has one digit or two.

There is a deeper lesson in the choice. General-purpose compression (gzip, zstd) encodes in blocks and may reach a better ratio, but **decompression must start from the beginning**: to read point one-million you must first inflate everything before it, so random access is gone, and the CPU cost is real. Gorilla gives up on squeezing out every bit of entropy in exchange for three things a storage engine values more:

- **Losslessness** — after lossy compression, `p99` is wrong, and `p99` is the whole point
- **Streaming, O(1) state** — each point depends only on the previous one, so writes and decodes both proceed incrementally
- **A bounded worst case** — however chaotic the data, a point costs only a few extra bits; there is no cliff where a stretch of data refuses to compress

And it teaches a transferable move: **what you compress is not the data, it is the part of the data that did not change.** Fixed intervals, slow drift, stable prefixes — anything a pipeline repeats for a long time is worth identifying and writing exactly once.

_Metaphor mapping_

- **A-Chao's ledger** — one time series in a TSDB
- **"Not a single character may be omitted"** — the lossless constraint; monitoring values may not be approximated
- **Recording every ten minutes** — a fixed scrape interval
- **Write "ten minutes" once, then only the interval** — *delta* timestamp encoding
- **Write only how much the interval changed — almost always zero** — *delta-of-delta* encoding; dod = 0 costs one bit
- **The single dot `·`** — the one-bit marker for dod = 0
- **One/two/three/four strokes plus a number** — the magnitude buckets: `10`+7 bits / `110`+9 bits / `1110`+12 bits / `1111`+32 bits
- **Two rows agreeing on the left end and the right end** — under IEEE 754, nearby values share high bits, and fixed-precision decimals leave trailing zeros
- **Copying only the differing middle** — *XOR* against the previous value, keeping only the meaningful bits
- **A single dot when the level did not move at all** — XOR is 0, encoded as a lone `0` bit
- **Recording "agrees from bit N on the left, up to bit M on the right"** — leading-zero and trailing-zero counts forming the window
- **Writing that pair on page one and reusing it forever** — the wrong fix: a global fixed window either corrupts on a jump or bloats every ordinary point
- **"Where the ends agree should be each row's own decision"** — the window is judged per value; this is Gorilla's key insight
- **`··` reusing the previous row's window** — control bits `10`: reuse, then write only the middle
- **`·|` announcing the window again** — control bits `11`: 5 bits of leading zeros + 6 bits of length, then the middle
- **The typhoon pages are thicker, but only those pages** — worst case is local inflation; correctness and the ordinary case are untouched
- **Ten years of ledgers in one cloth sack** — about 12x compression, 1.37 bytes per point
- **The Minister reconstructing every row, digit for digit** — lossless decoding with O(1) state: only the previous value is needed
</section>
