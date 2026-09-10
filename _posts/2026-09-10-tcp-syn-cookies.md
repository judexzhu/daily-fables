---
layout: fable
title: "烽火关的无字令 · The Blank Token at the Beacon Pass"
title_zh: "烽火关的无字令"
title_en: "The Blank Token at the Beacon Pass"
concept: "TCP SYN Cookies: Stateless Handshake and SYN Flood Defense"
tags: [networking, security, linux]
illustration: /assets/art/2026-09-10-tcp-syn-cookies.jpg
youtube_id: "FiVNXfdXJa8"
---
<section class="zh" markdown="1">
北境群山之间有一座要塞，叫烽火关。关下是一条狭长险峻的石峡，关里则是通往中原腹地的八百里通衢。

关城设有一道极严整的迎候仪轨：凡有商旅车队入关，必经三道往返。
第一道，商队的先头骑卒快马赶到关门下扣关，递上关牒，报称“后方有三十车茶叶将至，请开关拔栓”；
第二道，门丞验过来牒，从柜中取出一册名唤《候见簿》的硬皮账本，郑重录下骑卒的籍贯、车马数与扣关时刻，随后自账角撕下一道“候关竹筹”递出箭窗，同时高呼一声“关房已备，迎候贵客”；
第三道，半个时辰后商队正主辚辚而至，将那枚竹筹高举过顶。门丞核对竹筹与《候见簿》上的字迹分毫不差，这才落印放行，迎商队驶入内城瓮城。

百年来，这套三步规矩行之有效，商旅各按部就班。

直到有一年深秋，关外的胡狄换了打法。

他们不备云梯，也不排战阵，只差遣上千名轻装游骑，每隔数十息便有一骑飞驰到城关下，扯着嗓子大喊：“后有百骑粮草将至，速录名册！”
喊罢，骑卒伸手接了门丞递出的竹筹，拨转马头便钻入茫茫风沙，再无踪影。

门丞是个老实勤勉的关官。来一骑，他便伏案在《候见簿》上工整录下一笔，并备好一队执戟卫士在瓮城待命。
然而，那些高喊要来的大队车马，一个时辰没来，两个时辰依旧没来。

麻烦在于，关房里的《候见簿》统共只有二百五十六行，瓮城里的待命空席也只有二百五十六个。未满半日，整本账册密密麻麻全填满了那些凭空蒸发的游骑名字。

此时，真正从中原解运冬衣与救灾粟米的官道马队顶着风雪赶到关下。
门丞翻开早已写得毫无容针之地的《候见簿》，双唇干裂，只能从箭窗里绝望地摆手：“候客册子已经录满，关内再无闲笔闲席，诸位请在关外风雪里候着吧！”

关外狂风如刀，货马冻毙枕藉。
狄人没有折损一兵一卒，仅仅凭着千百句空头许诺，就让这座天下雄关彻底瘫痪。

消息报入幕府，一位须发皆白的老关令策马赶抵城头。他冷眼翻看那本被恶鬼般的名字塞满的《候见簿》，当着全关将领的面，一把将账册掷入了火盆。

“你们最大的破绽，”老关令沉声道，“是在对方尚未露面之前，就把自己金贵的纸墨与营房，平白押给了一句不知真假的口信。”

老关令立下了一道石破天惊的新规矩：
**自今日起，烧毁候见簿。前客扣关，一字不记，一席不留。**

门丞大惊：“若是不记名录，稍后商队真来了，凭什么验明正身？”

老关令从怀中取出一枚精铜密印与三只不同刻度的小漏壶。
“日后凡有游骑扣关，休要动笔磨墨。你只需抬眼看清四样东西：骑卒打哪条道来、报的是哪家商号、扣关是哪一刻钟、以及马背上负重几何。你将这四样数额合在一处，用这枚带暗齿的铜印随手在热蜡上一压，压出一枚‘无字蜡符’抛给他，只对他说一句：‘拿去，正主到了凭符相认。’”

“关内如何留档？”门丞追问。
“**不留档。**”老关令目光如铁，“一字不录，一卒不备。把关门闭拢，只管喝茶。”

游骑们依然呼啸而至，接走了一枚又一枚滚烫的蜡符，随后遁入荒原。但这一次，关城内一片空明澄澈，既没有被占满的账页，也没有空耗气力的守卫。任凭关外游骑千万，关内岿然不动。

