---
layout: fable
title: "千仓连樗与三锁同匣 · The Thousand Granaries and the Copyset Seals"
title_zh: "千仓连樗与三锁同匣"
title_en: "The Thousand Granaries and the Copyset Seals"
concept: "Copysets Replication"
tags: [storage, distributed-systems]
illustration: /assets/art/2026-10-14-copysets-replication.jpg
youtube_id: "XpRmN9VwUMc"
---
<section class="zh" markdown="1">
在大雍朝的北疆边陲，平原旷野上耸立着整整五千座坚固的皇家粮仓。

这里储存着整座帝国防备荒年的军粮与种粮。大雍朝廷定有严酷的铁律：每一批入库的雪白秋粮，都必须分装入三只一模一样的朱漆铁皮匣，分别送入三座不同的粮仓存放。只要三座仓房中还有一座完好，粮食便能分毫不差地复原出来；唯有当存有同一批粮食的三座粮仓全数化为灰烬，帝国才会遭受不可挽回的灭顶之灾——丢失粮卷。

起初，各仓的总掌事奉行的是古老的“掷樗散存之法”。

粮车驶入关口，守关吏便取来几枚刻着符咒的木樗（骰子），信手一掷，随机选出三座粮仓编号，发牌存粮。年复一年，数万车粮食就这么随性散落在广袤的平原上。掌事们算得极美：五千座粮仓，即便哪座粮仓遭了雷火，另外四千九百九十九座粮仓里有的是同僚，大伙儿一拥而上，每座粮仓只需搬出极少的一小袋粮食协助复原，不到一炷香的工夫，受灾粮仓就能彻底修葺如初。这种“天下仓帮一仓”的迅捷恢复，令户部官吏们拍案叫绝。

然而，天运无常。

有一年寒冬腊月，北风呼啸，极度干燥，罕见的连环地龙翻身撕裂了大营，漫天磷火与狂风席卷旷野，一夜之间，五千座粮仓里竟有五十座同时燃起了熊熊烈火！

五十座粮仓受损，不过占了粮仓总数区区百分之一。户部主事原以为，丢失粮食的概率应当微乎其微。可当快马探子带回厚厚的核账薄时，营中一片死寂：

帝国数万车战略军粮中，竟有足足十余批完全绝收，尸骨无存！

主事瘫坐在帅案前，百思不得其解：明明还有四千九百五十座粮仓完好无损，为何偏偏就有粮食的三份副本“全军覆没”？

直到户部新上任的度支司员外郎温大人策马赶到大营，在雪地上画出了一幅令人惊骇的算筹图。

“主事只知散存能让复原变快，却不知你们这套掷樗法，早已把帝国推入了万劫不复的深渊。”温大人指着雪地叹道，“全营五千座仓，你们每一次随机挑三座，年深日久，几乎把世间所有可能的三仓组合全给占满了！五千座仓里任意挑三座，能组合出的三仓搭档，足有两百多亿种；而你们为了让散存更均匀，生生让全营散布了数十万种不同的三仓绑定！”

“当五十座粮仓在烈火中坍塌时，这五十座仓之间，彼此能配出近两万组‘三仓绝灭’的厄运！全营到处都是你们随机编织的‘连环锁’，只要这近两万组厄运碰巧撞上你们散落出去的几十万种存粮组合，粮食就必定绝产！在你们那套法子下，五十座仓失火，粮卷彻底灭失的凶险，竟高达两成以上！”

众掌事冷汗浸透了重裘：“若不随性掷樗，难道要让同一组仓死守？那若倒了一座仓，其他两座仓岂不是要活活累死？”

温大人微微一笑，从袖中取出一卷名为《连环三匣令》的密卷。

“规矩照样要散，但绝不可任由乱掷。我们把全营五千座粮仓，划分为若干彼此交错的‘结义之盟’（*Copysets*）。”

温大人的法子精微绝伦：
每座粮仓不再与全营所有同僚随机搭伴，朝廷给每座仓定下固定的“结伴额度”（散布度 *Scatter Width*）。比如，甲仓一生只准与指定的九座仓轮流配成三仓同匣。全营五千座仓，最终在图谱上严格登记锁死为一万六千余组“钦定三仓盟”。

