---
layout: fable
title: "等回执的人 · The Two Who Waited for Each Other"
title_zh: "等回执的人"
title_en: "The Two Who Waited for Each Other"
concept: "Nagle's Algorithm and the Delayed-ACK Stall"
tags: [networking, performance]
illustration: /assets/art/2026-11-12-nagle-delayed-ack.jpg
youtube_id: "NavQz6oZm80"
---
<section class="zh" markdown="1">
青溪渡口只隔着三十丈水，来回一趟船不过几口气的工夫。

北岸管装船的是阿岚，南岸管收货的是老陈。两人素不相识，却各自守着一条祖上传下来的规矩。

阿岚的规矩是**不空跑**：只要上一趟船的回执还没递回他手上，岸上这几箱零碎货就先攒着，攒够一整船再走。他觉得这道理再明白不过——你连上一船到了没有都不知道，还一箱一箱往外送，万一船沉了呢？万一南岸没人接呢？一条条小船白白荡过去，那是浪费。


老陈的规矩是**不单开票**：来一箱货，他不急着写回执，等一等，看还有没有下一箱到了，两箱写一张票，省一张纸、省一趟人。人家问他一箱货的回执呢，他说：别急，兴许马上又来一箱。

于是那天早上就出了这么一件怪事。

北岸染坊的掌柜要传花样。第一箱花样递下去，阿岚手上没有未回执的船，立刻放行。船到南岸，老陈接了货，把箱子搬到廊下，笔搭在砚台上，心想：还有一箱吧？等等看。

与此同时，第二箱花样也递到了阿岚手上。他抬头看了看手上的牌子——**上一趟船还没回执**。他把箱子往边上一搁："等回执。"

南岸的老陈在等第二箱，好凑一张票；北岸的阿岚在等第一箱的回执，好放第二箱。两个人各自完全按规矩办事，隔岸相望，谁也不看谁，井水不犯河水，可这条三十丈的水路，就这么空着。

一直到老陈等得不耐烦了——他到底还有个底线：**再没货来，也不能把回执压过四十口气**。四十口气一到，他叹口气，单开一张票，让回程的小船带回去。

阿岚接到回执，放了第二箱。老陈接了货，又等第二张回执的时机……

一整个上午，染坊掌柜的每两箱花样，都要平白多等四十口气。他跑去渡口骂船慢、骂水急、骂艄公懒，把三十丈的水路量了三遍。水一点都不慢。慢的是两个都太会替对方省事的人。

后来还是染坊掌柜自己想明白了，他给了三条路子。

第一条，他让人给阿岚捎话：**从今往后不许等，有一箱走一箱，回执没回来也照样走。** 阿岚第一次听差点翻脸——这不是让他天天放空船吗？可水路通了。这就是后来人人挂在嘴上的那句"把等待关掉"。

第二条，掌柜的不再一箱一箱递：**两箱花样绑成一挑子，一次递下去。** 一挑子就是满满一船，阿岚的规矩本来就不拦整船的货——他只管零碎。货一满，规矩自动失效。这比第一条省心得多。

第三条，他托人跟老陈商量：**收到货立刻开票，别攒。** 老陈嘟囔了半天，说这样纸要费一半，但也就答应了。这一条治的是另一头。

——到这儿你大概已经认出来了：阿岚那"不空跑"的规矩就是 *Nagle's algorithm*，老陈那"不单开票"的规矩就是 *delayed ACK*，那"四十口气"的底线是延迟确认的定时器，而整条三十丈的水路空着不动的那个上午，就是**写—写—读**（*write-write-read*）场景下那个莫名其妙的延迟。

### 这是什么

TCP 上每一次发送，背后都有一笔"带宽税"要交：一个 40 字节的头部 + 一个必须被确认的序号。如果你的应用一次只写 1 字节，那这 1 字节的实际代价是 41 字节。

