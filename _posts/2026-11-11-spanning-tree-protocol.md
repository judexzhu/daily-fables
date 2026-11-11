---
layout: fable
title: "红布条下的湖 · The Lake Beneath the Red Cloth"
title_zh: "红布条下的湖"
title_en: "The Lake Beneath the Red Cloth"
concept: "Spanning Tree Protocol (STP)"
tags: [networking, distributed-systems]
illustration: /assets/art/2026-11-11-spanning-tree-protocol.jpg
youtube_id: "ICS-EPG0l94"
---
<section class="zh" markdown="1">
柳湾九村环着一片湖。九村之间的路不是水路，是石堤——宽的石堤能过一辆牛车，窄的只容两人错身。

石堤上跑的是**信童**。柳湾的规矩很老，也很简洁：**一个村子收到一封通告，抄一份，凡是来路以外的每条堤，都送出去。** 那封通告抄得清清楚楚，还在信封背面画了村子收到它的时辰。

早年的堤少——一条主堤串起五村，两条支堤各通两村，形状就是一棵树，从根到梢，一步一条，没有第二条路。这个规矩用了六十年，没出过事。

后来九村凑钱，修了环湖堤，又从中间横插了三道捷径，为的是赶集不用绕。路从一棵树，变成了一张网。修堤那年，谁也没多看这个变化一眼。

那年谷雨，第一封通告是"开市"。第一天没事。第二天有事：通告沿着环湖堤跑了一整圈，回到最初发出它的那个村口。村口的老门房接过抄件，按六十年的规矩办——抄一份，来路以外的每条堤都送出去。他没认出这是自己昨天早上送走的那封。

**同一封通告，现在有两份在路上跑。**

第三圈四份。第四圈八份。到第四十圈，十九条堤上双向全是信童，怀里抱着同一张纸，迎面撞上，谁也不让。村子越多，抄得越慢；抄得越慢，一份抄件占路越久；占路越久，路上的抄件越多。

桥头的老周后来总结了一句话，说得比谁都准：

**问题不在信太多，问题在信封上没有"走过九村就作废"这一行字。**

水路文书都有这一行——商号的札子上明写着"过三驿不达则毁"，船家一看就知道该烧了。石堤上的信没有。柳湾的规矩是"送到就算"，不是"送到为止"。**一份抄件可以永远在环上转，而且每转一圈就多一倍。**

第七天，湖东的粮船靠不了岸。不是堤堵了，是**码头承认不了货单**——账房要等"开市"通告落定才敢收粮，而通告永远在路上。九村的米价一天涨三回。柳湾的灯语也传不出去，因为灯语也是信童传的。

**第一个办法是限速。**

族长下令：每条堤上最多同时跑四个信童，多的一律拦住。

结果很怪：湖面安静了半刻钟，然后更糟。因为跑得最凶的那几封正好被放进了堤里，而正经的货单排在堤外，一动不能动。**限速只保证了路不塌，它一个字都没说谁该上路。**

**第二个办法，是老族长拿着一根刻了刻度的竹尺，坐在湖西老槐树下拿出来的。**

**一、定一个宗主。** 九村各在门楣上刻一个**名号**：先比门楣上那个数字，数字一样再比名号本身（比如"槐树村"排在"柳树村"前头，因为槐的顺序在前）。**名号最小的那个村子是宗主。** 它未必是最强的，它只是名字最小。这条规矩妙在**不需要开会**：你只要报得出你的名号和邻居的名号，你自己就能算出谁是宗主。

**二、量距离，但只跟自己邻居讲话。** 竹尺给每段堤一个**脚价**：堤越窄越绕，价越高；堤越宽越直，价越低。一个村子到宗主的"路程价" = 最近那条通往宗主方向的上游堤的价 + 上游村子报出来的价。**每个村子只需要报一个数给自己的邻居，也只需要听邻居报的数。** 没有人去把全湖走一遍。

**三、每个村子只留一条归路。** 从通往宗主的那些路里，挑**脚价最低**的一条，敞着走人。价一样时比上游村子的名号，再一样时比那一段堤的门牌号。一个村子**只有一条**归路。

**四、两村之间的那段堤，只认一个当值的村子。** 谁离宗主更近（路程价更低，一样则比名号），谁负责把这段堤上的信往外送，它的堤口敞着；**另一头的堤口，挂红布。**

**五、挂了红布的堤，人不能走，耳朵要留着。** 红布后面照旧站一个听差的，一刻不停地听宗主那边传过来的心跳木牌——宗主每两拍敲一次。他不送信。但他知道那条堤是死的还是活的。**柳湾九村的命，就悬在这件看起来最没用的事上。**

**六、等二十拍才认死，认死了还要先听、再认。** 听差连着二十拍听不见心跳，才判上游断了，那条归路作废；然后从挂了红布的路里挑最好的一条，改作归路。改之前还有两道：先"听"十五拍——什么都别送，只听两边的名号和价；再"认"十五拍——开始记路，先别送信；十五拍之后才放信童上堤。看着啰嗦，这是为了让全湖换路的时候**不会一边把旧账翻掉、一边又把新账用错**。