半个时辰后，一支真正运送盐铁的大商队冒雪抵达关门。领队翻身下马，恭敬地从怀中掏出方才哨骑接走的蜡符，高高举起。

门丞甚至不必低头去翻找什么早已不存在的账册。他接过蜡符，扫了一眼商队的道口、马印与此刻的天色，提起手中的铜印重新推算一番——密印暗纹与蜡符上的咬合齿痕丝丝入扣，分毫不差！

“符在人在，丝扣合缝！”门丞击掌大笑，当即开闸拔栓，将车马迎入瓮城，此刻方才提起朱笔，在正堂大账上划上第一笔。

狡黠的狄人游骑在关下嘶吼了三天三夜，嗓子喊哑，手里的蜡符扔了一地，却发现关内再也没有一丝一毫的慌乱。真正的商旅行人踏着风雪从容过关，关防大开，通衢重现。

不过老关令也私下叮嘱门丞：
这道无字密符虽能退敌，却因蜡块极小，刻不下车队的大车轴距与特别护送的关牒附记。若遇上极庞大的特异辎重，免不了要在正堂重新核定尺码。
但比起整座雄关被活活噎死在风雪里，丢弃几行琐碎记挂，又算得了什么？

— — —

### 这是什么

这就是计算机网络与安全领域中抵御拒绝服务攻击最富传奇色彩的典范设计——**TCP SYN Cookies**。

在标准的 TCP 三次握手（Three-way Handshake）中，连接的建立分为三步：
1. 客户端发送 `SYN` 报文，请求建立连接；
2. 服务端收到后，必须在内核中分配一块名为传输控制块（TCB / Transmission Control Block）的内存，保存客户端的 IP、端口和初始序列号，将该连接挂入**半连接队列（SYN Queue / Half-open Backlog）**，并向客户端回复 `SYN-ACK`；
3. 客户端回复 `ACK`，握手完成，连接移入**全连接队列（Accept Queue）**，等待应用程序调用 `accept()` 取出。

这套经典机制存在一个致命的资源不对称性：
发起 `SYN` 请求的代价极小（攻击者只需伪造源 IP 发送一个仅几十字节的原始数据包），而服务端响应 `SYN` 却需要**立即分配宝贵的内核内存与队列条目**，等待可能长达数十秒的最终确认。

黑客利用该漏洞发起的**SYN Flood（SYN 洪水攻击）**，即操纵大量虚假 IP 疯狂发送 `SYN` 包却绝不回复最后的 `ACK`。
极短时间内，服务端的半连接队列就会被彻底塞爆。操作系统内核由于无法分配新的 TCB，只能无奈将所有后续合法的连接请求直接丢弃，导致正常用户完全无法访问。

1996 年，著名密码学家与计算机科学家 Daniel J. Bernstein（DJB）提出了彻底颠覆握手逻辑的解法：**SYN Cookies**。

其核心哲学是**在信任建立之前，服务端保持彻底的无状态（Stateless）**：
当检测到半连接队列即将满溢时，内核不再为传入的 `SYN` 分配任何内存或队列条目，而是将连接所需的核心元数据（对端 IP、端口、时间戳计数器与最大报文段长度 MSS 索引），连同服务端的内核私钥，通过加密哈希函数计算成一个 32 位的数字——作为 `SYN-ACK` 的初始序列号（ISN / Initial Sequence Number），这个序列号就是 `SYN Cookie`。

服务端发完 `SYN-ACK` 后便直接“忘掉”该请求，本地内存开销为零。
若对方是假冒的游骑兵，便绝无可能收到 `SYN-ACK`；
若对方是合法客户端，必会按照 TCP 规范回复包含 `ack_seq = Cookie + 1` 的 `ACK` 报文。
服务端收到该 `ACK` 时，仅凭报文中的四元组与序列号逆向解算哈希，若验证通过，便在此时**凭空重建出连接上下文**并分配正式 Socket。

### 为什么重要

- **化被动为主动的逆向防守**：将高昂的“状态存储成本”转嫁为廉价的“密码学计算成本”，用 CPU 算力直接吸收并化解原本会导致内存耗尽的网络洪水；
- **自适应热启动**：在 Linux 系统中（由内核参数 `net.ipv4.tcp_syncookies = 1` 控制），平时握手依旧走高效的常规半连接分配；只有当半连接队列真正遭遇拥塞压迫时才自动无缝切入 Cookie 模式；
- **自包含认证（Cryptographic Self-containment）**：Cookie 本身即是通行凭证，无需任何共享存储、外部数据库或分布式锁的参与，单机即可毫秒级独立完成防御；
- **工程妥协与边界考量**：
  由于 TCP 序列号仅有 32 位（4 字节），Cookie 必须塞入时间戳、MSS 编码和散列值，导致原始 TCP Options（如窗口缩放因子 Window Scale、选择性确认 SACK）无法在 `SYN` 阶段被记录。
  现代 Linux 内核通过复用 `TCP Timestamps`（RFC 7323）字段中空闲的低位，将原本丢失的 TCP 选项安全藏入时间戳回显中，完美消除了早期 SYN Cookies 的性能折损。