*Nagle's algorithm*（RFC 896）就是针对这个的：**发出去的小段，只要还有未被确认的数据在途，就先攒着；攒到一整段（*MSS*）再发。** 注意它只拦小段——整段的数据随时可以走。这条规矩在 1984 年是纯赚：网络带宽比延迟贵得多，攒一攒是对的。

*Delayed ACK* 是同一条思路在接收端的镜像：收到一个段，不立刻回 ACK，而是**等大约 40 毫秒，看能不能等到第二个段，让一个 ACK 覆盖两段**（Linux 的上限就取在这个量级）。如果等不到，定时器到期，单独发一个 ACK。同样，它只对"小段单独到达"这种情况生效。

两条各自合理的规矩叠在一起，就出现了著名的互锁：发送方拿着第二段等 ACK，接收方拿着 ACK 等第二段。谁都没错，谁都在等对方。破局只能靠接收方那个 40 毫秒的定时器——于是每一个"写—写—读"的往返，都被平白加上一个 40 毫秒。RPC、数据库驱动、某些 HTTP/1.1 客户端里，单次请求几十毫秒的莫名其妙的固定延迟，常常就是这么来的。它在局域网里最刺眼：传输本身只花 0.2 毫秒，你却量到 40。

### 为什么重要

因为它是一种**看起来像"网络慢"的软件缺陷**。延迟不出现在任何一层的监控里，不丢包、不重传、CPU 不忙、带宽不满，你量什么都是正常的，只有端到端的往返时间稳稳地多出几十毫秒。于是团队花几个月换网卡、调内核、加机器、升级交换机，而真正的原因是：应用写数据的方式，和两条 1980 年代的礼貌规矩。

修法有三个方向，对应故事里那三条路：

- **关掉等待**——`TCP_NODELAY`。代价是零碎小包满地跑，带宽效率下降；对交互式、请求-响应式的协议基本无脑开。
- **把货绑成整挑子**——应用层合并写入（缓冲、`TCP_CORK`、批量成帧、协议里做 *pipelining*）。这是最优解：既没有 40 毫秒，也不浪费带宽。故事的教训在这里——**能在一层解决的问题，不要留给对端去猜。**
- **让接收方立刻开票**——`TCP_QUICKACK`。但它会被系统重置，不是稳定解，通常只适合在库内部临时用。

还有一点反直觉的：延迟不是恒定的 40 毫秒，而是"最坏时正好一个 RTT"。当往返时间本身很短时，40 毫秒的定时器显得巨大；在长肥管道上反而被淹没了。所以同一个 bug 在开发机（本机回环，延迟为零）上永远复现不出来，一上线到同机房就现形。

_隐喻对应表_

- **阿岚"不空跑"的规矩** —— *Nagle's algorithm*
- **老陈"不单开票"的规矩** —— *delayed ACK*
- **"上一趟船还没回执，这箱先攒着"** —— 有未确认数据在途时，小段被握在发送缓冲区
- **"攒够一整船再走"** —— 满 *MSS* 的段不受 Nagle 限制，立即发送
- **"两箱写一张票"** —— 一个 ACK 覆盖两个段
- **"四十口气"的底线** —— 延迟确认的定时器（Linux 约 40 毫秒）
- **第一箱花样立刻放行** —— 无在途数据时，第一个小段仍然立即发出
- **两人隔岸互相干等** —— *write-write-read* 互锁
- **每两箱多等四十口气** —— 每次往返被固定加上一个延迟确认周期
- **三十丈的水路空着** —— 带宽完全空闲，却仍然慢
- **掌柜骂船慢、量水路** —— 误判为网络问题，换网卡调内核
- **"从今往后不许等"** —— `TCP_NODELAY`
- **"两箱绑成一挑子"** —— 应用层合并写入 / `TCP_CORK` / 协议 *pipelining*（正解）
- **"收到货立刻开票"** —— `TCP_QUICKACK`（会被重置，不是稳定解）
- **本机回环上测不出来** —— 延迟为零时定时器纹丝不动，问题只在真实网络里显形
</section>
<section class="en" markdown="1">
The Green Creek ferry crossing is thirty yards of water. A boat makes the round trip in the time it takes to draw a few breaths.