十九条堤，敞着九条，挂红布十条。一封信从湖西到湖东，只走一条不重复的路；抄件的数量从"每圈翻一倍"变成"每村一份"。

**第三年春天，出了第二件事，比第一件更值得记。**

湖东三村嫌归路绕远——明明有一条直堤，走过去三里，绕归路要十一里。他们说服了老族长，试着把那条红布撤开半天。

**半刻钟。** 湖上的信童翻了三倍，两个村口的门房当场晕了。红布当天挂回去，从此没人再提。

九村后来懂了那句谁也没说出口的话：**红布不是浪费，红布是湖还活着的唯一条件。**

**第四年夏汛，湖心那道主堤塌了。**

半个湖接不上宗主。而更麻烦的事在同一刻发生：各村的"认路账"全错了——账上写着"寄湖东渔行的信走北堤"，可北堤那头已经没人了。信童抱着信跑到断口，折回，再跑，路上还照旧被别的信童拦住问路。

宗主的应对很省力：派出**催烧**的信童一路快跑，让每个村子把旧账烧得比平时快得多——平时账记三个月才擦，这几天二十天就擦。误投的信在半刻钟内就停了。

而这一次，全湖重新走通信只在**一拍**之内，不是以前的二十拍加十五拍加十五拍。

为什么？因为挂了红布那十条堤后面的听差，早就在跟对面的村子**对着话**——两边都留着上一轮的名号和价。归路一断，听差直接喊："我接。"对面立刻答："你接。"不用等"听十五拍、认十五拍"，也不用等二十拍判死。旧规矩是**死等**，新规矩是**先约好**。柳湾人后来把这套改法叫"对答改路"。

**第五年，出了两桩怪事，被记进了族规。**

第一桩：湖东新搬来一户大户，在自家门楣上刻了个比现任宗主还小的数字，想让全湖的信都绕到他家过一遍，好抽成。族规添了一条：**从村外大路进来的方向，凡报出的名号比现任宗主还小的，一律不理，并且把这个口封掉。** 宗主是你算出来的，不是你自己喊出来的。

第二桩：湖边一户人家院里自己绕成了一个圈——他买了两个院门，两边都通到正屋，正屋的抄件往两个门都送。院门口一旦收到心跳木牌，说明这个口本来不该有木牌。族规又添一条：**通到人家院子的门口不算堤；院门口收到木牌，立刻插门并喊人来查。** 别让一户人家把全湖拖下水。

到这儿，你大概已经认出来了：这就是**生成树协议**（*Spanning Tree Protocol*，*STP*）。

### 这是什么

先说清它要挡的那个东西是什么。

以太网的转发是**看门牌就送**：交换机收到一帧，查目的 MAC 在哪个口，从那个口送出去；查不到，就从**除了来路以外的所有口**送出去（*unknown unicast flooding*）。广播帧更是无条件往所有口送。

这套机制的全部安全感，建立在"路是一棵树"的假设上。而二层**没有 TTL**——IP 包头里有"跳数用完就丢"的字段，以太网帧里没有。**一个帧进了环，就永远不会死。** 它只会绕着环越跑越多：每个交换机对一个广播帧只复制一份，但十个交换机首尾相连，就是十份在绕；而未知单播的洪泛再叠上去，一圈之内就能把链路跑到线速。

三条后果同时发生：**带宽被吃光**；**每个交换机的 MAC 表都在疯狂翻转**（同一个源 MAC 从两个口交替出现），于是连本该精确转发的单播也被洪泛；**交换机的 CPU 被中断打满**，连管理面都连不上——你连上去改配置的通道，也是那条被打死的链路。

**STP（IEEE 802.1D）做的事极其干脆：它把有环的网，算成一棵树，然后把不在树上的口关掉。**

**一、选举根桥。** 每个桥有个 **8 字节桥 ID**：2 字节优先级（默认 `32768`，必须是 `4096` 的倍数）+ 6 字节 MAC。**优先级最低、再 MAC 最小者为根桥。** 之所以用优先级而不是"谁最强"，是为了让人能手工指定——你把它调成 `4096`，你就是根。Cisco 的 PVST+ 会把 VLAN ID 加到优先级低 12 位上（*sys-id-ext*），所以同一台物理交换机在不同 VLAN 里可以是不同的根。

**二、BPDU：心跳木牌。** 根桥每 **2 秒**（Hello）发一个配置 BPDU，目的 MAC 是 `01:80:C2:00:00:00`（交换机不会转发这个地址，所以心跳木牌不占信路）。BPDU 里四个关键字段：**根桥 ID**、**根路径开销**、**发送方桥 ID**、**发送方端口 ID**。非根桥收到后改写"根路径开销"和"发送方"再往下传——这就是"只跟邻居讲话，只报一个数"。802.1D 里只有根桥**产生** BPDU，其余桥转发；RSTP 里每个桥每 2 秒都自己发。

