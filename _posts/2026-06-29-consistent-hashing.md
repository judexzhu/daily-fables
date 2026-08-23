---
layout: fable
title: "环形街上的邮差 · The Postmen on the Ring Street"
title_zh: "环形街上的邮差"
title_en: "The Postmen on the Ring Street"
concept: "consistent hashing"
tags: [distributed-systems]
illustration: /assets/art/2026-06-29-consistent-hashing.jpg
---
<section class="zh" markdown="1">
在云脚镇，所有人家都沿着一条环形的石板路住着。这条路绕成一个完整的圆圈，没有起点，也没有终点——从镇口出发一直走，最后总会回到镇口。

镇上有几位邮差，各自守在路边固定的几盏灯笼下。规矩很简单：每当一封信送到邮局，老局长就转动墙上那只刻着无数小格的铜轮盘。轮盘"咔"地停下，指针落在某个刻度上，这个刻度对应着环形路上的某一个点。信不会凭空飞过去，它顺着路_往一个方向走_（永远是顺时针），遇到的第一位邮差，就归他投递。

这套规矩有个好处，镇上人一开始没看出来。

有一年，住在东头的老邮差告老还乡了。换作别的镇子，这种事得把全镇信箱重新分一遍——谁送哪条街，全乱套。可在云脚镇什么都没乱：只有"原本会走到老邮差灯笼下"的那一小段信，现在改由顺时针方向的_下一位_邮差接手。其余每一位邮差手里的活，一封都没变。

后来镇上人多了，新来一位年轻邮差。他也只是往环上某个空位一站，从此_只从他逆时针那位邻居手里_接过一小段信。又是只有一小段易主，全镇其余纹丝不动。

但有个麻烦：灯笼的位置是早年随手定的，有的邮差脚下管着一长段路，累得够味；有的只管巴掌大一块，闲得发慌。老局长想了个法子——不让一位邮差只站一盏灯笼，而是给每人发了_十几个分身_，散布在环的各个角落。这样一来，无论谁退、谁来，受影响的总被均匀摊薄成许多小段，没人会突然被压垮。

——到这儿你大概已经认出来了：这条没有起点终点的环路，就是一致性哈希（_consistent hashing_）里的那个 _ring_；铜轮盘是 _hash function_，邮差是 _node_，"顺时针遇到的第一个邮差"就是 key 的归属规则；老局长发的那些分身，正是 _virtual nodes_。

_这是什么_
一致性哈希是分布式系统里给"哪份数据归哪台机器"做映射的一种办法。把所有 _node_ 和所有 key 都 _hash_ 到同一个环上，key 顺时针找到的第一个 node 就是它的归属。它最关键的性质是：增删一个 node 时，_只有相邻的一小段 key 需要搬家_，平均只动 1/N，而不是像普通取模 hash（`key % N`）那样几乎全部重排。Amazon Dynamo、Cassandra、很多分布式 cache 都靠它把扩容、宕机时的数据迁移代价压到最小。_virtual nodes_ 则解决了环上分布不均、负载倾斜的问题。

_隐喻对应表_

- 环形石板路 → 哈希 _ring_（环形 key 空间）
- 铜轮盘、指针落点 → _hash function_，把 key/node 映射到环上的位置
- 每位邮差 → 一个 _node_（服务器 / 缓存节点）
- 信顺时针走到第一位邮差 → key 的归属规则（clockwise successor）
- 老邮差告老 → node 下线，只有它那段 key 迁给下一位
- 新邮差加入 → node 上线，只从一个邻居接管一小段
- 给每人发十几个分身 → _virtual nodes_，让负载均匀
- "全镇其余纹丝不动" → 最小化数据迁移（minimal rebalancing）
</section>
<section class="en" markdown="1">
In the town of Cloudfoot, every household sits along a single ring-shaped cobblestone road. The road bends into one complete circle — no beginning, no end. Set out from the town gate, keep walking, and you always arrive back at the town gate.

The town has a handful of postmen, each stationed beneath a few fixed lanterns along the road. The rule is simple: whenever a letter reaches the post office, the old postmaster spins a brass wheel on the wall etched with countless tiny notches. The wheel clicks to a stop, the needle lands on some notch, and that notch maps to one point on the ring road. The letter doesn't fly straight there — it travels along the road _in one direction_ (always clockwise), and the first postman it meets is the one who delivers it.

This rule has a hidden virtue the townsfolk didn't notice at first.

One year, the old postman on the east side retired. In any other town this means re-dividing every mailbox in the whole town — who covers which street descends into chaos. But in Cloudfoot, nothing fell apart: only the small stretch of letters that _used to walk up to the retired postman's lantern_ now passes to the _next_ postman clockwise. Every other postman's workload changed by not a single letter.

Later the town grew and a young postman arrived. He merely stood at an empty spot on the ring, and from then on took over _only a small stretch from his counter-clockwise neighbor_. Again, just one slice changed hands; the rest of the town didn't stir.

But there was a snag: the lanterns had been placed casually long ago, so some postmen guarded a long stretch and were worked to exhaustion, while others covered a sliver and stood idle. The postmaster found a fix — instead of letting one postman stand at a single lantern, he gave each man _a dozen stand-ins_ scattered around the ring. Now, whoever retires or arrives, the disruption is always thinned out evenly into many small stretches, and no one is suddenly crushed.

— By now you've probably recognized it: this beginning-less, end-less loop is the _ring_ of _consistent hashing_; the brass wheel is the _hash function_, each postman is a _node_, and "the first postman met going clockwise" is the key-ownership rule; the postmaster's stand-ins are _virtual nodes_.

_What it is_
Consistent hashing is a way for distributed systems to map "which piece of data belongs to which machine." You _hash_ every _node_ and every key onto the same ring; a key belongs to the first node it reaches going clockwise. Its crucial property: when you add or remove a node, _only a small adjacent stretch of keys has to move_ — on average just 1/N of them — instead of the near-total reshuffle you'd get from plain modulo hashing (`key % N`). Amazon Dynamo, Cassandra, and many distributed caches rely on it to keep data-migration cost minimal during scale-ups and outages. _Virtual nodes_ solve the uneven-distribution / load-skew problem on the ring.

_Metaphor mapping_

- The ring-shaped road → the hash _ring_ (circular key space)
- The brass wheel & where the needle lands → the _hash function_, mapping keys/nodes to positions on the ring
- Each postman → a _node_ (server / cache instance)
- A letter walking clockwise to the first postman → the key-ownership rule (clockwise successor)
- The old postman retiring → a node leaving; only its stretch of keys migrates to the next one
- The new postman arriving → a node joining; it takes over one small slice from a single neighbor
- Giving each man a dozen stand-ins → _virtual nodes_ for even load
- "The rest of the town didn't stir" → minimal data migration (minimal rebalancing)
</section>
