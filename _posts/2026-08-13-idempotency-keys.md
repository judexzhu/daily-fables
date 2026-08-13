---
layout: fable
title: "雨夜客栈的铜钩 · The Brass Hooks at the Lantern Inn"
title_zh: "雨夜客栈的铜钩"
title_en: "The Brass Hooks at the Lantern Inn"
concept: "Idempotency Keys and Exactly-Once Effects"
tags: [distributed-systems, kubernetes]
illustration: /assets/art/2026-08-13-idempotency-keys.jpg
---
<section class="zh" markdown="1">
雨下了整夜。灯笼客栈后厨的窗口只开了一条缝，跑腿的孩子们一个接一个从雨里冒出来，把湿透的纸条塞进去。

守窗口的是老板娘阿禾。她很早就认清了一件事：雨夜里，纸条会丢。孩子从河对岸的酒楼跑过来，路上滑一跤，纸条泡烂在泥里——点单的人等不到菜，就再写一张，再派一个孩子。也有时候第一个孩子其实到了，只是回程摔进了水沟，回执没送到。于是同一份"三碗羊汤"，可能在半个时辰里被送来三次。

早年阿禾的做法很朴素：来一张，做一份。结果是有人只想要三碗羊汤，最后收到九碗，还被算了三次钱。她慢慢明白：**路上的孩子不可靠，这件事她改变不了；她能改变的，只有窗口这一侧**。

于是灶台边挂上了一块木牌，木牌上钉着一排铜钩。规矩改成六条。

**第一，票号由客人自己写。** 每张纸条上必须有一个票号，不是阿禾编的，是点单的酒楼写的。同一笔单子无论重发多少次，票号都不变——因为只有客人知道"这两张纸条其实是同一件事"，阿禾不知道。孩子送来纸条，她第一眼不看菜名，看票号。

**第二，挂钩的动作就是判断的动作。** 这排铜钩有个讲究：一个钩子只挂得下一张票。她把票挂上去和检查钩子空不空，是同一个伸手的动作——挂得上，说明没人做过；挂不上，说明有人正在做或已经做完。哪怕两个孩子在同一瞬间扑到窗口，两张一模一样的纸条，也只有一张能挂上钩。另一个被请到火边坐着等。

**第三，票号要配指纹。** 票挂上钩之后，阿禾会在纸条背面记下这单的指纹：几碗、什么汤、送到哪桌。后来有一家酒楼图省事，把昨天用剩的票号写在新单子上——同样的票号，要的却是八碗面。阿禾把它退了回去。票号是用来认"同一件事"的，不是用来认"同一个人"的；同号不同事，那是记错了，不是重发。

**第四，上菜和销账必须是一笔写完的。** 这是最要紧的一条。从前她先把汤端出去，回头再在账上划一笔。有一晚厨房走了水，汤端出去了，账没划成——第二天那张票又来了，她照着空账本，又做了一遍。现在羊汤舀进碗里、库房的羊肉减掉三份、票号在账上落定，这三件事在同一行墨里完成：要么全成，要么全不成。半途翻了灯，整行作废，票从钩上摘下来，当作从没来过。

**第五，回执要留副本。** 阿禾发现光"不重做"还不够。第二个孩子举着一模一样的纸条站在窗口，总得给他点什么带回去。于是挂票的钩子上，一并挂着当时那张回执的副本："三碗羊汤，已上，东厢第二桌。"重发的单子来了，她不做菜，她把副本抄一份交给孩子。点单的人收到的答复和第一次一模一样——他甚至不知道自己发了三次。

**第六，木牌只有那么大。** 阿禾每隔三天摘一次旧票。她赌的是：一个孩子在雨里迷路，最多迷三天。三天之后还举着老票号回来的，那不叫重发，那是别的故事了。

——到这儿你大概已经认出来了：阿禾守的不是一块木牌，是 *idempotency*。

## 这是什么

