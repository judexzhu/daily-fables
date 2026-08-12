---
layout: fable
title: "拉高车的人 · The Carter Who Loaded Too High"
title_zh: "拉高车的人"
title_en: "The Carter Who Loaded Too High"
concept: "Path MTU Discovery and PMTU Black Holes"
tags: [networking, linux, kubernetes]
illustration: /assets/art/2026-08-12-path-mtu-discovery.jpg
---
<section class="zh" markdown="1">
老周在河谷镇拉货，二十年了。

从镇口到下游的白石渡，一条水路边的官道，中间要穿过七座石拱桥。桥是不同朝代修的，高矮不一，而且老周从来没量过——他只知道货得送到。

那天他把箱子摞到了车顶能摞的最高，还在最上面那只箱子上用粉笔写了四个字：**不许拆包**。这是他的规矩：货是整的，要么整车过去，要么原样退回来，绝不允许沿路的人把他的箱子拆开分几趟运。分开运的货，收货人拼不回去，还容易丢。

第一座桥，过。第二座桥，过。第三座桥前，守桥人伸手拦住了他。

"过不去。"守桥人说。

"那你说多高能过？"

守桥人从怀里摸出一张小纸条，写了个数，塞给老周："我这道拱，只有七尺二。"

老周退回镇口，把最上面两层卸下来，重新摞成七尺二，又出发。第三座桥过了。第五座桥又拦住他,又是一张纸条：六尺八。老周再回去,再卸,再走。

来回折腾了三趟,他终于摸出了这条路上**最矮的那道拱**——不是每座桥都要量,只要知道那个最小的数,整条路就通了。他回到家,在自家门框上用粉笔写下"白石渡:六尺八",从此按这个数装车,一次成。

粉笔字他不擦,但也不当永久的。发大水改道、桥翻修加固,数就不作准了。所以每隔一阵子,他会故意装高一点试一次——万一路变宽敞了呢。

——真正的麻烦是从新来的那个守桥人开始的。

那年第四座桥换了人。新来的年轻人被上头交代:少跟外人搭话,别乱发纸条,省事。于是老周的车照旧驶进那道昏暗的拱洞,车顶擦到石头,货散了一地,车夫和骡子灰头土脸地退回来——**但是没有纸条**。

老周在镇口等回信,等不到。他不知道货卡在哪儿,也不知道该卸多少。他只知道:小件的货能到,大件的货一去不回。他试了又试,每次都装同样高,每次都消失在同一个黑洞里。他甚至开始怀疑是收货人在骗他。

后来是渡口的老账房解决的。老账房不去查是哪座桥,他只在镇口立了块牌子:**从这里出发的车,一律不许超过六尺**。宁可每车少装一点、多跑一趟,也好过货在半路人间蒸发。

