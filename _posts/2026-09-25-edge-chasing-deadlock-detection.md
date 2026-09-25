---
layout: fable
title: "连环水闸与铜铃溯波 · The River Sluice and the Silver Bell"
title_zh: "连环水闸与铜铃溯波"
title_en: "The River Sluice and the Silver Bell"
concept: "Chandy-Misra-Haas Deadlock Detection"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-09-25-edge-chasing-deadlock-detection.jpg
youtube_id: "PB2fyYRwGyw"
---
<section class="zh" markdown="1">
九龙峡的梯级运河，是大梁王朝最为险峻的水陆咽喉。

两岸削壁千仞，江水自高峡奔涌而下，落差达百余丈。为了让往来南北的重载货船能够逆流翻山，工部依山势开凿了四座巨大的连环石闸——自下而上依次名为青龙闸、白虎闸、朱雀闸与玄武闸。

每座水闸皆由一名经验老到的闸长独立掌管。闸门厚达三尺，皆由生铁铸造；闸室两侧立着合抱粗的巨型绞盘与系船石桩。

为了防范山洪倒灌与船只碰撞，开山祖师在石壁上刻下了铁铸的行船律令：
**"闸闭舟入，系缆方可行绞；一舟未离前闸，后闸断不可启。"**

换言之，任何货船要穿过一级水闸，必须先用坚韧的棕缆系牢当前闸室的系船桩，再向前方的下一级水闸申请挂上牵引绞盘；唯有当整艘船的船头彻底越过前方的闸槛，后方闸室的缆绳才能完全解开。

百年以来，四座水闸各自为政，闸长们只需盯着自己眼前的一方水塘，运河秩序井然。

直到那个深秋大雾弥漫的清晨。

峡谷中白雾如絮，四艘满载货物的吃水大船几乎在同一时刻驶入了九龙峡各处：
青龙闸内，赵船主的青盐重驳庞大如山，已牢牢系紧了青龙闸的底桩，正昂首等待前方的白虎闸开启水门；
白虎闸内，孙管事的巨型红木排横贯水塘，正在等待朱雀闸的重力绞盘牵引上行；
朱雀闸内，钱员外的万石粮船吃水极深，正静静等待最顶层的玄武闸放空积水、腾出入闸泊位；
而玄武闸内，吴舵把子的生铁战船本已准备顺流下行，但山峡水急浪高，依令必须借助青龙闸底端设立的特大生铁稳桩系死尾缆，方敢开闸破浪！

大雾越来越浓。

青龙闸的闸长倚在铁栏杆上，望着紧闭的白虎闸水门，端着热茶自言自语："赵船主莫急，白虎闸里有木排挡着，等孙管事挪开了，咱们自然放行。"
白虎闸的闸工敲着烟锅："朱雀闸的绞盘还没空出来呢，着什么急？"
朱雀闸的掌舵人探头看着玄武闸的水位："顶上的战船还没下水，咱们只能候着。"
玄武闸的老舵工看着空荡荡的峡口："我的尾缆还没在青龙闸挂死，谁敢冒失开闸？"

一个时辰过去了。两个时辰过去了。

一整天过去了，峡谷里没有发生任何争执，更无半点撞船的喧嚣。没有铁缆崩断，没有闸门损坏。每一位船主都温和守礼地坐在船头烹茶，每一位闸长都兢兢业业地恪守着祖宗规矩。

江水在石壁间静静回旋，发出微弱的呜咽。

整座九龙连环大水闸，就在这绝对的规矩、绝对的平静与绝对的礼貌中，彻底僵死了。

没有任何一条船能够前进半寸，没有任何一座闸门能够开启缝隙。四艘重船与四座大闸，悄无声息地咬合成了一个首尾相连的死扣。

两岸码头堵塞的客货小舟排开三十里，消息报到郡衙，新上任的转运使大人勃然大怒。

转运使大人自诩精明果断，当即拍案推出了第一道雷霆军令——**"燃香斩缆法"**：
"自货船入闸之刻起，各闸前案头点燃一支三寸降真香！香尽而前闸未启者，视为怠工阻道！巡河兵丁即刻挥斧斩断其牵引缆绳，驱逐出闸，退回江口重排！"

令旗一下，峡谷内顿时人仰马翻。

