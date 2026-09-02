---
layout: fable
title: "磨坊主的木栓 · The Miller's Tally Peg"
title_zh: "磨坊主的木栓"
title_en: "The Miller's Tally Peg"
concept: "Optimistic Concurrency Control"
tags: [distributed-systems, kubernetes, databases]
illustration: /assets/art/2026-08-16-optimistic-concurrency-control.jpg
youtube_id: "dDSbQlUMEms"
---
<section class="zh" markdown="1">
溪水推着石磨转了一百年，磨坊里最要紧的东西却不是那块磨盘，而是柜台上那本厚账簿。

村里每一户存了多少斗麦、赊了多少袋面、还欠磨坊几个铜板，全在里面。一户一页。

老磨坊主有个规矩：**账簿从不出门**。

你想改自己那页，就走到柜台前，把它抄到自己的小石板上——抄完就走，愿意在河边坐一下午算，愿意回家跟婆娘吵三个钟头再算，都行。柜台从不拦人，也从不上锁。同一个下午，可以有五个人同时在抄，十个人同时在算。磨坊主照样磨他的麦。

规矩藏在页边。

每一页的边上，都拴着一根小木栓，木栓上刻着一排细细的**刻痕**。你抄页的时候，得把刻痕数一并记在石板角上。

等你算完了回来，把石板递过去，磨坊主不看你写了什么，先看那个数。

他伸手捏住木栓，眼睛在木栓和你石板的角上来回一趟——

数一样：他抄下你的新数目，然后**当场在木栓上添一道新刻痕**。捏住、比对、落笔、添痕，一气呵成，中间连头都不抬。这几息之间，柜台前谁也插不进手。

数不一样：他把石板推回来，摊开手掌。

"你抄走以后，有人动过这一页了。你手上那份，是旧的。"

上个月磨坊主的儿子问他，为什么不学镇上那家的做法——谁要改账，就把整页从簿子上拆下来揣进兜里，改完再装回去，谁也别想插队。

老头子哼了一声："那你去看看他们柜台前排多长的队。一个人揣着页去田里干半天活，后面九个干瞪眼。更别说他要是喝多了，那页就一夜不回来。"

"可要是不拴那根木栓呢？"

"那才叫真出事。"老头把烟斗搁下，"张三抄了他那页，写着欠二十。李四也抄了同一页，也是二十。张三先回来，还了五，柜上记十五。李四后回来，他算的是在二十上还八，写十二——写下去，张三那五个铜板就**从这世上消失了**。木栓不是防人写，是防人拿一份过期的东西盖掉别人刚做的事。"

儿子还是不服气："被退回来的人不冤吗？"

"冤什么，重抄一遍再算就是了。"磨坊主指了指窗外空荡荡的院子，"平常日子，一天退不了两回。"

只有到了赶集那天不一样。全村都盯着公用麦仓那一页，人来人往，木栓上的刻痕一炷香添七八道，几个手脚慢的抄了算、算完被退、退了再抄，从早跑到晚，硬是一笔没记上。老磨坊主看着，最后干脆把那页收起来，喊了一嗓子："公仓的账，一个一个来。"

——到这儿你大概已经认出来了：那根木栓就是 *version*，磨坊主捏栓比数、记账、添痕的那一气呵成，就是一次 *compare-and-swap*；而被推回来的石板，是那个你今天大概率还会再见一次的 `409 Conflict`。

## 这是什么

**Optimistic concurrency control（OCC，乐观并发控制）** 的赌注是：冲突很少发生，所以别为它预先付费。读的时候不加锁，随便读、随便并发；只在**写回的那一刻**才检查——"我读到它之后，有没有人动过？"检查的凭据是一个随对象走的 *version*（版本号、时间戳，或整条记录的哈希）。

关键在于比对和递增必须是**同一个不可分割的动作**（*atomic compare-and-swap*）。分成两步，中间就有缝，缝里就能塞进另一个写者。

它挡住的是 *lost update*：两个人各读一份旧值、各自算出新值、先后写回，后者悄悄抹掉前者。注意它抹得**没有任何报错**——这是这类 bug 最难查的地方。

Kubernetes 把这套规矩摆在明面上。每个对象的 `metadata.resourceVersion` 就是那根木栓；`kubectl apply`、controller 的 update 调用，本质都是"带着我读到的 resourceVersion 写回去"；apiserver 转手交给 etcd 做一次事务性的 compare-and-swap，不匹配就返回 409。所以写 controller 时那句 `retry.RetryOnConflict(...)` 不是防御性编程的客套，是这个协议要求你实现的另一半——**磨坊主只负责拒绝，重抄重算是你的事**。

代价也和赶集日一样：OCC 只在低竞争下划算。热点对象上人人重试，吞吐会塌，甚至有人反复失败（*starvation*）。这时候正确的做法不是加大重试次数，是像老磨坊主那样换个协议——排队（pessimistic lock）、拆分对象，或者把"在旧值上加减"改成让存储自己去做的原子增量。

## 隐喻对应表

- 厚账簿的一页 → 一个数据对象 / Kubernetes resource
- 抄到自己的小石板上 → 读取（read，不加锁）
- 边上木栓的刻痕数 → *version* / `resourceVersion`
- 抄页时把刻痕数一起记下 → 读取时带回版本号
- 磨坊主捏栓、比数、落笔、添痕一气呵成 → *atomic compare-and-swap*
- 刻痕数一致，写入并添一痕 → 写入成功，版本递增
- 石板被推回来 → `409 Conflict` / write rejected
- 重抄一遍再算 → *retry on conflict*（读-改-写整个循环重来，不是只重发）
- 镇上那家：把页揣进兜里 → *pessimistic locking*，写者互斥、读者也等
- 张三那五个铜板消失了 → *lost update*，且悄无声息
- 平常日子一天退不了两回 → 低竞争，OCC 的最佳工况
- 赶集日的公用麦仓页 → 热点对象，高竞争下重试风暴与 *starvation*
- "公仓的账，一个一个来" → 对热点改用悲观锁 / 拆分 / 原子操作
</section>
<section class="en" markdown="1">
The stream had been turning the millstone for a hundred years, but the most important thing in the mill was not the stone. It was the thick ledger on the counter.