**三、端口角色与阻塞。** 每台非根桥选**一个根端口**（*root port*）：到根的路径开销最低，平手比发送方桥 ID，再平手比发送方端口 ID。每条链路（网段）上选**一个指定端口**（*designated port*）：这段上离根最近的那台桥负责向外送，平手比桥 ID。**剩下的端口就是阻塞端口**（*alternate* / *backup*）——它们**不转发数据帧，但照常收、照常解析 BPDU**。这一点是整套东西的支点：**阻塞不等于断电，阻塞是"闭嘴听"**。听差必须一直知道那条堤是活的。

开销默认值（802.1t 长整型）：`20,000,000 / 速率(Mbps)` —— 10M 是 2,000,000，100M 是 200,000，1G 是 20,000，10G 是 2,000。老 802.1D-1998 的短整型是 10M=100、100M=19、1G=4、10G=2，小到 10G 和 100G 都是 2，**根本分不出快慢**——这就是为什么现代网络必须用长开销。

**四、状态与计时器。** 802.1D 有五个状态：`Disabled`、`Blocking`、`Listening`（15 秒）、`Learning`（15 秒）、`Forwarding`。一个端口从"决定要开"到"真的转数据"，要经过听、学两段，各 **Forward Delay = 15 秒**；判定上游死了要等 **Max Age = 20 秒**。所以最坏收敛 **20 + 15 + 15 = 50 秒**（常说的 30–50 秒就是这么来的）。听和学的区别在于：听的时候连 MAC 都不学（因为拓扑还没定，学了也是错的），学的时候学但不转发。

**五、拓扑变了，全湖赶紧烧旧账。** 一台桥发现链路变化，就往根的方向发 **TCN BPDU**；根收到后置 **TC 标志**，并在 `Max Age + Forward Delay`（35 秒）内让全部桥把 MAC 表老化时间从默认 **300 秒**缩到 **15 秒**。这就是"催烧"：不烧，流量会一直往那个已经不存在的方向送，送到 TTL 之外（二层没有 TTL，所以更糟——它会一直送到有人把它冲掉）。

**六、RSTP：把"死等"改成"先约好"。** IEEE 802.1w（已并入 802.1D-2004）把五个状态压成三个（`Discarding` / `Learning` / `Forwarding`），角色扩成四个（根、指定、**alternate**、**backup**），并加了**提议/同意**（*proposal / agreement*）握手：一个端口想立刻转发，就向下游发一个带 proposal 位的 BPDU；下游把自己的非边缘口全部同步到 discarding，回一个 agreement，这边立刻进转发。**不必再等两个 15 秒**，典型收敛在**亚秒到几秒**。它还把"边缘口"（*edge port*，接终端的口）直接置为转发，并把 alternate 端口预先留在身边——根口一断，alternate 立刻顶上。

**七、MSTP：一棵树不够用。** IEEE 802.1s 允许把 VLAN 映射到多个**实例**，每个实例一棵独立的树。于是可以让 VLAN 10 堵这条路、VLAN 20 堵那条路，**同一条被 STP 判死的链路对另一半流量是活的**。这是解决"红布浪费带宽"怨言的第一个正式答案。

### 为什么重要

**一、二层环不是慢，是秒级致命，而且看起来像别的问题。** IP 路由环会被 TTL 挡住，最坏也就是丢几十个包。以太网环没有 TTL，一个帧可以在里面转到天亮。症状是**广播包速率瞬间打到线速、交换机 CPU 100%、MAC 表在两个口之间跳（MAC flapping）、全网丢包**——而你连 SSH 进去看的那条链路，用的正是同一个被打死的广播域。**在二层，你可能是自己关掉了自己唯一的救援通道。**

**二、收敛时间不是一个性能数字，它决定你敢不敢做双上行。** 802.1D 的 30–50 秒足够让一个 OSPF/BGP 邻居掉光、让一次数据库主从切换完成、让几千个 TCP 连接超时重连。这就是为什么现网几乎都在用 RSTP/MSTP，也是为什么在 L2 上做"双活上行"总带着一点心虚。

**三、STP 最大的问题不是它算得慢，是它必须堵掉一半链路。** 一棵树用不到环里的全部带宽，这不是实现缺陷，这是"树"的定义。所以现代数据中心的做法不是把 STP 调快，而是**根本不让二层成环**：把 L2 域压到机柜内（"L2 到接入交换机为止"），上行全部跑 L3 路由，用 **ECMP** 把每条链路都用满（路由有 TTL，成环不会杀死你）；或者用 **MLAG / vPC / EVPN 多归属** 让两条上行同时转发；或者干脆用 **EVPN-VXLAN** 把二层语义搬到三层网络之上，让底层靠路由收敛。**从"堵掉一半"到"全都用上"，从来不是把 STP 调快能达到的。**

**四、绝大多数 STP 事故是误配，不是协议缺陷。** 三种最高频：**根桥被随便一台接入交换机抢走**（有人优先级没配、或者买了一台默认 `32768` 但 MAC 更小的新设备）→ 全网流量重新绕行一条 1G 的接入链路。**边缘口接了个小交换机、小交换机自己绕成环** → 广播风暴从机柜里长出来。**单向链路（光纤只断了一个方向）导致阻塞口收不到 BPDU，误以为上游死了，把自己打开** → 环回来了，而这次没人知道为什么。对应三道防线：**Root Guard**（在朝外的口上，凡收到优于现任根的 BPDU 就把它挡在 discarding，不允许别人篡位）、**BPDU Guard**（在边缘口上，一旦收到 BPDU 说明这里不该是交换机，立刻 err-disable）、**Loop Guard / UDLD**（阻塞口一直收不到 BPDU 时，把它锁死在阻塞态，而不是打开）。

