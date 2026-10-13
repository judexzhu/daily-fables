---
layout: fable
title: "素数环泊与轮序签筹 · The Prime Ring Wharf and the Permuted Tallies"
title_zh: "素数环泊与轮序签筹"
title_en: "The Prime Ring Wharf and the Permuted Tallies"
concept: "Maglev Consistent Hashing"
tags: [networking, distributed-systems]
illustration: /assets/art/2026-10-13-maglev-consistent-hashing.jpg
youtube_id: "funWvAAhcHY"
---
<section class="zh" markdown="1">
在大江九水汇流的三角洲上，有一座开凿于千年前的巨型圆形石泊——“素水大泊”。

泊岸是一整圈巨大的青石围堤，沿堤整整齐齐凿有六万五千五百三十七个泊位，每个泊位上都刻着清晰的编号，从零号直到六万五千五百三十六号。之所以定为六万五千五百三十七，是因为老水吏们懂得天道精微：这是一个极大的素数（质数），除了一与它自身，任何整数都无法将其整除。

每天，数以万计的货船乘风破浪驶入水闸。每艘货船的船尾上，都盖着一道朱红官印，上头烙着五样印记：发货客商、出发水驿、收货掌柜、目的栈房，以及一道货签密纹。

而在大泊深处，驻扎着数支专门承接转运的漕运大船帮——青龙帮、玄武帮、白虎帮、朱雀帮。所有进闸的货船，都必须按官印精确分派给各大船帮卸货转运。

早些年间，大泊的守闸官用的是最简陋的法子：数一数今天有几个船帮当值，拿货船官印上的密数除以船帮总数，余几便分给哪一帮。然而有一年盛夏暴雨，白虎帮的几艘龙骨大船在礁石上受损，不得不临时封舱退修。船帮总数少了一个，所有货船重新取余，原本昨天还在青龙帮卸货卸到一半的数千艘连环货船，眨眼间全被派去了玄武帮与朱雀帮！账册错乱，缆绳打结，整座大泊瘫痪了整整半个月。

后来，水吏们改用了江湖上盛传的“青丝圆环法”——把一根丝带首尾相接绕成圆环，船帮在环上随机下桩，进来的货船也落在一个点上，顺时针滑向最近的木桩。可这法子依然祸事不断：几位船帮帮主丢骰子落桩，落点极不均匀，朱雀帮分到的水面又宽又阔，船工累到吐血；而青龙帮分到的水域只有弹丸之地，船夫整日坐在石阶上嗑瓜子。更致命的是，每当早潮货船如飞箭般涌入水闸，守闸官必须捧着厚厚的丝带图卷，顺着环形图谱一点一点往前寻查找桩，水门外千帆拥堵，进退两难。

直到工部巡水使祁大人来到大泊，立下了一面铜锣与三条铁律。

祁大人站在水闸望台上对众人道：“江潮不等人，进门看一眼就得放行；各帮分货必须均如斗量；某帮撤修，未受牵连的万千货船绝不能挪移一寸。”

这套法子，被水夫们称为**“定数轮筹”**。

第一条铁律：**各执私律，素数回环**。
大泊不再用丝带画环，而是以那座凿有六万五千五百三十七个青石泊位的长堤为根本基石。
祁大人按每个船帮的立帮印信，推算出两个固定之数：一个叫**起锚位（Offset）**，一个叫**行步跨度（Skip）**。
因为泊位总数是素数，而行步跨度永不为零，所以任何一个船帮只要从起锚位出发，按照自己固定的跨度向前跳步，就必定能且只能把六万五千五百三十七个泊位不重不漏地踏遍一整圈！这一圈跳完，便形成了该船帮心中最钟爱的泊位“偏好序列”。

