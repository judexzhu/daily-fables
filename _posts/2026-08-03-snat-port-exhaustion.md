---
layout: fable
title: "望海楼的九根铜插销 · The Nine Brass Pegs of the Harbour Hotel"
title_zh: "望海楼的九根铜插销"
title_en: "The Nine Brass Pegs of the Harbour Hotel"
concept: "SNAT and connection tracking (conntrack) port exhaustion"
tags: [networking, aws, kubernetes]
illustration: /assets/art/2026-08-03-snat-port-exhaustion.jpg
---
<section class="zh" markdown="1">
望海楼有三百个房间，每个房间床头都有一部电话。可这三百部电话，一部也打不到楼外去。房号只在楼里算数——你跟城里人说"我住 412"，人家只会问你："哪个 412？"

楼外的世界只知道一个号码：望海楼。所有进出的话音，都得从那间闷热的小屋里过一遍。屋里坐着林姨，面前是一面立起来的木板，板上插着一排铜插销，连着往城里去的线路。她手边永远摊着一本册子。

规矩很简单。412 房的客人要给鱼市打电话，先摇铃到小屋。林姨挑一根空着的铜插销——比如第 37 根——插上，在册子上写下一行：**37 号销，412 房，找鱼市**。然后她才把线接出去。鱼市那头的伙计接起来，听见的是"望海楼，37 号销"。他一辈子都不会知道有个 412 房。

回话进来的时候，也只报得出"给望海楼，37 号销"。林姨低头翻册子，找到那一行，把铃摇到 412。**册子上没有的，她一律挂断**——哪怕对方喊得再急、听着再像正经生意，她也只回一句："我这儿没这笔记录。"

铜插销并不多。但林姨精得很：第 37 根销正接着鱼市，同一根销她照样能再接一路去布庄——**一根销配一个去处，两两不撞，就不算冲突**。所以平日里三百个房间同时打电话，也从没出过岔子。

出岔子是在庙会那天。

那天全楼的人像商量好了一样，都要给同一个地方打电话：城南那家戏票行。第一位客人拿走一根销，第二位又一根，第三位、第四位……到晌午，册子上"找戏票行"那一栏已经排满，每一根铜插销都在这个去处上占了位。这时 251 房又摇铃来要戏票行，林姨手一摸——没销了。她只能回："占线，等等再来。" 可她抬头看那面板子，明明还有好些销在闲着——它们都还能打鱼市、打布庄、打码头，**唯独打不了戏票行**。

更磨人的是，那天挂了电话的人不少，销却没马上空出来。林姨的老习惯是：客人搁下听筒，那一行先不划掉，让它在册子上再挂一会儿，等沙漏走完一遍才销。她怕的是有迟到的回话——对面刚才那句"好嘞明天送到"要是这会儿才传回来，行没了，那句话就永远送不到 412 房。于是庙会那天下午，一半的销挂在**已经结束却还没敢销掉**的行上，明明没人说话，位子就是腾不出来。

到傍晚，连册子本身都写满了。册子是有页数的。新的话要不到行，林姨连"占线"都来不及回——铃响过，没人接，就那么落了空。

——到这儿你大概已经认出来了：林姨就是一台做 *SNAT* 的网关（NAT Gateway、egress IP、或者你集群外面那台防火墙），那本册子就是 *conntrack* 连接跟踪表，铜插销就是**源端口**。

它是什么：内网地址出不了公网，所以网关会把每条出向连接的源地址和源端口，改写成自己的公网 IP 加一个临时端口（*ephemeral port*），并在连接跟踪表里记一行五元组：`协议 + 源IP + 源端口 + 目的IP + 目的端口`。回包进来时按这一行反查、改写回去，送回真正的发起者。表里没有对应条目的包，一律丢弃。

关键在于**唯一性是按五元组算的**：同一个公网 IP 的同一个端口，去往不同目的地互不冲突；但**对同一个目的 IP+端口**，可用端口就那么六万多个。所以当大量 Pod 同时去连同一个下游（同一个 registry、同一个 S3 endpoint、同一个第三方 API），端口会在**那一个目的地上**被耗尽——AWS NAT Gateway 的上限是每个唯一目的地约 55,000 条并发连接，超了就报 `ErrorPortAllocation`，而此时网关整体带宽和连接数看上去都还很闲。再加上连接关闭后条目要等 `TIME_WAIT` / idle timeout 才回收，端口的实际周转比你想的慢得多。

