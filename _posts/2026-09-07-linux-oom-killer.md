---
layout: fable
title: "沉压的粮栈 · The Sinking Storehouse"
title_zh: "沉压的粮栈"
title_en: "The Sinking Storehouse"
concept: "Linux OOM Killer and oom_score_adj"
tags: [linux, memory, kubernetes]
illustration: /assets/art/2026-09-07-linux-oom-killer.jpg
youtube_id: "FSD2gh8g0V0"
---
<section class="zh" markdown="1">
云岩山下的官栈，建在一片架空的粗大石梁与硬木悬桩之上。栈房三面环谷，脚下是奔流的浊水。

在这座大栈房管事三十年的老沈，手里捏着一本全县最离奇的账册。栈房统共只有一千间储室的承重，老沈发出去的租契，白纸黑字加起来却有三千间。

这并非老沈贪墨，而是县衙默许的常例——**虚契**。

南来北往的货商多半只图买个心安，契约签得顶格，平日里运来的皮货、山茶与铁料，充其量只占签下舱位的两三成。只要大家不约而同把货全堆进来，木梁便受得住，栈房还能多收三倍的规银。几十年来，栈内清风徐来，相安无事。

直到那年深冬，寒潮突降，山道冰封。

恐慌在一天之内传遍了山城。所有的商户都认定唯有把全部家当搬进官栈才能避难，于是成群结队的骡车冒雪涌上栈道。粮商滚来了两人高的油缸，布商卸下了浸过桐油的生丝，陶商搬来了整箱整箱的黑瓷。

入夜时分，木梁开始发出渗人的咯吱声。悬桩下的碎石噼噼啪啪滚入山涧，整座栈房的地面沉沉下陷了半尺。空位彻底告罄，走道里塞满了货箱，连侧身落脚的地界都没了。

就在这千钧一发之际，药铺的小学徒气喘吁吁地推开栈门，怀里抱着**一小篓干甘草**。

学徒踮着脚，小心翼翼地把这篓不过两斤重的草药放在了门槛内侧的一角。

**“喀嚓——！”**

一声闷雷般的断裂声自地底炸响。中轴上一根合抱粗的松木托梁，顺着木纹猛地裂开了一道两指宽的缝隙。整片木地板肉眼可见地下沉倾斜，悬崖上的铁索被拉得火星四溅。

大祸临头。木梁已经到了极限，若不在半刻钟内卸掉至少万斤重物，整座栈房便会连人带货一起坠入深渊。

小学徒吓得瘫倒在地，放声大哭，以为是自己那篓甘草压塌了官仓。商客们群情激愤，围上来指着学徒破口大骂，甚至有人要夺过甘草扔出窗外。

老沈拄着沉香木算筹，拨开众人，冷喝一声：“住手！”

他看都没看那篓甘草一眼。

“天底下没有因一根灯草压死战马的道理，”老沈冷冷道，“把草扔了，能挪出两斤分量？这根梁照样得断！”

老沈展开手中的铁皮名册。在深渊边缘，他必须斩决一个商户，直接把整舱的货物推下山崖，以空出足够让木梁复位的生机。但这刀挥向谁，有一套铁打的苛酷算则：

第一，**绝不能碰带免死铁牌的仓位**。栈房最深处的三间暗仓，上面锁着知县的官印与烽火台军粮。无论栈房怎么倾危，哪怕天塌下来，老沈的筹子也绝不能点向那三间屋子。那叫“免死金券”，动了，全县立时大乱。

第二，**挑吃重最深的大蠹**。灾难是整个栈房的浩劫，杀掉三五个只放了一小包生姜的散客，根本凑不足退梁的分量，徒增冤魂。要挑，就得挑那个舱房占得最满、堆物最庞大臃肿的人。

第三，**参验身份的轻重薄厚**。有些行商只交了最便宜的“游脚席位”，契上早写明了遇灾先黜；而有些老商号抵押了祖产，契位极为贵重。

老沈手里的算筹飞快拨动。目光掠过军粮仓（金券护体，不动），掠过药铺学徒（分量微末，杀了无用），掠过几个焦急的小布商。最后，他的筹子重重落在了临江第一大户钱掌柜的名册上。

钱掌柜一人占了七百石发酵的湿麦，还私自加塞了二十口沉重的铅坛。

“开北窗暗门！”老沈面无表情地下令，“拉吊索，起闸板！”

伙计们手起斧落斩断麻绳，地板轰然翻转，钱掌柜整整七百石沉重的麦仓如瀑布般顺着悬崖倾泻入江。狂风卷着麦屑呼啸而上，木梁发出长长一声如释重负的呻吟，猛地向上回弹了三寸，地底的断裂声戛然而止。

栈房保住了。

钱掌柜跪在悬崖边捶胸顿足，如丧考妣；而怀抱着那篓甘草的小学徒，在死寂的人群中安然无恙地捡回了一条命。