第二条铁律：**铜锣轮序，逐次填筹**。
每当开市定规矩的那天，大泊铜锣敲响。各大船帮齐聚中军大帐，谁也不争抢。
第一轮铜锣响，青龙帮报出自己偏好序列里的第一位，占下一个青石泊；玄武帮接着报出自己的第一位，也占下一泊；白虎帮、朱雀帮各占一泊。
第二轮铜锣响，各帮报出自己的第二顺位。倘若算出来的那个泊位早被别的船帮占了，那便毫不犹豫按自己的跨度再迈一步，直到找到一个尚无人占领的空位填上自家旗号。
铜锣一轮一轮敲响，各大船帮像转轮般轮流出筹。当六万五千五百三十七个泊位全被占满时，奇迹出现了：各大船帮所分得的泊位总数几乎分毫不差，宛如天平秤过一般均匀！

第三条铁律：**一眼定泊，雷厉分流**。
当清晨满载生鲜粮草的货船疾驰入闸，守闸官连案卷都不必翻。他只需将船尾官印上的密纹除以六万五千五百三十七，取下余数，直接抬头望向对应编号的石泊——石泊上插着谁家的令旗，就喊谁家的大名！
这是一步到位的查验，不排队、不检索、不比对，水闸开阖如雷，万船如离弦之箭各自入港。

最妙的是暴风雨来临的时刻。
某日白虎帮突遭雷击失火，只得撤出。水吏们不慌不忙，只消将六万五千五百三十七个泊位里插着白虎旗的空位拔掉，其余青龙、玄武、朱雀三帮依着原先的跳步偏好，再轮流把这些空位补齐即可。
原先属于青龙帮的泊位，依然是青龙帮；属于玄武帮的，依然归玄武。那些正在装卸中的万千商船，没有一艘需要重新换港！

从此，素水大泊千帆竞渡，怒涛拍岸，纵使天下商船齐聚，也未曾再起一丝拥堵。

---

读到这里，聪明的你或许已经认出：这正是现代高并发网络分流架构中威名远扬的**麦格列夫一致性哈希算法（Maglev Consistent Hashing）**。

### 这是什么

**Maglev 哈希**是 Google 在 2016 年 NSDI 顶会论文《*Maglev: A Fast and Reliable Software Network Load Balancer*》中披露的核心一致性哈希与查表算法，广泛应用于 Google 边缘网络负载均衡、Envoy 代理以及 Cilium 等现代云原生网络基础设施中。

传统的分布式负载均衡一致性哈希算法（如基于哈希环的 Ketama 算法）存在两大痛点：
1. **分布不均匀与高内存开销**：在圆环上，为了让后端节点（Backend）负载相对均衡，必须为每个节点生成成百上千个“虚拟节点（Virtual Nodes）”，不仅消耗大量内存，分布方差依然显著。
2. **查找开销高（$O(\log N)$）**：数据包到达时，网关必须在有序的哈希环上执行二分查找（Binary Search）以定位最近的节点，难以在高吞吐网卡线速（Line Rate）下实现极低时延处理。

Maglev 提出了完全不同的构想：
- **素数查找表（Lookup Table of Prime Size $M$）**：构建一个大小为素数 $M$（如 $M = 65537$）的固定数组，远大于后端节点数 $N$。
- **确定性伪随机置换（Permutation Generation）**：对每个后端节点 $i$，通过两组独立的哈希函数计算出初始偏移量 $offset_i = hash_1(i) \pmod M$ 和跨度步长 $skip_i = hash_2(i) \pmod {(M - 1)} + 1$。由于 $M$ 为素数，$skip_i$ 与 $M$ 必然互质，因此生成出的序列 $P_i[j] = (offset_i + j \times skip_i) \pmod M$ 能够完美遍历整个表中的所有槽位，形成该节点的全局槽位偏好排列。
- **轮询填表算法（Round-Robin Allocation）**：算法按轮次迭代运行，每轮让每个存活的后端节点依次在其偏好序列中寻找下一个尚未被占用的槽位并认领。若已被占用则继续向前跳步，直至全部 $M$ 个槽位被填满。
- **常数级极速路由（$O(1)$ Lookup）**：网络数据包进入时，交换机或软件网卡只需对数据包五元组（5-tuple: 源 IP、源端口、目的 IP、目的端口、协议）做哈希并对 $M$ 取模，直接以数组下标读取目标节点：`backend = lookup_table[hash(packet) % M]`。全程无锁、无分支、无树检索，达到硬件级的 $O(1)$ 吞吐性能。

