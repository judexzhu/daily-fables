---
layout: fable
title: "老秦的竹牌 · Old Qin's Bamboo Tally"
title_zh: "老秦的竹牌"
title_en: "Old Qin's Bamboo Tally"
concept: "Session guarantees (read-your-writes, monotonic reads)"
tags: [distributed-systems, kubernetes]
illustration: /assets/art/2026-08-21-read-your-writes-session-guarantees.jpg
---
<section class="zh" markdown="1">
榕树村只有一面"正墙"——祠堂后头那面白灰墙。全村的告示都写在那儿：谁家丢了羊、圩期改到哪天、地界怎么分。可村子铺得散，从最远的西塘走到祠堂要一个半时辰，老人们走不动。

于是有了七面"抄墙"。书吏每写完一张告示，就交给跑腿的后生，后生分头跑七个巷口，把它照抄到各自的墙上。跑腿的有快有慢，下雨路滑更慢，所以抄墙上的东西总比正墙晚一点。晚多少，谁也说不准。

多数时候没人计较。直到老秦丢了羊。

老秦一早赶到祠堂，请书吏写了寻羊的告示，亲眼看着它贴上正墙，然后一路走回西塘。到了巷口他抬头看抄墙——空的。等了半炷香，还是空的。

老秦当场就炸了："我自己写的告示，我自己都看不见？"

书吏赶来，没有争辩。他从袖子里摸出一片竹牌，用刀刻了个数：**四千零二十一**。"这是您那张告示在正墙上的号。往后您站到任何一面抄墙前，先把牌递给守墙人。"

守墙人也有一块牌，刻着自家墙抄到了第几号。老秦递上竹牌，守墙人一看自家才四千零九——还差着——便说："您坐一坐，跑腿的快到了。"若是等不及，守墙人就指路："东巷那面已经到四千一百了，您上那边看。"

从这天起，老秦再没有看不见自己写的告示。

第二桩事出在王婶身上。她清早在东巷墙上看见圩期改到初八，午后路过南巷，那面墙上还写着初六。她信了后一面墙，误了圩。

书吏又发了一块竹牌，这回刻的不是"我写到哪儿"，而是"**我看到过哪儿**"。王婶每看一次墙，守墙人就把她牌上的数往上刻一格。此后她再站到任何一面墙前，若牌上的数比墙高，守墙人就不许她看："您已经知道得比我这堵墙多了，看了反而糊涂。"

第三桩最微妙。有人在抄墙上看见"李家的牛走失"，随手贴了张"牛在我地里"。两张纸交给了不同的跑腿，走了不同的路，结果南巷墙上先出现了"牛在我地里"，底下空空——没人知道说的是哪头牛。书吏于是定规矩：**回话的告示必须带着它回的那一张一起走，前一张没上墙，后一张就不许上墙**。

至于同一个人先后写的两张——"我要卖田"和"田不卖了"——书吏从不允许它们调换次序，哪怕跑腿的抄了近道。

村子还是那个村子。正墙照旧领先，抄墙照旧落后，跑腿的照旧有快有慢。什么都没有变快，每个人手里只是多了一块竹牌。

——到这儿你大概已经认出来了：这不是告示墙的故事。正墙是 primary，抄墙是 read replica，跑腿的后生是异步复制，竹牌上的数是 *version token*（etcd 叫它 resourceVersion，数据库叫它 LSN）。这是 *session guarantees* 的故事，其中最有名的一条叫 *read-your-writes consistency*。

### 这是什么

*Eventual consistency* 只承诺一件事：如果写入停止，所有副本终将一致。它对**沿途**只字未提——尤其没说，同一个客户端在这段时间里会看见什么。而绝大多数"数据库明明写进去了，页面却查不到"的怪事，都发生在这段沿途上。

1994 年 Terry 等人在 Bayou 系统里给出的解法，不是把系统变强，而是给**每个会话**单独立四条规矩：

- *Read your writes*：你写过的东西，你后续一定读得到。
- *Monotonic reads*：你读到过的东西，不会在下一次读时消失——时间不许倒流。
- *Writes follow reads*：你基于某次读写下的东西，不会跑到那次读的前面去（因果顺序）。
- *Monotonic writes*：你自己的写，按你发出的顺序生效。

