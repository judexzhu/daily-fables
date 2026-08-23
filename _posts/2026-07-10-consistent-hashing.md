---
layout: fable
title: "环湖驿站的送信人 · The Lakeside Mail Carriers"
title_zh: "环湖驿站的送信人"
title_en: "The Lakeside Mail Carriers"
concept: "consistent hashing"
tags: [distributed-systems]
illustration: /assets/art/2026-07-10-consistent-hashing.jpg
---
<section class="zh" markdown="1">
云溪湖是个圆形的湖，沿湖住着几千户人家，家家户户的门牌号乱七八糟地散落在湖岸各处，谁也说不准下一家挨着哪家。镇上负责送信的驿站想了个法子：先把整圈湖岸看成一条首尾相连的环形路——门牌号（说白了就是姓名+地址）经过一套固定的换算规则，会落在这条环路上的某个具体位置，谁的门牌换算出来数字越大，就落在环路上越靠"后"的位置，最大的数字绕回来又和最小的数字挨在一起，成了一个圈。

驿站原来招了四个送信人：老周、老陈、阿吴、小林。每个人在环路上选一个点插一面旗子，作为自己的"驻点"。规矩很简单：沿着环路顺时针走，从自己的旗子开始，一直走到遇到下一面旗子为止，中间这一整段路上的人家，全归自己管。老周的旗插在环路"起点"附近，他就负责从自己的旗子一路顺时针数过去、直到碰上老陈的旗子为止的所有人家；老陈接着管到阿吴的旗子；依此类推，最后小林的旗子那一段又绕回去接上老周。四面旗子把整个环分成四段，谁都不重叠、谁都不落下。

镇上生意越做越大，驿站决定加招一个新人小马。小马在环上选了个空当插旗——恰好插在老陈那一段的中间。这一插，只有从小马的旗子往回数到老陈的旗子之间那一小段路上的人家，需要改归小马管；老周、阿吴、小林原来负责的那一段，一户都不用动。整个镇子几千户人家，真正换了"责任人"的，只有小马旗子附近那一小撮。

后来老周年纪大了要退休，把旗子一撤，规矩也很简单：紧跟在他旗子后面（顺时针方向）原本属于老周的那段路，自动并给下一面旗子——也就是老陈接手，别人一概不受影响。

驿站还发现一个麻烦：旗子插的位置全凭手气，赶上运气不好，某人的旗子附近正好扎堆住着几百户人家，忙得脚不沾地，另一个人的旗子附近却稀稀拉拉没几户，闲得慌。后来驿站想了个补救办法：每个送信人不再只插一面旗，而是在环路上多个不同位置各插几面小旗——都算自己的驻点，只是分散开来。这样一来，就算某一片区域恰好人多，也大概率会落在某个人的某一面小旗附近，忙闲能被摊得更匀，不至于因为旗子插的位置运气不好就让一个人累死、另一个人闲死。

驿站的老站长还记得更早的办法：直接拿每户门牌号除以送信人数，看余数是几就归第几个人管。这法子简单，可一旦驿站增员或裁员——哪怕只加一个人——分母一变，几乎家家户户算出来的余数都会变，等于全镇的信件几乎要重新分一遍，新老送信人交接起来乱作一团。相比之下，插旗子分段的这套办法，每次增减人手，受影响的都只是紧挨着那面旗子的一小段路。

——到这儿你大概已经认出来了：这个环形驿站的分段法，讲的其实是 _consistent hashing_（一致性哈希），插多面小旗子分散负载的做法，讲的是它的优化手段 _virtual nodes_（虚拟节点）。

_概念解释_
一致性哈希是分布式系统里做数据分片、负载均衡时常用的一种映射方式：把节点（服务器、缓存实例、送信人）和数据键（key）都通过同一套哈希函数映射到同一个环形的数值空间上，每个键归属于沿环顺时针方向遇到的第一个节点。它最大的好处，正是这则寓言想强调的：增加或删除一个节点时，只有该节点两侧相邻的那一小段区间需要重新分配，绝大多数键的归属完全不受影响——这和传统的"key mod N"取余分片形成鲜明对比，后者只要节点数 N 一变，几乎全部的键都要重新映射，代价极高。DynamoDB、Cassandra、memcached 的分布式缓存方案、CDN 的边缘节点路由，乃至 Kubernetes 里某些负载均衡/会话保持策略，都用到这套思路。而"某个节点碰巧分到过大或过小一段区间"这种数据倾斜问题，正是靠 _virtual nodes_（给每个物理节点在环上分配多个虚拟位置）来打散、拉平的。

_隐喻对应表_

