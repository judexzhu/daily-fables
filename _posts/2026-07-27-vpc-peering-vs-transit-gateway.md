---
layout: fable
title: "架不完的私桥，与河心的驿场 · The Bridges You Can't Stop Building, and the Yard in the River"
title_zh: "架不完的私桥，与河心的驿场"
title_en: "The Bridges You Can't Stop Building, and the Yard in the River"
concept: "vpc-peering-vs-transit-gateway"
tags: [aws, networking]
illustration: /assets/art/2026-07-27-vpc-peering-vs-transit-gateway.jpg
---
<section class="zh" markdown="1">
河谷里散落着好几座庄园，每一座都围着高墙，自成一方天地——庄里有自己的道路、货栈、集市，外人进不来，庄里的车也出不去，除非专门为它架一座桥。

起初只有红庄和蓝庄想通商。两家各出工匠，在两庄之间架起一座_私桥_，桥的两头各设一道门，只认这两家的马车。红庄的车能直抵蓝庄的货栈，蓝庄的车也能回访红庄。生意兴隆，两庄都很满意。

后来绿庄也想加入。于是红庄与绿庄之间又架起一座私桥。现在红庄既通蓝庄、又通绿庄，像个小小的枢纽。可有一天，蓝庄的商人要把货送去绿庄，他驾车驶上通往红庄的桥，到了红庄，却被门吏拦下:"我这座桥只认红蓝两家的车。你要去绿庄，自己去和绿庄架一座桥。"——桥是不能_借道接力_的。蓝要到绿，红庄这座现成的桥半点忙帮不上，非得蓝绿之间再单独架一座不可。

庄园越来越多，麻烦也越来越大。四座庄园两两互通，要六座桥;五座要十座;等到十座庄园都想彼此往来,竟要架_四十五_座桥。而且每新来一座庄园,就得同时与现有的每一座各架一座新桥、各开一道门、各记一本账。工匠们疲于奔命,河谷上空私桥密如蛛网,谁也说不清哪座通着哪座。

这时一位老驿丞想了个别的法子。他不再在庄与庄之间架桥,而是在河谷正中的空地上,圈起一座大大的中央_驿场_。规矩全变了:每座庄园只需修_一条_路,通到这座驿场,仅此一条。红庄一条、蓝庄一条、绿庄一条,后来的庄园也都只修这一条。

驿场正中坐着驿丞,案头摊着一本大_账_:哪条路通往哪座庄园,记得清清楚楚。蓝庄的车要去绿庄,只管驶进驿场,报一声"往绿庄";驿丞翻账、一挥手,指向绿庄那条路,车便借着驿场_转接_过去了。蓝去绿、绿去红、红去橙,全在这一座驿场里周转,再没有哪两家需要单独架桥。

从前十座庄园要四十五座私桥;如今只要十条路,外加河心一座驿场。再来一百座庄园,也不过是各修一条路而已。当然,天下没有白转的车:每一辆进驿场周转的马车,都要在门口交一枚_过路钱_,驿丞靠这个养着驿场。庄主们算过账,都觉得这钱花得值——省下的,是那漫山遍野再也理不清的私桥。

——到这儿你大概已经认出来了:庄与庄之间那种只认两家、不能借道接力的私桥,就是 VPC _peering_;而河心那座人人只修一条路、却能借它转去任何一家的中央驿场,就是 _Transit Gateway_。

**这是什么、为什么重要。** VPC _peering_ 把两个 VPC 直接对接,是_点对点_且_非传递_(non-transitive)的:A–B 有 peering、B–C 有 peering,并不会让 A 通到 C——每一对要通信的 VPC 都得自己拉一条 peering。于是 N 个 VPC 全互联需要 `N(N-1)/2` 条连接,随规模 _O(N²)_ 爆炸,每条还要在两边各自维护路由表条目,大规模下几乎无法管理。_Transit Gateway_ 则是一个 region 级的中心路由 _hub_:每个 VPC 只 _attach_ 一次,TGW 用它自己的 _route table_ 做_传递路由_,于是任意两个挂上来的 VPC 都能互通,连接数从 O(N²) 降到 _O(N)_。代价是 TGW 有每小时的 attachment 费和每 GB 的数据处理费,而且所有跨 VPC 流量都要经过这个中心点(需要靠多张 route table 做分段隔离、并留意它可能成为瓶颈)。当你有几十上百个 VPC、跨多账户、还要接 VPN / Direct Connect 做混合云时,中央驿场几乎总是比满谷私桥更划算。

**隐喻对应表:**

