---
layout: fable
title: "九骑环塘 · Nine Riders Around the Ring Canal"
title_zh: "九骑环塘"
title_en: "Nine Riders Around the Ring Canal"
concept: "Ring All-Reduce"
tags: [distributed-systems, performance]
illustration: /assets/art/2026-11-02-ring-all-reduce.jpg
youtube_id: "uw_vgU6MMxY"
---
<section class="zh" markdown="1">
玉环塘是圆的。塘心一座孤山，山脚一圈水道，像一枚戒指套在石头上。水道上九座磨坊，坊坊有闸，闸闸通塘。

井伯守这九道闸，守了四十年。规矩不复杂：每逢整点，九坊各把自家九闸的水位刻上竹册——一坊一册，一册九章——再把九册合成一本总账。有了总账，才知这一时辰谁该放、谁该收。塘水不认人，只认总账。

头几年九坊相安。井伯的老办法是：每个整点，每坊派出八名骑手，把自家九章的整本册子，送与另外八坊各一份。八八六十四骑，一共七十二骑在路上跑。

册子薄，马也快，一个时辰跑得完。可井伯心里一直有个说不出的毛病：骑手到得有早有晚。先到的坊先合账，后到的坊后合，九坊的钟点前后差着小半个时辰，闸门便跟着抖。水一抖，下游水车抖；磨盘一抖，面就一阵细一阵粗。

真正的麻烦在第七年。塘西新开了五座磨坊，九坊成了十四坊。

井伯照老规矩排骑手：十四坊，坊坊十三骑，共一百八十二骑。塘边那条土路一次只容一骑，长龙从晌午排到日落。册子还是那么薄，路还是那么长，可骑手翻了一倍——因为"每坊送每坊"这件事，是乘出来的：坊数翻一倍，路上的骑手翻四倍。

第十四年，塘上已是二十七坊。井伯不敢算了。

那年春天，他的孙女阿芦从县学回来，站在塘边看了一整个时辰的长龙，说：“爷爷，您把册子寄错了。”

“寄错？”

“您让每人寄整本书。可要合总账，一个人只需管一章。”

阿芦在沙地上画了个圈，点了九个点。

“第一趟，*reduce-scatter*，散减。每坊只派一名骑手，只带一章。一坊带第一章，二坊带第二章，九坊带第九章，谁不带重样的。九骑同时上马，从九坊同时出发，同向而行，谁也追不上谁，谁也不挡谁。

骑手到了下一坊，坊主不问话，只做一件事：翻开自家册子的那一章，把自家的数加上去。鞍袋里那一章，从"我的数"变成"我和你的数"，再变成"我、你、他，三千五百亩的数"。骑手不停，一站一站往前走。

走完八站，骑手回到出发的坊。鞍袋里那一章，已是九坊九个数加起来的真数。这一坊，从此就是这一章的主人。”

“那别的坊呢？”

“八个骑手，各自回到各自的坊，各自捧回一章真数。九坊，九章，章章有主。这就是第一趟的全部。”

井伯盯着沙地看了很久：“第二趟？”

“第二趟，*all-gather*，扬。如今每坊手里有一章真数，缺其余八章。还是九骑，还是那个圈，还是八站。只是这一趟鞍袋里没有账可算——骑手带的是已经合好的那一章。到了下一坊，坊主抄一遍，还他；骑手继续走。八站走完，九坊架上，九章齐了。”

“抄比算重。”井伯说。

“是重。可它重得不变。您看，”阿芦在沙上写，“两趟加起来，一名骑手走十六站。九坊是八加八，十四坊是十三加十三，二十七坊是二十六加二十六。每多一个坊，只多两站路。

可每一站鞍袋里背的东西，薄了。二十七坊的时候，一章只是整本书的二十七分之一。所以一个坊前后背出去的总分量，是 (坊数−1)/坊数 的两倍本书——坊越多，这个数越靠近二，永远不过二。

您的老办法呢？每坊要背 (坊数−1) 本。九坊八本，十四坊十三本，二十七坊二十六本。它不是不过二，它是一路往上长，长到马背压塌。”