On the north bank, the man who loads the boats is called Lan. On the south bank, the man who receives the cargo is called Chen. They have never met, and each keeps one rule handed down from his own people.

Lan's rule is **never sail half-empty**: as long as the receipt from the previous crossing has not come back into his hand, small crates sit on the dock and wait until there is a full boatload. The logic seems unanswerable. You don't even know whether the last boat arrived — why would you keep pushing crates out one at a time? What if it sank? What if nobody is on the south bank to take delivery? A string of little boats drifting across for nothing is pure waste.

Chen's rule is **never write a single ticket**: when a crate arrives he doesn't rush to write a receipt. He waits a moment to see whether another crate is coming, then writes one ticket for two — saving a sheet of paper and a trip. Asked where the receipt for the first crate is, he says: easy, another one is probably right behind it.

And so, that morning, something strange happened.

A dyer on the north bank needed to send his patterns across. The first crate went down to the dock, and Lan had no unacknowledged crossing on his board, so he released it at once. The boat reached the south bank, Chen took the crate, set it under the eaves, and laid his brush on the inkstone. There'll be another one, he thought. Let's see.

At that same moment, the second crate of patterns arrived at Lan's dock. He glanced at the board — **the last crossing has no receipt yet**. He set the crate aside. "Wait for the receipt."

Chen on the south bank was waiting for a second crate so he could write one ticket for two. Lan on the north bank was waiting for the first crate's receipt so he could release the second. Each was following his own rule perfectly. Thirty yards of water, two men facing each other, neither looking across, and the river lay completely empty.

Eventually Chen ran out of patience — he did, after all, have a bottom line: **a receipt must never be held longer than forty breaths**. When forty breaths were up, he sighed, wrote a single ticket, and sent it back with the returning boat.

Lan took the receipt and released the second crate. Chen received it and began waiting again for the right moment to write the receipt…

All morning long, every two crates of the dyer's patterns cost him an extra forty breaths for nothing. He went down to the ferry and cursed the boats for being slow, the current for being fast, the boatmen for being lazy. He measured the thirty yards of water three times. The water was not slow. What was slow was two men each trying a little too hard to save the other some trouble.

In the end the dyer worked it out himself, and he offered three fixes.

The first: he sent word to Lan — **from now on, no waiting. One crate, one boat, receipt or no receipt.** Lan nearly took offence. Was he to sail empty boats all day? But the river started moving. This is the fix everyone remembers: turn the waiting off.

The second: the dyer stopped handing crates over one at a time. **He bound two crates into one yoke and sent them down together.** A yoke is a full boatload, and Lan's rule never blocked full boats — it only governed odds and ends. Fill the boat and the rule disables itself. Far less trouble than the first fix.

The third: he had someone talk to Chen. **Write the ticket the moment the crate lands. Don't save up.** Chen grumbled about spending twice the paper, but he agreed. This one treats the other end.

— By now you've probably recognised it: Lan's "never sail half-empty" is *Nagle's algorithm*, Chen's "never write a single ticket" is *delayed ACK*, the "forty breaths" bottom line is the delayed-ACK timer, and that whole morning with thirty yards of empty water is the mysterious delay in the **write-write-read** pattern.

### What it is

Every send on TCP pays a bandwidth tax: a 40-byte header plus a sequence number that has to be acknowledged. If your application writes one byte at a time, one byte of payload really costs 41 bytes on the wire.

*Nagle's algorithm* (RFC 896) exists to fix that: **a small outgoing segment is held back whenever there is still unacknowledged data in flight; it goes out once a full segment (*MSS*) has accumulated.** Note that it only holds small segments — a full-sized segment always leaves immediately. In 1984 this was a straight win, because bandwidth was far more expensive than latency, and batching was right.

