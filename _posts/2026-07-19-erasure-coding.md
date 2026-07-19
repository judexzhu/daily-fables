---
layout: fable
title: "雪隘账房先生 · The Quartermaster of the Pale Pass"
title_zh: "雪隘账房先生"
title_en: "The Quartermaster of the Pale Pass"
concept: "erasure coding"
tags: [storage, distributed-systems]
illustration: /assets/art/2026-07-19-erasure-coding.jpg
---
<section class="zh" markdown="1">
北境的冬天来得早。谷仓满的那天,商队就得赶在封山前,把过冬的粮从山这头运到山那头的镇子。只有一条路——雪隘,一线窄道贴着崖壁盘上去。深冬里雪崩说来就来,一掌推下去,能把路上的车连人带马埋掉。埋哪几辆,谁也说不准;埋几辆,也没个数。

镇上祖辈传下的法子既笨又贵:每样粮都装两份,分放两辆车。指望的无非是——同一样东西的两辆车,别恰好一起被埋。可这样车队要拉双倍的车、喂双倍的马、在隘口多耗一倍的天数。而且雪崩偏偏不讲道理,真要把那两辆凑巧挨在一起的车一齐推下崖,这份粮就算彻底没了。冬天的粮本就是掐着指头算的,镇长年年为这笔冤枉开销发愁。

那年来了个新账房先生,瘦高个,总夹着一本谁也看不懂的册子。他说,粮不必装两份。

他把当年的粮,分门别类装进 *十辆* "正车",每辆装的都不一样,合起来正好是一整份,一点不多、一点不少。这十辆之外,他另备 *四辆* "副车"。副车里装的,不是哪一辆正车的翻版——是把十辆正车里的粮各舀出一些,按他册子上的一道配方兑在一起的混合粮。

关键在这儿:四辆副车,每辆的配方都不一样。头一辆是每样舀一瓢;第二辆是头一样舀一瓢、第二样舀两瓢、第三样舀三瓢……越往后越重;第三、第四辆又各是另一套只有他自己算得清的比例。伙计不解:"混在一块儿,回头怎么分得开?"账房先生只说:"配方各不相同,分得开。"

雪崩那天,风声是从崖顶滚下来的。等尘雪落定,商队清点,少了三辆——两辆正车、一辆副车,埋在哪个雪窝里都刨不出来了。伙计脸都白了。账房先生却翻开册子,把活着到镇的十一辆车里的粮,一一记下数,代进那几道配方,倒着一步步推——没被埋的正车里装的什么,本就明明白白;被埋那两辆正车里原该装什么、装多少,靠幸存副车里那几道"混着的线索"反解出来。不是估、不是猜,是分毫不差地算回原样。

伙计还在后怕:"要是那天埋的是三辆副车呢?或者三辆正车挨在一处呢?"账房先生摇头:"埋哪三辆都一样。只要活着到镇的车凑够十辆——正也好、副也好——我就能把整份粮补齐。少一辆都不行,多的每一辆都是白赚的余地。"

镇长年底一算账:比起从前每样装两份、车队足足翻倍的老法子,如今只多带四辆副车,车队长了不到半成,能扛的雪崩却一点没缩水。只有一样代价瞒不过他:从前一辆车埋了,货架上还搁着一模一样的另一辆,搬过来就完事;如今要补被埋的那辆,账房先生非得把十辆幸存车的粮都过一遍、把那几道配方从头解到尾才成——省下的是路上的车,添上的是到镇后的一番功夫。

——到这儿你大概已经认出来了:这就是 *erasure coding*(纠删码)。

**概念解析**:erasure coding 是分布式存储系统(Ceph、HDFS、S3 的纠删码存储类、RAID 6 等)用更低存储开销换取容错的编码方式。它把原始数据切成 *k* 个数据块,再用一套编码算法算出额外的 *m* 个*校验块*。校验块不是任何单个数据块的副本,而是**全部 k 个数据块的线性组合**——且每个校验块用的系数各不相同(通常取自 *Vandermonde* 矩阵这类结构)。系数互不相同这一点是要害:它保证了 n=k+m 个块里**任意 k 个**幸存块所对应的方程组都线性无关、可逆,因而丢失的块能通过解线性方程组被精确重建。这个"任意 k 个都够"的性质叫 *MDS*(Maximum Distance Separable),是 *Reed-Solomon* 码的核心优势——不管坏的是哪几块,只要坏的不超过 m 块就能全恢复。相比副本复制(replication)把整份数据复制 n 份、开销 n 倍,纠删码只需 (k+m)/k 倍存储,却能容忍任意 m 块同时丢失。代价是重建(rebuild)时要读齐 k 个幸存块并做一遍解码运算——即所谓读放大与计算开销,这也是为何热数据常用副本、冷数据才转纠删码。

_隐喻对应表_