那一年汛期来得早。三月十七夜里塘水暴涨，水位一个时辰一变，总账得改成一刻一合。井伯算过老办法的骑手，算到一半就把笔放下了。

他照阿芦的法子办。

二十七坊，五十四骑，每骑鞍袋里薄薄一张。夜里塘边一盏灯笼接一盏灯笼，骑手们同时上马、同时到站、同时加数、同时回头——头一趟走完，天刚亮；第二趟走完，日上三竿。九坊的钟点第一次齐了，闸门一次也没抖。

井伯站在塘边，忽然明白了一件他四十年没想通的事：这些年他一直在想法子让马跑得快些，其实该做的是让鞍袋轻些。

有一回，三坊的骑手在泥里折了马腿，整环的骑手都在下一坊等他。井伯没骂人，只叫人多备了两匹驿马。他晓得——这条环上的规矩，是所有人等最慢的那一个。

到了这一步，你大概已经认出它了：这就是 *Ring All-Reduce*。

### 这是什么

*Ring All-Reduce* 是多机训练的集合通信原语：把 N 个节点上各自持有的向量（通常是梯度），求和成同一个总量，并让**每个节点都拿到完整结果**。它拆成两段：

第一段 *reduce-scatter*：把向量切成 N 块，第 i 个节点负责第 i 块。N−1 步内，每一块沿着环走一圈，每经过一个节点就把该节点对应的那一段加进去，最终每块的真和停在它的主人手里。

第二段 *all-gather*：已经算好的块再沿环走一圈，被各节点抄走，N−1 步后人人齐全。

总步数 2(N−1)，每步只传 V/N 的数据，所以每个节点发出的总量是 2(N−1)/N · V —— 趋近 2V，与 N 无关。这正是它被称为 *bandwidth-optimal*（带宽最优）的原因。

### 为什么重要

朴素的做法是 all-to-all：每个节点把完整向量发给其他 N−1 个节点，单节点开销是 (N−1)V，随集群规模线性增长。把集群从 8 卡扩到 512 卡，通信量涨 64 倍，而算力只涨 64 倍——于是训练彻底卡在网络上。

环形算法把单节点通信量钉在 2V 附近。扩集群时，你多付的是一条更长的路，不是一副更重的担子。

代价也有两个：所有节点严格同步，一个慢节点拖住整环（*straggler* 敏感）；2(N−1) 步的延迟在小消息下会压过带宽收益，所以小模型反而可能更慢。这也是为什么现代集群里它常与 *tree*、*NCCL hierarchical* 等算法按拓扑混用。

_隐喻对应表_

- 九座磨坊 → 集群里的 N 个 worker（GPU/节点）
- 一坊一册九章的水位 → 每个节点本地的一份梯度向量，按位置切成 N 块
- 每坊都需要的那本总账 → *all-reduce* 的语义：每个人都拿到求和结果
- 老办法：每坊派八骑送整本书 → *all-to-all*，单节点通信量 (N−1)V
- 坊数翻倍、骑手翻四倍 → 通信量随 N 线性增长，扩机器反而更慢
- 只带一章上路的骑手 → *reduce-scatter* 的块归属
- 骑手边走边把每坊的数加到鞍袋里 → 沿环累积（accumulate-in-flight），原始数据不出门，只有running sum 在走
- 走完八站回到本坊，这一章的真数归它 → reduce-scatter 结束，每节点持有唯一一个完整归约块
- 第二趟只抄不算 → *all-gather*：传播已归约的块，不再做算术
- 十六站 = 八加八 → 总步数 2(N−1)
- 每站只背 1/N 本书，总分量趋近两本 → 单节点通信量 2(N−1)/N·V，趋近 2V，与 N 无关
- 同时上马、同时到站，闸门不再抖 → 环形算法步数一致，收尾天然对齐
- 三坊折了马腿，整环等他 → *straggler* 敏感性，同步环的固有代价
- 井伯多备两匹驿马 → 工程上用冗余、超时、分层拓扑缓解掉队者
</section>
<section class="en" markdown="1">
The Jade Ring Pond was round. An isolated hill sat at its centre, and a channel ran around the hill's foot like a ring slipped over a stone. Along the channel stood nine mills. Every mill had a sluice, and every sluice opened into the pond.