次日正午，一艘运送两淮官银的万石重驳因船身过重、山水逆冲，绞盘提拉本就极其耗时，三寸细香燃尽之时，巨舰离出闸仅剩三丈之遥。巡河武士不由分说，数柄雪亮钢斧凌空劈下！
胳膊粗的棕缆应声崩断，巨舰在滔天白浪中如失控的脱缰野马倒卷向下游，撞碎民船三艘，银箱坠江无数，险些酿成惊天惨祸。

粗暴的焚香计时，看似立竿见影，实则盲人瞎马：它分不清谁是真的陷入了连环死锁，谁只是因水深船重而自然缓行。 innocent 的重载大船屡遭腰斩，峡谷水道非但未能疏通，反而怨声载道，人人自危。

碰了壁的转运使大人不肯认输，又在半山腰立起了一座巍峨的三层飞檐高楼——**"锁水总枢台"**。

他下令：四座水闸的每一名闸工，凡遇船只系桩、申请绞盘、等待开闸者，皆须立刻书写快报，命背插红旗的轻骑信使在栈道上飞马奔驰，送往总枢台。总枢台内设三十名文书，在两丈长的宣纸上实时绘制全峡船只位置与牵引关系图，由转运使亲自裁决谁该让路。

然而，栈道崎岖，山高雾锁。轻骑在崖壁间奔波换气，快报往往在路上便耽搁了半刻钟。
当总枢台的文书好不容易在图上勾画出"甲船等待乙船"时，乙船早已交接了缆绳；而当文书以为丙船正在航行时，丙船早已停下申请了绞盘。
总枢台案头的图纸堆积如山，朱墨狼藉，错漏百出；快骑踏碎了栈道木板，三十名书吏累得吐血，整座九龙峡却依然动辄瘫痪，总枢台本身反倒成了峡谷中最臃肿迟钝的瓶颈。

到了第三个月，大理寺司直、曾任南漕水部都料的沈老舟师奉旨按察九龙峡。

老舟师布衣芒鞋，不设帅案，亦不增置快骑。他来到青龙闸头，看着在闸室里苦等了半日的赵船主，从怀中摸出了一枚用细竹筒装着的铜质小风铃。

竹筒表面刻着一行工整的小字：`【发铃者：青龙赵船；承递者：青龙闸；受牒者：白虎孙排】`。

沈老舟师将竹筒递给赵船主，微笑道：
"莫要枯坐空等，亦无须向半山总阁报备。从今往后，各闸立一条极轻简的新规：凡入闸候行超过半炷香而不得开门者，便向你所等待的前船递出这枚'溯波铃'。

"前船若正在顺流航行，便随手将铃铛沉入江底，无需理会；但前船若也在苦苦等待下一级水闸，他便不得扣押此铃，必须在筒底添上自己的去向，顺着他所等待的牵引索，将铃铛递往更前方！"

赵船主依言照办。他将竹筒用飞爪绳镖凌空一掷，稳稳落在了白虎闸孙管事的红木排上。

孙管事捡起竹筒，他正因等待朱雀闸而烦躁，展开一看，见发铃者是后方的赵船，而自己正被朱雀闸钱员外所阻。孙管事毫不迟疑，在筒内信笺添上一笔，顺着牵引绞盘的缆线，将竹筒滑向了朱雀闸的粮船。

朱雀闸钱员外收到竹筒，见自己正等待玄武闸，又将竹筒顺水递给了玄武闸的吴舵把子。

玄武闸上，吴舵把子解开竹筒。他看着自己手中那根死死扣在最下游青龙闸底桩上的特大尾缆，目光落在了信笺抬头的朱砂小字上——
`【发铃者：青龙赵船】`。

吴舵把子心头猛然一震！他举目望向山脚下的青龙闸，跨越四级险滩，大雾深处，那艘正把尾缆死死锁在石桩上的青盐大驳，赫然正是发铃之人！

吴舵把子将竹筒系在一枚响箭上，搭弓拉弦，"嗖"的一声锐啸，长箭带着铜铃划破峡谷薄雾，精准地钉在了青龙闸赵船主的桅杆之上！

风铃在晨风中发出清脆的"丁零"脆响。

赵船主拔下响箭，展开竹筒里的信纸。
那一刹那，满船水手尽皆失色——信笺辗转四闸，上面依次盖着青龙、白虎、朱雀、玄武四道印信，而最终回到自己手中的，竟是自己在半刻钟前亲手发出的那一枚初始印信！

环扣合拢，无可抵赖。

无需惊动半山腰的总枢台，亦无需巡河武士挥斧乱砍。赵船主看着桅杆上的铜铃，哑然失笑：原来自己苦等前路开启，却浑然不知正是自己的船尾，锁死了最顶端战船的去路！