它为什么重要：这是 OpenShift/ROSA 出向流量最典型的一类"莫名其妙的间歇性超时"。现象不是"网断了"，而是**只有去某一个地方的连接会失败，别的都好**；也不是持续失败，而是流量一冲高就失败。诊断要看的是 `ErrorPortAllocation` 指标和 `nf_conntrack_count`，而不是丢包率。缓解的方向也很清楚：多挂几个出口 IP（把端口池乘几倍）、给热门下游走 VPC Endpoint 绕开 NAT、用长连接和连接池减少连接数、必要时调短 timeout 让端口早点还回来。另外别忘了：NAT 是**有状态**的——回包必须经过同一台网关，路径不对称的话，册子上没有那一行，包就被静静丢掉了。

隐喻对应表：

- 望海楼的三百个房号 → 内网私有地址（出了这张网就没有意义）
- 楼外只知道"望海楼"这一个号码 → 网关的公网 IP（*SNAT* 后的源地址）
- 林姨与她的小屋 → 做 source NAT 的网关（NAT Gateway / egress node / 防火墙）
- 一排铜插销 → 可用的**源端口池**（约 64K 个 ephemeral port）
- 那本册子的每一行 → *conntrack* 表中的一条连接跟踪条目
- 行里记的"某号销、某房、找某处" → 五元组 `proto + srcIP + srcPort + dstIP + dstPort`
- 鱼市只听见"望海楼 37 号销" → 下游看到的源地址是网关，看不见真实客户端
- 册子上没有的一律挂断 → 无匹配条目的入向包被丢弃（NAT 的**有状态**性；也是路径不对称会挂掉的原因）
- 一根销可同时接鱼市和布庄 → 端口可复用，只要**目的地不同**就不冲突
- 庙会全楼都打同一家戏票行 → 大量客户端并发访问**同一个目的 IP+端口**
- 销用光了，别的去处却还通 → 端口耗尽是**按目的地**发生的（`ErrorPortAllocation`）
- 挂了电话仍留一会儿的行 → `TIME_WAIT` / idle timeout，条目延迟回收，端口周转变慢
- 怕迟到的回话找不到人 → 保留条目是为了正确处理迟到/重传的报文
- 册子本身写满、铃响没人接 → conntrack 表打满（`nf_conntrack_max`），新连接被静默丢弃
</section>
<section class="en" markdown="1">
The Harbour Hotel had three hundred rooms, and a telephone at every bedside. Not one of them could reach the world outside. Room numbers meant something only inside the building — tell a man in town "I'm in 412" and he'd only ask you: "412 of what?"

The world outside knew a single number: the Harbour Hotel. Every voice going out or coming in had to pass through one hot little room. In it sat Aunt Lin, facing an upright wooden board studded with a row of brass pegs, each peg wired to a line running into the city. A ledger lay open at her elbow, always.

The rule was simple. A guest in 412 wanted the fish market: he rang the little room first. Aunt Lin chose a free brass peg — say the thirty-seventh — pushed it home, and wrote a line in the ledger: **peg 37, room 412, calling the fish market.** Only then did she put the call through. The lad at the fish market picked up and heard "Harbour Hotel, peg thirty-seven." He would go his whole life not knowing a room 412 existed.

When the reply came back, it could name only "the Harbour Hotel, peg thirty-seven." Aunt Lin bent over the ledger, found the line, and rang 412. **Anything not in the ledger, she hung up on** — however urgent the caller sounded, however legitimate the business. She had one answer: "I've no record of that here."

There were not many pegs. But Aunt Lin was shrewd: peg 37 might be carrying the fish market and, at the same moment, a second call out to the cloth merchant — **one peg per destination, and no two collide, so no conflict at all.** Which is why three hundred rooms could all be talking at once on an ordinary day and nothing ever went wrong.

What went wrong, went wrong on temple-fair day.

That morning the whole hotel seemed to have agreed on it: everyone wanted the same place — the opera ticket office in the south of the city. The first guest took a peg, the second took another, then a third, a fourth. By noon the ledger's "calling the ticket office" column was full, every brass peg on the board spoken for against that one destination. Then 251 rang down wanting the ticket office, and Aunt Lin's hand came up empty. All she could say was: "Engaged. Try again later." Yet looking up at her board, she could see plenty of pegs sitting idle — every one of them still good for the fish market, the cloth merchant, the docks. **Only not for the ticket office.**

