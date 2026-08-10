---
layout: fable
title: "木牌背面的红点 · The Red Dot on the Back of the Tally"
title_zh: "木牌背面的红点"
title_en: "The Red Dot on the Back of the Tally"
concept: "Distributed Tracing: Context Propagation and Sampling"
tags: [observability, distributed-systems]
illustration: /assets/art/2026-08-10-distributed-tracing-context-propagation.jpg
---
<section class="zh" markdown="1">
老城西头有家饭馆叫"三进堂"，一到掌灯时分就满得转不开身。

它不是一间厨房，是六间：前厅、冷碟房、炉灶、烤炉、汤锅、甜点台。一桌客人点的东西，往往要在这六间里各走一遭，最后由传菜的小工合到一个托盘上端出去。

生意好，麻烦也就跟着来。客人拍桌子说"我这道菜等了四十分钟"，掌柜的挨间去问，六个师傅都摇头：我这儿三分钟就出手了。每个人说的都是真话，可那四十分钟确实丢在了什么地方——只是谁也没见过它长什么样。

掌柜的想了个笨办法。

他在门口摆了个门房。客人一落座，门房就递一块木牌，牌上刻一串谁也不重样的号码，这号码跟着这一桌走到底。

然后是纸条。每个工位干完自己那一段，都得撕一张小纸条，写四样东西：木牌号码、自己工位的名字、什么时候接的手、什么时候撒的手。还有第五样，最要紧的——**上一张纸条的编号**。冷碟房从前厅手里接的单，就写"承前厅第七号"；炉灶从冷碟房手里接的，就写"承冷碟第十二号"。

于是纸条不再是一堆散沙，而是一条能顺着往回摸的链子。谁在等谁、谁卡了谁，摊在桌上一眼就看得出来。

传菜的小工被反复叮嘱一句话：**号码必须抄给下一个人**。抄漏了，下游那几张纸条就成了没爹没娘的孤儿——它们记的时刻千真万确，却再也接不回那一桌上去。掌柜的清点时最怕看见这种纸条：明明有人流了汗，却不知道是替谁流的。

可六间屋子一晚上要出三百桌，纸条堆起来比帐本还厚，账房先生抄到天亮也抄不完。

门房于是多了一件事：每来一桌，他掷一次骰子，二十桌里挑中一桌，就在那块木牌的背面烧一个小红点。规矩只有一条——**只看红点，不许自己拿主意**。牌背有红点，这一路六间屋子全都老老实实写纸条；没有红点，六间屋子谁也不写。

这条规矩听着专断，却是整件事的命门。要是让每个工位自己掂量"这道菜值不值得记"，最后拿到的就是六段互不搭界的残片：炉灶记了、烤炉没记，中间那截空白比什么都不记还误导人。一桌，要么整条链子都在，要么干脆一张都没有——**半条链子是最坏的那一种**。

后来账房先生还试过另一套路子：干脆全都写，纸条先不归档，摊在后堂一张长桌上；等这一桌的菜全齐了、客人筷子都放下了，他再回头看这一整条链子——只有真出了岔子的（等太久的、退回来的），才收进柜子存着，其余的一把扫进灶膛。这样最该留的那一桌绝不会漏掉，代价是那张长桌得一直空着，而且得撑到人家吃完才能动手。

还有件小事，掌柜的琢磨了很久。六间屋子各有各的沙漏，谁也不比谁准，差个一两分钟是常事。所以纸条上的时刻凑在一起对不齐——炉灶写的"接手"，竟比冷碟房写的"撒手"还早了一点。掌柜的最后认了：**时刻只当参考，真正靠得住的是那句"承某某第几号"**。次序不是从钟面上读出来的，是从"谁承了谁"里读出来的。

——到这儿你大概已经认出来了：这是 *distributed tracing*，以及它的两根支柱：*context propagation* 与 *sampling*。

## 这是什么

一个请求进到微服务系统里，会横穿十几个进程。每个进程的日志都只看得见自己那一小段，而"慢"往往不在任何一段里，在段与段之间。