真相大白，死结瞬解。
赵船主当机立断，命水手松开底桩缆绳，将青盐驳暂时退出半个身位，泊入青龙闸旁的避险回水湾。
刹那间，玄武闸的尾缆骤然解脱，吴舵把子的战船顺流轰然出闸；朱雀粮船顺势移入玄武闸，白虎木排破浪挺进朱雀闸，赵船主的青盐驳重新驶入闸门……沉寂了整整两日的九龙峡，顿时千帆竞发，两岸号子震天动地，汹涌的江水在四级石闸间奔流无阻！

——到这儿你大概已经认出来了：这就是分布式系统与数据库并发控制中声名显赫的核心算法——*Chandy-Misra-Haas Algorithm*（**Chandy-Misra-Haas 分布式死锁检测算法 / 边缘追踪算法 Edge Chasing**）。

### 这是什么

在分布式数据库（如 Google Spanner、CockroachDB、Vitess）、多分片事务系统与跨节点资源调度中，事务往往需要跨越多个独立的物理节点获取互斥锁（*Exclusive Locks / 2PL*）。

当多个事务并发执行并相互争夺资源时，极易产生**分布式死锁**（*Distributed Deadlock*）：
事务 $T_1$ 在节点 A 上持有资源 $R_1$，并等待节点 B 上的资源 $R_2$（被 $T_2$ 持有）；
事务 $T_2$ 在节点 B 上持有 $R_2$，并在等待节点 C 上的资源 $R_3$（被 $T_3$ 持有）；
事务 $T_3$ 在节点 C 上持有 $R_3$，却反过来等待节点 A 上的资源 $R_1$（被 $T_1$ 持有）。

在此场景下，系统形成了一条跨越物理网络拓扑的**环形等待链**（*Wait-For Graph Cycle*）：
$$T_1 \to T_2 \to T_3 \to T_1$$

没有任何一个单独的节点拥有全局的视角。每个节点只能看到局部的等待边（例如节点 A 只知道 $T_1 \to T_2$），所有事务都在完全合规、静默地等待，若无外力介入，系统将陷入永久停滞。

传统的解决手段往往存在巨大缺陷：
1. **静态超时机制（Timeout / Lock Wait Timeout）**：设定一个死板的时间上限，超时未拿到锁便强制回滚事务。如同"燃香斩缆"，它无法区分究竟是网络抖动、大事务执行耗时较长，还是真的发生了死锁。大事务频繁被冤杀（*False Aborts*），造成昂贵的重试风暴与资源虚耗。
2. **中心化死锁检测（Centralized Deadlock Detection）**：设置一个全局协调中心，所有节点定期将本地的等待关系图（*Local Wait-For Graph*）通过 RPC 上报汇总。如同"锁水总枢台"，在大规模分布式集群中，网络延迟导致汇总上来的全局拓扑充满幻影与延迟（*Phantom Deadlocks*），且中心节点极易沦为通信瓶颈与单点故障（*SPOF*）。

计算机科学家 K. Mani Chandy、Jayadev Misra 与 Laura M. Haas 于 1983 年发表了里程碑式的论文，提出了**边缘追踪算法（Edge Chasing / Probe Computation）**，实现了完全去中心化、零全局快照的分布式死锁精准检测：

1. **探测报文（Probe Message）**：
   算法定义了一个结构极轻简的探测消息三元组：
   $$\text{Probe}(i, j, k)$$
   其中 $i$ 是发起死锁检测的初始事务（*Initiator*），$j$ 是当前发送报文的事务或节点（*Sender*），$k$ 是接收报文并持有后续锁的事务或节点（*Receiver*）。
2. **边缘追踪传播逻辑（Edge Chasing Rule）**：
   - **发起**：当事务 $T_i$ 阻塞在 $T_j$ 持有的锁上，且阻塞时长超过了合理的怀疑等待窗口（避免为微秒级的正常锁争用浪费网络资源），$T_i$ 便向 $T_j$ 发送初始探测报文 $\text{Probe}(i, i, j)$。
   - **转发**：当节点或事务 $T_k$ 收到探测报文 $\text{Probe}(i, j, k)$ 时：
     - 若 $T_k$ 当前正在正常执行（活跃状态，未阻塞），说明等待链在它这里中断，并非死锁，直接将该探测报文**就地丢弃**；
     - 若 $T_k$ 此时同样处于阻塞状态，正在等待事务集合 $\{T_m\}$ 所持有的锁，则节点顺着局部的等待边（*Wait-For Edges*），将报文更新为 $\text{Probe}(i, k, m)$ 并转发给所有的 $\{T_m\}$。