**五、它的设计哲学值得抄：用"最小的号"而不是"最强的号"选主。** 根桥不是选举出来的，是**算出来**的——只要每个桥知道自己的 ID 和邻居的 ID，全网必然算出同一个根，不需要任何共识协议、不需要投票、不需要多数派。这是分布式系统里最省事的一类选举：**把决策压成一个全序，然后让每个节点独立算同一个 argmin。** 它的代价也很清楚：**没有防欺骗**——任何人报一个更小的号就能篡位。STP 用 Root Guard 在边界上补这一刀，而不是在协议里修。

_隐喻对应表_

- 一封信被抄给"来路以外的每条堤"，路成环则同一封信无限翻倍 → 交换机对未知单播与广播帧向除入端口外的所有口洪泛，二层成环即广播风暴
- 信封上没有"走过九村就作废"这一行 → 以太网帧没有 TTL，环里的帧永远不会自己死掉
- 四条堤上双向都是信童，迎面撞上，谁也不让 → 环上的帧在线速上自我复制，链路饱和
- 村子越多抄得越慢，抄得越慢占路越久 → 洪泛随拓扑规模放大，交换机 CPU 与中断被广播吃掉
- 各村的"认路账"记着某个方向 → 交换机 MAC 地址表（CAM 表）
- 信童跑到断口，折回，再跑 → 陈旧 MAC 表项把流量送往已不存在的方向
- 门楣上刻的号：先比数字，数字一样比名号 → 8 字节桥 ID：2 字节优先级（默认 32768，4096 的倍数）+ 6 字节 MAC，优先级低者优先，再比 MAC 小者
- 号最小的村子是宗主，不用开会，自己算得出 → 根桥选举：全网独立算同一个最小桥 ID，不需要任何共识协议
- 竹尺给每段堤一个脚价：越窄越绕越贵 → 端口路径开销，802.1t 为 `20,000,000 / 速率(Mbps)`（1G = 20,000）
- 每个村子只报一个数给邻居，只听邻居的数 → BPDU 里的根路径开销，非根桥收到后加上本地开销再向下游宣告
- 心跳木牌每两拍敲一次，宗主发 → 配置 BPDU：Hello 2 秒，由根桥产生
- 木牌走的是专门的传令牌道，不占信路 → BPDU 目的 MAC `01:80:C2:00:00:00`，交换机不转发
- 每个村子只留一条最低价的归路 → 每台非根桥只有一个根端口（*root port*）
- 两村之间那段堤只认离宗主更近的那个村当值 → 每条网段上只有一个指定端口（*designated port*），由离根最近（平手比桥 ID）的桥承担
- 另一头的堤口挂红布 → 阻塞端口（*alternate* / *backup*），不转发数据
- 挂红布的堤后面照旧站一个听差的 → 阻塞口仍然接收并处理 BPDU，这正是它能在故障时立刻接管的理由
- 连二十拍听不见心跳才认死 → Max Age 20 秒
- 认死之后先"听"十五拍，再"认"十五拍，才放信童 → Listening 15 秒 + Learning 15 秒（Forward Delay），学习期学 MAC 但不转发
- 最坏要等 20 + 15 + 15 拍 → 802.1D 最坏收敛约 50 秒（常说的 30–50 秒）
- 催烧信童让各村把旧账烧得比平时快：三个月改二十天 → TCN BPDU 触发 TC 标志，MAC 表老化时间从 300 秒缩到 Forward Delay（15 秒），持续 `Max Age + Forward Delay`（35 秒）
- 挂红布的堤后面那位听差早就跟对面"对过话"，归路一断直接喊"我接" → RSTP 的 proposal / agreement 握手，把收敛从"等计时器"变成"换手一次"
- 恢复只要一拍，不是二十加十五加十五 → RSTP 典型收敛亚秒到几秒，而不是 802.1D 的数十秒
- 预先留在身边的那条挂红布的路 → alternate 端口，根端口失效时立刻顶上
- 挂红布的堤人不能走但耳朵留着，所以知道它是死是活 → 阻塞态仍收 BPDU；这也是 Loop Guard 的依据
- 湖东三村嫌绕远，撤了半天红布，半刻钟信童翻三倍 → 打开一个被 STP 阻塞的冗余链路即立刻成环；"堵掉一半"不是可选项
- 同一批 VLAN 让不同的堤挂红布 → MSTP（802.1s）把 VLAN 映射到多个实例，每个实例一棵树，用阻塞链路做负载分担
- 村外大路进来却报出比现任宗主更小的号，一律不理并封口 → Root Guard：在朝外端口上收到优于现任根的 BPDU 时置为 discarding，防止根桥被抢
- 通到院子的门口收到心跳木牌，立刻插门喊人 → BPDU Guard：边缘口收到 BPDU 说明下面接了交换机，置 err-disable
- 单向断了的堤：听差收不到木牌，以为上游死了，把红布撤了 → 单向链路导致阻塞口收不到 BPDU 而转入转发，Loop Guard / UDLD 把它锁死在阻塞态
- 石堤上赶集的路越修越多，路从树变成网 → L2 域扩张、冗余链路增加，环的概率从"不可能"变成"迟早"
</section>
<section class="en" markdown="1">
Nine villages ring a lake called Willow Bay. The roads between them are not water but stone causeways — the wide ones take an ox cart, the narrow ones barely let two people pass.