### 隐喻对应表

| 故事元素 | 计算机概念 | 技术细节与工程映射 |
| :--- | :--- | :--- |
| **关前狭长险峻的石峡** | 网络物理链路 / 入口带宽 | 外部所有流量进入系统的唯一物理通道 |
| **商队先遣骑卒扣关** | `TCP SYN` 请求报文 | 客户端发起建连请求，携带客户端初始序列号（ISN_c） |
| **《候见簿》与二百五十六个待命空席** | 半连接队列（SYN Backlog Queue） | 内核保存未完成握手连接的数据结构（TCB），容量有限 |
| **虚假游骑高喊后遁入风沙** | SYN Flood 攻击（伪造源 IP） | 只发 SYN、不发 ACK，蓄意耗尽服务端队列资源的恶意流量 |
| **候客册满导致真正商队受阻** | 拒绝服务（Denial of Service） | 内核半连接队列打满，合法请求被直接丢弃（Drop） |
| **老关令下令烧账、一字不记** | 无状态连接握手（Stateless Handshake） | 开启 SYN Cookies，面对未经验证的 SYN 坚决不分配内存 |
| **精铜密印与漏壶时刻推演** | 加密哈希函数与慢速时钟计数器 | 结合 4-tuple、内部密钥与 64 秒滴答的时钟计算 Cookie |
| **滚烫的无字蜡符** | SYN Cookie（服务端初始序列号 ISN_s） | 编码了 MSS、时间戳和散列签名的 32 位序列号 |
| **正主携蜡符归来验合暗齿** | 客户端回复的第三次握手 `ACK` 报文 | 校验 `ack_seq - 1` 是否与本地哈希重算结果吻合 |
| **验合成功方才在瓮城正堂登账** | 移入全连接队列（Accept Queue） | 握手完全确认后，才正式为 Socket 分配内存并交付应用层 |
| **蜡块微小记不下大车轴距** | 传统模式下丢失 TCP Options | 早期 32 位限制丢失 Window Scale / SACK，现借时间戳弥补 |
</section>

<section class="en" markdown="1">
High in the northern frontier stood the Beacon Pass, a colossal stone gate straddling a precipitous mountain gorge. Behind it lay eight hundred leagues of uninterrupted imperial highway leading straight to the empire's heartland.

For centuries, the fortress followed a strict three-step ritual for admitting caravans:
First, a courier from the approaching convoy would gallop up to the iron gate, hammering on the timber portal to announce: "Thirty wagon-loads of tea approach behind me; open the pass and ready the escort!"
Second, the gate registrar examined the courier's permit, opened a heavy parchment ledger known as the *Waiting Register*, and meticulously inked the traveler's home province, wagon count, and precise hour of arrival. He then tore a notched bamboo tally from the ledger's margin, pushed it out the arrow slit, and called out: "Quarters readied; we await your convoy!"
Third, an hour later, the main merchant train rumbled into view. The caravan master held the tally aloft. The registrar verified that the notches on the tally matched the entry in the *Waiting Register*, sealed the travel permit, and swung open the double gates to welcome the laden carts into the grand inner courtyard.

For a hundred years, this courtly ritual maintained order across the northern march.

Until one freezing autumn, the northern nomad tribes discovered a new way to wage war.

They brought no siege towers, no battering rams, and no armored cavalry. Instead, they dispatched a thousand light phantom riders. Every few dozen seconds, a lone rider would thunder to the base of the gate, shout through cupped hands: "A hundred wagons of winter wheat approach behind me; log our name!"
The rider reached out, snatched the bamboo tally thrust through the arrow slit, spun his mount around, and vanished into the swirling desert dust.

The registrar was a conscientious official. With every shout, he dipped his brush, carefully inscribed a row in the *Waiting Register*, and detailed a squad of armed guards to stand waiting in the cold inner yard.
Yet the vast caravans promised by the riders never came. An hour passed; two hours passed; the gorge remained barren and silent.