3. **环路闭合与裁决（Deadlock Confirmation & Resolution）**：
   探测报文沿着跨节点的依赖边在网络中穿梭。如果有一天，由事务 $T_i$ 发起的探测报文 $\text{Probe}(i, *, i)$ 兜兜转转，最终又被送回到了事务 $T_i$ 本身——
   **这意味着依赖图中绝对存在一条自 $T_i$ 出发、最终又回到 $T_i$ 的闭合有向环路！分布式死锁得到数学意义上的严格确证！**
   此时，系统无需任何中心协调器，只需按照既定策略（如回滚代价最低的事务、年轻事务让路年老事务，或由发起者 $T_i$ 主动牺牲）选择一个牺牲者（*Victim*）将其回滚，即可精准切断死锁环路，释放被占用的锁资源，让整个分布式集群瞬间恢复流通。

### 为什么重要

*Chandy-Misra-Haas* 边缘追踪算法是现代分布式数据库与分布式协调服务实现高吞吐、强一致事务处理的底座基石：

1. **绝对的去中心化与零单点瓶颈（No Central Coordinator）**：
   算法完全基于点对点的局域消息推进。在没有死锁发生时，如果前置事务正常提交释放了锁，探测报文便自然随风而逝；仅在真实存在环状死扣时，报文才会完整绕回。没有中央节点，没有全局锁表的汇总与同步开销，集群规模可以水平线性扩展至上万节点。
2. **彻底消灭"冤杀"与重试雪崩（Zero False Aborts）**：
   与基于超时粗暴回滚的机制相比，边缘追踪算法是**确证性（Deterministic & Sound）**的。只要收到自己发出的探测报文，就证明百分之百存在死锁环路。跑了数小时的大型分析事务或批处理任务不会再因为网络偶发拥堵被无情斩断，极大地保障了长事务系统的稳定性。
3. **极低的网络通信代价（Minimal Message Overhead）**：
   算法只在"确定发生等待"的有向边上定向传播消息。对于 $N$ 个节点构成的死锁环，检测到死锁所需的消息数量仅正比于环的长度（$O(N)$），而非全集群广播的 $O(N^2)$。探测报文体积微小，且仅由处于闲置阻塞中的事务按需触发，对正常的高频事务读写几乎零侵入。

从跨分片分布式 SQL 引擎的锁冲突排查，到微服务长事务框架中的资源防死锁设计，那枚在茫茫峡谷中顺着铁缆悄然漂流的轻灵铜铃，始终在看不见全局拓扑的迷雾网络中，以最轻盈的足迹化解着最致命的锁死僵局。

_隐喻对应表_

- 九龙峡连环石闸与狭窄船塘 → 物理分片节点与分布式系统中的独立数据分区（*distributed partitions / shards*）
- 赵船、孙排、钱船、吴船等各色载重货船 → 并发运行并申请资源的分布式事务（*distributed transactions*）
- 闸室内的系船石桩与厚重铁铸水门 → 事务所持有的互斥锁与临界区资源（*exclusive locks / critical sections*）
- 各闸长恪守祖规导致四船首尾咬合的无声僵局 → 跨节点事务相互等待构成的分布式死锁环（*distributed deadlock in global wait-for graph*）
- 转运使大人点燃细香斩断缆绳的粗暴举措 → 基于静态超时强行回滚事务的容错机制（*lock wait timeout & false aborts*）
- 半山腰设立飞檐高楼、快骑飞奔的锁水总枢台 → 汇总全局依赖图的中心化死锁检测器（*centralized deadlock detector & SPOF*）
- 竹筒信笺内标记的发铃者、承递者与受牒者 → 探测报文三元组 $\text{Probe}(i, j, k)$（*probe message tuple: initiator, sender, receiver*）
- 若前船正在航行便随手将风铃沉入江底 → 遇到活跃执行事务时探测报文自动终止丢弃（*probe discarded at active non-blocked transaction*）
- 若前船受阻便顺着牵引缆将竹筒递往前方的下一艘船 → 探测报文顺着局部等待边向前推进（*forwarding probe along outgoing wait-for edges*）
- 响箭穿透晨雾将铜铃送回赵船主桅杆上的清脆丁零 → 初始事务收到自己发起的探测报文确证死锁环路闭合（*probe loop closure confirming cyclic dependency*）
- 赵船主主动解开底桩缆绳、退入避险湾化解全峡危机 → 选定牺牲事务主动中止回滚以打破死锁（*victim selection & transaction abort to resolve deadlock*）
</section>
<section class="en" markdown="1">
The tiered canal of Nine Dragon Gorge was the most perilous water passage in the Great Liang Empire.