The causeways carry **letter boys**. Willow Bay's rule is old and very simple: **a village that receives a notice copies it once and sends it out along every causeway except the one it came in on.** Each copy is made neatly, and on the back of the envelope is written the hour the village received it.

In the early years there were few causeways — one main causeway strung through five villages, two branch causeways reaching two villages each. The shape was a tree: one step to the next, from root to tip, with no second path. The rule served for sixty years without incident.

Then the nine villages pooled their money, built a causeway around the whole lake, and cut three shortcuts straight across the middle, so nobody would have to walk the long way to market. The roads went from a tree to a net. Nobody looked twice at that change the year it was built.

That spring, at the Grain Rain festival, the first notice was "market opens." The first day was fine. On the second day the notice ran the entire ring causeway and came back to the village gate that had first sent it out. The old gatekeeper took the copy and did what sixty years of rule said to do — copy it once, send it along every causeway but the one it came in on. He did not recognize the notice he had sent out the previous morning.

**There were now two copies of the same notice on the roads.**

Third lap, four. Fourth lap, eight. By the fortieth lap both directions of all nineteen causeways were solid with letter boys carrying the same sheet of paper, meeting head-on, nobody giving way. The more villages, the slower the copying; the slower the copying, the longer each copy held the road; the longer it held the road, the more copies were on it.

Old Zhou at the bridgehead later put it better than anyone:

**The problem was not too many letters. The problem was that the envelope had no line reading "destroy after nine villages."**

Water-borne documents all have that line — the merchants' pouches say plainly *destroy if undelivered after three post stations*, and the boatmen know to burn them. The letters on the causeways had nothing. Willow Bay's rule was "delivered and done," not "delivered or dead." **A copy could circle the ring forever, and double at every turn.**

On the seventh day the grain boats could not tie up on the east shore. Not because the causeway was jammed, but because **the quay could not accept a bill of lading** — the clerk would not take grain until the "market opens" notice had settled, and the notice was permanently on the road. Rice prices in the nine villages rose three times a day. The lantern signals failed too, because the lantern signals were also carried by letter boys.

**The first remedy was a speed limit.**

The headman ordered: no more than four letter boys on any causeway at once, everyone else held back.

The result was strange. The lake went quiet for a quarter hour, and then it got worse. The letters running hardest were exactly the ones that got through, while legitimate bills of lading stood outside the causeway and did not move at all. **A speed limit guarantees the road will not collapse. It says not one word about who should be on it.**

**The second remedy came from the old headman, sitting under the scholar tree on the west shore with a bamboo rule marked in graduations.**

**One: name a sovereign.** Each of the nine villages carves an **appellation** above its gate — first compare the number carved there, and if the numbers tie, compare the appellations themselves (say, Scholar Tree Village ahead of Willow Village, because scholar comes first). **The village with the smallest appellation is the sovereign.** It is not necessarily the strongest. It merely has the smallest name. The beauty of the rule is that **no meeting is required**: given your own appellation and your neighbor's, you can compute the sovereign yourself.

**Two: measure distance, but speak only to your neighbors.** The bamboo rule gives every causeway a **toll**: the narrower and more winding, the higher; the wider and straighter, the cheaper. A village's "journey toll" to the sovereign = the toll of its cheapest upstream causeway toward the sovereign + the toll its upstream neighbor announced. **Each village announces exactly one number to its neighbors and listens to exactly one number from them.** Nobody walks the whole lake.

**Three: each village keeps only one way home.** Of the roads leading toward the sovereign, keep the **cheapest** one open for traffic. Ties go to the upstream village with the smaller appellation, and if that ties, to the smaller gate number on that stretch of causeway. A village has **exactly one** way home.

**Four: on any stretch between two villages, only one village is on duty.** Whichever is closer to the sovereign (lower journey toll, and on a tie, the smaller appellation) is responsible for carrying letters out along that stretch, and its gate stands open; **the gate at the other end hangs a red cloth.**

**Five: a causeway under red cloth carries no people, but keeps its ears.** A listener still stands behind the cloth, hearing the heartbeat tokens relayed from the sovereign — the sovereign strikes one every two beats. He carries no letters. But he knows whether that causeway is alive or dead. **The life of the nine villages hangs on this least useful-looking job in the whole town.**