— — —

到这儿你大概已经认出来了：这是操作系统中最残酷也最坚决的保命机制——Linux 的 **OOM Killer（Out-Of-Memory Killer）** 与 **`oom_score_adj`**。

### 这是什么

Linux 内核为了榨干物理内存，默认开启了**内存超售（Memory Overcommit，`vm.overcommit_memory = 0`）**。它就像老沈超发的三倍租契：程序调用 `malloc()` 申请内存时，内核只给虚拟地址空间，并不立即分配物理页面，赌的是“绝大多数进程不会同时用光它们声称要用的所有内存”。

然而一旦突发负载，所有进程真的同时往页面里写数据（内存缺页异常，Page Fault），物理内存和 Swap 空间被彻底耗尽，且内核无法通过页面回收（Page Reclaim）腾出任何空闲内存时，**OOM（Out of Memory）危机**爆发。

为了防止整台服务器内核崩溃或永久挂死，内核的 `mm/oom_kill.c` 会被瞬间唤醒，派出 OOM Killer，挑出一个进程用 `SIGKILL（kill -9）` 予以枪决，强行回收其占用的所有物理页。

OOM Killer 断案绝不是“谁触发了最后一次内存申请就杀谁”（绝不杀无辜的小学徒），而是通过 `oom_badness()` 函数给每个候选进程计算出一个 **`oom_score`（0 到 1000 的坏蛋指数）**：
1. **基础分**：进程当前占用的物理内存页（RSS）、页表消耗和 Swap 用量占系统总可用内存的比例；占用越大，基础分越高；
2. **权重调节（`oom_score_adj`）**：用户空间可以通过 `/proc/<PID>/oom_score_adj` 手动干预判决。取值范围从 `-1000`（完全免疫，绝不击杀）到 `+1000`（最优先级牺牲品）。

### 为什么重要

在现代云计算与 Kubernetes / OpenShift 集群中，OOM Killer 是决定微服务生死的最高法官。

Kubernetes 将容器的 `resources.requests` 和 `limits` 映射成了三类服务质量等级（QoS Class），其底层就是靠操作 Linux 的 `oom_score_adj` 实现阶梯式保命：
- **Guaranteed（保活金牌）**：`requests` 等于 `limits` 的高优先级核心服务。Kubernetes 会为其容器被打上 `oom_score_adj = -997`。除非整台宿主机上除了核心守护进程外已无其他生灵，否则 OOM Killer 绝不敢动它们；
- **Burstable（弹性波动的骨干）**：`requests` 小于 `limits`。内核根据其申请与超额的比例计算一个介于 2 到 999 的分值，占用超额比例越高，挨刀几率越大；
- **BestEffort（随风飘摇的游客）**：完全没有配置 requests/limits。Kubernetes 直接给它打上满额的 `oom_score_adj = 1000`，宿主机只要内存吃紧，第一把火必定烧向它们。

理解这套机制，能解释生产排障中无数看似玄学的疑案：为什么明明是一个微不足道的定时脚本触发了报警，死掉的却是后台狂吃内存但没有配 limits 的 Java 进程？为什么容器退出码是典型的 `137`（128 + 9，即被 SIGKILL 终止）？

因为在内存耗尽的悬崖边，系统的正义不是惩罚最后一滴水，而是砸碎那只体积最大、防备最弱的蓄水缸。

### 隐喻对应表

- **悬空的石梁与粗木栈房** —— 宿主机的物理内存（RAM）与 Swap
- **发出去三倍的租契（虚契）** —— Linux 内核的内存超售（Memory Overcommit）
- **租客们在暴雪夜全量运货入栈** —— 进程并发爆发式分配并写入实际物理内存（Page Fault 填满物理页）
- **抱甘草进门的小学徒** —— 压垮内存的最后一个极其微小的 `malloc` 请求
- **地底断裂的巨梁与沉坠危机** —— 系统内存彻底耗尽（Out of Memory）
- **老沈手里的铁皮算筹与苛刻算法** —— 内核计算坏蛋指数的算法（`oom_badness`）
- **不能碰的知县印玺与军粮仓** —— `oom_score_adj = -1000`（内核/关键进程免疫，OOM_SCORE_ADJ_MIN）
- **便宜的游脚席位** —— Kubernetes `BestEffort` 容器（`oom_score_adj = 1000`，最先牺牲）
- **老商号按比例抵押的贵重契位** —— Kubernetes `Burstable` 与 `Guaranteed` 容器（`-997` 到 `999`）
- **钱掌柜私藏的七百石湿麦与铅坛** —— 内存占用极大（高 RSS / Swap）且没有特权免疫的无良进程
- **开闸放货坠江** —— 发送 `SIGKILL`（退出码 137）强行回收物理内存页
- **老沈保住全栈** —— OOM Killer 避免整机死锁（Kernel Panic）
</section>
<section class="en" markdown="1">
The government warehouse below Cloud Cliff was built entirely on aerial stone piers and heavy cedar stilts. Three sides of the hall hung over a deep ravine, above the roaring rapids of the mountain river.