The catastrophe was structural: the fortress's *Waiting Register* contained exactly two hundred and fifty-six rows, and the waiting courtyard had room for only two hundred and fifty-six wagons. Within half a day, the heavy register was filled to the final line with the names of phantoms who had evaporated into thin air.

At that moment, genuine imperial convoys laden with winter grain and relief supplies struggled through the mountain blizzard to the gate.
The registrar opened the swollen, ink-drenched ledger, his lips parched and trembling. He could only wave his hands helplessly through the iron slit: "The register is full to the edge! We have not a single free line nor a spare guard. Turn back into the snow!"

The blizzard was merciless; merchant horses froze to death in the drifts.
Without firing an arrow or shedding a drop of blood, the nomads had strangled the empire's most critical gateway using nothing more than hollow words.

When the disaster was reported to the imperial commandery, a battle-hardened old magistrate rode hard to the battlements. He coldly paged through the bloated ledger choked with ghost names, and before the assembled commanders, cast the heavy volume straight into the brazier.

"Your fatal flaw," the old magistrate said quietly, "was committing your precious state, your ink, and your barracks to an unverified stranger before he proved he actually existed."

He established a startling new law:
**"Burn the waiting register. From this day forth, when a traveler knocks, record not a single character, and allocate not a single room."**

The registrar turned pale: "If we keep no record, how shall we verify them when the caravan arrives?"

The old magistrate produced a carved bronze cipher stamp and three water clocks of differing capacities.
"When a rider knocks, touch neither brush nor ink. Look only at four things: the valley road he rode in on, the clan insignia on his horse, the quarter-hour on the water clock, and his reported wagon weight. Fold these four figures together under the secret intaglio of this bronze ring, press a wax seal with the resulting mark onto a copper token, and cast it down with one command: *'Take this. When your master arrives, present the token.'*"

"And our register?" the registrar stammered.
"**We keep none.**" The magistrate's voice was like iron. "No ink, no reservations, no escorts. Lock the gate, and sit by the brazier."

The phantom riders thundered in as before, snatching token after token and vanishing into the canyon. But inside the pass, all was quiet. There was no swollen register, no exhausted guards, and no wasted space. Let ten thousand ghost couriers scream at the gate; the fortress remained untroubled and serene.

An hour later, a real merchant train carrying salt and iron emerged from the blizzard. The caravan master dismounted, reverently drew from his breast the copper token handed to his scout, and held it high above the snow.

The registrar did not need to consult any ledger—there was none to be found. He took the token, noted the road, the clan stamp, and the current hour, and recalculated the secret cipher through the bronze ring. The teeth of the calculation engaged the wax stamp with microscopic precision.

"The seal carries its own proof!" the registrar cried out in delight.
He raised the portcullis, swept the caravan into the courtyard, and only then dipped his brush in fresh vermillion ink to record their formal arrival in the permanent master log.

For three days and three nights, the nomads shouted their phantom promises beneath the walls until their throats were hoarse, littering the snow with discarded wax tokens. Yet the pass remained unshakeable. Honest caravans passed smoothly through the gale, the gate swung wide, and the highway stayed open.

The old magistrate left the registrar with only one quiet caveat:
Because the wax token was small, it lacked room to record rare, cumbersome wagon dimensions or special cargo permits. Such extraordinary caravans had to re-verify their dimensions at the grand hall.
"Yet compared to allowing the entire mountain pass to choke to death in the winter snow," the magistrate smiled, "what is the sacrifice of a few margins?"

— — —

### What it is

This is the legendary mechanism designed to defeat Denial of Service attacks in networking and operating systems: **TCP SYN Cookies**.

In a standard TCP Three-Way Handshake, establishing a connection involves three stages:
1. The client sends a `SYN` packet to initiate the connection;
2. The server receives it and immediately allocates a Transmission Control Block (TCB) in kernel memory, records the client IP, port, and initial sequence number, enqueues the connection into the **SYN Queue (Half-open Backlog)**, and returns a `SYN-ACK`;
3. The client returns an `ACK`, completing the handshake. The connection is moved to the **Accept Queue**, where user-space applications retrieve it via `accept()`.

This design carries a fatal resource asymmetry:
Transmitting a `SYN` costs almost nothing (an attacker can blast raw, spoofed IP packets of merely dozens of bytes), whereas processing a `SYN` forces the server to **immediately allocate valuable kernel memory and consume a queue slot**, holding that state for up to dozens of seconds awaiting the final handshake.