**Six: wait twenty beats before calling it dead, and after that, listen first and learn second.** Only after twenty consecutive beats with no heartbeat does the listener rule the upstream dead and void the way home; then he picks the best of the red-clothed causeways and makes it the new way home. Two steps before that: first **listen** for fifteen beats — send nothing, only hear the names and tolls on both sides; then **learn** for fifteen beats — start recording the roads, still send nothing; only after those fifteen may letter boys onto the causeway. It sounds fussy. It is what keeps the whole lake from **burning the old ledger and misusing the new one in the same instant**.

Nineteen causeways: nine open, ten under red cloth. A letter from the west shore to the east shore now travels one non-repeating path; the number of copies drops from "double every lap" to "one per village."

**The third spring brought a second event, more worth recording than the first.**

Three eastern villages complained that the way home was a long way round — there was a straight causeway, three li, while the way home was eleven. They talked the old headman into pulling the red cloth off that causeway for half a day.

**A quarter hour.** Letters on the lake tripled, and two gatekeepers fainted outright. The red cloth went back that same day, and nobody raised it again.

Afterward the nine villages understood a sentence nobody had said aloud: **the red cloth is not waste. The red cloth is the single condition under which the lake is still alive.**

**In the fourth summer's floods, the main causeway at the lake's heart collapsed.**

Half the lake lost the sovereign. And something worse happened in the same instant: every village's **route ledger was wrong** — the ledger said *letters to the east fishery go by the north causeway*, but there was nobody at the north causeway anymore. Letter boys ran to the break, turned back, ran again, and were still stopped along the way by other letter boys asking directions.

The sovereign's response was effortless: he sent **burn-order** letter boys at a run, telling every village to burn its old ledger far faster than usual — where an entry normally lasted three months, for these days it lasted twenty. Misdelivered letters stopped within a quarter hour.

But this time the whole lake was talking again **within one beat** — not twenty beats plus fifteen plus fifteen.

Why? Because the listeners behind those ten red cloths had been **talking to the village opposite them all along** — both sides held each other's names and tolls from the previous round. The moment the way home broke, the listener simply called out "I take it," and the far side answered at once "you take it." No waiting twenty beats to declare death, no fifteen to listen, no fifteen to learn. The old rule was **wait until you are certain**. The new rule was **agree in advance**. The villages later called this revision "answering the road."

**In the fifth year, two odd incidents were written into the clan rules.**

The first: a wealthy house newly moved to the east shore carved a number above its gate smaller than the reigning sovereign's, hoping all the lake's letters would detour through his courtyard so he could take a cut. A rule was added: **coming in from the outer highway, any appellation smaller than the reigning sovereign's is ignored outright, and that gate is sealed.** The sovereign is something you compute, not something you declare.

The second: a lakeside household had looped itself — it had bought two courtyard gates, both leading to the main hall, and the hall copied outgoing notices to both. A heartbeat token arriving at a courtyard gate means no token should ever have been there. Another rule: **a gate into someone's courtyard is not a causeway; if a token arrives at one, bar the gate at once and shout for someone to come look.** Do not let one household drag the whole lake down.

By now you have probably recognized it: this is the **Spanning Tree Protocol** (*STP*).

### What it is

Start with what it has to hold back.

Ethernet forwarding is **read the house number and deliver**: a switch receives a frame, looks up which port holds the destination MAC, sends it there; if it does not know, it sends it out **every port except the one it arrived on** (unknown unicast flooding). Broadcast frames go out every port unconditionally.

The entire safety of that scheme rests on the assumption that the roads form a tree. And layer 2 **has no TTL** — an IP header carries a hop counter that discards the packet when exhausted; an Ethernet frame carries nothing of the kind. **A frame that enters a loop never dies.** It only multiplies around the loop: each switch makes one copy of a broadcast frame, but ten switches in a ring means ten copies circling; add unknown-unicast flooding on top and a single lap saturates the link.

Three consequences arrive together: **bandwidth is consumed entirely**; **every switch's MAC table flips wildly** (the same source MAC alternating between two ports), so even unicast that should be forwarded precisely gets flooded; and **switch CPUs are consumed by interrupts**, so you cannot even reach the management plane — the channel you would use to fix the config runs over the same dead broadcast domain.

**What STP (IEEE 802.1D) does is brutally simple: compute a tree out of a looped network, then shut the ports that are not on the tree.**

**One: elect a root bridge.** Every bridge has an **8-byte bridge ID**: a 2-byte priority (default `32768`, must be a multiple of `4096`) plus a 6-byte MAC. **Lowest priority, then lowest MAC, becomes the root.** Priority rather than "who is strongest" exists so a human can pin it — set it to `4096` and you are the root. Cisco's PVST+ adds the VLAN ID into the low 12 bits of the priority (*sys-id-ext*), so one physical switch can be root of one VLAN and not another.

**Two: BPDU, the heartbeat token.** The root sends a configuration BPDU every **2 seconds** (Hello), to destination MAC `01:80:C2:00:00:00` (switches never forward that address, so heartbeat tokens do not consume the letter roads). Four fields matter: **root bridge ID**, **root path cost**, **sender bridge ID**, **sender port ID**. A non-root bridge rewrites the cost and the sender, then relays it downstream — that is "speak only to your neighbors, announce one number." In 802.1D only the root **originates** BPDUs and everyone else relays; in RSTP every bridge sends its own every 2 seconds.