Uncle Jing had kept those nine sluices for forty years. The rule was not complicated. On the hour, each of the nine mills carved its nine sluice readings onto a bamboo ledger — one mill, one ledger; one ledger, nine chapters — and the nine ledgers were combined into a single master account. With the master account, you knew which mill opened and which mill closed for that hour. Pond water does not recognize people. It recognizes only the master account.

For the first few years the nine mills lived in peace. Uncle Jing's old method: every hour, each mill sent out eight riders, carrying its complete nine-chapter ledger, one copy to each of the other eight mills. Eight eights make sixty-four, plus nothing from home — seventy-two riders on the road.

The ledgers were thin and the horses were fast; an hour was enough. But Uncle Jing carried a complaint he could never name. The riders arrived early and late. The mills that received first combined first; the mills that received last combined last. The nine clocks differed by nearly half an hour, and the sluices trembled in sympathy. When the water trembled, the downstream waterwheels trembled. When the millstones trembled, the flour came out fine for a while and coarse for a while.

The real trouble came in the seventh year. Five new mills opened west of the pond. Nine mills became fourteen.

Uncle Jing arranged the riders by the old rule: fourteen mills, thirteen riders each, one hundred and eighty-two horses. The dirt road along the pond admitted one rider at a time. The queue ran from noon to sunset. The ledgers were still thin, the road still the same length — but the riders had doubled, because "every mill to every mill" multiplies. Double the mills, quadruple the horses.

By the fourteenth year there were twenty-seven mills on the pond. Uncle Jing stopped doing the arithmetic.

That spring his granddaughter Alu came home from the county school. She stood at the pond's edge and watched the queue for a full hour. Then she said: "Grandfather, you are sending the wrong book."

"The wrong book?"

"You make every rider carry the whole ledger. But to build a master account, a rider only needs to carry one chapter."

Alu drew a circle in the sand and marked nine points on it.

"The first pass is *reduce-scatter*. Each mill sends exactly one rider, carrying exactly one chapter. Mill one carries chapter one, mill two carries chapter two, mill nine carries chapter nine — no duplicates anywhere. Nine riders mount at the same instant, leave nine mills at the same instant, ride the same direction. Nobody overtakes. Nobody blocks.

When a rider reaches the next mill, the miller asks no questions. He does one thing: he opens that chapter in his own ledger and adds his numbers to the rider's. The chapter in the saddlebag stops being "my numbers" and becomes "my numbers and yours," then "mine and yours and his — thirty-five hundred acres of numbers." The rider does not stop. He goes on, station by station.

After eight stations the rider returns to the mill he started from. The chapter in his saddlebag is now the true sum of nine mills' nine numbers. That mill is the owner of that chapter, from then on."

"And the others?"

"Eight riders return to eight mills, each carrying one chapter's true sum. Nine mills, nine chapters, every chapter owned. That is the whole of the first pass."

Uncle Jing stared at the sand for a long while. "The second pass?"

"The second pass is *all-gather*. Now each mill holds one true chapter and lacks the other eight. Still nine riders, still that circle, still eight stations. Only this time there is no arithmetic in the saddlebag — the rider carries a chapter already summed. At the next mill, the miller copies it and hands it back; the rider rides on. Eight stations later, all nine chapters sit on all nine shelves."

"Copying is heavier than adding," said Uncle Jing.

"Heavier, yes. But it stays heavy in the same amount. Look." Alu wrote in the sand. "Both passes together, one rider covers sixteen stations. Nine mills is eight plus eight. Fourteen mills is thirteen plus thirteen. Twenty-seven mills is twenty-six plus twenty-six. Each new mill adds two stations of road, and nothing more.

But what the saddlebag holds at each station gets thinner. At twenty-seven mills, one chapter is one twenty-seventh of the whole ledger. So the total weight a single mill sends out over both passes is two × (mills − 1) / mills ledgers — and the more mills there are, the closer that gets to two, and it never passes two.