On either side, sheer granite cliffs pierced the clouds, while the river cascaded through the gorge with a drop of more than a hundred fathoms. To allow heavily laden cargo barges traveling between the northern salt flats and the southern tea hills to climb against the roaring torrent, the Ministry of Works had carved four colossal stone locks out of the living mountain rock—named from lowest to highest: Azure Dragon Lock, White Tiger Lock, Vermilion Bird Lock, and Black Tortoise Lock.

Each lock was commanded independently by a veteran lockmaster. The sluice gates were three feet thick, cast from solid pig iron; on either side of the lock basins stood colossal timber winches and stone mooring bollards as thick as ancient trees.

To prevent catastrophic flash floods and barge collisions, the founding masters had chiseled an iron law deep into the canyon wall:
**"Gate closed, vessel berthed, cables tied before winches turn; no forward gate shall lift until the vessel astern has cleared."**

In other words, for any cargo vessel to pass through a lock, it had to first lash its thick hemp hawsers securely to the bollards of its current basin, then request the towing cables from the winches of the next lock ahead. Only when the bow of the vessel had crossed entirely past the forward threshold could the mooring ropes in the basin behind be uncoupled.

For a hundred years, the four locks operated under strict local sovereignty. Each lockmaster needed only to watch the water in his own stone basin, and the river ran without flaw.

Until a morning when the autumn mist swallowed the gorge whole.

Dense river fog hung like wet fleece between the cliffs. Four heavily laden vessels entered different reaches of Nine Dragon Gorge almost at the exact same hour:
Inside Azure Dragon Lock at the bottom, Master Zhao's salt barge, massive as an iron fortress, had lashed its stern cables to the bedrock bollards and rested its bow against the current, waiting for White Tiger Lock ahead to raise its iron gate;
Inside White Tiger Lock, Master Sun's colossal timber raft spanned the entire basin, waiting for the heavy winches of Vermilion Bird Lock to hoist it uphill;
Inside Vermilion Bird Lock, Merchant Qian's grain barge, laden with ten thousand bushels of wheat, sat low in the churning pool, quietly waiting for Black Tortoise Lock at the summit to empty its water and vacate a berthing slot;
And inside Black Tortoise Lock at the mountain crest, Helmsman Wu's iron-clad patrol galley was prepared to descend the rapids, but by the laws of the gorge, navigating the violent descent required securing a heavy stern stabilization cable to the primary iron anchor bollard located at Azure Dragon Lock far below before its upper gates dared release the mountain waters!

The fog thickened.

The lockmaster of Azure Dragon Lock leaned on the iron balustrade, peering at the closed gate of White Tiger Lock through the steam of his tea. "Patience, Master Zhao," he muttered. "Sun's timber raft blocks White Tiger Lock. Once he moves, we open."
At White Tiger Lock, the operator tapped his pipe against the stone. "Vermilion Bird hasn't freed its winches yet. What is the rush?"
At Vermilion Bird Lock, the pilot leaned over the parapet toward Black Tortoise Lock: "The summit galley hasn't dropped down. We can only wait."
And at Black Tortoise Lock, the old helmsman stared into the misty gorge: "My stern cable isn't hitched to the bollard at Azure Dragon Lock. Who would dare run the rapids unanchored?"

An hour passed. Two hours passed.

A full day drifted by. Not a single argument broke out across the canyon. No hulls scraped; no timber splintered; no cables snapped. Every boatmaster sat courteously beneath his canvas awning sipping tea; every lockmaster rigorously obeyed the ancestral statutes.

The mountain river lapped gently against the dark granite walls, whispering in the quiet chill.

Yet the entire chain of four giant locks was utterly, completely, irrevocably frozen.

Not a single vessel could advance an inch. Not a single iron gate could open a crack. Four great ships and four massive locks had silently, politely locked themselves into an unbreakable circular knot.

Within days, merchant boats were backed up for thirty leagues along the riverbanks. When word reached the provincial governor, the newly appointed Transport Commissioner was incandescent with rage.

