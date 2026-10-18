---
layout: fable
title: "第二封信使 · The Second Messenger"
title_zh: "第二封信使"
title_en: "The Second Messenger"
concept: "Hedged Requests"
tags: [sre, distributed-systems, performance]
illustration: /assets/art/2026-10-17-hedged-requests-tail-latency.jpg
youtube_id: "1dcYFRa1UyI"
---
<section class="zh" markdown="1">
雾岭镇坐落在大山的褶皱里，镇上只有一家药铺，叫回春堂。

回春堂有一桩要紧的营生：镇上人家半夜急病——小儿高热、产妇血崩——药铺柜上常见的方子都有，唯独几味冷僻的旧方，只存在山里。云雾深处有三座古寺，云隐寺、雪窦寺、灵岩寺，寺寺都藏着一整套完整的《百草丹经》。哪座寺近、哪座寺有缘，就派信使去哪座寺誊抄。

老规矩是老掌柜定的：**一次只派一名信使。**

"多派一个，就是白养一个闲人。"老掌柜捻着胡子说。信使阿满腿脚快，翻山越岭，一来一回通常一炷香的功夫。可是不知怎么，有时一炷香烧完了，人没回来；两炷香烧完了，人还没回来。等到第三炷香烧过半，阿满才一身泥水地撞进门——半路塌了桥，在渡口排了一个时辰的队，又赶上寺里晚课，知客僧誊方誊得慢。

问急了，老掌柜也没办法："你总得等。万一前头那个已经抄好了呢？再等等。"

镇上人最怕的就是这三个字：再等等。等，就是拿人命赌信使没摔下山崖。

沈鹊接手回春堂那年，做的第一件事不是改方子，是记账。

她把过去一百趟取方的行程都记在小册子上，烧着香数：一百趟里，九十六趟在一炷香之内回来；剩下四趟，有的一炷半，有的快烧到三炷。**毛病不在"平均多快"，在那最慢的几趟。**她还发现一件更扎心的事：抓一副"七味回魂汤"，七味药要问过七间库房，七间库房都点头，这副药才算齐——七间里只要有一间人手忙不过来，整副药就得陪着等。问的门越多，撞上"慢"的机会越大。**最后回来的那趟，定死了整副药的时辰。**

第二件事，才是改规矩。

新规矩三条。

第一条：**照旧只派一个信使，先派近的。**不许一上来就往三座寺各派一个——那是把三座寺的门都堵上。

第二条：**在香上刻一道红线。**红线的位置大有讲究：刻在一炷香烧到"十趟里九趟能回来"的地方。信使出门，点香。香烧到红线，人还没回来，即刻加派第二名信使——**往另一座寺去**，不走阿满那条路。

第三条：**先回来的方子生效。**两份方子谁先到用谁的；迟到的那个，好酒好饭招待，方子原封不动地退回去——那份辛苦白费了，认了。

镇上的老先生们炸了锅。"胡闹！白白多养一倍的信使！""烧到红线就加人？那不成了打两张牌？"

沈鹊翻开账册："急什么。红线刻在九成六能赶回的地方——一百趟里，只有四趟会烧过红线。也就是说，**一百趟里九十六趟，还是一名信使，一个铜板不多花**。可那最慢的四趟，从前要等三炷香，如今第二个人从另一条路、另一座寺包抄过去，多半一炷半就回来了。**平常的日子一文不多花，要命的日子有人来救。**"

账册还有后话：往另一座寺加派，救的是"这一条路慢"；可若三座寺的路口一起起了大雾，信使们全堵在山腰上，加几个也是白搭。那一年腊月大雾封山，沈鹊没再派人，改在镇口点起了狼烟，请各寺的僧人看着狼烟把方子送下山来——那是另一码事了。

至于"一上来就往三座寺齐发"的主意，邻镇回生堂倒是试过。瘟疫那年，他们家掌柜心急，每一单急症都三名信使同时出门，三座寺门口排起了长队，知客僧誊方誊到手软，原本一炷香的活儿堵成了三炷香。**齐发图快，反而把寺门挤成了渡口。**回春堂的红线规矩传开之后，邻镇掌柜专程来抄了帖子。

那年之后，回春堂的急症簿上再没记过"三炷香"。

讲到这儿，你多半已经认出来了：这就是分布式系统里的 *hedged requests*——对冲请求，对付长尾延迟的招数。

---

### 这是什么

*Hedged request*（对冲请求）是一种**客户端**的长尾延迟对策：把请求发给一个副本后**先等一小会儿**——通常等到该类请求延迟分布的 95 分位（*p95*）——若还没得到响应，就**再向另一个副本发一份相同的请求**；**先回来的响应生效，迟到的直接丢弃**。

主意出自 Jeffrey Dean 与 Luiz André Barroso 2013 年的文章《The Tail at Scale》。文中给出一组 Google *BigTable* 的实测：把对冲时机设在 p95，长尾（*p99.9*）延迟从约 1,800 毫秒压到 74 毫秒，代价只是**约 1% 的额外请求**——绝大多数请求根本不会触发第二份拷贝。