*Distributed tracing* 的做法是：在入口处生成一个全局唯一的 *trace ID*，让它随请求穿过每一次进程边界（HTTP header、gRPC metadata、消息头，即 *W3C traceparent*）。每个服务把自己的工作记成一个 *span*，span 里带着 trace ID、自己的 span ID，以及 *parent span ID*。收集器按 parent 关系把散落的 span 拼成一棵树，于是"四十分钟花在哪儿"变成了一张可以直接读的图。

两个关键设计：

*Sampling* 决定要不要记。*Head-based sampling* 在入口就掷骰子，把 sampled 这一位塞进传播的 context 里，下游一律服从——这保证了一条 trace 要么完整要么没有，绝不出现半棵树。*Tail-based sampling* 反过来：先全收，在 collector 里按 trace ID 缓冲，等一条 trace 的 span 都到齐了再决定留不留（通常只留慢的和出错的）。它抓得准，但需要内存缓冲，还得等一个"trace 已完整"的超时。

*Causality over clocks*：各服务的时钟有 *clock skew*，跨机器的时间戳对不齐是常态，子 span 的开始时间早于父 span 并不稀奇。所以 trace 的拓扑靠 parent-child 引用建立，时间戳只用来量时长。

而 tracing 最常见的失效方式，从来不是采样率调错，是**某个中间件没把 header 传下去**——链子在那里断了，下游全成了 orphan span。

## 隐喻对应表

- 木牌上那串不重样的号码 —— *trace ID*
- 每张小纸条 —— 一个 *span*（工位名 = span name，接手/撒手 = start/end timestamp）
- "承前厅第七号" —— *parent span ID*，trace 树的边
- 传菜小工必须把号码抄给下一个人 —— *context propagation*（*W3C traceparent* header 跨进程边界传递）
- 抄漏号码后的那几张纸条 —— *orphan span*：instrumentation 断链，最常见的故障
- 门房掷骰子、二十桌挑一桌 —— *head-based sampling*，1/20 采样率
- 木牌背面烧的小红点 —— context 里的 *sampled* 标志位
- "只看红点，不许自己拿主意" —— 采样决策在入口做一次、全链路遵从，保证 trace 完整
- 后堂那张摊满纸条的长桌 —— *tail-based sampling* 的 collector 缓冲区
- 等客人放下筷子才决定留不留 —— 等 trace 完成后按"慢/错"筛选
- 六间屋子各有各的沙漏 —— *clock skew*
- "时刻只当参考，靠得住的是承接关系" —— 拓扑由 causality 决定，时间戳只用于测时长
</section>
<section class="en" markdown="1">
There was an old restaurant at the west end of the city, and by lamp-lighting hour there was no room to turn around in it.

It was not one kitchen but six: the front room, the cold-dish bench, the flame stove, the domed oven, the soup cauldron, the pastry counter. A single table's order usually had to pass through all six, and only then did a runner gather it onto one tray and carry it out.

Good business brought its own trouble. A guest would slap the table — *I waited forty minutes for this dish* — and the owner would go room to room, and all six cooks would shake their heads: it was out of my hands in three minutes. Every one of them was telling the truth. And yet those forty minutes had been lost somewhere. Nobody had ever seen what they looked like.

So the owner tried something clumsy.

He put a doorman at the entrance. The moment guests sat down, the doorman handed them a wooden tally carved with a number that matched no other tally in the house, and that number followed the table all the way through.

Then came the slips. Every station, on finishing its part, had to tear off a small slip and write four things: the tally number, the station's own name, when it took the work, when it let it go. And a fifth thing, the important one — **the number of the previous slip**. The cold-dish bench, taking the order from the front room, wrote "follows front room, no. 7." The stove, taking it from the cold-dish bench, wrote "follows cold-dish, no. 12."

The slips were no longer a loose heap. They were a chain you could feel your way back along. Who was waiting on whom, who was holding up whom — laid out on a table, you could see it at a glance.

The runners were told one thing over and over: **the number must be copied to the next pair of hands**. Miss it once, and every slip downstream became an orphan — the times on them perfectly true, and no way ever to attach them back to that table. Those were the slips the owner dreaded finding at the end of the night: somebody clearly sweated, and no one knew on whose behalf.