实现手段就是那块竹牌：客户端随身带一个 *version token*，请求时一并递上。副本自己有个 token，比一比，只有三条路——**等**（等复制追上）、**改道**（路由到足够新的副本，或直接回主库）、**拒绝**（拒绝提供比你已知更旧的视图）。

关键在于这些保证是**每客户端**的，副本之间不需要额外协调。所以它比 *linearizability* 便宜得多：不是让整个系统看起来像一台机器，而只是让**每个人自己的那条时间线**说得通。

### 为什么重要

这是"读写分离"架构里最常被踩的坑：写主库、读只读副本，用户提交完表单立刻刷新，看见的是提交前的世界。Aurora / RDS 的 replica lag、跨 region 复制、CDN 与 DNS 缓存，全是同一个病。

Kubernetes 里更贴身：`kubectl apply` 之后马上 `kubectl get` 偶尔看不到改动，就是请求落到了另一台 kube-apiserver 的 *watch cache* 上（`resourceVersion=0` 意味着"随便多旧都行"）。正确的做法是把写响应里的 resourceVersion 带进下一次 List，配合 `ResourceVersionMatch: NotOlderThan`——把那块竹牌递给守墙人。controller 里也是同理：写完对象立刻按自己的 informer cache 做决策，是经典的 stale read，所以才需要 *expectations*、才需要 resync。

最后是它的**破绽**：竹牌一丢，保证就没了。用户换了设备、负载均衡把会话打到别的池子、token 没在服务之间透传下去（浏览器 → BFF → 后端，中间断了链），那条时间线就断了。这时候唯一诚实的退路，是回主库读一次。

### 隐喻对应表

- 正墙 —— primary / leader，唯一的写入点
- 七面抄墙 —— read replicas，异步、各自落后不同的量
- 跑腿的后生 —— 异步复制，延迟不确定且不均等
- 刻着 4021 的竹牌 —— 写入返回的 *version token*（resourceVersion / LSN）
- 守墙人自己的号牌 —— 副本当前已应用到的版本
- "坐一坐，跑腿的快到了" —— read-after-write 等待复制追上
- "上东巷那面看" —— 路由到足够新的副本，或回退到 primary
- 王婶那块"我看到过哪儿"的牌 —— *monotonic reads*，会话已读版本的下界
- "回话必须带着原话一起走" —— *writes follow reads*，因果依赖
- 卖田与不卖田不许换次序 —— *monotonic writes*
- 竹牌丢了 —— 会话 token 丢失，保证降级为纯 eventual
</section>
<section class="en" markdown="1">
Banyan Village had only one Great Wall of Notices — the whitewashed wall behind the ancestral hall. Every announcement in the village was written there: whose goat had wandered off, which day the market had moved to, where a boundary now ran. But the village sprawled, and from the far hamlet of Xitang it was a ninety-minute walk to the hall. The old people could not manage it.

So seven copy-walls were built. Each time the clerk finished a notice, he handed it to a young runner, and the runners fanned out to seven lane-mouths and copied it onto the wall there. Some runners were quick, some slow; in the rain, slower still. So the copy-walls always trailed the Great Wall by a little. By how much, nobody could say.

Most of the time nobody cared. Then Old Qin lost his goat.

He walked to the hall at dawn, had the clerk write a notice, watched it go up on the Great Wall with his own eyes, and walked all the way home to Xitang. At the lane-mouth he looked up at the copy-wall — empty. He waited. Still empty.

Old Qin exploded. "I wrote it myself and I can't even see it myself?"

The clerk came, and did not argue. He drew a bamboo tally from his sleeve and cut a number into it: **four thousand and twenty-one**. "That is your notice's number on the Great Wall. From now on, before you read any copy-wall, hand this to its keeper."

Every keeper had a tally of his own, cut with the number his wall had copied up to. Old Qin handed his over; the keeper saw his own wall stood at four thousand and nine — not there yet — and said, "Sit a moment, the runner is close." And when Qin could not wait, the keeper pointed: "The east-lane wall is already at four thousand one hundred. Read it there."

From that day Old Qin never again failed to find his own notice.

The second trouble was Aunt Wang's. In the morning, at the east-lane wall, she read that the market had moved to the eighth. That afternoon she passed the south-lane wall, which still said the sixth. She believed the second wall, and missed the market.