不可靠的网络只能提供 *at-least-once delivery*：请求可能丢，回复也可能丢，而发送方无法区分这两者，于是只能重试。重试意味着同一个请求可能被服务端收到多次。所谓 *exactly-once*，在网络层面是做不到的——能做到的是 *exactly-once effect*：收多少次都行，**副作用只发生一次**。

做法就是 *idempotency key*：客户端为每一次逻辑操作生成一个唯一 key（重试时复用同一个 key），服务端把 key 存进一张去重表，用一次**原子的 insert-if-absent** 来抢占它，并且——这是最常被写错的地方——把"业务副作用"和"写入 key"放进**同一个事务**提交。如果先做副作用再记 key，中间崩溃就等于没记；如果先记 key 再做副作用，中间崩溃就等于永远丢单。

还要存下第一次的响应：重复请求应当**重放当时的回复**，而不是返回一个"重复请求"的错误——对客户端来说，重试和首次调用必须看起来完全一样。key 通常绑定请求内容的 hash（防止 key 复用到不同请求），并带 TTL（去重表不能无限增长），TTL 必须长于客户端可能的最大重试窗口。

这东西无处不在：Stripe 的 `Idempotency-Key` header、AWS API 的 `ClientToken`、Kafka 的 producer id + sequence number、Kubernetes controller 里"先读 status 再决定动不动手"的 *reconcile* 逻辑。写 controller 的人尤其该记住：*reconcile* 会被重复触发是常态，不是异常。

## 隐喻对应表

- 雨夜与摔跤的孩子 —— 不可靠网络：请求丢失、响应丢失，发送方无法区分
- 同一份汤被送来三次 —— *at-least-once delivery* 下的重复请求
- 客人自己写的票号 —— *idempotency key*，必须由**客户端**生成并在重试时复用
- 一排铜钩、一钩一票 —— 去重表上的 **atomic insert-if-absent**（唯一索引 / 条件写）
- 两个孩子同时扑到窗口，只有一张能挂上 —— 并发重复请求下的竞态，靠原子性而非"先查后写"解决
- 纸条背面的指纹 —— key 绑定 request payload 的 hash，防止 key 被复用到不同请求
- "同一行墨"里完成上菜与销账 —— 副作用与 key 写入必须在**同一个 transaction** 内提交
- 灯翻了整行作废、票从钩上摘下 —— 事务 rollback，key 一并释放，允许重试
- 挂在钩上的回执副本 —— 缓存首次响应，重复请求**重放**它而不是报错
- 每三天摘一次旧票 —— 去重表的 **TTL**，必须长于客户端最大重试窗口
</section>
<section class="en" markdown="1">
It rained all night. The kitchen hatch at the Lantern Inn was open just a crack, and the runner boys kept emerging from the downpour one after another, pushing soaked paper slips through the gap.

The woman at the hatch was He, who kept the inn. She had learned one thing early: on rainy nights, slips get lost. A boy runs over from the tavern across the river, slips in the mud, and the paper dissolves into the ditch — so the customer, hearing nothing, writes another slip and sends another boy. Sometimes the first boy actually arrived, and only fell on the way _back_, so the receipt never made it home. Either way, one order of "three bowls of mutton soup" might reach her window three times in an hour.

In her early years He did the obvious thing: one slip, one order. Which is how a man who wanted three bowls of soup ended up with nine, and a bill for three. Slowly she understood: **the boys on the road are unreliable and she cannot change that. The only thing she can change is her side of the window.**

So a wooden board went up beside the stove, studded with a row of brass hooks, and the rules became six.

**One: the customer writes the number.** Every slip must carry a ticket number — not one He invents, but one the ordering tavern writes. However many times the same order is resent, the number stays the same, because only the customer knows that two slips are secretly the same event. He does not. When a boy arrives, the first thing she looks at is not the dish. It is the number.

**Two: hanging the ticket _is_ the check.** The hooks have a peculiarity: one hook holds exactly one ticket. Hanging the slip and checking whether the hook is free are the same reach of the same hand — if it hangs, nobody has done this; if it won't, someone is doing it or has done it. Even if two boys lunge at the window in the same instant with identical slips, only one of them hangs. The other is sat down by the fire to wait.