**Three: port roles and blocking.** Every non-root bridge elects **one root port**: lowest path cost to the root, ties to lowest sender bridge ID, then lowest sender port ID. On every link (segment) **one designated port** is chosen: the bridge on that segment closest to the root handles outbound traffic, ties to lowest bridge ID. **Whatever is left is a blocked port** (*alternate* / *backup*) — it **forwards no data frames, but still receives and parses BPDUs**. That is the fulcrum of the whole design: **blocked does not mean powered off. Blocked means shut up and listen.** The listener must keep knowing whether that causeway is alive.

Default costs (802.1t long form): `20,000,000 / speed in Mbps` — 10M is 2,000,000; 100M is 200,000; 1G is 20,000; 10G is 2,000. The old 802.1D-1998 short form was 10M=100, 100M=19, 1G=4, 10G=2 — so coarse that 10G and 100G are both 2 and **cannot be told apart**, which is why modern networks must use long costs.

**Four: states and timers.** 802.1D has five states: `Disabled`, `Blocking`, `Listening` (15 s), `Learning` (15 s), `Forwarding`. A port that has decided to open still spends **Forward Delay = 15 seconds** listening and 15 learning before it carries data; declaring the upstream dead takes **Max Age = 20 seconds**. Worst-case convergence is therefore **20 + 15 + 15 = 50 seconds** (the familiar 30–50 s figure). The difference between listening and learning: while listening, not even MAC addresses are learned, because the topology is not settled and anything learned would be wrong; while learning, addresses are learned but nothing is forwarded.

**Five: when the topology changes, the whole lake burns its old ledger.** A bridge detecting a link change sends a **TCN BPDU** toward the root; the root sets the **TC flag**, and for `Max Age + Forward Delay` (35 seconds) every bridge drops its MAC aging time from the default **300 seconds** to **15 seconds**. That is the burn order. Without it, traffic keeps being sent toward a direction that no longer exists — and at layer 2 it is worse than an IP loop, because there is no TTL to stop it; it continues until something flushes it.

**Six: RSTP turns "wait until certain" into "agree in advance."** IEEE 802.1w (folded into 802.1D-2004) collapses five states into three (`Discarding` / `Learning` / `Forwarding`), expands roles to four (root, designated, **alternate**, **backup**), and adds a **proposal/agreement** handshake: a port wanting to forward immediately sends a BPDU with the proposal bit set; the downstream bridge synchronizes all its non-edge ports to discarding and answers with agreement, and this port goes straight to forwarding. **No two 15-second waits**, so typical convergence is **sub-second to a few seconds**. It also puts **edge ports** (those facing end hosts) into forwarding immediately and keeps an alternate port warm by its side — when the root port dies, the alternate takes over at once.

**Seven: MSTP, because one tree is not enough.** IEEE 802.1s maps VLANs onto multiple **instances**, each with an independent tree. So VLAN 10 can block this link while VLAN 20 blocks that one — **the link STP condemned is alive for half your traffic.** This is the first formal answer to the complaint that red cloth wastes bandwidth.

### Why it matters

**One, a layer-2 loop is not slow, it is lethal within seconds, and it looks like something else.** An IP routing loop is stopped by TTL; worst case you lose a few dozen packets. An Ethernet loop has no TTL, and a frame can circle until morning. The symptoms are **broadcast rate instantly at line rate, switch CPU at 100%, MAC table flapping between two ports, loss across the entire domain** — and the SSH session you would use to inspect it runs over that same dead broadcast domain. **At layer 2, you may have closed your only rescue route yourself.**

**Two, convergence time is not a performance number; it decides whether you dare build dual uplinks.** The 30–50 seconds of 802.1D is long enough to drop an OSPF/BGP adjacency entirely, to complete a database failover, and to time out thousands of TCP connections. That is why production runs RSTP/MSTP almost everywhere, and why "active/active uplinks" at layer 2 always carries a certain nervousness.

**Three, STP's real problem is not that it converges slowly; it is that it must block half the links.** A tree cannot use the bandwidth of a ring. That is not an implementation defect, it is the definition of a tree. So modern data centers do not tune STP faster, they **stop letting layer 2 form loops at all**: shrink the L2 domain to the cabinet ("L2 no further than the access switch"), run every uplink as layer 3, and use **ECMP** to spend all links at once (routing has TTL, so a loop cannot kill you); or use **MLAG / vPC / EVPN multihoming** so both uplinks forward simultaneously; or put layer-2 semantics on top of a layer-3 fabric with **EVPN-VXLAN** and let the underlay converge by routing. **Getting from "block half" to "use all" has never been achievable by making STP faster.**

**Four, the overwhelming majority of STP incidents are misconfiguration, not protocol defect.** Three highest-frequency causes: **the root gets stolen by some random access switch** (nobody set a priority, and a new box shipped at `32768` with a lower MAC) → the whole network re-routes through one 1G access link. **An edge port gets a small switch plugged into it, and the small switch loops itself** → a broadcast storm grows out of one cabinet. **A unidirectional link** (fibre broken in one direction only) leaves a blocked port receiving no BPDUs, so it concludes the upstream is dead and opens itself → the loop is back, and this time nobody knows why. Hence three defenses: **Root Guard** (on outward-facing ports, a superior BPDU puts the port in discarding — nobody gets to usurp the root), **BPDU Guard** (on edge ports, any BPDU means a switch is not supposed to be there — err-disable immediately), and **Loop Guard / UDLD** (a blocked port that stops receiving BPDUs is pinned in blocking instead of being opened).

