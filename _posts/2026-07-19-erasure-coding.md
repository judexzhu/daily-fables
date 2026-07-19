---
layout: fable
title: "关隘上的备用车队 · The Reserve Wagons of the Pale Pass"
title_zh: "关隘上的备用车队"
title_en: "The Reserve Wagons of the Pale Pass"
concept: "erasure coding"
tags: [storage, distributed-systems]
illustration: /assets/art/2026-07-19-erasure-coding.jpg
---
<section class="zh" markdown="1">
在北境,入冬前商队要把过冬的粮食从谷仓运过"雪隘"送到山那头的小镇。雪隘只有一条窄路,深冬常有雪崩,会把路上的车队埋掉一两辆——谁也说不准是哪一辆,也说不准会埋几辆。

镇上原本的规矩简单也很贵:每样货都装两份,分放两辆车,只求雪崩别把两辆一起埋了。可这样车队要拉平时两倍的车,马也要两倍,过隘的时间也拖长一倍——冬天粮食本就紧张,谁都舍不得这笔多出来的开销。

后来商队换了个账房先生,他有一套不一样的算法。他把粮食分装进 *k* 辆"本车",每辆装的东西各不相同、互不重复。除此之外,他另备 *m* 辆"备车"——备车里装的不是某一辆本车的原样货物,而是他按一套固定比例、把 k 辆本车里的东西各取一点、按精确配方"兑"在一起的混合粮。这配方写在他随身带的一本密账里,谁都看不懂,只有他自己会用。

雪崩来的时候,谁也不知道会埋哪几辆车,也可能一辆都不埋,也可能连着埋了两三辆——本车、备车,埋哪种都说不准。商队到达镇上清点车辆,只要没被埋的车数(本车加备车)凑够了 k 辆,账房先生就翻开密账,把幸存车里的数字一一代入配方公式,倒着一步步解出被埋的本车里原本装的是什么、装了多少——不是估计,是分毫不差地算回来。哪怕丢的正好是两辆备车,或者两辆本车,或者一本一备,只要幸存车数不少于 k,公式照样能解开。

镇长后来算了一笔账:比起过去每样货都拉两份、车队多一倍的老办法,现在只需多带 m 辆备车,车队总长只涨了 m/k,能扛住的雪崩规模却一点没打折扣。

——到这儿你大概已经认出来了:这就是 *erasure coding*(纠删码)。

**概念解析**:erasure coding 是分布式存储系统(如 Ceph、HDFS、S3 的纠删码存储类、RAID 6)用来在更低存储开销下换取容错的编码方式。它把原始数据切成 *k* 个数据块,再用一套编码算法(如 *Reed-Solomon*)计算出额外的 *m* 个*校验块*——校验块不是任何一个数据块的复制品,而是所有数据块按特定系数线性组合的结果。只要 k+m 个块里存活的块数不少于 k,不论具体丢失哪几个块,都能通过解线性方程组把丢失的块精确重建。相比副本复制(replication)把每份数据整份复制 n 次、存储开销是 n 倍,纠删码只需 (k+m)/k 倍的存储,却能容忍最多 m 个块同时丢失——这正是大规模对象存储在保证持久性的同时压低成本的关键手段,代价是重建(rebuild)时需要额外的计算和读放大。

_隐喻对应表_

- 雪隘商队一次运货 → 分布式存储系统里的一次写入/编码过程
- k 辆本车 → k 个原始数据块(data shards)
- m 辆备车(混合粮) → m 个校验块(parity shards)
- 账房先生的密账/配方 → 编码矩阵,如 Reed-Solomon 生成矩阵
- 雪崩埋掉任意车 → 磁盘/节点故障,可发生在任意位置
- 清点车辆只要凑够 k 辆 → 只要存活块数 ≥ k 即可重建
- 解密账公式倒推粮食 → 解码/重建算法,通过线性代数精确恢复丢失数据
- 两倍车队(全复制)vs 多带 m 辆备车 → 副本复制存储开销 n 倍 vs 纠删码开销 (k+m)/k 倍
</section>
<section class="en" markdown="1">
Every year before winter set in, the caravan had to haul grain from the granary, over the Pale Pass, to the town on the far side of the mountain. The pass was a single narrow road, and deep winter brought avalanches that could bury a wagon or two along the way — no one could say in advance which ones, or how many.

The town's old rule was simple and expensive: load every kind of goods twice, split across two wagons, and hope an avalanche never buried both. But that meant hauling twice the usual number of wagons, twice the horses, and twice the time to cross the pass — and in a winter already short on grain, nobody wanted to pay for it.

Then the caravan hired a new quartermaster, who worked a different kind of arithmetic. He split the grain across *k* "plain wagons," each carrying a distinct, non-overlapping share of the load. Alongside them, he loaded *m* "reserve wagons" — and what went into a reserve wagon wasn't a copy of any single plain wagon's cargo. It was a blend: a little of every plain wagon's grain, mixed in exact proportions according to a recipe he kept in a private ledger that no one else could read, or use.

When an avalanche struck, no one could predict which wagons it would bury — plain or reserve, one or several, or perhaps none at all. When the caravan reached town and the surviving wagons were counted, as long as the number that survived — plain and reserve together — added up to at least *k*, the quartermaster opened his ledger, plugged the numbers from the surviving wagons into his recipe, and solved backward, step by step, for exactly what had been in each buried wagon and how much. Not an estimate — an exact reconstruction. It didn't matter if the buried wagons were two reserves, two plains, or one of each; as long as at least k wagons survived, the recipe always solved.

The town's mayor eventually did the math: compared to the old rule of doubling every load, the caravan now only needed m extra reserve wagons — the total length of the train grew by a factor of m/k — while it could withstand exactly the same scale of avalanche as before.

— By now you've probably recognized it: this is *erasure coding*.

**What it is**: Erasure coding is how distributed storage systems — Ceph, HDFS, S3's erasure-coded storage classes, RAID 6 — trade lower storage overhead for fault tolerance. Original data is split into *k* data blocks, and an encoding scheme (such as *Reed-Solomon*) computes an additional *m* *parity blocks* — not copies of any single data block, but linear combinations of all of them under a fixed set of coefficients. As long as at least k of the k+m total blocks survive, regardless of which specific blocks are lost, the missing blocks can be reconstructed exactly by solving a system of linear equations. Compared to replication, which stores n full copies of the data at n× overhead, erasure coding needs only (k+m)/k× the storage while still tolerating up to m simultaneous block losses — the key technique large-scale object storage uses to hold durability steady while driving cost down, at the price of extra compute and read amplification during rebuild.

_Metaphor mapping_

- One caravan run over the pass → one write/encode operation in a distributed storage system
- The k plain wagons → the k original data shards
- The m reserve wagons (blended grain) → the m parity shards
- The quartermaster's private ledger/recipe → the encoding matrix, e.g. a Reed-Solomon generator matrix
- An avalanche burying any wagons → disk/node failure, which can strike any block
- Counting survivors and reaching at least k → reconstruction requires only that surviving blocks ≥ k
- Solving the ledger's recipe backward for the buried grain → the decode/rebuild algorithm, exactly recovering lost data via linear algebra
- Doubling the caravan (full replication) vs. adding m reserve wagons → replication's n× storage overhead vs. erasure coding's (k+m)/k× overhead
</section>