**Three: the number needs a fingerprint.** Once a ticket is on its hook, He writes the order's fingerprint on the back: how many bowls, which soup, which table. A tavern once got lazy and reused yesterday's leftover ticket number on a fresh order — same number, but eight bowls of noodles this time. She sent it back. A ticket number identifies _the same event_, not _the same customer_. Same number, different event, is a clerical error, not a resend.

**Four: serving and settling must be one stroke of ink.** This is the rule that matters most. She used to carry the soup out first and mark the ledger afterward. One night a fire broke out in the kitchen: the soup went out, the ledger line never got written — and the next day, when that ticket came back, she read her empty ledger and cooked the whole thing again. Now the soup going into the bowls, three portions of mutton coming off the storeroom count, and the ticket number settling into the ledger all happen in one line of ink: all of it, or none of it. If the lamp tips over halfway, the whole line is void, the ticket comes off its hook, and it is as if the slip never arrived.

**Five: keep a copy of the receipt.** He discovered that _not cooking it twice_ wasn't enough. The second boy is still standing at the window with an identical slip, and he needs something to carry home. So the hook holds not just the ticket but a copy of the receipt she wrote the first time: "three bowls mutton soup, served, east wing table two." When the resend arrives, she cooks nothing — she copies the receipt and hands it over. The customer's answer is identical to the first one. He never learns that he asked three times.

**Six: the board is only so big.** Every three days He clears the old tickets off. Her wager is that a boy lost in the rain stays lost for at most three days. Anyone still waving an ancient ticket number after that isn't resending. That's a different story.

— By now you've probably recognized her: what He is really guarding is not a board of hooks. It's _idempotency_.

## What this is

An unreliable network can only offer _at-least-once delivery_. The request may be lost; the reply may be lost; and the sender cannot tell those two cases apart, so it retries. Retrying means the server may receive the same request many times. True _exactly-once_ delivery is unachievable at the network level. What _is_ achievable is an _exactly-once effect_: receive it as often as you like, **the side effect happens once**.

The mechanism is the _idempotency key_. The client generates a unique key per logical operation and reuses it on every retry. The server claims that key in a deduplication table with a single **atomic insert-if-absent**, and — this is the step most often gotten wrong — commits the business side effect and the key insert **in the same transaction**. Do the effect first and record the key after, and a crash in between means it never happened. Record the key first and do the effect after, and a crash in between loses the order forever.

You must also store the first response. A duplicate request should **replay the original reply**, not return a "duplicate request" error: to the client, a retry has to look exactly like a first call. The key is normally bound to a hash of the request payload (so a reused key on a different request is rejected rather than silently swallowed), and carries a TTL — the dedup table cannot grow forever — where the TTL must outlive the client's maximum retry window.

It's everywhere: Stripe's `Idempotency-Key` header, `ClientToken` on AWS APIs, Kafka's producer id plus sequence number, and every Kubernetes controller whose _reconcile_ reads observed state before deciding whether to act. Controller authors especially should remember: being invoked repeatedly for the same event is the normal case, not the failure case.

## The mapping

- The rain, and the boys who fall — an unreliable network: lost requests, lost responses, indistinguishable to the sender
- The same soup ordered three times — duplicate requests under _at-least-once delivery_
- The ticket number written by the customer — the _idempotency key_, which must be generated **client-side** and reused across retries
- One hook, one ticket — an **atomic insert-if-absent** on the dedup table (unique index or conditional write)
- Two boys lunging at once, only one ticket hanging — the concurrent-duplicate race, solved by atomicity rather than check-then-write
- The fingerprint on the back of the slip — binding the key to a hash of the request payload, so a reused key on a different request is rejected
- Serving and settling in one stroke of ink — side effect and key insert committed in **the same transaction**
- The tipped lamp voiding the line, ticket off the hook — transaction rollback releases the key so the retry can succeed
- The receipt copy hanging on the hook — caching the first response and **replaying** it for duplicates instead of erroring
- Clearing the board every three days — the dedup table's **TTL**, which must exceed the client's maximum retry window
</section>