**Five, the design philosophy is worth copying: choose a leader by the smallest name, not the strongest.** The root is not elected, it is **computed** — as long as each bridge knows its own ID and its neighbors', the whole network necessarily computes the same root, with no consensus protocol, no voting, no majority. This is the cheapest kind of election in distributed systems: **collapse the decision into a total order, then let every node independently take the same argmin.** Its cost is equally clear: **there is no defense against lying** — anyone announcing a smaller number usurps the root. STP patches that with Root Guard at the boundary rather than in the protocol.

_Metaphor mapping_

- A notice copied to every causeway but the one it arrived on; with a ring, the same notice doubles forever → switches flood unknown unicast and broadcast out every port but the ingress; a layer-2 loop means a broadcast storm
- The envelope has no "destroy after nine villages" line → Ethernet frames have no TTL, so a frame in a loop never dies on its own
- Four causeways solid in both directions, letter boys meeting head-on, nobody giving way → frames replicate themselves at line rate around the loop, saturating the links
- The more villages, the slower the copying; the slower the copying, the longer each copy holds the road → flooding scales with topology size and eats switch CPU and interrupts
- Each village's route ledger names a direction → the switch MAC address table (CAM table)
- Letter boys run to the break, turn back, run again → stale MAC entries send traffic toward a direction that no longer exists
- The carved number above the gate: compare numbers, then names → the 8-byte bridge ID: 2-byte priority (default 32768, multiple of 4096) plus 6-byte MAC; lower priority wins, then lower MAC
- The smallest appellation is sovereign, no meeting needed, computable by anyone → root bridge election: every node independently computes the same minimum bridge ID; no consensus protocol required
- The bamboo rule's toll: narrower and more winding costs more → port path cost, 802.1t `20,000,000 / Mbps` (1G = 20,000)
- Each village announces one number to its neighbors and hears one back → the root path cost field in the BPDU; non-root bridges add local cost and re-announce
- Heartbeat tokens every two beats, struck by the sovereign → configuration BPDUs, Hello 2 s, originated by the root
- Tokens travel a reserved channel that does not consume the letter roads → BPDU destination MAC `01:80:C2:00:00:00`, never forwarded by switches
- Each village keeps only its cheapest way home → exactly one root port per non-root bridge
- On any stretch, the village closer to the sovereign is on duty → one designated port per segment, held by the bridge closest to the root (tie: lower bridge ID)
- The gate at the other end hangs red cloth → a blocked port (*alternate* / *backup*) forwards no data
- A listener still stands behind the red cloth → blocked ports still receive and process BPDUs; that is exactly why they can take over instantly
- Twenty beats with no heartbeat before calling it dead → Max Age, 20 seconds
- After death, listen fifteen beats, then learn fifteen, then send → Listening 15 s + Learning 15 s (Forward Delay); learning populates the MAC table without forwarding
- Worst case is 20 + 15 + 15 beats → 802.1D worst-case convergence near 50 seconds (the familiar 30–50 s)
- Burn-order boys shorten the ledger's life from three months to twenty days → TCN BPDU sets the TC flag; MAC aging drops from 300 s to Forward Delay (15 s) for `Max Age + Forward Delay` (35 s)
- Listeners behind the red cloth had already "talked" to the far side and call "I take it" the instant the way home breaks → the RSTP proposal/agreement handshake turns convergence from waiting on timers into one exchange
- Recovery in one beat rather than twenty plus fifteen plus fifteen → RSTP converges in sub-second to a few seconds instead of tens of seconds
- The red-clothed road kept warm at one's side → the alternate port, ready to take over the moment the root port fails
- Red cloth stops people but keeps listening, so its health is known → blocked ports keep receiving BPDUs; this is also what Loop Guard relies on
- Three eastern villages pull the red cloth for half a day and letters triple in a quarter hour → unblocking one STP-blocked redundant link creates a loop immediately; "block half" is not optional
- Different causeways under red cloth for different batches of villages → MSTP (802.1s) maps VLANs to instances, one tree each, so blocked links carry the other half of the traffic
- An appellation smaller than the reigning sovereign's arriving from the outer highway is ignored and the gate sealed → Root Guard puts an outward port in discarding on a superior BPDU, preventing root usurpation
- A heartbeat token at a courtyard gate means bar the gate and shout → BPDU Guard err-disables an edge port that receives a BPDU, because a switch does not belong there
- A causeway broken in one direction only: the listener hears nothing and pulls the red cloth → a unidirectional link starves a blocked port of BPDUs until it forwards; Loop Guard / UDLD pins it in blocking
- More and more market roads until the shape goes from tree to net → L2 domain growth and added redundant links turn loops from impossible into inevitable
</section>