——到这儿你大概已经认出来了:老周的车是 *IP packet*,粉笔写的"不许拆包"是 *DF (Don't Fragment) bit*,每座桥的高度是那一段链路的 *MTU*,守桥人递的纸条是 ICMP 的 *"Fragmentation Needed"* 报文,而那个不发纸条的年轻守桥人,就是运维最恨的 **PMTU black hole**。

## 这是什么

一个 IP 包从源到目的地要经过若干条链路,每条链路能承载的最大帧长(*MTU*)不同。整条路径上最小的那个值,叫 *Path MTU*。发送方并不预先知道它——**Path MTU Discovery (PMTUD)** 的做法是:先按本地接口 MTU 发,并置上 *DF* 位表示"不许分片";途中哪个路由器发现包太大又不能分片,就丢包并回一个 ICMP Type 3 Code 4 报文,里面带着"我这一跳只能过 N 字节"。发送方收到后缩小包长重发,如此反复,直到收敛到路径上的最小值,并把结果缓存起来(有过期时间,路径会变)。

它为什么重要:整条机制建立在**一个假设**上——ICMP 能回得来。而现实里防火墙、安全组、负载均衡器经常一刀切地丢掉所有 ICMP。这时候就出现最恶心的故障形态:小包(握手、ping、`curl -I`)全通,大包(实际数据、TLS 证书链、大 response)全部静默丢弃,连接卡死在半路,没有任何错误。

这在容器网络里尤其常见,因为 *VXLAN* / *IPsec* / *WireGuard* 这类封装每包都要吃掉几十字节的头部,把有效 MTU 从 1500 压到 1450 甚至更低。于是老账房那招——在 TCP 握手时把 *MSS* 直接改小(**MSS clamping**,`--clamp-mss-to-pmtu`)——成了最常用的兜底:不指望纸条回得来,干脆一开始就装矮点。

## 隐喻对应表

- **老周的货车** —— IP packet
- **车上的货高** —— packet size
- **粉笔写的"不许拆包"** —— *DF (Don't Fragment)* bit
- **每座石拱桥的高度** —— 每条链路的 *MTU*
- **整条路上最矮的那道拱** —— *Path MTU*
- **守桥人递回的纸条** —— ICMP *Fragmentation Needed* (Type 3, Code 4),带 next-hop MTU
- **来回卸货重装的三趟** —— PMTUD 的迭代收敛过程
- **门框上的粉笔字** —— PMTU cache(内核 route cache 里的条目)
- **隔一阵子故意装高试一次** —— PMTU 缓存老化后的重新探测
- **发大水改道 / 桥翻修** —— 路由变化导致路径 MTU 改变
- **不发纸条的新守桥人** —— 防火墙 / 安全组丢弃 ICMP
- **小件能到、大件人间蒸发** —— PMTU black hole 的典型症状:握手成功、传大数据卡死
- **镇口立的"一律不超六尺"牌子** —— *MSS clamping*(或直接调低接口 MTU)
</section>
<section class="en" markdown="1">
Old Zhou had been carting goods out of River Valley Town for twenty years.

The road to White Stone Ferry, downstream, runs along the canal and passes under seven stone arch bridges. They were built in different centuries, they are not the same height, and Zhou had never measured a single one of them. He only knew the goods had to arrive.

That morning he stacked the crates as high as the cart would take, and on the topmost crate he wrote four words in chalk: **do not unpack**. That was his rule. A load travels whole or it comes back whole; nobody along the road gets to break his crates open and ferry them across in pieces. Split loads never reassemble properly at the other end, and pieces go missing.

First bridge, through. Second bridge, through. At the third, the bridge keeper put out a hand and stopped him.

"You won't fit."

"Then how high will fit?"

The keeper pulled a small slip of paper from his coat, wrote a number on it, and handed it over. "My arch takes seven foot two. No more."

Zhou drove back to town, took two layers off the top, restacked to seven-two, and set out again. The third bridge let him through. The fifth stopped him — another slip of paper: six foot eight. Back to town, unload, restack, go.

After three round trips he had found what he actually needed: not the height of every bridge, but **the lowest arch on the whole road**. One number and the entire route opens up. He went home and chalked it on his own door frame — *White Stone Ferry: six-eight* — and loaded to that figure ever after, one trip, no drama.

He never wiped the chalk off. But he never quite trusted it forever either. Floods reroute the road; bridges get rebuilt and reinforced. So every so often he'd deliberately load a little high and try — in case the road had grown generous.

— The real trouble started with the new keeper.

That year the fourth bridge changed hands. The young man who took over had been told from above: don't chat with strangers, don't go handing out notes, keep it simple. So Zhou's cart rolled into that dim arch as usual, the top crates scraped stone, the load came apart, and the carter and his mule backed out filthy and shaken — **but with no slip of paper**.

Zhou waited at the town gate for word. No word came. He didn't know which bridge had stopped him or how much to take off. All he knew was this: small loads arrived, large loads left and were never heard from again. He tried, and tried, stacking to the same height every time, and every time the cart vanished into the same dark hole. He started to suspect the customer was lying to him.

It was the old clerk at the ferry office who finally solved it. He never bothered finding out which bridge was to blame. He simply put up a board at the town gate: **no cart leaving here may load above six foot**. Better to carry a little less on every trip than to have goods evaporate somewhere in the middle of the road.

— By now you've probably recognised it: Zhou's cart is an *IP packet*, the chalked "do not unpack" is the *DF (Don't Fragment)* bit, each arch height is a link's *MTU*, the keeper's slip of paper is an ICMP *"Fragmentation Needed"* message — and the young keeper who was told not to hand out notes is the thing every operator dreads: a **PMTU black hole**.

## What this actually is

An IP packet crosses a series of links on its way to a destination, and each link has its own maximum frame size (*MTU*). The smallest value along the whole route is the *Path MTU*, and the sender doesn't know it in advance. **Path MTU Discovery (PMTUD)** works like this: send at the local interface MTU with the *DF* bit set, meaning "do not fragment this." Any router that finds the packet too large to forward and is forbidden to fragment it drops the packet and returns an ICMP Type 3 Code 4 message carrying "this hop only takes N bytes." The sender shrinks and retries, iterating until it converges on the path minimum — then caches the result, with an expiry, because paths change.

Why it matters: the entire mechanism rests on **one assumption** — that the ICMP message makes it back. In practice, firewalls, security groups, and load balancers routinely drop all ICMP as a blanket policy. That produces the nastiest failure shape in networking: small packets (handshakes, pings, `curl -I`) all succeed, large packets (actual payload, TLS certificate chains, big responses) are silently discarded, and the connection just hangs with no error anywhere.

This bites hardest in container networking, where encapsulation like *VXLAN*, *IPsec*, or *WireGuard* eats tens of bytes of header per packet and drops the effective MTU from 1500 to 1450 or lower. Which is why the old clerk's move — rewriting the *MSS* down during the TCP handshake (**MSS clamping**, `--clamp-mss-to-pmtu`) — is the standard defence: stop depending on a note coming back, and just load lower from the start.

## Metaphor mapping

- **Zhou's cart** — an IP packet
- **How high the cart is loaded** — packet size
- **The chalked "do not unpack"** — the *DF (Don't Fragment)* bit
- **The height of each stone arch** — the *MTU* of each link
- **The lowest arch on the whole road** — the *Path MTU*
- **The slip of paper handed back** — ICMP *Fragmentation Needed* (Type 3, Code 4), carrying the next-hop MTU
- **Three round trips of unloading and restacking** — the iterative convergence of PMTUD
- **Chalk on the door frame** — the PMTU cache (kernel route cache entry)
- **Occasionally loading high on purpose** — re-probing after the cached PMTU ages out
- **Floods and rebuilt bridges** — routing changes that alter the path MTU
- **The keeper who hands out no notes** — a firewall or security group dropping ICMP
- **Small loads arrive, big loads vanish** — the classic PMTU black hole symptom: handshake fine, bulk transfer hangs
- **The board at the town gate** — *MSS clamping* (or simply lowering the interface MTU)
</section>
