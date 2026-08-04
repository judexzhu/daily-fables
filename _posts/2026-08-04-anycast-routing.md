---
layout: fable
title: "每个村口都有一家石磨坊 · The Same Sign in Every Village"
title_zh: "每个村口都有一家石磨坊"
title_en: "The Same Sign in Every Village"
concept: "Anycast routing (BGP anycast)"
tags: [networking, aws, distributed-systems]
illustration: /assets/art/2026-08-04-anycast-routing.jpg
---
<section class="zh" markdown="1">
从北原到南渡，隔着七八个村子。村与村之间只有土路，和路口那一根歪脖子木头指路牌。

这一带有件怪事：每隔几十里，就有一家"石磨坊"。招牌一样，木头的颜色一样，连门口那口歪着的水缸都一样。外乡人第一回来，总要问："到底哪家才是石磨坊？"本地人只笑："都是。"

你要给石磨坊捎个口信，不必写清是哪个村。你只要把口信交给邮差，说一句"送到石磨坊"。

邮差走到第一个路口，抬头看牌子——上头刻着"石磨坊"，箭头朝西，底下一行小字："三站路"。他就往西走。走到下一个路口，又有一块牌子，也写着石磨坊，这回箭头朝北，"两站路"。他照着走。第三个路口的牌子直接指着眼前一扇门。他推门进去，把口信交了，转身回家。

他从来不知道自己送到的是哪一家。他也不需要知道。

这些牌子不是官府立的。是驿站的人自己吆喝出来的。哪家石磨坊开张，头一天就跟最近的驿站说一声："我这儿是石磨坊。"驿站便刻一块牌子，写"石磨坊，零站路"，然后转身冲隔壁驿站喊："我这边通石磨坊，一站路。"隔壁听见了，也刻一块，写"两站路"，再往外喊。一圈一圈传出去，整个北原到南渡的路口就都有了牌子。

每个路口只记一件事：**朝哪边走最近**。要是同时听见两家吆喝，就留下站数少的那一块，另一块丢掉。所以口信总是去最近的那一家——谁也没安排过，它自己就是这样。

去年冬天，西边那家的磨盘裂了。掌柜的没有关门大吉，也没有让人四处传话说"以后改送别家"。他只做了一件事：跟驿站说，"我这几天不吆喝了。"驿站的牌子撤下来，消息一圈圈传出去，沿路的牌子跟着改了朝向。第二天清早，本该往西去的邮差走到路口，抬头看见箭头朝北，写着"四站路"。他就往北走了。他甚至没觉出哪里不对——口信照样送到了石磨坊。

也不全是好事。

有一年庙会开在东头，半个县的口信都从东边来。东边那家石磨坊挤得水泄不通，伙计从天亮忙到掌灯；而西边那家，一整天只进来三个人。指路牌只认路近，不认谁忙——**近的那家就是要挨着**。掌柜的想匀一匀，办法也粗糙：要么在西边再开两家，要么干脆让东边那家歇一天不吆喝，把人往别处赶。

还有更磨人的。有位客商要跟石磨坊谈一笔大生意，前后得来回捎七八趟口信。头三趟都送到了西边那家，谈得好好的。第四趟出门的时候，中间某个路口的牌子刚巧改了朝向——邮差照着牌子走，把第四句话送进了北边那家的门。北边的掌柜听得一头雾水："什么叫'就按昨天说的办'？昨天我们说什么了？"这一单就这么黄了。

后来跑生意的都学乖了：**一句话说得完的事，交给石磨坊最省心；要来回扯上半天的事，头一趟先问清那家的私名，往后指名送。**

——到这儿你大概已经认出来了：石磨坊那块到处都一样的招牌，就是一个 *anycast* 地址。

**概念解释。** anycast 是让同一个 IP 前缀（比如 `1.1.1.1/32`）在多个地理上分散的站点同时通过 *BGP* 对外通告。中间的路由器并不知道"这个地址有好几份"，它只是按 BGP 的选路规则（local pref、AS path 长度、MED、IGP metric）为这个前缀挑出唯一一条最优路径，装进转发表。结果是：每个客户端的报文被送到**网络拓扑意义上最近**的那个站点——注意是拓扑最近，不一定是地理最近。