Your old method? Each mill carried (mills − 1) full ledgers. Eight at nine mills. Thirteen at fourteen. Twenty-six at twenty-seven. It does not stop at two. It climbs until the horses' backs give out."

The flood season came early that year. On the night of the seventeenth of the third month the pond rose fast. The water level changed hourly, and the master account had to be rebuilt every quarter hour. Uncle Jing began calculating the old method's riders, and put down his brush halfway through.

He did it Alu's way.

Twenty-seven mills, fifty-four riders, a single thin sheet in each saddlebag. That night the lanterns along the pond lit one after another. The riders mounted together, arrived together, added together, turned back together. The first pass finished at first light. The second pass finished with the sun well up. For the first time, the nine clocks agreed, and the sluices did not tremble once.

Standing at the pond's edge, Uncle Jing understood something he had missed for forty years. All that time he had been trying to make the horses faster. What he should have done was make the saddlebags lighter.

Once, a rider from the third mill broke his horse's leg in the mud, and the whole ring waited for him at the next mill. Uncle Jing did not curse. He simply ordered two spare post horses. He understood: on this ring, everyone waits for the slowest one.

By now you have probably recognized it: this is *Ring All-Reduce*.

### What it is

*Ring All-Reduce* is the collective-communication primitive behind multi-node training: take the vectors each of N workers holds (usually gradients), sum them into one total, and give **every worker the complete result**. It splits into two halves.

The first half, *reduce-scatter*: cut the vector into N chunks, worker i owning chunk i. Over N−1 steps, each chunk travels the ring once, picking up that worker's slice of the sum at every hop, until each chunk's true total rests with its owner.

The second half, *all-gather*: the finished chunks travel the ring again and are copied out by each worker, so that after N−1 steps everyone holds everything.

Total steps: 2(N−1), each moving only V/N of the data. So the volume a single worker sends is 2(N−1)/N · V — approaching 2V, independent of N. That is why it is called *bandwidth-optimal*.

### Why it matters

The naive approach is all-to-all: every worker ships its full vector to the other N−1 workers, costing (N−1)V per worker and growing linearly with cluster size. Scale from 8 GPUs to 512 and communication grows 64×, exactly as much as compute — so training stalls on the network.

The ring algorithm pins per-worker traffic near 2V. Scaling out costs you a longer road, not a heavier load.

Two costs come with it. Every worker is strictly synchronized, so one slow worker stalls the whole ring (*straggler* sensitivity). And 2(N−1) steps of latency can overwhelm the bandwidth win on small messages, which is why small models sometimes run slower on a ring. That is why real clusters mix it with *tree* and *NCCL hierarchical* algorithms chosen by topology.

_Metaphor mapping_

- Nine mills → the N workers (GPUs/nodes) in a cluster
- One mill's nine-chapter water ledger → each worker's local gradient vector, sliced into N chunks by position
- The master account every mill needs → the semantics of *all-reduce*: everyone receives the sum
- The old method: eight riders per mill carrying whole ledgers → *all-to-all*, (N−1)V per worker
- Mills double, riders quadruple → traffic grows linearly with N; adding machines makes things slower
- The rider carrying only one chapter → chunk ownership in *reduce-scatter*
- The rider adding each mill's figure as he rides → accumulate-in-flight; raw data never leaves home, only the running sum travels
- Eight stations later the chapter's true total belongs to that mill → reduce-scatter done, each worker holds exactly one fully reduced chunk
- The second pass only copies, never computes → *all-gather* propagates reduced chunks with no arithmetic
- Sixteen stations = eight plus eight → total step count 2(N−1)
- Each station carries 1/N of a ledger; total weight approaches two ledgers → per-worker traffic 2(N−1)/N·V, approaching 2V regardless of N
- Mounting together, arriving together, sluices steady → uniform step counts, naturally aligned completion
- The third mill's broken horse stalls the whole ring → *straggler* sensitivity, the inherent cost of a synchronous ring
- Uncle Jing ordering spare post horses → engineering mitigations: redundancy, timeouts, hierarchical topologies
</section>