Attackers exploit this vulnerability through a **SYN Flood**: flooding the server with fake `SYN` requests from spoofed addresses while never sending the final `ACK`. Within milliseconds, the server's SYN queue is completely exhausted. The operating system, unable to allocate new TCB structures, drops all subsequent connection attempts—leaving legitimate users completely shut out.

In 1996, renowned cryptographer and computer scientist Daniel J. Bernstein (DJB) proposed a revolutionary solution: **SYN Cookies**.

Its central philosophy is: **Remain completely stateless until trust is proven.**
When the kernel detects that the SYN queue is overflowing, it ceases allocating memory or backlog entries for incoming `SYN` packets. Instead, it takes the connection's core metadata (client IP, client port, server IP, server port, a coarse time counter, and an encoded Maximum Segment Size index) and hashes them using a private internal kernel key to generate a 32-bit integer. This value serves as the Initial Sequence Number (ISN) of the outgoing `SYN-ACK`—the **SYN Cookie**.

Once the `SYN-ACK` is sent, the server retains zero state in memory.
If the request was from a phantom attacker, the packet is forgotten with zero resource leak.
If the client is legitimate, the TCP specification requires it to echo an `ACK` packet with `ack_seq = Cookie + 1`.
Upon receiving this `ACK`, the server reconstructs the hash on the fly using the packet's headers. If the cryptographic signature checks out, the kernel **manufactures the connection state out of thin air** and allocates a formal socket.

### Why it matters

- **Asymmetric Defense**: Shifts the attacker's weapon from exhausting stateful RAM to cheap cryptographic calculation, allowing CPU hashing to absorb network floods that would otherwise crash the machine;
- **Adaptive Activation**: Controlled in Linux by `net.ipv4.tcp_syncookies = 1`. During normal traffic, standard low-latency queue allocations are used; the kernel dynamically and transparently switches to Cookie mode only when queue pressure threatens collapse;
- **Cryptographic Self-Containment**: The cookie carries its own proof. No distributed cache, database, or shared memory is needed, enabling single-node kernel-level resilience;
- **Engineering Trade-offs & Modern Fixes**:
  Because the TCP sequence number is constrained to 32 bits, the cookie must pack timestamp counters, MSS indices, and cryptographic hashes into limited bits, initially sacrificing TCP Options like Window Scale and Selective Acknowledgment (SACK).
  Modern Linux kernels gracefully bypass this limitation by encoding original TCP options into unused low bits of the `TCP Timestamps` option (RFC 7323), eliminating performance compromises.

### Metaphor Mapping

| Story Element | Computing Concept | Technical Details & Architecture Mapping |
| :--- | :--- | :--- |
| **Narrow, precipitous mountain gorge** | Physical network link & ingress bandwidth | The constrained physical channel through which all packets enter |
| **Courier knocking at the gate** | `TCP SYN` request packet | Client initiating a connection with Initial Sequence Number (`ISN_c`) |
| **The 256-row Waiting Register** | Half-open SYN Backlog Queue | In-kernel data structure storing incomplete handshakes (TCBs) |
| **Phantom riders shouting and fleeing** | SYN Flood attack (Spoofed IP) | Attacker sending SYNs without answering ACKs to exhaust server queues |
| **Full ledger blocking true merchant caravans** | Denial of Service (DoS) | Saturated SYN queue forcing kernel to drop legitimate user connections |
| **Old magistrate burning the ledger** | Stateless handshake architecture | Enabling SYN Cookies; refusing to commit local RAM to untrusted SYNs |
| **Bronze cipher stamp & water-clock ticks** | Cryptographic hash & coarse time counter | Hashing 4-tuple + server secret key + slow-ticking 64-second time index |
| **The stamped wax token** | SYN Cookie (Server Initial Sequence Number) | 32-bit sequence number encoding MSS index, timestamp, and crypto hash |
| **Merchant presenting the token upon arrival** | Third handshake `ACK` (`ack_seq = Cookie + 1`) | Valid client acknowledging the SYN-ACK, returning the cookie challenge |
| **Cipher re-verification & opening inner hall** | Connection promotion to Accept Queue | Sockets instantiated and handed to application only after handshake completes |
| **Wax seal unable to record wagon dimensions** | Traditional loss of TCP Options | Historical loss of Window Scale/SACK, resolved in modern kernels via Timestamps |
</section>