新粮入关，守关吏不得再私掷木樗，只能从这一万六千组明文登记的盟约中抽取一组存放。绝不允许在图册之外，私自将粮食散存入任何未结盟的三座粮仓。

掌事们起初忧心忡忡。直到第二年仲夏，酷暑天降，雷火再次击毁了其中的一座粮仓。与它结盟的另外几座仓立刻开仓运粮，虽然分担复原的仓房数量比昔日略少，但不过是多花了两三炷香的时间，受损仓房同样顺利复原。

更为奇迹的是，到了秋末，塞外风暴突袭，又有整整五十座粮仓遭遇沙暴顶倾坍塌。

当度支司官员们颤抖着核对账目时，奇迹出现了：
整整五千座仓、数万车存粮，在五十座粮仓齐损的惨剧下，**竟无一袋粮食三匣全失**！全营所有粮卷，百分之百完好无损！

大帐之内，欢声雷动。众掌事这才如梦初醒：原来随意散漫的“天下分担”，看似灵巧，实则是将全军的性命押在每一次厄运的排列组合之中；唯有恪守边界的“规整结盟”，方能在天崩地裂之时，筑起真正牢不可破的金汤防线。

---

到这里，你大概已经认出来了：这就是分布式存储系统领域著名的 **Copysets 复制算法**（Copysets Replication），由斯坦福大学 Asaf Cidon 等人在 USENIX ATC 2013 顶会上提出，并荣获 Best Paper 奖项。

### 这是什么

**Copysets** 是一种专为大规模分布式存储系统（如 HDFS、Ceph、RAMCloud、Cassandra 以及各类云原生对象存储）设计的**副本放置算法**，其核心目标是：在不降低单节点故障后并行恢复速度的前提下，彻底扼杀因机架掉电、PDU故障、网络分区等**相关性多节点并发故障**（*Correlated Failures*）引发的灾难性永久数据丢失。

在现代分布式存储中，每个数据块通常保存 $R$ 个副本（工业界常设为 $R = 3$）：
1. **随机副本放置的陷阱（Random Replication）**：
   传统的分布式文件系统（如早期 HDFS 或 Ceph 的默认 CRUSH 规则）倾向于将每个数据块的 3 个副本完全随机散落到集群中的不同节点上。为了在单个节点宕机时，能有尽可能多的健康节点并发协助拉取数据、极速完成重建，系统会刻意让每个节点的副本伙伴分散给许多不同的机器——这个指标被称为**散布度（*Scatter Width*, $S$）**。
   然而，完全随机放置会导致集群中实际产生的数据三元组数量爆炸，接近：
   $$N \times \binom{S}{R-1}$$
   在一个拥有 $N = 5000$ 个节点、设定散布度 $S = 10$ 的集群中，随机放置会生成约 $225,000$ 个不同的三节点副本组合。
2. **相关性故障下的灭顶之灾**：
   在真实云数据中心，服务器宕机很少是孤立发生的——供电分配单元（PDU）断电、机架顶置交换机（TOR）宕机、内核补丁滚动升级崩溃，经常瞬间导致整个集群 1%~2% 的节点（例如 $f = 50$ 台机器）同时下线。
   当 50 台机器同时宕机时，这 50 台机器本身能形成 $\binom{50}{3} = 19,600$ 种三元组组合。在随机放置下，由于全网散落着高达 22.5 万种数据块三元组，这近两万个故障组合撞上哪怕某一个数据块三元组的概率高达：
   $$P(\text{Data Loss}) \approx 1 - \left(1 - \frac{225,000}{\binom{5000}{3}}\right)^{19,600} \approx 19.1\%$$
   这意味着：每发生五次小规模机架掉电或网络风暴，就会有一次遭遇不可逆的永久丢数据！