Priding himself on uncompromising decisiveness, the commissioner slammed his gavel and issued his first emergency decree—**"The Incense Severance Edict"**:
"From the moment a vessel enters a lock, an incense stick shall be lit upon the lockmaster's desk! If the incense burns to ash before the forward gate lifts, the ship shall be deemed obstructionist! River guards shall instantly sever its towlines with broadaxes, expelling it back to the river mouth to restart its journey!"

Pandemonium ensued.

The very next afternoon, a massive imperial silver transport barge, fighting the fierce alpine current with thousands of stone of bullion, was slowly being winched against the roaring cascade. Just as the three-inch incense stick crumbled into gray ash, the silver vessel was a mere three fathoms from the threshold. The river guards showed no mercy. Broadaxes flashed in the sun!
Hawsers as thick as a man's thigh snapped like dry twigs. The great vessel was cast adrift in the boiling foam, careening down the rapids like a wild beast, smashing three fishing sampans and dumping countless chests of silver into the abyss.

Crude timeout timers were blind: they could not distinguish between a fatal circular deadlock and an honest, heavy ship moving naturally slowly against an alpine current. Innocent heavy carriers were repeatedly cut adrift; rather than clearing the waterway, the river was engulfed in panic and fury.

Unwilling to admit defeat, the commissioner erected a towering three-story pavilion on the cliffside—**"The Grand River Sluice Registry"**.

He decreed that whenever a ship moored, requested a winch, or waited for a gate, the lockmasters had to write a dispatch and dispatch a red-flag courier on horseback galloping along the precipitous plank roads to the pavilion. Inside, thirty scribes worked on a thirty-foot scroll, painting real-time maps of ship positions and winch dependencies so the commissioner himself could judge who should yield.

Yet mountain paths were treacherous, and fog swallowed the cliffs. By the time a breathless courier reached the summit, fifteen minutes had slipped away.
When the scribes finally brushed a stroke showing "Ship A waits for Ship B," Ship B had already transferred its cable; when they assumed Ship C was sailing freely, Ship C had already docked to wait for a winch.
Parchment stacked to the rafters, stained with frantic ink. Couriers broke horse hooves against the mountain stone; thirty clerks coughed blood from exhaustion; yet Nine Dragon Gorge froze just as frequently as before. The Grand Registry had become the single most swollen, fragile bottleneck on the river.

In the third month, Elder Boatmaster Shen, retired Chief Surveyor of the Southern Canals, arrived by imperial commission.

Wearing a simple coarse robe and straw sandals, Master Shen set up no commanding pavilion and summoned no galloping messengers. He walked directly to Azure Dragon Lock, stood beside Master Zhao's stalled salt barge, and drew from his sleeve a small brass bell sealed inside a light bamboo cylinder.

Carved upon the bamboo was a clean inscription: `[Initiator: Zhao Barge; Sender: Azure Dragon Lock; Receiver: White Tiger Sun Raft]`.

Elder Shen handed the cylinder to Master Zhao and smiled:
"Do not sit in silent despair, nor send couriers to the mountain heights. From this day forward, let there be one lightweight rule: Whenever you have waited beyond a modest span of patience without the forward gate lifting, cast this 'Ripple-Chasing Bell' to the ship you are waiting upon.

"If that forward ship is actively sailing, let them drop the bell into the riverbed and pay it no mind. But if that ship is likewise blocked and waiting on another, they must not hoard the bell; they must append their own heading and forward the cylinder along the very cable of the vessel they await!"

Master Zhao obeyed. He tied the bamboo cylinder to a throwing grapple and hurled it across the churning foam, landing squarely upon Master Sun's timber raft at White Tiger Lock.

Master Sun picked up the cylinder. Brooding over his delay at Vermilion Bird Lock, he slid out the parchment, saw that the inquiry came from Zhao astern, and noted that he was waiting on Merchant Qian ahead. Sun immediately dipped his brush, added his entry, and slid the cylinder along the taut winch cable to the grain barge at Vermilion Bird Lock.

Merchant Qian received it, saw he was waiting on Black Tortoise Lock, and cast it forward to Helmsman Wu's patrol galley.

At Black Tortoise Lock, Helmsman Wu caught the cylinder. He looked down at the massive stern cable in his hands, which stretched miles down the canyon to the bedrock mooring bollard of Azure Dragon Lock. Then his eyes fell upon the red vermillion heading at the top of the parchment:
`[Initiator: Zhao Barge]`.

Helmsman Wu gasped. He looked down through the misty chasm. Far below in the deep gorge, the great salt barge holding that very bedrock bollard was none other than the ship that had sent this bell!