**为什么重要。** 第一，故障切换不改地址、不等 DNS TTL：站点停止通告（*withdraw*），BGP 收敛后流量自动流向次优站点，客户端全程无感；同理，维护前先撤通告就是一次干净的 drain。第二，DDoS 被路由系统天然分摊，攻击流量在各自最近的站点就地吸收，而不是全砸向一个机房。第三，代价是**负载分布由拓扑决定，不由你决定**：热点区域的站点可能被打满而远处站点闲置，只能靠加站点、AS path prepend、调 local pref 或撤通告来粗调。第四，无状态、单次往返的协议最适配——DNS over UDP 是经典用例（根服务器、`8.8.8.8`、`1.1.1.1` 都是 anycast）。长连接理论上会在路由变动时被打断：新路径落到另一个站点，那边没有这条 TCP/TLS 状态，直接 RST；实践中路由足够稳定，所以 CDN 和 AWS Global Accelerator 也用 anycast 承载 TCP/TLS，但必须把"路由翻动 = 连接重置"当成正常事件来重试。最后，别把它和 load balancer 混为一谈：LB 是在某一台设备上分发，anycast 是把分发这件事交给互联网的路由系统本身，因此没有那个单点。

**隐喻对应表：**

- 到处都一样的"石磨坊"招牌 → *anycast* IP 地址（同一前缀在多处通告）
- 每一家石磨坊本身 → 各个站点 / PoP（边缘节点）
- 路口的木头指路牌 → 路由器的转发表条目
- 驿站一圈圈往外吆喝 → *BGP* 路由通告的传播
- 牌子上的"几站路" → AS path 长度等选路 metric
- 听见两家只留站数少的那块 → BGP best path selection（每个前缀只留一条）
- 邮差不知道送到了哪一家 → 客户端对具体站点完全无感知
- 磨盘裂了就"不吆喝了" → route *withdraw*，故障切换与站点 drain
- 牌子改朝向后邮差自动改道 → BGP 收敛，流量自行转移
- 庙会那天东边挤爆、西边清闲 → 负载按拓扑分布，而非按容量均分
- 半路改牌子，第四句话送错了门 → 路由翻动打断长连接（新站点没有状态 → RST）
- 一句话说完的事最省心 → 无状态短交互（DNS/UDP）是最佳场景
- 谈大生意先问私名再指名送 → 用 anycast 做入口发现，长会话切到 unicast 地址
</section>
<section class="en" markdown="1">
From Northfield to Southferry lie seven or eight villages, joined by nothing but dirt roads and, at each crossroads, one crooked wooden fingerpost.

There is an oddity in these parts: every twenty miles or so, there is a "Stonemill Bakery." Same sign, same weathered wood, down to the same lopsided water jar by the door. Strangers always ask, the first time: "But which one is *the* Stonemill?" The locals just smile. "All of them."

If you want to send word to the Stonemill, you needn't say which village. You hand your message to a postman and say, simply, "To the Stonemill."

At the first crossroads he looks up. The post reads *Stonemill*, arm pointing west, and beneath it in smaller letters: "three stages." So he walks west. At the next crossroads, another post, also *Stonemill*, this time pointing north: "two stages." He follows it. At the third crossroads the arm points at a door right in front of him. He pushes it open, hands over the message, and walks home.

He never learns which Stonemill he delivered to. He doesn't need to.

Nobody official put those signs up. The relay houses shouted them into existence. On the day a Stonemill opens, its keeper tells the nearest relay house: "There is a Stonemill here." The relay carves a board — *Stonemill, zero stages* — then turns and calls to the next relay house down the road: "I have a way to the Stonemill, one stage." That one carves its own board saying two, and calls onward. Ring by ring the shout travels out, until every crossroads from Northfield to Southferry has a board.

Each crossroads remembers exactly one thing: **which way is nearest**. If it hears two shouts, it keeps the board with the smaller number and throws the other away. So messages always end up at the closest Stonemill. Nobody arranged this. It simply falls out that way.