### 为什么重要

1. **线速转发与极致的查找性能（$O(1)$ Line-Rate Forwarding）**：
   在 40GbE/100GbE 乃至更高带宽的现代软件定义网络中，每个数据包的处理窗口只有几微秒甚至纳秒。传统哈希环的 $O(\log N)$ 二分查找会频繁造成 CPU 缓存失效（Cache Miss）。Maglev 将哈希路由退化为一次单次内存数组寻址，数据局部性极高，使通用 x86 服务器甚至 eBPF 程序能够直接跑满网卡线速。

2. **近乎完美的负载均衡度（Minimal Load Variance）**：
   传统哈希环由于虚拟节点的泊松分布特性，节点之间的负载方差通常高达 5%~10% 以上。Maglev 采用确定性轮序填表，使得每个后端节点认领到的槽位数量极为平均（每个节点槽位数为 $\lfloor M/N \rfloor$ 或 $\lceil M/N \rceil$），天然消除了热点倾斜。

3. **最小化连接中断（Minimal Disruption & Connection Consistency）**：
   当后端服务器故障下线或扩容上线时，Maglev 重新填充查找表。数学与工程实践证明，仅有故障节点原先占据的槽位会被其他节点瓜分；存活节点的绝大多数槽位归属完全保持不变。这意味着已有连接（Flows）不会被重置，完美保障了长连接有状态服务的网络连续性。

_隐喻对应表_

- 六万五千五百三十七个青石泊位 → 素数大小为 $M$ 的固定查找表（Lookup Table，如 $M=65537$）
- 货船五样印记（五印官防） → 网络数据包五元组（5-tuple：源IP、源端口、目的IP、目的端口、协议）
- 漕运各大船帮 → 后端服务器集群（Backend Servers / Endpoints）
- 起锚位与行步跨度（私律） → 节点的哈希偏移量 $offset$ 与跨度步长 $skip$
- 轮踏泊位的偏好序列 → 节点的确定性伪随机全排列（Permutation $P_i$）
- 击锣轮流插旗填筹 → 轮询填表算法（Round-Robin Population of Lookup Table）
- 进闸取余、抬头见旗 → 常数时间复杂度数据包寻址（$O(1)$ Array Lookup）
- 某帮撤修只填空位、万舟不动 → 节点宕机时的最小化连接中断（Minimal Disruption / Consistent Mapping）
</section>
<section class="en" markdown="1">
At the delta where nine mighty rivers converged into one torrential estuary stood an immense circular stone basin carved a thousand years ago: the Sushui Prime Basin.

The shoreline formed a vast, unbroken circle of dressed granite, chiseled with exactly sixty-five thousand five hundred thirty-seven numbered mooring berths, ordered from berth zero to berth sixty-five thousand five hundred thirty-six. Why sixty-five thousand five hundred thirty-seven? Because the ancient watermasters understood subtle mathematics: it was a massive prime number, divisible by nothing in heaven or earth save one and itself.

Every dawn, tens of thousands of cargo barges rode the tide through the outer water gate. Burned into the stern of every barge was an imperial vermillion seal carrying five distinct impressions: the sender merchant, departure dock, recipient warehouse, destination wharf, and a unique cargo tally secret.

Deep within the prime basin operated four prestigious transport guilds—the Azure Dragon, Black Tortoise, White Tiger, and Vermilion Bird fleets. Every arriving barge had to be routed to one of these fleets for immediate unloading and distribution.

In the early days, the gatekeepers used the simplest arithmetic: count how many guilds were on duty today, divide the barge's tally secret by that count, and send the boat to whoever matched the remainder. But during one violent summer thunderstorm, three heavy keelboats of the White Tiger fleet were shattered against the shoals and forced into drydock. With the guild count reduced by one, the modulo calculation shifted for every single vessel. Thousands of chained barges midway through unloading at the Azure Dragon wharves were abruptly rerouted overnight to the Black Tortoise and Vermilion Bird! Ledgers tore, mooring ropes tangled, and the entire harbor froze in gridlock for half a month.