它是"延迟容忍"（*tail-tolerant*）设计里最便宜的一招。同门的变体还有更激进的 *tied requests*（同时发给多个副本、留下"待撤销"记号）；生产系统里，AWS 曾公开分享过 *DynamoDB* 用对冲读（*hedged reads*）压尾延迟的实践。注意它与 *request coalescing*／*singleflight* 恰好是一对镜像：**那是服务端把多个相同的并发请求合并成一次；这是客户端把一个请求复制成多份赛跑**——方向相反，常常搭配着用。

### 为什么重要

1. **长尾会被放大成常态**：一个用户请求在服务端常常扇出成成百上千个后端调用（七味药、七间库房），整体延迟由**最慢的一个**决定。扇出越宽，"撞上一次慢"就越不是小概率——平均值漂亮，p99 却要人命。
2. **慢的来源防不胜防**：共享服务器上的排队、后台守护任务、*GC* 停顿、限流、坏盘的慢响应——这些方差无法根除，只能被"对冲"。
3. **便宜的保险**：阈值设在 p95，意味着一百次里九十几次不多花一分钱；真正拖长尾的少数请求，却被另一条路径救回。极少的技术能用 1% 的开销换十倍以上的尾部改善。
4. **有边界，也有纪律**：对冲救的是"这一条路慢"（独立方差），救不了"三座寺一起封山"（相关故障）；而一上来就向所有副本齐发，会把副本压垮——回生堂的长队就是教训。**等一等，再对冲**，是这个模式全部的克制所在。

_隐喻对应表_

- 取急方的回春堂 → 发请求的客户端（*client*）
- 三座各藏一部全本《百草丹经》的古寺 → 持有同一份数据的多个副本（*replicas*）
- 上山誊方的信使 → 一次 RPC 请求（*request*）
- 香上的红线（十趟九趟能赶回的刻度） → 对冲阈值，设为延迟分布的 p95（*hedge threshold*）
- 香过红线即加派第二人、往另一座寺 → 对冲请求发往另一个副本（*hedged duplicate*）
- 先回来的方子生效、迟到者原封退回 → 首个响应胜出，后续重复响应被丢弃（*first response wins*）
- 一百趟里只有四趟加派 → 仅约 1%–5% 的请求产生额外负载（*load amplification*）
- 迟到信使的白跑一趟 → 丢弃重复响应浪费的服务端工作（*wasted work*）
- 七味药问七间库房、最慢一间定乾坤 → 高扇出下整体延迟取各调用的最大值（*tail amplification*）
- 大雾封三山、加人也无用 → 相关故障（*correlated failure*），对冲救不了
- 回生堂三寺齐发、寺门堵成长队 → 向所有副本立即复制的"蜂拥"反噬（*herd / overload*）
- 老掌柜的"再等等" → 无超时的盲目等待（*wait without timeout*）
</section>
<section class="en" markdown="1">
Mist-Ridge Town sat folded into the mountains, and it had exactly one pharmacy: Spring-Return Hall.

Spring-Return Hall had one weightiest trade: townsfolk struck down at midnight—infants burning with fever, mothers losing blood. The common prescriptions sat in its drawers; but a handful of rare old formulas existed only in the mountains. Deep in the clouds stood three ancient temples—Cloud-Hidden, Snow-Mouth, and Spirit-Crag—each holding a complete set of the *Hundred-Herb Canon*. Whichever temple was nearer, or luckier, the shop sent a runner there to copy out the prescription.

The old rule was the old master's: **one runner at a time.**

"A second runner is a second mouth to feed for nothing." Runner A-Man had strong legs; over the ridge and back usually took the burn of one incense stick. Yet sometimes the stick burned out and no one came. A second stick burned out; still no one. When the third was half gone, A-Man would crash through the door covered in mud—a bridge had washed out, or he'd queued an hour at the ford, or the temple's evening service had kept the guest-monk from his brush.

Pressed for an answer, the old master could only shrug: "You must wait. What if he's already copied it? Wait a little longer."

What the town feared most were those words: *a little longer*. Waiting meant betting a life that the runner hadn't fallen off the cliff.

The year Shen Que took over Spring-Return Hall, her first act was not to change a single prescription. It was to keep a ledger.

She recorded the last hundred formula-runs, timing each against burning incense: ninety-six of a hundred came back within one stick. The remaining four might take one and a half, or nearly three. **The sickness was not in the average, but in those slowest few.** And she found something worse: to fill the Seven-Ingredient Returning-Soul Decoction, seven storerooms had to be asked, and the decoction was ready only when all seven nodded—if one storeroom's hands were full, the whole decoction waited. The more doors you asked, the better your odds of knocking on a slow one. **The last door to answer fixed the hour of the whole prescription.**

Her second act was to change the rules.

Three rules.

Rule one: **still send only one runner—first to the nearer temple.** Never dispatch to all three temples at once—that jams every temple gate in the mountains.

Rule two: **carve a red line into the incense stick.** The line's placement mattered greatly: it sat at the point where, in ten runs, nine had made it home. The runner departs; the incense is lit. If the stick burns down to the red line and no one has returned, immediately send a second runner—**to a different temple**, by a path A-Man is not on.