The clerk issued her a tally too — but this one was cut not with *what I wrote* but with **what I have already seen**. Each time she read a wall, its keeper notched her tally upward. After that, whenever her number stood higher than a wall's, the keeper would not let her read it. "You already know more than this wall does. Reading it would only confuse you."

The third trouble was the subtlest. Someone read "the Li family's ox has strayed" on a copy-wall and pinned up a reply: "the ox is in my field." The two slips went to different runners by different paths, and on the south-lane wall the reply appeared first, with nothing beneath it — nobody knew which ox. So the clerk made a rule: **a reply must travel together with the notice it answers, and may not go up until that notice is up.**

And two notices from the same hand — "my field is for sale" and "the field is no longer for sale" — the clerk never let the runners reorder, however tempting the shortcut.

The village was the same village. The Great Wall still led, the copy-walls still trailed, the runners were still fast or slow. Nothing had been made faster. Everyone simply carried a small bamboo tally now.

— By now you have probably recognized it: this is not a story about notice walls. The Great Wall is the primary, the copy-walls are read replicas, the runners are asynchronous replication, and the number on the tally is a *version token* (etcd calls it resourceVersion; a database calls it an LSN). This is the story of *session guarantees*, whose most famous clause is *read-your-writes consistency*.

### What it is

*Eventual consistency* promises exactly one thing: if writes stop, replicas will converge. It says nothing about the journey — and in particular nothing about what a single client sees along the way. Almost every "I definitely saved it, but the page shows the old value" mystery lives on that journey.

The answer Terry et al. gave in the Bayou system in 1994 was not to strengthen the store, but to attach four rules to **each session**:

- *Read your writes*: what you wrote, you will subsequently read.
- *Monotonic reads*: what you have seen will not vanish on your next read — time may not run backwards.
- *Writes follow reads*: a write you made on the basis of a read cannot land before that read (causal order).
- *Monotonic writes*: your own writes take effect in the order you issued them.

The mechanism is the tally: the client carries a *version token* and presents it with each request. The replica compares it against its own applied version, and has only three moves — **wait** for replication to catch up, **redirect** to a fresh-enough replica (or straight to the primary), or **refuse** to serve a view older than what the client already knows.

The crucial property is that these guarantees are **per-client**, requiring no extra coordination between replicas. That makes them far cheaper than *linearizability*: instead of making the whole system look like one machine, you only make each person's own timeline coherent.

### Why it matters

This is the standard trap in read/write splitting: write to the primary, read from a replica, and the user who refreshes right after submitting sees the world as it was before they acted. Aurora/RDS replica lag, cross-region replication, CDN and DNS caches — all the same disease.

Closer to home, in Kubernetes: `kubectl get` right after `kubectl apply` occasionally misses the change, because the request landed on another kube-apiserver's *watch cache* (`resourceVersion=0` means "arbitrarily stale is fine"). The fix is to carry the resourceVersion from the write response into the next List with `ResourceVersionMatch: NotOlderThan` — handing the tally to the keeper. Controllers face it too: acting on your own informer cache immediately after writing an object is the classic stale read, which is why *expectations* and resyncs exist at all.

And finally, its **failure mode**: lose the tally and the guarantee is gone. The user switches devices, the load balancer lands the session on a different pool, or the token isn't propagated across a hop (browser → BFF → backend) — and the timeline breaks. When that happens, the only honest fallback is to read from the primary.

### Metaphor mapping

- The Great Wall — the primary/leader, the single point of write
- The seven copy-walls — read replicas, asynchronous, each lagging differently
- The young runners — asynchronous replication with unbounded, uneven delay
- The tally cut with 4021 — the *version token* returned by the write (resourceVersion / LSN)
- The keeper's own tally — the version the replica has currently applied
- "Sit a moment, the runner is close" — read-after-write wait for replication to catch up
- "Read it at the east-lane wall" — routing to a fresh-enough replica, or falling back to the primary
- Aunt Wang's "what I have seen" tally — *monotonic reads*, a lower bound on the session's read version
- "A reply travels with the notice it answers" — *writes follow reads*, causal dependency
- The two field notices that may not be reordered — *monotonic writes*
- Losing the tally — losing the session token; the guarantee degrades to plain eventual consistency
</section>