How many bushels each household had stored, how many sacks they'd taken on credit, how many coppers they still owed the mill — all of it lived in there. One page per household.

The old miller had one rule: **the ledger never leaves the building**.

If you wanted to change your page, you walked up to the counter and copied it onto your own little slate — then off you went. Sit by the river and work out your sums all afternoon; go home and argue with your wife about them for three hours first. Nobody stopped you. Nothing was locked. On a single afternoon five people might be copying and ten more out somewhere doing arithmetic. The miller kept right on grinding wheat.

The rule lived in the margin.

Tied to the edge of every page was a small wooden peg, and cut into the peg was a row of thin **notches**. When you copied the page, you had to copy the notch count too, in the corner of your slate.

When you came back and handed the slate over, the miller didn't read what you'd written. He looked at that number first.

He'd take the peg in his hand, his eyes going once between the peg and the corner of your slate —

Same number: he copied your new figures into the book and **cut a fresh notch into the peg on the spot**. Grip, compare, write, notch — one unbroken motion, head never lifting. For those few breaths nobody else could get a hand in edgewise at that counter.

Different number: he slid the slate back and opened his palm.

"Someone's been at this page since you copied it. What you're holding is stale."

Last month the miller's son asked him why they didn't do it the way the shop in town did — whoever wants to make a change tears the whole page out, pockets it, and puts it back when they're done. No queue-jumping possible.

The old man snorted. "Go look at the line in front of their counter. One fellow carries a page off to the fields for half a day and nine people stand there blinking. And if he gets into the drink, that page doesn't come home till morning."

"But what if there were no peg at all?"

"*That's* when you'd have real trouble." He set down his pipe. "Zhang copies his page: owes twenty. Li copies the same page: also twenty. Zhang comes back first, pays five, book says fifteen. Li comes back later — his sum was twenty minus eight, so he writes twelve. And the moment that goes in, Zhang's five coppers **vanish from the world**. The peg isn't there to stop people writing. It's there to stop a man covering up what someone just did with something he copied out too long ago."

The son still wasn't satisfied. "Isn't it unfair on the one who gets turned away?"

"Unfair? He copies it again and redoes his sums." The miller nodded at the empty yard outside. "On an ordinary day I turn away fewer than two."

Market day was the exception. The whole village had its eye on the communal granary page, people in and out all morning, seven or eight notches going onto that peg in the time it takes an incense stick to burn. The slower ones copied, calculated, got turned away, copied again — ran back and forth from dawn to dusk and never got a single figure entered. The old miller watched it for a while, then lifted the page out of the book altogether and called across the room: "Granary accounts — one at a time."

— By now you've probably recognized it: that peg is a *version*, the miller's single unbroken grip-compare-write-notch is a *compare-and-swap*, and the slate pushed back across the counter is the `409 Conflict` you'll most likely meet again today.

## What this is

**Optimistic concurrency control (OCC)** bets that conflicts are rare, so you shouldn't pay for them up front. Reads take no lock — read freely, read concurrently. The check happens only at **the moment you write back**: has anyone touched this since I read it? The evidence is a *version* that travels with the object — a counter, a timestamp, or a hash of the record.

The crux is that comparing and incrementing must be **one indivisible act** (*atomic compare-and-swap*). Split it into two steps and you've opened a gap, and another writer will fit into the gap.

What it prevents is the *lost update*: two readers take the same stale value, each computes a new one, and the second write silently erases the first. Note *silently* — no error is raised anywhere. That's what makes this class of bug so miserable to chase.

Kubernetes puts the whole protocol out in the open. Every object's `metadata.resourceVersion` is the peg. `kubectl apply`, a controller's update call — all of them are "write this back, carrying the resourceVersion I read." The apiserver hands that to etcd as a transactional compare-and-swap, and a mismatch comes back as 409. Which is why `retry.RetryOnConflict(...)` in your controller isn't defensive politeness; it's the half of the protocol you're required to implement. **The miller only refuses. Re-copying and redoing the sums is your job.**

And the cost is market day. OCC only pays off under low contention. On a hot object everyone retries, throughput collapses, and some writers fail over and over (*starvation*). The fix is not a bigger retry budget — it's to change protocol the way the old miller did: queue up (pessimistic locking), split the object into finer pieces, or replace "read a value and add to it" with an atomic increment the store performs itself.

## Metaphor mapping

- A page of the ledger → a data object / Kubernetes resource
- Copying it onto your slate → the read (no lock taken)
- The notch count on the peg → the *version* / `resourceVersion`
- Copying the notch count along with the page → carrying the version back with you
- Grip, compare, write, notch in one motion → *atomic compare-and-swap*
- Counts match: entry written, fresh notch cut → write succeeds, version bumped
- The slate slid back across the counter → `409 Conflict` / write rejected
- Copy it again and redo the sums → *retry on conflict* (rerun the whole read-modify-write, don't just resend)
- The town shop pocketing the page → *pessimistic locking*: writers exclude each other, readers wait too
- Zhang's five coppers vanishing → the *lost update*, and it happens silently
- Fewer than two turned away on an ordinary day → low contention, OCC's happy path
- The granary page on market day → a hot object: retry storms and *starvation*
- "Granary accounts — one at a time" → switching hot paths to pessimistic locks, sharding, or atomic ops
</section>