Last winter the western mill cracked its grindstone. The keeper did not shutter the place, and he sent no runners around the county announcing a new address. He did one thing only: he told the relay house, "I'll stop shouting for a few days." The board came down; the silence propagated ring by ring; the fingerposts along the roads quietly swung to face elsewhere. Next morning a postman who would have gone west looked up and saw the arm pointing north — "four stages" — and went north instead. He never noticed anything had happened. The message reached the Stonemill all the same.

It is not all gift, though.

One year the fair was held at the eastern end, and half the county's messages came from the east. The eastern Stonemill was mobbed from dawn to lamplight, while the western one saw three visitors all day. The fingerposts know only what is near; they know nothing of who is busy — **the closest mill simply takes the beating**. The only remedies were blunt: open two more mills in the west, or have the eastern one stop shouting for a day and push the crowd elsewhere.

And worse. A merchant once had a large deal to negotiate with the Stonemill, seven or eight messages back and forth. The first three went to the western mill, and it was going well. When the fourth set out, a fingerpost somewhere in the middle had just swung around — the postman followed it faithfully and delivered the fourth sentence through a northern door. The keeper there read it and blinked: "'Proceed as we agreed yesterday' — what did we agree yesterday?" The deal died on the spot.

Traders learned the lesson: **anything you can say in one sentence, send it to the Stonemill. Anything that takes an afternoon of back-and-forth — ask on the first trip for that mill's private name, and address everything after that to the name.**

— By now you have probably recognized it: that identical sign hanging in every village is an *anycast* address.

**The concept.** Anycast means announcing the same IP prefix (say `1.1.1.1/32`) via *BGP* from many geographically separate sites at once. The routers in between have no idea there are several copies; each simply applies BGP best-path selection (local pref, AS path length, MED, IGP metric) to pick exactly one route for that prefix and installs it. The effect is that every client's packets land at the site that is **nearest in network topology** — topologically nearest, which is not always geographically nearest.

**Why it matters.** First, failover needs no address change and no DNS TTL wait: a site *withdraws* its announcement, BGP reconverges, traffic slides to the next-best site, and clients notice nothing — the same trick makes maintenance drains clean. Second, DDoS traffic is absorbed at whichever site is closest to each attacker instead of concentrating on one datacenter. Third, the price is that **load distribution is dictated by topology, not by you**: a site near a hot region can be saturated while a distant one idles, and your only levers are adding sites, AS path prepending, local pref tuning, or pulling an announcement. Fourth, stateless single-round-trip protocols fit best — DNS over UDP is the canonical case (the root servers, `8.8.8.8`, `1.1.1.1` are all anycast). Long-lived connections can in principle be severed by a route change: the new path lands at a different site, which holds no TCP/TLS state for that flow and answers with a RST. In practice routes are stable enough that CDNs and AWS Global Accelerator carry TCP/TLS over anycast anyway — but you must treat "route flap equals connection reset" as an ordinary event and retry. Finally, do not confuse it with a load balancer: an LB spreads traffic inside one box; anycast hands the spreading to the internet's own routing system, and so has no such single point.

**Metaphor mapping:**

- The identical "Stonemill" sign everywhere → the *anycast* IP address (one prefix announced from many places)
- Each individual Stonemill → a site / PoP (edge location)
- The wooden fingerpost at each crossroads → a router's forwarding table entry
- Relay houses shouting outward, ring by ring → *BGP* route announcement propagation
- "Three stages" carved under the arm → AS path length and other selection metrics
- Hearing two shouts, keeping the smaller number → BGP best path selection (one route per prefix)
- The postman never knowing which mill he reached → the client is unaware of the specific site
- "I'll stop shouting for a few days" → route *withdraw*, failover and site drain
- Fingerposts swinging, postmen rerouting themselves → BGP reconvergence, traffic shifts automatically
- The fair day: east mobbed, west idle → load follows topology, not capacity
- A post swinging mid-negotiation, sentence four misdelivered → route flap breaking a long-lived connection (no state at the new site → RST)
- One-sentence errands are safest → stateless short exchanges (DNS/UDP) are the ideal fit
- Asking for the mill's private name before a long deal → anycast for entry discovery, unicast address for long sessions
</section>