Later, the harbor masters adopted the widely praised "Silk Ribbon Ring"—a closed ribbon where guilds pinned their markers at random intervals, and arriving vessels dropped onto the ring to slide clockwise toward the nearest post. Yet misfortune persisted. When the guild masters rolled dice to set their stakes, the distribution clustered terribly. The Vermilion Bird fleet was assigned an ocean of water and worked their sailors to exhaustion, while the Azure Dragon fleet held only a cramped puddle where crews spent afternoons cracking melon seeds. Worse still, when morning tides brought an avalanche of ships through the gate, the gatekeeper had to unroll yards of silk scrolls, scanning back and forth through the markers to find each match. Ships backed up outside the river gates for leagues.

Then arrived Imperial Surveyor Qi from the Board of Works. He set down a bronze gong and three unbending rules.

Standing atop the watchtower, Surveyor Qi addressed the harbor masters: "River tides wait for no man; a boat passing the gate must be dispatched at a single glance. Workloads must be balanced down to the grain. And when one fleet withdraws for repairs, not a single undisturbed ship shall ever shift its mooring by an inch."

The watermen came to call this system **"The Permuted Tallies."**

First Rule: **Private Strides Across the Prime Circle.**
The harbor abandoned silk rings. The sixty-five thousand five hundred thirty-seven stone berths carved into the granite quay became the sole foundation.
Using each fleet's official heraldic seal, Surveyor Qi computed two fixed integers: an **Anchor Offset** and a **Stride Skip**.
Because the total number of berths was a prime number, and the stride was never zero, any fleet marching from its offset with fixed leaps was mathematically guaranteed to visit every single berth across the circle without a single omission or duplicate. This sequence became that fleet's private, immutable preference list for all berths.

Second Rule: **The Gong of Alternating Tallies.**
On the day when berths were assigned for the season, the bronze gong sounded across the basin. The guild masters assembled before the pavilion, competing no longer.
At the first gong strike, the Azure Dragon fleet declared its first-choice berth and staked its banner. The Black Tortoise declared its first choice. The White Tiger and Vermilion Bird did the same.
At the second gong strike, each fleet declared its second preference. If a berth had already been claimed by an earlier banner, that fleet simply marched forward along its stride rhythm until it found an unoccupied stone berth.
Round by round, gong by gong, the guilds claimed berths in lockstep turns. When all sixty-five thousand five hundred thirty-seven berths were claimed, a marvel emerged: every fleet held virtually the exact same number of mooring slips, balanced as if weighed upon a jeweler's scale!

Third Rule: **The Instant Glance.**
When dawn cargo barges surged through the water gate, the gatekeeper never unfurled a scroll. He simply took the remainder of the barge's five-seal tally divided by sixty-five thousand five hundred thirty-seven, looked directly up at that numbered stone berth, and shouted the name on the flag:
"Berth thirty-four thousand, Azure Dragon!"
It was an instantaneous dispatch: no queues, no binary tree searches, no iterative scans. The water gate swung open like thunder, and thousands of ships sped straight to their berths like loosed arrows.

The true elegance shone during the next tempest.
When the White Tiger fleet took lightning damage and withdrew to drydock, the harbor masters simply pulled the White Tiger banners from their stone slips. The surviving Azure Dragon, Black Tortoise, and Vermilion Bird fleets merely ran their stride rhythms to fill those vacated slips.
Berths originally belonging to Azure Dragon remained Azure Dragon. Those belonging to Black Tortoise remained Black Tortoise. Among the tens of thousands of barges unloading throughout the prime basin, not a single one was forced to change its port!

From that day forth, ten thousand sails crested the Sushui waters, and even when all the empire's commerce gathered at once, the gates never jammed again.

---

By now you've probably recognized it: this is the celebrated **Maglev Consistent Hashing Algorithm**, designed and published by Google in their 2016 network load balancing architecture.

### What it is