- 一趟过隘运粮 → 一次写入/编码(encode)
- 十辆正车,合起来正好一整份 → k 个数据块(data shards)
- 四辆副车,装各正车兑成的混合粮 → m 个校验块(parity shards),是 k 个数据块的线性组合
- 每辆副车配方都不一样 → 校验块系数互不相同(Vandermonde 结构),保证任意 k 个方程线性无关
- 雪崩埋掉哪几辆、埋几辆都说不准 → 任意位置、至多 m 个块的故障
- 只要活着到镇的车凑够十辆就能补齐 → 只要幸存块数 ≥ k 即可完整重建(MDS 性质)
- 埋三辆副车、三辆正车都一样能解 → 不论坏的是哪几块,≤ m 块皆可恢复
- 倒着解配方推回被埋的粮 → 解线性方程组进行解码/重建
- 从前装两份 vs 如今多带四辆副车 → 副本复制 n 倍开销 vs 纠删码 (k+m)/k 倍开销
- 补一辆车要过十辆车、重解配方 → 重建的读放大与计算开销(rebuild cost)
</section>
<section class="en" markdown="1">
Winter comes early in the north. The day the granary fills, the caravan must move the town's whole winter store from this side of the mountain to the town on the far side, before the snows seal the road for good. There is only one road — the Pale Pass, a single thread of track hugging the cliff as it climbs. In deep winter an avalanche needs no warning; one push of the mountain's hand can bury a wagon, horses and all. Which wagons it takes, no one can say. How many, no one can count.

The town's inherited method was clumsy and dear: load every kind of grain twice, on two separate wagons, and pray the two wagons carrying the same thing were never buried together. But that meant hauling double the wagons, feeding double the horses, spending double the days on the pass — and the avalanche kept its own counsel. Let it happen to push both of a matched pair off the cliff at once, and that grain was simply gone. Winter rations were counted on fingers; every year the mayor fretted over the waste.

That year a new quartermaster arrived — lean, tall, forever tucking a ledger no one else could read under his arm. He said the grain need not be loaded twice.

He sorted the year's store into *ten* "plain wagons," each carrying something different, together making exactly one whole store — no more, no less. Beside those ten he prepared *four* "reserve wagons." What went into a reserve was no copy of any single plain wagon. It was a blend: a scoop drawn from each of the ten plain wagons, mixed according to a recipe in his ledger.

Here was the crux: each of the four reserves used a *different* recipe. The first took one ladle of everything. The second took one ladle of the first grain, two of the second, three of the third — heavier as it went. The third and fourth followed still other proportions only he could keep straight. A hand asked, puzzled, "Blended together like that — how will you ever tell them apart again?" The quartermaster only said, "The recipes all differ. They come apart."

The day the avalanche fell, its roar rolled down from the ridge. When the snow-dust settled and the caravan counted, three wagons were short — two plain, one reserve, buried in some drift no digging would find. The hands went white. But the quartermaster opened his ledger, wrote down the grain in the eleven wagons that reached town, fed the numbers into his recipes, and solved backward, step by step. What the surviving plain wagons held was plain enough already; what the two buried plain wagons should have held, and how much, he drew back out of the "blended clues" carried in the surviving reserves. Not an estimate, not a guess — an exact reconstruction of the original.

The hand was still shaken. "And if it had buried three reserves that day? Or three plain wagons side by side?" The quartermaster shook his head. "Any three, all the same. As long as ten wagons reach town — plain or reserve, it makes no difference — I can rebuild the whole store. One fewer and I cannot. Every wagon past ten is margin, freely won."

At year's end the mayor did the sum: against the old way of loading everything twice and doubling the whole train, he now carried just four extra reserves — the caravan grew by less than half a tenth — and lost not an ounce of the avalanche it could withstand. Only one cost he couldn't hide from himself. In the old days, when a wagon was buried, an identical one still sat on the shelf; you fetched it and were done. Now, to replace a buried wagon, the quartermaster had to pass over all ten survivors and solve his recipes from end to end — what he saved on the road, he paid back in labor once the caravan reached town.

— By now you've probably recognized it: this is *erasure coding*.

**What it is**: Erasure coding is how distributed storage systems — Ceph, HDFS, S3's erasure-coded storage classes, RAID 6 — trade lower storage overhead for fault tolerance. Original data is split into *k* data blocks, and an encoding scheme computes *m* additional *parity blocks*. A parity block is no copy of any single data block; it is a **linear combination of all k data blocks** — and each parity block uses *different* coefficients (typically drawn from a structure like a *Vandermonde* matrix). That the coefficients all differ is the whole point: it guarantees that the equations corresponding to *any k* of the n=k+m surviving blocks are linearly independent and invertible, so lost blocks can be reconstructed exactly by solving a linear system. This "any k will do" property is called *MDS* (Maximum Distance Separable), the core strength of *Reed-Solomon* codes — no matter which blocks are lost, as long as no more than m are gone, everything recovers. Compared to replication, which stores n full copies at n× overhead, erasure coding needs only (k+m)/k× the storage while tolerating any m simultaneous losses. The price: rebuild must read k surviving blocks and run a decode pass — read amplification and compute cost — which is why hot data often stays replicated and only cold data is converted to erasure coding.

_Metaphor mapping_

- One run over the pass → one write/encode operation
- The ten plain wagons, together exactly one whole store → the k data shards
- The four reserve wagons of blended grain → the m parity shards, linear combinations of the k data shards
- Each reserve using a different recipe → parity coefficients all differ (Vandermonde structure), making any k equations linearly independent
- The avalanche burying an unpredictable which and how-many → failures at any position, up to m blocks
- Any ten survivors sufficing to rebuild → reconstruction needs only surviving blocks ≥ k (the MDS property)
- Three reserves or three plain wagons, all recoverable alike → any ≤ m lost blocks recover, regardless of which
- Solving the recipes backward for the buried grain → decoding/rebuilding by solving a linear system
- Loading twice (old way) vs. four extra reserves → replication's n× overhead vs. erasure coding's (k+m)/k×
- Passing over all ten survivors to replace one wagon → rebuild's read amplification and compute cost
</section>