Wu lashed the cylinder to a whistling signal arrow, drew his bowstring to his ear, and let fly. With a piercing shriek, the arrow cut through the canyon mist, thudding with precision into the cedar mast of Master Zhao's salt barge at Azure Dragon Lock!

The brass bell swung in the alpine breeze, chiming with a crisp, unmistakable ring.

Master Zhao pried the arrow loose and unrolled the parchment.
Every sailor on the deck stood frozen in astonishment: the parchment had traversed Azure Dragon, White Tiger, Vermilion Bird, and Black Tortoise, stamped by every lock in sequence—and the token that had returned to his fingers was his very own seal!

The loop had closed. The proof was absolute.

No mountain pavilion had been consulted; no broadaxes had severed lines in haste. Master Zhao looked at the chiming bell and burst into hearty laughter: all this time, while cursing the stubborn gates ahead, he had never realized that his own heavy stern cable was the very anchor pinning the summit galley in place!

With the circle laid bare, the deadlock dissolved in minutes.
Master Zhao immediately ordered his crew to slacken the bollard line, easing his salt barge back into a sheltered eddy beside the lock.
Instantly, the tension on Black Tortoise Lock's stern cable vanished. Helmsman Wu's patrol galley plunged through the upper gates; Qian's grain barge glided into Black Tortoise Lock; Sun's timber raft surged into Vermilion Bird Lock; and Zhao's salt barge was pulled smoothly into the cleared basin. The gorge, silent for days, erupted with the roar of cataracts, the creak of winches, and the triumphant songs of a thousand boatmen as the river rushed into boundless life!

—By now you've probably recognized it: this is the celebrated foundational algorithm in distributed transaction processing and concurrency control—the *Chandy-Misra-Haas Algorithm* (**Chandy-Misra-Haas Distributed Deadlock Detection / Edge Chasing**).

### What it is

In distributed databases (such as Google Spanner, CockroachDB, and Vitess), multi-shard transaction processors, and distributed schedulers, transactions frequently acquire exclusive locks (*2PL / Two-Phase Locking*) across independent physical nodes.

When multiple concurrent transactions compete for distributed resources, **distributed deadlocks** become inevitable:
Transaction $T_1$ on Node A holds resource $R_1$ and waits for resource $R_2$ on Node B (held by $T_2$);
Transaction $T_2$ on Node B holds resource $R_2$ and waits for resource $R_3$ on Node C (held by $T_3$);
Transaction $T_3$ on Node C holds resource $R_3$ and in turn waits for resource $R_1$ on Node A (held by $T_1$).

Here, the system forms a cyclic dependency chain across the physical network:
$$T_1 \to T_2 \to T_3 \to T_1$$

No single node possesses a global view of the **Wait-For Graph (WFG)**. Each node only sees its local slice (for instance, Node A only knows that $T_1$ waits for $T_2$). Every transaction waits politely and legitimately; without external intervention, the distributed system freezes permanently.

Traditional remedies suffer from severe operational flaws:
1. **Static Lock Wait Timeouts**: Setting a hardcoded deadline where transactions that fail to acquire locks are summarily aborted. Like the "Incense Severance Edict," it cannot tell whether a delay is caused by temporary network jitter, an expensive analytical batch, or a true circular deadlock. Legitimate long transactions are repeatedly slaughtered (*false aborts*), creating catastrophic retry storms.
2. **Centralized Deadlock Detection**: Directing every node to periodically ship its local dependency graph via RPC to a central coordinator. Like the "Grand River Sluice Registry," network latency causes the centralized graph to suffer from stale anomalies and "phantom deadlocks," while the central coordinator becomes a severe scalability bottleneck and a Single Point of Failure (*SPOF*).

In 1983, computer scientists K. Mani Chandy, Jayadev Misra, and Laura M. Haas published their landmark paper introducing **Edge Chasing (Probe Computation)**, achieving completely decentralized distributed deadlock detection without global snapshots:

1. **Probe Messages**:
   The algorithm defines a lightweight probe message triplet:
   $$\text{Probe}(i, j, k)$$
   where $i$ is the initial transaction that initiated the deadlock detection (*Initiator*), $j$ is the transaction/node transmitting the probe (*Sender*), and $k$ is the blocked-on transaction/node (*Receiver*).