3. **Copysets 的数学破局**：
   Copysets 论文指出了一个决定性真理：**丢失数据的根本原因，不是节点宕机，而是宕机节点恰好完整覆盖了某一个副本集合**。因此，系统应当在保证散布度 $S$ 不变（维持极速并行重建能力）的前提下，**将全网创建的副本集合总数最小化**。
   - 系统将 5000 个节点通过排列算法划分为固定、最小化的副本集合族（Copysets），使得每个节点恰好出现在 $S$ 个 Copyset 中。全网的副本组合总数被锁死为：
     $$\text{Copysets} = \frac{N \times S}{R} = \frac{5000 \times 10}{3} \approx 16,666$$
   - 任何新数据块写入时，只能严格从这 16,666 组登记在册的 Copyset 中挑选一组存放，严禁随意在全网自由组合。
   - 在完全相同的 50 节点并发宕机下，数据丢失概率从 19.1% 断崖式暴跌至 1.5% 左右，将数据耐受度直接提升了一个数量级！

### 为什么重要

1. **筑牢云原生存储的容灾底线**：
   在千台规模以上的云原生集群中，单机故障是常态，多机相关性并发故障是致命杀手。Copysets 极大压缩了故障集合与数据分布的几何交集，杜绝了多机并发宕机演变成业务不可逆灾难。

2. **解耦恢复速度与容灾安全的矛盾**：
   在 Copysets 问世之前，存储架构师普遍认为“极速修复”与“多机容灾”是不可调和的矛盾（散布越广修复越快，但死得越惨）。Copysets 证明了通过精巧的子集族划分，既能享受高散布度带来的全集群并发恢复带宽，又斩断了漫天随机排列组合带来的厄运。

3. **现代工业级存储的拓扑基石**：
   Copysets 的理论深刻影响了后续工业界存储系统的副本拓扑设计，包括 Google Colossus、Facebook Tectonic、Apache Hadoop 的 HDFS-9226 副本放置策略等。在超大规模存储架构中，结构化副本约束已经成为标配防线。

_隐喻对应表_

- 五千座皇家粮仓 → 拥有 $N = 5000$ 个存储节点的分布式存储集群
- 三只朱漆铁皮匣 → 三副本冗余容错机制（$R = 3$）
- 关口掷木樗散存 → 完全无约束随机副本放置（Random Replication）
- 全营仓房协助复原 → 维持高散布度（$S$）带来的快速并行节点修复
- 五十座粮仓同时遭焚 → 机架供电或交换机引发的 50 节点相关性并发故障（$f = 50$）
- 近两万组绝灭厄运 → 宕机节点组合出的失败三元组撞击数据分布
- 连环三匣令（结义之盟） → 结构化 Copysets 约束放置策略
- 钦定结伴额度 → 固定的散布度 $S = 10$ 与最小化的 Copyset 集合规模（$N \times S / R$）
</section>
<section class="en" markdown="1">
On the vast frontier plains of the Grand Yong Empire stood five thousand fortified imperial granaries.

Here rested the empire's strategic grain reserves, shielded against drought and war. By decree of the imperial court, an iron rule governed all provisions: every harvest cart arriving at the border had to divide its grain equally into three identical red-lacquered iron chests, each dispatched to a different granary. So long as a single granary among the three remained intact, the harvest could be restored to the very last grain. Only if all three storehouses harboring chests from the same harvest fell into ashes would the empire suffer catastrophic, irrecoverable data loss: the death of a ledger.

In earlier years, the chief stewards adhered to the venerable "Rule of the Casted Dice."

Whenever a grain wagon reached the frontier gate, the tollkeeper tossed carved wooden divination dice, randomly selecting three granary seals from across the plain. Year after year, tens of thousands of wagons scattered their seed across the landscape. The stewards congratulated themselves on their cleverness: across five thousand granaries, even if a stray bolt of lightning torched one storehouse, four thousand nine hundred and ninety-nine comrades stood ready. Every surviving granary needed only to carry out a single basket of grain to rebuild the fallen neighbor. In less than the burning of an incense stick, the destroyed granary was restored. The imperial revenue ministers marveled at this miraculous "all-empire recovery speed."

Yet the heavens are indifferent to human vanity.

One bitter winter, howling northern gales swept across the dry steppe. A rare seismic tremor cracked the earth, unleashing spontaneous subterranean fires that tore through the compound. Overnight, amidst the suffocating smoke and roaring winds, fifty granaries burned to the ground simultaneously!

Fifty granaries accounted for merely one percent of the empire's vast storehouses. The minister assumed that the risk of permanent grain loss would be astronomically small. But when the dust settled and the exhausted couriers presented the post-disaster audit, the pavilion plunged into petrified silence:

Across the strategic reserves, more than a dozen distinct harvest batches had been completely wiped out! All three copies had burned to ashes.

The minister collapsed behind his desk, trembling in disbelief: with four thousand nine hundred and fifty granaries standing completely untouched, how could a dozen harvests lose every single one of their three chests?

It was then that Minister Wen, the newly appointed Imperial Auditor of the Treasury, rode into camp and knelt upon the snow, drawing a staggering diagram with his ivory tally.

"The stewards only understood that scattering goods quickens recovery, yet remained blind to the mathematical abyss their dice were carving," Minister Wen sighed, pointing across the snow-blown plain. "Across five thousand granaries, every time you randomly chose three, you steadily populated nearly every combination under heaven! Among five thousand granaries, the possible triplets exceed twenty billion. To spread the work as evenly as possible, your dice wove hundreds of thousands of distinct three-granary bindings throughout our territory!"

"When fifty granaries crumbled in the inferno, those fifty ruins formed nearly twenty thousand distinct lethal triplets among themselves! Across our empire lay hundreds of thousands of randomly tangled tripwires. The moment those twenty thousand lethal combinations collided with your scattered stores, irrecoverable extinction became inevitable! Under your random dice, a fifty-granary blaze carries a greater than twenty percent chance of total ledger annihilation!"

Cold sweat drenched the heavy furs of the stewards. "If we do not toss the dice, must the same granaries remain bound forever? If one collapses, will the others not die of exhaustion during the rebuild?"

Minister Wen smiled gently and withdrew a sealed parchment titled *The Edict of the Copyset Leagues*.

"We still scatter, but we shall never again toss dice into the dark. We shall organize the five thousand granaries into a strictly registered family of sworn alliances—*Copysets*."

Minister Wen's method was masterfully disciplined:
No granary was permitted to pair haphazardly with all comrades across the empire. Instead, each granary was assigned a fixed "Scatter Width." For instance, Granary Alpha was authorized to form three-chest pacts with only nine designated peers. Across the entire plain, the five thousand granaries were partitioned into roughly sixteen thousand imperial alliances, recorded indelibly in the central ledger.

When fresh grain arrived at the gate, the gatekeeper was forbidden from casting dice; he had to select an alliance directly from the sixteen thousand registered leagues. No grain could ever be deposited into an unchartered triplet.

At first, the stewards were skeptical. But when midsummer arrived and lightning torched an isolated storehouse, its designated allies immediately opened their doors. Although fewer granaries shared the burden than in the days of total randomness, the reconstruction finished in just a few incense sticks—smooth, rapid, and orderly.

Then, in late autumn, a catastrophic sandstorm battered the frontier, collapsing fifty granaries under crushed timber and stone.

When the treasury auditors checked the rolls with bated breath, a true miracle emerged:
Across five thousand granaries and tens of thousands of wagons of grain, despite fifty storehouses suffering simultaneous ruin, **not a single harvest batch lost all three chests**. Every single imperial ledger remained intact!

Cheers echoed through the frontier pavilion. The stewards finally understood: indiscriminate randomness, though seemingly flexible, gambled the empire's life upon the merciless dice of combinatorial catastrophe; only structured, mathematically bounded alliances can forge an impregnable fortress when heaven and earth shatter.

---

By now you've probably recognized it: this is the celebrated **Copysets Replication Algorithm**, introduced by Asaf Cidon et al. (Stanford University) in their landmark USENIX ATC 2013 paper *Copysets: Reducing the Frequency of Data Loss in Cloud Storage*.

### What it is

**Copysets** is a replication placement algorithm designed for large-scale distributed storage systems (such as HDFS, Ceph, RAMCloud, Cassandra, and cloud object stores) to minimize the probability of catastrophic data loss caused by correlated or simultaneous multi-node failures.