Old Shen, who had managed the storehouse for thirty years, kept the most eccentric ledger in the county. The stilts beneath the floor had a bearing capacity of exactly one thousand storage bays. Yet if you added up the white-paper leases Shen had issued over the decades, the total came to three thousand.

This was no private graft; it was the accepted custom sanctioned by the magistrate’s yamen: **overbooking**.

Merchants from north and south leased storage bays purely for peace of mind. They claimed the maximum on paper, yet in ordinary seasons the hides, mountain teas, and ironware they actually delivered barely filled twenty or thirty percent of their rented quotas. As long as they did not all arrive with full wagons on the exact same day, the timbers held effortlessly, and the yamen collected triple the storage fees. For thirty years, the mountain breezes blew through the airy halls, and peace reigned.

Until the dead of winter, when an unprecedented cold snap struck the passes.

Panic swept through the mountain city in a single morning. Convinced that their livelihoods would freeze or be looted outside, every merchant hauled their entire accumulated harvest toward the warehouse. Mule carts lined the snow-slicked wooden approaches for miles. Grain dealers rolled in oil tubs as tall as two men; silk merchants stacked bales of tung-oiled raw thread; potters carried in heavy crates of ironstone glaze.

By nightfall, the cedar timbers began to groan with a sickening, grinding shriek. Beneath the piers, loose shale clattered into the river gorge below. The entire floor sagged half a foot under the monstrous burden. The bays were packed to the rafters; crates overflowed into the corridors until a man could not squeeze through sideways.

At that desperate hour, an apprentice boy from an herbalist’s shop pushed through the heavy doors, gasping for breath, clutching **a single tiny wicker basket of dried licorice root**.

The boy stood on tiptoe and gingerly set the basket down on the wooden floorboards just inside the threshold.

**CRACK!**

A thunderous snap exploded from the foundations. A center support strut, thick as a man’s chest, split clean down the grain with a two-finger gap. The floorboards visibly dipped toward the abyss, and the iron anchor cables along the cliff face screamed under the sudden tension.

Catastrophe. The beams had reached absolute structural failure. Unless tens of thousands of pounds were cleared within moments, the entire storehouse — goods, merchants, and keepers — would plunge into the gorge.

The apprentice collapsed to the floor in tears, wailing that his little basket of licorice had destroyed the county warehouse. Furious merchants surrounded him, shaking their fists, and one man lunged forward to seize the licorice and fling it out into the snow.

Old Shen raised his heavy abacus and barked: "Hold!"

He did not glance at the licorice.

"No horse in history was ever crushed by a blade of grass," Shen said coldly. "Throw the grass away, and do you lighten the timbers by two pounds? This beam will still break."

Shen unrolled his brass-bound tally ledger. On the edge of the abyss, he had to execute an irreversible judgment: designate one merchant’s stores and dump them down the mountain chutes into the river, restoring enough buoyancy for the framing to snap back into place. But deciding whose cargo died was governed by an iron rule:

First, **never touch the bays bearing the Imperial Bronze Plate**. Deep in the rock alcove lay three vaulted chambers locked with the magistrate’s seal and stocked with dried garrison grain. No matter how violently the floorboards bowed, Shen’s tally stick could never gesture toward those doors. That was royal immunity; touching it would trigger immediate rebellion across the border.

Second, **strike the bloated monster**. In a systemic crisis, casting out three small peddlers who stored a sack of ginger each would not shave enough weight to relieve the strut. If you must cut, you must cut the tenant who consumed the most vast, unwieldy mass of timber space.

Third, **weigh the status of the lease**. Casual hawkers who had paid only for "itinerant floor mats" had agreed in their contracts to be evicted first in an emergency; while ancient trading houses who had pledged family estates carried heavy covenants.

Shen’s tally stick flickered across the pages. He passed the garrison stores (shielded by royal immunity), passed the apprentice boy (his tiny load saved nothing), passed several frantic cloth merchants. Finally, the tip of his stick slammed down upon the account of Master Qian, the richest grain magnate on the river.

Qian alone had crammed seven hundred bushels of moisture-heavy wheat into his bay, and had secretly wedged twenty heavy lead-sealed storage vats behind them.

"Open the north river chute!" Shen ordered without a flicker of emotion. "Cut the trip-ropes! Release the floor latch!"

The warehousemen swung their axes. The counterweighted floorboards swung open into the night, and Qian’s seven hundred bushels of grain roared down the sheer cliff into the black rapids like a stone avalanche. A blast of wind and chaff surged up into the hall. The timber frame gave a long, shuddering sigh and sprang upward three inches, and the splintering sound below fell instantly silent.