- 云溪湖沿岸的环形路 → 一致性哈希的哈希环（hash ring，通常是 0 到 2^32-1 的数值空间）
- 每户门牌号换算出的位置 → 一个数据键（key）在环上的哈希值
- 送信人插的旗子/驻点 → 节点（node）在环上的位置
- "顺时针走到下一面旗子之前，都归自己管" → 每个节点负责环上到下一个节点之间的区间
- 新人小马插旗，只影响紧邻的一小段 → 增加节点时，只有相邻区间的 key 需要重新分配
- 老周退休、旗子撤走，紧邻区间自动并给下一个人 → 删除节点时，其区间合并给顺时针方向的下一个节点
- 一人插多面小旗，分散在环上不同位置 → virtual nodes，用多个虚拟位置打散负载，避免数据倾斜
- 老办法"门牌号除以人数取余数" → 传统的 key mod N 取余分片
- 送信人数一变，几乎家家户户都要重新分配 → mod N 分片在节点数变化时几乎全量重新映射的高代价
</section>
<section class="en" markdown="1">
Lake Yunxi was perfectly round, and thousands of households lived scattered along its shore, their house numbers strewn about in no particular order — nobody could tell which house sat next to which. The town's post office came up with a scheme: treat the entire shoreline as one continuous ring road. Every house number (really just name plus address) got run through a fixed hashing formula that placed it at some specific point on that ring — the larger the resulting number, the further "along" the ring it landed, and the largest number wrapped right back around to sit next to the smallest, closing the loop.

The post office had originally hired four carriers: Old Zhou, Old Chen, Ah Wu, and Xiao Lin. Each picked a spot on the ring and planted a flag there as his "post." The rule was simple: walking clockwise from your own flag until you hit the next flag, every household along that stretch was yours to deliver to. Old Zhou's flag sat near the ring's "starting point," so he covered everything clockwise from his flag until Old Chen's flag; Old Chen covered from there to Ah Wu's flag; and so on, until Xiao Lin's stretch wrapped back around to meet Old Zhou. Four flags split the whole ring into four segments — no overlaps, no gaps.

As the town grew, the post office decided to hire a fifth carrier, Xiao Ma. Xiao Ma picked an open spot on the ring and planted his flag — right in the middle of what had been Old Chen's stretch. That single move meant only the small stretch of households between Xiao Ma's flag and Old Chen's flag (counting backward) needed to be reassigned to Xiao Ma; everything Old Zhou, Ah Wu, and Xiao Lin had been covering stayed exactly as it was. Out of the town's several thousand households, only the small cluster near Xiao Ma's flag actually changed hands.

Later, when Old Zhou grew old and retired, pulling up his flag was just as simple: the stretch that had been his (immediately clockwise of his old spot) automatically merged into the next flag along — Old Chen picked it up, and nobody else was affected at all.

The post office also ran into trouble: where a flag landed was pure luck. Sometimes a carrier's flag happened to land near a dense cluster of hundreds of households and he'd be run off his feet, while another carrier's flag landed in a sparse stretch with barely any households, leaving him idle. The fix they eventually landed on: instead of planting just one flag, each carrier planted several small flags at different points scattered around the ring — all counted as his own posts, just spread out. That way, even if some region happened to be crowded, it would likely fall near _someone's_ small flag, and the workload got smoothed out much more evenly — no longer left to the bad luck of where a single flag happened to land.

The old postmaster still remembered an even older method: just take each house number, divide it by the number of carriers, and whoever matched the remainder got the delivery. Simple enough — but the moment the post office hired or let go even a single carrier, the divisor changed, and almost every household's remainder changed with it. That meant re-sorting almost the entire town's mail from scratch, and the handoff between old and new carriers turned into chaos. By comparison, the flag-and-segment scheme meant that adding or removing a carrier only ever touched the small stretch immediately next to that one flag.

——By now you've probably recognized it: this ring-shaped post office's segmenting scheme is really describing _consistent hashing_, and the trick of planting several small flags to spread out the load is its optimization, _virtual nodes_.

_The concept, and why it matters_
Consistent hashing is a mapping scheme widely used in distributed systems for data sharding and load balancing: both nodes (servers, cache instances, mail carriers) and data keys are run through the same hash function onto a shared ring-shaped numeric space, and each key belongs to the first node encountered going clockwise around the ring. Its biggest advantage — and the point of this fable — is that adding or removing a node only requires reassigning the small interval immediately adjacent to it; the vast majority of keys are completely unaffected. This stands in sharp contrast to traditional "key mod N" sharding, where changing the node count N at all forces nearly every key to be remapped — a very expensive operation. DynamoDB, Cassandra, distributed caching schemes like memcached, CDN edge-node routing, and even some Kubernetes load-balancing/session-affinity strategies all rely on this idea. As for the problem of a node landing an unfairly large or small interval purely by chance (data skew), that's exactly what _virtual nodes_ — giving each physical node multiple virtual positions on the ring — are designed to smooth out.

_Metaphor mapping_

- The ring road along Lake Yunxi's shore → the hash ring in consistent hashing (typically a numeric space from 0 to 2^32-1)
- The position each house number hashes to → a data key's position on the ring
- A carrier's planted flag/post → a node's position on the ring
- "Everything clockwise until the next flag is mine" → each node owns the interval up to the next node on the ring
- Xiao Ma's new flag only affecting the adjacent stretch → adding a node only requires reassigning the adjacent interval's keys
- Old Zhou's flag pulled at retirement, his stretch auto-merging into the next → removing a node merges its interval into the next node clockwise
- One carrier planting several small flags at scattered points → virtual nodes, spreading load across multiple virtual positions to avoid skew
- The old "house number mod number of carriers" method → traditional key-mod-N sharding
- Nearly every household needing reassignment when the carrier count changes → the high cost of full remapping when N changes under mod-N sharding
</section>