In modern distributed storage, data objects (or blocks) are replicated across $R$ nodes (typically $R = 3$) for fault tolerance:
1. **The Trap of Random Replication**:
   Traditional systems (like default HDFS or Ceph CRUSH rules) place the $R$ replicas of each chunk on randomly selected nodes across the cluster. To optimize background recovery time and minimize recovery latency after a single-node failure, each node distributes its replicas across many distinct peers—a metric called **Scatter Width** ($S$). With random replication, the number of distinct node triplets holding data approaches:
   $$N \times \binom{S}{R-1}$$
   In a cluster of $N = 5000$ nodes with $S = 10$ and $R = 3$, random replication generates around $225,000$ distinct replication triplets out of the universe of possible triplets.
2. **The Correlated Failure Catastrophe**:
   In cloud data centers, simultaneous multi-node failures are routine: power distribution unit (PDU) outages, top-of-rack switch crashes, rolling kernel panic updates, or cooling failures frequently take down 1% to 2% of nodes simultaneously (e.g., $f = 50$ nodes dying at once).
   When $f$ nodes fail, they form $\binom{f}{R} = \binom{50}{3} = 19,600$ unique failing triplets. With random replication, because those $225,000$ data-holding triplets are scattered haphazardly across the cluster, the probability that *at least one* data triplet intersects the set of dead nodes is extremely high:
   $$P(\text{Data Loss}) \approx 1 - \left(1 - \frac{\text{Data Triplets}}{\text{Total Triplets}}\right)^{\binom{f}{R}} \approx 19.1\%$$
   Nearly **1 out of every 5** correlated failure events destroys data permanently!
3. **The Copyset Solution**:
   Copysets proves that data loss occurs if and only if all nodes in a data set's replication group fail simultaneously. Therefore, the goal is to **minimize the number of distinct node sets (copysets) created**, while holding the scatter width $S$ constant to preserve fast parallel recovery.
   - The cluster defines an exact, minimal family of subsets of size $R$, such that each node appears in at least $S$ copysets. The total number of copysets is reduced to just:
     $$\text{Copysets} = \frac{N \times S}{R} = \frac{5000 \times 10}{3} \approx 16,666$$
   - Every chunk or object must choose its replicas strictly from one of these pre-computed copysets, rather than picking nodes independently at random.
   - Under the same 50-node failure event, the probability of data loss plummets from ~19.1% down to ~1.5%—slashing data loss frequency by more than an order of magnitude without compromising single-node repair speed.

### Why it matters

1. **Defending Against Correlated Multi-Node Outages**:
   Power outages, rack-level failures, firmware bugs, and network partitioning do not kill servers one by one; they kill dozens of machines at the exact same second. Copysets drastically reduces the overlap between failure sets and data sets, preventing correlated outages from turning into permanent data corruption.

2. **Decoupling Repair Speed from Data Loss Risk**:
   Storage engineers previously believed there was a fundamental trade-off: high scatter width meant faster single-node recovery (many nodes pull missing blocks in parallel), but guaranteed high data loss during multi-node crashes. Copysets broke this trade-off by proving that structured permutation assignment achieves both: you maintain a high scatter width $S$ for fast parallel rebuilds, while eliminating the astronomical tail of dangerous replication combinations.

3. **Foundation for Cloud Storage Orchestration**:
   The insights from Copysets directly influenced replication topologies in Google Colossus, Facebook Tectonic, Apache Hadoop (HDFS-9226 Copyset Placement Policy), and Kubernetes volume topology orchestrators. Whenever a distributed storage cluster scales beyond hundreds of nodes, unconstrained random placement becomes an existential liability; structured copyset families are the mathematical shield.

_Metaphor mapping_

- Five thousand imperial granaries → Distributed storage cluster of $N = 5000$ storage nodes
- Three red-lacquered iron chests → Triplicate replication ($R = 3$ replicas per block)
- Casting wooden dice at the frontier gate → Unconstrained random replica placement (Random Replication)
- Fireplace help from all surviving granaries → High scatter width ($S$) for rapid parallel single-node recovery
- Fifty granaries burned simultaneously → Correlated multi-node failure event ($f = 50$ nodes down)
- Over twenty thousand combinations of doom → Number of failing triplets $\binom{f}{R}$ intersecting scattered data groups
- Order of the Three-Chest League (*Copysets*) → Structured Copysets placement policy
- Fixed kinship quota for each storehouse → Bounded scatter width $S = 10$ with minimal copyset cardinality ($N \times S / R$)
</section>