The warehouse was saved.

Master Qian knelt at the edge of the chute beating his chest, mourning his fortune; while the apprentice boy, still cradling his little basket of licorice root, stood unharmed amidst the silent crowd.

— — —

By now you've probably recognized it: this is the operating system's most ruthless survival mechanism — the Linux **OOM Killer (Out-Of-Memory Killer)** and **`oom_score_adj`**.

### What it is

To extract maximum utility from physical RAM, the Linux kernel enables **Memory Overcommit (`vm.overcommit_memory = 0`)** by default. It is identical to Old Shen’s triple leases: when a program calls `malloc()`, the kernel only reserves virtual address space without immediately provisioning physical RAM. It gambles that the majority of processes will never write to all of their claimed memory simultaneously.

However, during traffic spikes or memory leaks, when processes actively write data into those pages (triggering page faults), physical RAM and swap space become completely exhausted. When kernel direct page reclaim can no longer free even a few pages, the system enters a fatal **Out of Memory (OOM)** condition.

To prevent the entire machine from experiencing a kernel panic or freezing irrevocably, the kernel’s `mm/oom_kill.c` awakens the OOM Killer. It selects a victim process and terminates it with `SIGKILL` (`kill -9`) to immediately reclaim its physical pages.

The OOM Killer never punishes the process that made the final, tiny memory allocation (it spares the apprentice boy). Instead, the `oom_badness()` function calculates an **`oom_score` (from 0 to 1000)** for every runnable task:
1. **Base Badness**: The proportion of physical RAM (RSS), page table allocations, and swap space consumed by the process relative to total available memory. The more bloated the process, the higher its score;
2. **Heuristic Adjustment (`oom_score_adj` exposed in `/proc/<PID>/oom_score_adj`)**: Userspace can bias the verdict with values ranging from `-1000` (`OOM_SCORE_ADJ_MIN`, completely immune from OOM sacrifice) to `+1000` (`OOM_SCORE_ADJ_MAX`, first in line for termination).

### Why it matters

In cloud computing and Kubernetes / OpenShift clusters, the OOM Killer is the supreme executioner determining which workloads survive node pressure.

Kubernetes maps a Pod's `resources.requests` and `limits` directly into three Quality of Service (QoS) classes by configuring Linux `oom_score_adj`:
- **Guaranteed**: Containers where `requests` equal `limits`. Kubernetes sets their `oom_score_adj` to `-997`. System daemons like `kubelet` and `containerd` sit at `-998` and `-999`. Unless every other user process on the node is already dead, the OOM Killer will never touch a Guaranteed workload;
- **Burstable**: Containers where `requests` are less than `limits`. Kubernetes calculates a score between 2 and 999 based on the percentage of memory requested relative to the node’s capacity. The more memory a container consumes beyond its request, the higher its badness score;
- **BestEffort**: Containers with neither requests nor limits configured. Kubernetes assigns them `oom_score_adj = 1000`. The instant node memory tightens, they are the very first to be executed.

Understanding this mechanism dispels one of production engineering's most frequent mysteries: why a lightweight cron job requesting 4 KB triggered an alarm, but a giant, leaky Java container without memory limits was the one terminated with exit code `137` (`128 + 9`, SIGKILL).

In an out-of-memory crisis, justice is not about punishing the last drop of water. It is about smashing the largest, least-protected reservoir in the room.

### Metaphor Mapping Table

- **The aerial timber storehouse on wooden stilts** — Physical RAM and Swap capacity
- **Three thousand leases issued for one thousand bays** — Linux Memory Overcommit (`vm.overcommit_memory`)
- **Merchants rushing in with full wagons during the blizzard** — Concurrent processes allocating and writing physical pages (Page Faults)
- **The apprentice boy with the basket of licorice** — The final tiny `malloc` call that triggers the allocation failure
- **The splintering center beam and imminent collapse** — Fatal Out of Memory (OOM) condition
- **Old Shen's tally stick and selection rules** — The `oom_badness()` calculation algorithm
- **The Imperial Bronze Plate on the garrison grain bays** — `oom_score_adj = -1000` (immunity from OOM sacrifice)
- **The cheap itinerant floor mats** — Kubernetes `BestEffort` QoS class (`oom_score_adj = 1000`)
- **Trading houses with pledged estates** — Kubernetes `Burstable` and `Guaranteed` QoS classes
- **Master Qian's 700 bushels of wet wheat and lead vats** — A memory-hogging process (high RSS and swap) with no immunity
- **Opening the floor chute to dump grain into the river** — Sending `SIGKILL` (exit code 137) to reclaim pages
- **The storehouse springing back and surviving** — OOM Killer preventing a complete kernel panic and system lockup
</section>