But six rooms turned out three hundred tables in an evening, and the slips piled up thicker than the ledger. The bookkeeper could copy till dawn and never finish.

So the doorman got one more duty. For each arriving table he threw a die; one table in twenty, he burned a small red dot into the back of its wooden tally. There was exactly one rule — **look at the dot, and do not decide for yourself**. Dot on the back, and all six rooms dutifully wrote their slips. No dot, and not one room wrote anything.

The rule sounds high-handed, and it is the hinge the whole thing turns on. Let each station judge for itself whether a dish is worth recording, and what you end up holding is six unrelated fragments: the stove wrote, the oven didn't, and the blank stretch in the middle misleads you worse than no record at all. A table either has its whole chain or none of it — **half a chain is the worst kind**.

Later the bookkeeper tried a second method. Write everything down, but file nothing yet; spread the slips on a long table in the back room, and only once a table's dishes had all gone out and the guests had set down their chopsticks, look back over the whole chain — keep only the ones that actually went wrong (the long waits, the dishes sent back) and sweep the rest into the stove. This way the table that most deserved keeping was never missed. The price was that the long table had to stay empty and waiting, and nothing could be decided until people had finished eating.

And one small thing the owner puzzled over for a long while. Each of the six rooms had its own hourglass, none more honest than another, and a minute or two of disagreement was ordinary. So the times on the slips wouldn't line up — the stove's "took it" was, somehow, a little earlier than the cold-dish bench's "let it go." In the end the owner made his peace with it: **the times are a reference; what you can actually trust is the line that says "follows so-and-so, no. such."** The order isn't read off a clock face. It's read off who took over from whom.

— By now you've probably recognized it: this is *distributed tracing*, and its two pillars, *context propagation* and *sampling*.

## What it is

A request entering a microservice system crosses a dozen processes. Each process's logs see only their own sliver, and "slow" usually doesn't live inside any one sliver — it lives between them.

Distributed tracing generates a globally unique *trace ID* at the edge and carries it across every process boundary (HTTP header, gRPC metadata, message header — the *W3C traceparent*). Each service records its work as a *span* carrying the trace ID, its own span ID, and a *parent span ID*. A collector reassembles the scattered spans into a tree by parent reference, and "where did the forty minutes go" becomes a picture you can simply read.

Two design decisions matter most.

*Sampling* decides what gets recorded. *Head-based sampling* throws the die at the edge, stuffs a sampled bit into the propagated context, and every downstream service obeys it — which is what guarantees a trace is either whole or absent, never half a tree. *Tail-based sampling* inverts this: collect everything, buffer by trace ID at the collector, and decide only once a trace's spans have all arrived (usually keeping the slow ones and the failed ones). It catches exactly what you want, at the cost of memory and of waiting on a "trace is complete" timeout.

*Causality over clocks*: services suffer *clock skew*, cross-machine timestamps routinely disagree, and a child span starting before its parent is unremarkable. So the shape of a trace is built from parent-child references; timestamps are only used to measure duration.

And the way tracing usually fails is never a mis-tuned sample rate. It's **one middleware that forgot to forward the header** — the chain breaks there, and everything downstream becomes an orphan span.

## Metaphor mapping

- The unrepeated number on the wooden tally — the *trace ID*
- Each small slip — one *span* (station name = span name, took/let go = start/end timestamps)
- "Follows front room, no. 7" — the *parent span ID*, an edge in the trace tree
- Runners must copy the number to the next pair of hands — *context propagation* (the *W3C traceparent* header across process boundaries)
- The slips after a missed copy — *orphan spans*: broken instrumentation, the most common failure
- The doorman's die, one table in twenty — *head-based sampling* at a 1/20 rate
- The red dot burned on the tally's back — the *sampled* flag in the propagated context
- "Look at the dot, don't decide for yourself" — the sampling decision made once at the edge and obeyed everywhere, keeping traces whole
- The long back-room table covered in slips — the *tail-based sampling* collector buffer
- Deciding only after chopsticks are down — filtering on slow/errored traces once the trace is complete
- Six rooms, six hourglasses — *clock skew*
- "Times are a reference; the handover line is what you trust" — topology from causality, timestamps only for duration
</section>