- 一座座围墙庄园:一个个 _VPC_(彼此隔离的私有网段)
- 两庄之间只认两家车的私桥:一条 VPC _peering_ 连接,点对点、只连这两个 VPC
- 门吏不许借红庄的桥去绿庄:peering 的_非传递性_——A–B 与 B–C 不让 A 到 C
- 庄园两两架桥、桥数翻着番爆长:全互联需 `N(N-1)/2` 条 peering,_O(N²)_ 规模爆炸
- 河谷正中的中央驿场:_Transit Gateway_(region 级中心路由 hub)
- 每庄只修一条通往驿场的路:每个 VPC 只 _attach_ 一次到 TGW,连接数降到 _O(N)_
- 驿丞案头那本大账:TGW 的 _route table_(路由表)
- 报一声目的地、驿丞翻账挥手转接:TGW 的_传递路由_,任意两个 attachment 互通
- 进驿场交的过路钱:TGW 的每 GB 数据处理费 + 每小时 attachment 费
- 所有车都从这一座驿场周转:中心 hub 是流量必经点(靠多张 route table 分段,也需防其成瓶颈)
</section>
<section class="en" markdown="1">
Several estates are scattered across the river valley, each ringed by a high wall, each a world of its own — with its own roads, warehouses, and market inside. No outsider gets in, and no cart gets out, unless a bridge is built for it.

At first only the Red estate and the Blue estate wished to trade. Each sent its masons, and between the two they raised a _private bridge_, a gate at either end that admitted only these two houses' carts. Red's carts could roll straight to Blue's warehouse, and Blue's could visit Red in return. Business flourished; both were pleased.

Later the Green estate wanted in. So a second private bridge went up between Red and Green. Now Red reached both Blue and Green, a little hub of sorts. But one day a Blue merchant, carrying goods bound for Green, drove onto the bridge to Red — and at Red's gate the keeper stopped him: "This bridge admits only Red and Blue carts. If you want Green, go raise your own bridge to Green." A bridge cannot be _borrowed as a relay_. Red's ready-made bridge was of no help at all; Blue and Green had to build one of their very own.

As estates multiplied, so did the trouble. Four estates fully interconnected need six bridges; five need ten; and by the time ten estates all wished to reach one another, it took _forty-five_ bridges. Worse, every new estate had to raise a fresh bridge — a fresh gate, a fresh ledger — to _each and every_ existing one at once. The masons ran themselves ragged; private bridges webbed the sky over the valley, and no one could say anymore which crossed to which.

Then an old post-warden tried something else. He stopped bridging estate to estate. Instead, on the open ground at the very center of the valley, he enclosed one great central _yard_. The rule changed entirely: each estate need build only _one_ road, running to this yard, and only that. Red built one, Blue one, Green one; every later estate, only that one.

At the heart of the yard sat the warden, a great _ledger_ open before him: which road led to which estate, all set down plainly. When a Blue cart wanted Green, it simply drove into the yard and called out "to Green"; the warden turned a page, waved a hand toward Green's road, and the cart was _relayed_ onward through the yard. Blue to Green, Green to Red, Red to Amber — all turned about within this one yard, and no two houses ever needed a private bridge again.

Where ten estates had once needed forty-five bridges, now they needed ten roads and one yard in the river. Add a hundred more estates, and it is still just one road apiece. Of course, no cart turns for free: each one that passes through the yard drops a _toll_ at the gate, and on that the warden keeps the yard running. The estate lords did the sums and found it well spent — what they saved was the endless, untraceable thicket of private bridges.

— By now you have probably recognized it: the private bridge between two estates, admitting only those two and refusing to relay, is VPC _peering_; and the central yard in the river, where everyone builds a single road yet reaches anyone through it, is the _Transit Gateway_.

**What it is, and why it matters.** VPC _peering_ connects two VPCs directly, and it is _point-to-point_ and _non-transitive_: A–B peered and B–C peered does not let A reach C — every pair that must talk needs its own peering. So fully meshing N VPCs takes `N(N-1)/2` connections, an _O(N²)_ blow-up, each one requiring route-table entries maintained on both sides — nearly unmanageable at scale. A _Transit Gateway_ is instead a region-level central routing _hub_: each VPC _attaches_ just once, and the TGW uses its own _route table_ to do _transitive routing_, so any two attached VPCs can reach each other — dropping the connection count from O(N²) to _O(N)_. The price is that a TGW charges an hourly fee per attachment and a per-GB data-processing fee, and all cross-VPC traffic funnels through this central point (you segment it with multiple route tables and watch that it doesn't become a bottleneck). When you run dozens or hundreds of VPCs across many accounts, plus VPN / Direct Connect for hybrid cloud, the central yard almost always beats a valley full of private bridges.

**Metaphor mapping:**

- The walled estates: individual _VPCs_ (isolated private networks)
- A private bridge admitting only two houses: one VPC _peering_ connection — point-to-point, just those two VPCs
- The keeper refusing to relay through Red's bridge to Green: peering's _non-transitivity_ — A–B and B–C don't give A→C
- Bridges multiplying pair by pair: full mesh needs `N(N-1)/2` peerings, an _O(N²)_ blow-up
- The great yard at the valley's center: the _Transit Gateway_ (region-level central routing hub)
- Each estate building a single road to the yard: each VPC _attaches_ once to the TGW — connections fall to _O(N)_
- The warden's open ledger: the TGW _route table_
- Calling a destination, the warden relaying you onward: TGW _transitive routing_ — any two attachments interconnect
- The toll dropped on entering the yard: TGW's per-GB data-processing fee + hourly attachment fee
- Every cart turning through this one yard: the central hub is on every path (segmented via multiple route tables, and a potential bottleneck)
</section>