*Delayed ACK* is the same idea mirrored at the receiver: on receiving a segment, don't send an ACK at once. **Wait roughly 40 milliseconds to see whether a second segment arrives, so one ACK can cover two** (that's the order of magnitude Linux uses). If nothing comes, the timer expires and a lone ACK goes out. Again, it only applies to small segments arriving alone.

Stack two individually sensible rules and you get the famous interlock: the sender holds the second segment waiting for an ACK, the receiver holds the ACK waiting for a second segment. Neither is wrong. Each is waiting for the other. The only way out is the receiver's 40-millisecond timer — so every write-write-read round trip pays a flat 40 milliseconds for nothing. The odd fixed delay of tens of milliseconds in RPC calls, database drivers, and some HTTP/1.1 clients usually comes from exactly this. It is most glaring on a LAN: the transfer itself takes 0.2 milliseconds and you measure 40.

### Why it matters

Because it is a **software defect that looks like a slow network**. The latency shows up in no layer's monitoring: no loss, no retransmits, idle CPU, empty bandwidth. Everything you measure looks fine, yet end-to-end round-trip time is steadily dozens of milliseconds longer than it should be. Teams then spend months swapping NICs, tuning kernels, adding machines, upgrading switches — when the real cause is how the application writes its data, plus two polite rules from the 1980s.

There are three directions of fix, matching the story's three paths:

- **Turn the waiting off** — `TCP_NODELAY`. The cost is small packets everywhere and worse bandwidth efficiency; for interactive, request-response protocols it's essentially a reflex.
- **Bind the crates into one yoke** — coalesce writes in the application (buffering, `TCP_CORK`, batch framing, protocol *pipelining*). This is the best answer: no 40 milliseconds and no wasted bandwidth. The story's real lesson is here — **a problem you can fix at your own layer shouldn't be left for the peer to guess about.**
- **Make the receiver write its ticket immediately** — `TCP_QUICKACK`. But the stack resets it, so it isn't a stable solution and generally belongs inside a library, used temporarily.

One counter-intuitive point: the delay is not a constant 40 milliseconds, it's "at worst one full RTT." On a very short path the 40-millisecond timer is enormous; on a long fat pipe it gets drowned out. Which is why the same bug never reproduces on a developer machine over loopback — zero latency, timer never fires — and appears the moment you deploy to a real network in the same datacentre.

_Metaphor mapping_

- **Lan's "never sail half-empty" rule** — *Nagle's algorithm*
- **Chen's "never write a single ticket" rule** — *delayed ACK*
- **"The last crossing has no receipt, so hold this crate"** — small segments held in the send buffer while unacked data is in flight
- **"Wait until the boat is full, then sail"** — full-*MSS* segments bypass Nagle and leave immediately
- **"One ticket for two crates"** — a single ACK covering two segments
- **The "forty breaths" bottom line** — the delayed-ACK timer (roughly 40 ms on Linux)
- **The first crate released at once** — with nothing in flight, the first small segment still goes out immediately
- **Two men on opposite banks waiting each other out** — the *write-write-read* interlock
- **Every two crates costing an extra forty breaths** — a flat delayed-ACK period added to each round trip
- **Thirty yards of empty water** — bandwidth completely idle, yet still slow
- **The dyer cursing the boats and measuring the river** — misdiagnosing it as a network problem, swapping NICs and tuning kernels
- **"From now on, no waiting"** — `TCP_NODELAY`
- **Binding two crates into one yoke** — application-level write coalescing / `TCP_CORK` / protocol *pipelining* (the right fix)
- **"Write the ticket the moment the crate lands"** — `TCP_QUICKACK` (gets reset, not a stable fix)
- **It never reproduces over loopback** — at zero latency the timer never fires, so the problem only shows on a real network
</section>