2. **Edge Chasing Propagation Rules**:
   - **Initiation**: When transaction $T_i$ blocks on a lock held by $T_j$ and remains blocked beyond a reasonable grace period (preventing probe spam for sub-millisecond lock contention), $T_i$ generates and sends an initial probe $\text{Probe}(i, i, j)$ along the wait-for edge.
   - **Forwarding**: When a transaction or node $T_k$ receives $\text{Probe}(i, j, k)$:
     - If $T_k$ is actively executing (not blocked), the wait chain is broken. There is no deadlock; the probe is **discarded immediately**.
     - If $T_k$ is also blocked waiting for locks held by a set of transactions $\{T_m\}$, the node updates the probe to $\text{Probe}(i, k, m)$ and forwards it along all outgoing local wait-for edges to $\{T_m\}$.
3. **Loop Closure and Resolution**:
   The probe traverses physical nodes strictly along active dependency edges. If a probe originating from transaction $T_i$—namely $\text{Probe}(i, *, i)$—eventually circles back and arrives at $T_i$ itself:
   **A directed cycle in the global wait-for graph has been mathematically proven! A distributed deadlock exists!**
   Without any central coordinator, the system deterministically selects a victim transaction (e.g., the youngest transaction, lowest-cost query, or the initiator $T_i$) and aborts it. Breaking this single edge releases the contested locks, and the entire distributed cluster resumes progress instantaneously.

### Why it matters

The *Chandy-Misra-Haas* edge-chasing algorithm is a foundational pillar of modern distributed relational databases and consensus engines:

1. **Complete Decentralization and Zero Single Point of Failure**:
   The algorithm runs purely through point-to-point peer messages along dependency paths. When locks are released normally, probe messages vanish naturally into thin air; only true circular knots cause probes to loop back. Without a central lock coordinator, the system scales horizontally to thousands of nodes without coordination bottlenecks.
2. **Elimination of False Aborts and Retry Avalanches**:
   Unlike crude timeout mechanisms, edge chasing is **deterministic and sound**. A loop closure proves beyond doubt that a deadlock exists. Long-running analytical batches and heavy data migrations are never killed needlessly due to temporary network blips, eliminating wasted work and cascading retry avalanches.
3. **Minimal Network Overhead**:
   Probes travel only along edges where active waiting occurs. In a cycle of $N$ transactions, finding the deadlock requires only $O(N)$ messages, avoiding $O(N^2)$ global broadcasts. Probes are tiny metadata packets, dispatched only by blocked processes on demand, imposing virtually zero overhead on high-throughput OLTP workloads.

From multi-shard distributed SQL query planners to microservice saga orchestration, that nimble brass bell traveling along the mooring lines continues to dissolve the most treacherous circular knots in distributed systems with mathematical elegance and minimal footprint.

_Metaphor mapping_

- Tiered stone locks and narrow basins of Nine Dragon Gorge → Physical database shards and distributed partitions (*distributed partitions / shards*)
- Merchant vessels (Zhao's salt barge, Sun's timber raft, Qian's grain ship, Wu's galley) → Concurrent distributed transactions competing for resources (*distributed transactions*)
- Mooring bollards and iron sluice gates in each basin → Exclusive locks and critical section resources held by transactions (*exclusive locks / critical sections*)
- Strict local statutes locking all four ships into silent paralysis → Distributed deadlock forming a circular wait in the global wait-for graph (*distributed deadlock in global wait-for graph*)
- Transport Commissioner's crude Incense Severance Edict chopping cables → Hardcoded lock wait timeouts that falsely abort innocent heavy transactions (*lock wait timeouts & false aborts*)
- Towering cliffside Grand River Sluice Registry with galloping couriers → Centralized deadlock detector suffering from stale graphs and coordinator bottlenecks (*centralized deadlock detector & SPOF*)
- Sealed bamboo cylinder recording Initiator, Sender, and Receiver → Probe message triplet $\text{Probe}(i, j, k)$ (*probe message tuple: initiator, sender, receiver*)
- Dropping the bell in the riverbed if the forward ship is actively sailing → Immediate probe termination when reaching an active non-blocked transaction (*probe discarded at active non-blocked transaction*)
- Passing the bamboo cylinder along the forward towline if blocked → Forwarding probe messages along outgoing wait-for edges (*forwarding probe along wait-for edges*)
- Whistling arrow returning Zhao's own bell with a crisp chime → Probe looping back to initiator $\text{Probe}(i, *, i)$ proving cycle closure (*probe loop closure confirming cyclic dependency*)
- Zhao slacking his stern cable to unfreeze the gorge → Deterministic victim selection and transaction rollback to break the deadlock (*victim selection & transaction abort to resolve deadlock*)
</section>