Rule three: **the first prescription back goes to work.** Whichever copy returns first is used; the latecomer gets a warm meal and his scroll returned unopened—that trip was wasted, and we accept it.

The town elders erupted. "Nonsense! Twice the runners to feed!" "Send a second man at a red line? Playing two tiles at once?"

Shen Que opened the ledger. "Calm down. The line sits where ninety-six runs in a hundred have already come home—only four in a hundred will ever burn past it. **Ninety-six runs in a hundred still cost one runner, not one coin more.** But those slowest four, which once waited three sticks, now get rescued: a second runner closes in from another path, another temple, and is back in a stick and a half more often than not. **On ordinary days, spend nothing extra; on deadly days, send rescue.**"

The ledger had a postscript: dispatching to a *different* temple rescues "this one path is slow." But when fog seals all three mountain roads at once, runners stack up on the mountainside, and a second or third helps nothing. One twelfth-month, when the great fog shut every pass, Shen Que sent no one at all—she lit the wolf-smoke beacon at the town gate, and the monks, watching for the smoke, carried the formulas down themselves. That is another tale.

As for "send to all three temples at once"—the rival pharmacy, Life-Return Hall in the next valley, did try it. In the plague year their impatient master dispatched three runners for every emergency, every time. Queues coiled at all three temple gates; guest-monks' brushes cramped; work that took one incense stick jammed into three. **Flooding every path to look fast had turned the temple gates into a ford.** After Spring-Return Hall's red-line rule spread, the rival master came in person to copy the posting.

From that year on, Spring-Return Hall's emergency register never again recorded "three incense sticks."

By now you've probably recognized it: this is *hedged requests*—the distributed-systems answer to tail latency.

---

### What it is

A *hedged request* is a **client-side** counter to tail latency: send the request to one replica and **wait a short while**—typically until the 95th percentile (*p95*) of that request type's latency distribution—and if no response has arrived, **fire an identical copy at another replica**. **The first response to return wins; late duplicates are discarded.**

The idea comes from Jeffrey Dean and Luiz André Barroso's 2013 paper, *The Tail at Scale*. It reports a Google *BigTable* measurement: with hedging triggered at p95, tail latency (*p99.9*) fell from roughly 1,800 ms to 74 ms, at the cost of **only about 1% additional requests**—the overwhelming majority of requests never spawn a second copy.

It is the cheapest of the *tail-tolerant* designs. A more aggressive sibling is the *tied request* (send to several replicas at once, leaving cancellation marks behind); in production, AWS has publicly shared how *DynamoDB* tamed read-latency tails with hedged reads. Note the mirror image of *request coalescing*/*singleflight*: **that merges many identical concurrent requests into one, server-side; this copies one request into a race of several, client-side**—opposite directions, often used together.

### Why it matters

1. **The tail amplifies into the norm**: one user-facing request fans out into hundreds or thousands of backend calls (seven ingredients, seven storerooms), and overall latency is set by **the slowest one**. The wider the fan-out, the less "hitting one slow call" is a rarity—a beautiful average can coexist with a murderous p99.
2. **Slowness cannot be engineered away**: queuing on shared servers, background daemons, *GC* pauses, rate limits, slow disks answering badly—the variance is irreducible; it can only be hedged.
3. **Cheap insurance**: with the threshold at p95, ninety-odd trips in a hundred cost nothing extra, while the rare tail event is rescued from a fresh path. Very few techniques buy a tenfold tail improvement for 1% overhead.
4. **It has bounds, and discipline**: hedging rescues "this one path is slow" (independent variance), not "fog seals all three temples" (correlated failure); and blasting all replicas at once simply crushes them—Life-Return Hall's queue is the cautionary tale. **Wait a little, then hedge**—that restraint is the whole method.

_Metaphor mapping_

- Spring-Return Hall fetching urgent formulas → the client issuing a request (*client*)
- Three temples each holding a complete *Hundred-Herb Canon* → multiple replicas holding the same data (*replicas*)
- The runner sent up the mountain → a single RPC request (*request*)
- The red line on the incense (the nine-in-ten mark) → the hedge threshold, set at the latency distribution's p95 (*hedge threshold*)
- Past the red line, a second runner to another temple → the hedged duplicate sent to a different replica (*hedged duplicate*)
- First prescription back wins; the latecomer's scroll returned unopened → first response wins, later duplicates discarded (*first response wins*)
- Only four runs in a hundred trigger a second runner → only ~1%–5% of requests incur extra load (*load amplification*)
- The latecomer's wasted trip → server-side work wasted on discarded duplicate responses (*wasted work*)
- Seven ingredients from seven storerooms, the slowest sets the hour → high fan-out: overall latency is the max across calls (*tail amplification*)
- The great fog sealing all three mountains, extra runners useless → correlated failure (*correlated failure*)—hedging cannot save you
- Life-Return Hall flooding all three temples, gates jammed into queues → the herd backlash of replicating to all replicas immediately (*herd / overload*)
- The old master's "wait a little longer" → blind waiting with no timeout (*wait without timeout*)
</section>