Worse, plenty of guests had hung up by then, and still the pegs didn't come free. Aunt Lin's long habit was this: when a guest set down the receiver, she did not strike the line out at once. She let it hang on the page a while longer, until the sand-glass had run through, and only then cleared it. What she feared was a late reply — if the "right you are, delivered tomorrow" from the other end came trailing back after the line was gone, that message would never find room 412. And so, all that afternoon, half her pegs were held by lines **already finished but not yet dared struck out**. Nobody speaking, and still no room on the board.

By evening the ledger itself had run out of pages. A ledger has only so many. A new call could get no line at all, and Aunt Lin hadn't even the chance to say "engaged" — the bell rang, nobody answered, and it simply fell into nothing.

— By now you've probably recognized it: Aunt Lin is a gateway performing *SNAT* (a NAT Gateway, an egress IP, the firewall in front of your cluster), the ledger is the *conntrack* connection-tracking table, and the brass pegs are **source ports**.

What it is: private addresses cannot travel the public internet, so the gateway rewrites the source address and source port of every outbound connection to its own public IP plus a temporary port (an *ephemeral port*), and records a five-tuple in its connection-tracking table: `protocol + srcIP + srcPort + dstIP + dstPort`. Return packets are matched against that entry, rewritten back, and delivered to the true originator. Packets with no matching entry are dropped.

The crux is that **uniqueness is computed over the whole five-tuple**: the same port on the same public IP can serve many different destinations without conflict — but **toward one specific destination IP and port**, there are only about sixty-odd thousand ports to go around. So when a great many Pods reach for the same downstream at once (the same registry, the same S3 endpoint, the same third-party API), ports are exhausted **against that one destination**. AWS NAT Gateway caps out around 55,000 simultaneous connections per unique destination; past that you get `ErrorPortAllocation` — while the gateway's overall bandwidth and connection count still look comfortably idle.

Why it matters: this is the classic shape of "inexplicable intermittent timeouts" on OpenShift/ROSA egress. The symptom isn't "the network is down"; it is that **only connections to one particular place fail, and everything else is fine** — and not failing steadily, but failing whenever traffic spikes. The thing to look at is the `ErrorPortAllocation` metric and `nf_conntrack_count`, not a packet-loss graph. The remedies follow directly: attach more egress IPs (multiplying the port pool), route hot downstreams through a VPC Endpoint to bypass NAT entirely, use keep-alives and connection pooling to need fewer connections at all, and shorten timeouts where appropriate so ports come back sooner. And don't forget: NAT is **stateful** — return traffic must come back through the same gateway. If the path is asymmetric, there's no line in the ledger, and the packet is dropped in silence.

Metaphor map:

- Three hundred room numbers → private addresses, meaningless outside their own network
- The world knowing only "the Harbour Hotel" → the gateway's public IP (the post-*SNAT* source address)
- Aunt Lin and her little room → the gateway doing source NAT (NAT Gateway / egress node / firewall)
- The row of brass pegs → the pool of available **source ports** (~64K ephemeral ports)
- Each line in the ledger → one entry in the *conntrack* table
- "Peg N, room R, calling P" → the five-tuple `proto + srcIP + srcPort + dstIP + dstPort`
- The fish market hearing only "Harbour Hotel, peg 37" → downstream sees the gateway as the source; the real client is invisible
- Hanging up on anything not in the ledger → inbound packets with no matching entry are dropped — NAT's **statefulness**, and why asymmetric routing breaks
- One peg serving both the fish market and the cloth merchant → ports are reusable so long as the **destination differs**
- The whole hotel calling one ticket office → many clients hitting the **same destination IP and port** concurrently
- Pegs gone while other destinations still work → port exhaustion happens **per destination** (`ErrorPortAllocation`)
- Lines left hanging after the receiver is down → `TIME_WAIT` / idle timeout: entries linger, so ports turn over slowly
- Fearing the late reply would find no one → entries are held to handle delayed and retransmitted packets correctly
- The ledger itself running out of pages → the conntrack table hitting `nf_conntrack_max`; new connections dropped silently
</section>