**Maglev Hashing** is the core consistent hashing and table lookup algorithm presented by Google in the 2016 NSDI paper *Maglev: A Fast and Reliable Software Network Load Balancer*. It powers Google's edge software load balancers, Envoy Proxy's `maglev` cluster load balancing, and modern cloud-native networking engines like Cilium.

Traditional consistent hashing architectures (such as ring-based Ketama) suffer from two core limitations:
1. **Uneven Load Distribution & High Memory Footprint**: To achieve acceptable balance across backends on a traditional hash ring, hundreds or thousands of "virtual nodes" must be allocated per server. This inflates memory consumption while still leaving significant load variance.
2. **Lookup Latency ($O(\log N)$)**: For every incoming packet, the load balancer must perform a binary search across an ordered array or red-black tree to locate the nearest point on the ring, introducing CPU cache misses and stalling packet processing at line rate.

Maglev introduced a radically different lookup-table design:
- **Prime Lookup Table ($M$)**: A fixed-size array of prime size $M$ (typically $M = 65537$), substantially larger than the number of backends $N$.
- **Deterministic Permutation Generation**: For each backend $i$, two independent hash functions compute an initial offset $offset_i = hash_1(i) \pmod M$ and a stride $skip_i = hash_2(i) \pmod {(M - 1)} + 1$. Because $M$ is prime, $skip_i$ is coprime to $M$, ensuring that the sequence $P_i[j] = (offset_i + j \times skip_i) \pmod M$ generates a full pseudo-random permutation visiting all $M$ slots without repetition.
- **Round-Robin Table Population**: An iterative allocation loop runs across rounds. In each round, every healthy backend claims its next preferred unoccupied slot. If a slot is already taken, it advances along its permutation until finding an empty slot, continuing until all $M$ slots are claimed.
- **Instant $O(1)$ Line-Rate Lookup**: When a packet arrives, the router computes the hash of the packet's 5-tuple (source IP, source port, destination IP, destination port, protocol) modulo $M$, and reads the assigned backend directly by array index: `backend = lookup_table[hash(packet) % M]`. This constant-time, branchless lookup requires no synchronization and executes with near-zero latency.

### Why it matters

1. **True $O(1)$ Line-Rate Forwarding**:
   In modern 40GbE/100GbE software-defined networks, packet processing budgets are measured in nanoseconds. Maglev replaces $O(\log N)$ binary searches with a single flat array access. The lookup table fits comfortably within CPU L2/L3 caches, enabling commodity x86 servers and kernel eBPF programs to dispatch millions of packets per second at wire speed.

2. **Near-Zero Load Imbalance (Minimal Load Variance)**:
   Unlike the Poisson-distributed variance of traditional hash rings, Maglev's turn-by-turn round-robin allocation guarantees that each backend is assigned virtually the exact same number of slots ($\lfloor M/N \rfloor$ or $\lceil M/N \rceil$). Hotspots and traffic skews are eliminated by construction.

3. **Minimal Connection Disruption (Consistent Flow Affinity)**:
   When a backend fails or new capacity is added, the lookup table is regenerated. Crucially, slots previously belonging to surviving backends overwhelmingly retain their original assignments; only the slots belonging to failed nodes are redistributed among healthy peers. Existing network flows maintain consistent backend routing without connection resets or session drops.

_Metaphor mapping_

- Sixty-five thousand five hundred thirty-seven berths → Prime-sized lookup table ($M = 65537$)
- Five-seal impression on barge sterns → Network packet 5-tuple (source IP/port, destination IP/port, protocol)
- Transport guilds → Backend servers / endpoints
- Anchor offset and stride skip → Backend hash parameters $offset$ and $skip$
- Stride permutation across berths → Deterministic pseudo-random permutation ($P_i$)
- Gong strikes and round-robin flag placement → Iterative round-robin lookup table population algorithm
- Gatekeeper's single glance and modulo index → Constant-time $O(1)$ direct array lookup
- Tempest repairs filling only vacant slips → Minimal disruption and stable flow affinity during node failure
</section>
