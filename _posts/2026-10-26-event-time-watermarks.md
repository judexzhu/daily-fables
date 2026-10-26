---
layout: fable
title: "封账之前 · Before the Ledger Closes"
title_zh: "封账之前"
title_en: "Before the Ledger Closes"
concept: "Event-Time Watermarks"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-10-26-event-time-watermarks.jpg
youtube_id: "043uatfCbyo"
---
<section class="zh" markdown="1">
青溪渡在沅江的拐弯处。

渡口有一间账房，账房里坐着一个姓沈的老账房。他的活计很简单：把上游十一个村子送下来的谷子记进册子，傍晚交给省仓的船带走。

规矩是这样的：每一船谷子，船上都压着一张签，签上写着谷子**离村**的时辰——卯时装的、午时装的、酉时装的。省仓要的不是"什么时候到渡口"，是"什么时候出的村"。因为收成有汛期，汛期前收的和汛期后收的，价钱不同。

所以老沈每天要做的事，是把到岸的船，按签上的时辰，分进三个更次：晨汛、午汛、夜汛。

— —

头一年他做得很笨。

他想，来的船总有齐的时候。于是他坐在渡口等，等到江面上再没有一条船，再开始记。

问题是，上游发过一场暴雨。

雨落在上坪、下坪、杉木坳的山里，冲垮了两条栈道。船不是不来，是慢。慢没关系，可省仓的船不等。三更鼓起，仓门关，那一夜的册子就作废了。

老沈第一次等齐，等到四更。册子第二天才到，晨汛的谷子记成了午汛，汛期前的一批被算成了汛期后。

那年冬天，省仓罚了他半年的米。

— —

第二年起，他去上游走了一趟，看驿站。

他看出了一件事：船会迟，驿站不会。每个驿站都有一面旗。一根木杆，一面青布。凡是这个驿站辖内出村、装好谷子的船，走完一船，驿站就把旗升到高处；一村未走完，旗压在半腰。

旗不是同时升起来的。上坪的雾散得早，旗升得早；杉木坳在山背后，要晚两个时辰。

老沈在渡口的粉墙上画了一道线。

他不看最高的旗。他看**最低的那面**。

三个驿站，两面升到了午时，一面还压在辰时——他就把红线画在辰时。因为只有那面压着的旗，才代表一句话：**辰时以前装的船，都已经走完了。**

红线一旦画过某个时辰，老沈就认定：那个时辰装的谷子，要么已经到岸，要么永远不会来。

他管这道线叫水位。

— —

第一夜按线封账，册子赶上了三更鼓。

可线是谎话。

不是恶意的谎话，是人做的谎话。杉木坳的旗压着，只说明那面旗还没升。也许是驿卒病了，也许是雾太厚看不清路，也许是有条船在半途翻了，谷子喂了鱼。线画过去，不等于没有船。

第九夜，真有船越线。

一条小船，签上写着辰时，天亮才到。老沈的册子已经封了，封泥都盖上了。

他想了一夜：烧掉这张签，还是拆封重写？

拆封重写，省仓会收到两份晨汛：一份少的，一份补的。老沈不知道省仓怎么想，但他知道谷子是实在的。他拆了封。

从此他定了第二条规矩：**线过之后，每一本册子再留一更不封。**

留的这一更叫缓封。缓封期内来的船，签上写哪个时辰，就补回哪个时辰的册子，另附一行小字：补。

越线太久的，缓封也不收。那种船他记进另一本"漏账"，月底一并算。漏账不进当日的三更册。

— —

第三年的梅雨，杉木坳的旗三天没升。

三天里，红线一动不动，老沈的三本册子一片空白。岸上的谷子堆到棚顶，上坪、下坪的驿卒按时打马过来问：为什么不封账？

老沈说：还有一面旗压着。

他没说出口的是：他不知道那面旗是自己不动，还是被风缠住了。

第四天旗升起来，红线一口气从辰时跳到未时，三本册子一个时辰内全赶上了。老沈在册子边上写下四个字：一线卡死。

— —

到第四年，老沈的册子成了青溪渡最要紧的东西。

省仓的粮官来过一次，翻他的册子，问：你怎么知道线画对了？

老沈说：不知道。线从来不保证。

粮官说：那不就是在赌？

老沈说：是赌。但赌的是"什么时候认输"。线画早了，我输掉晚来的船；线画晚了，我输掉三更鼓。缓封那一更，就是让我认输认得不必那么绝。

粮官又问：一更一更地封，不嫌碎？

老沈说：不碎。晨汛是晨汛，午汛是午汛。三更鼓要的是三个数，不是一个总数。你把一整天搅成一锅，谁也看不出汛期在哪儿翻。

粮官沉默了一会儿，又加了一条：不只一日三个更次。要随时能给我看"眼下往前数三个更次"的合计，他要看汛情的走势。

老沈就多备了一副算盘。每来一条船，把最早那一更的谷子拨出去，把新到的一更拨进来。窗子一直在江面上滑。

— —

老沈死的那年，教了个徒弟，姓何。

何徒弟问：师父，线画到哪儿，才算把今天画完了？

老沈躺在床上，说：画到最后一个驿站升旗。

何徒弟说：那不就等齐了吗？

老沈说：不一样。等齐是等船。画线是等信。船会翻，会沉，会被人吞掉半舱；信不会。信只说一句话——在我之上，没有船了。

何徒弟说：可这话也可能是假的。

老沈笑了一下：所以我不封死。我留一更。

— —

你应该已经认出来了：这就是流处理里的**事件时间水位线**（*event-time watermark*）。

### 这是什么

流处理里，每一个**事件**都带着两个时间：

- *事件时间*（*event time*）：它真正发生的时刻——签上写的装船时辰。
- *处理时间*（*processing time*）：系统看见它的时刻——船靠上渡口的时辰。

这两个从不相等。网络会乱序，来源会积压，上游分区的读速会参差；按处理时间切窗，等于让机器的手快慢决定答案，同一批数据重放两次就是两个结果。所以正确的做法是按事件时间切。

但按事件时间切，就立刻撞上老沈的难题：谁也不能无限期等一条可能永远不来的事件。

**水位线**（*watermark*）是系统对完整度的一个断言：**不再会有事件时间 ≤ T 的事件到达**。它是一个单调推进的标量，从各上游分区的进度里取**最小值**（*low watermark*）推出，然后随数据流向下游传播。它不是一个保证，只是一个赌注；赌错的那部分，就叫**迟到数据**（*late data*）。

围绕这条线，有五件配套的机制：

- **窗口**（*window*）：按事件时间把流切开——固定窗口（晨汛、午汛、夜汛）、滑动窗口（随时往前数三个更次）、会话窗口（相邻事件间隔超过阈值就断开）。
- **触发器**（*trigger*）：什么时候输出。可以是"水位线越过窗口末尾"（按时的、最终的），也可以是"每积够五百石先报一次"或"每隔一炷香报一次"（*early / speculative firing*），让下游不必等到封账才开始干活。
- **允许延迟**（*allowed lateness*）：窗口末尾被水位线越过之后，还继续收多久的迟到数据；收到就修订已经发出的结果。这就是老沈的缓封一更。
- **迟到侧输出**（*late side output*）：超过允许延迟的事件不再进入窗口，而是被路由到另一条流里单独处理——漏账本。
- **累计与丢弃**（*accumulating vs. discarding*）：补上来的结果，是重发一份完整的新合计，还是只发一个增量。老沈选了重发，因为省仓只认完整的册子。

### 为什么重要

按处理时间做聚合，是流处理系统最普遍的默认错误。它在演示里永远是对的，因为演示的数据不迟到。上线之后，源头一积压、消费者一重启、跨区链路一抖动，同一份历史数据重放两次就得到两个不同的数——而账单、告警、计费、报表全都建在那个数上面。

水位线把"什么时候结果算完整了"从一个隐含的、由机器速度决定的东西，变成显式的、可调、可观测、可讨论的东西。它逼你把三个问题一次说清：

- 你按哪个时钟切窗口？
- 你愿意等多久？等得越久，延迟越高，迟到的数据越少。
- 等不到的那部分去哪儿？丢掉、侧输出，还是留一个可以合并的更正？

它也是"流批一体"能够成立的桥：批处理只是**水位线一次推到无穷大**的特例。同一套窗口、触发器、允许延迟的语义，跑在无界流上是有界的近似，跑在有界数据上就是精确的。Google 的 Dataflow 模型（Akidau 等，2015）把这些拆成"窗口、触发、累计、水位线"四个正交的部分，Apache Flink、Beam、Spark Structured Streaming 都建立在这个分解之上。

代价在那根最慢的管子：水位线取全局最小值，一个分区卡住就把整条流的水位线钉死，窗口永远不触发，状态随着积压无上限地长。这不是实现的 bug，是设计的内生属性——要"完整"，就得让最慢的那个说话。生产系统因此要加水位线空闲检测（*idleness*）把空分区从最小值里摘掉，要有状态存活上限（*state TTL*）给迟到兜底，要监控分区间的水位线偏斜——老沈在册子边上写的那四个字，就是这项监控。

_隐喻对应表_

- 签上写的装船时辰 → 事件时间（*event time*）
- 船靠上渡口的时辰 → 处理时间（*processing time*）
- 上游十一个村子的船，到达顺序与装船顺序不一致 → 多个分区上乱序到达的事件
- 晨汛、午汛、夜汛三本册子 → 固定窗口（*tumbling window*）
- 随时往前数三个更次的那副算盘 → 滑动窗口（*sliding window*）
- 驿站升到高处的青布旗 → 单个分区的进度，即分区水位线
- 老沈只看最低的那面旗 → 取全局最小值的水位线（*low watermark*）
- 粉墙上画过的红线 → 水位线 T："不再会有事件时间 ≤ T 的数据"
- 只等最后一面旗才封账 → 水位线推到无穷大，也就是批处理
- 越线才到的小船 → 迟到数据（*late data*）
- 拆封重写、另附一行"补" → 允许延迟（*allowed lateness*）与结果修订
- 每本册子多留一更不封 → 窗口越过末尾后仍保留一个允许延迟区间
- 每积够五百石先派一次快马 → 早期／推测触发（*early trigger*）
- 三更鼓前的最后一本册子 → 窗口的最终触发（*final trigger*）
- 重发整本册子而不是只报差额 → 累计模式（*accumulating mode*）
- 漏账本，月底一并算 → 迟到侧输出（*late side output*）
- 一整天搅成一锅的总数 → 丢掉窗口边界后无法回答的问题
- 驿卒病了、雾太厚、船翻了 → 水位线是断言而非保证；迟到数据的根因
- 三更鼓一响仓门关 → 下游对延迟的硬约束
- 杉木坳的旗三天不升，红线钉死 → 水位线偏斜（*watermark skew*）：一个落后分区卡住全局进度
- 把空驿站从"最低旗"里摘掉 → 空闲分区检测（*idleness*）
- 缓封之后仍然太晚的船 → 超过允许延迟后驱逐，由状态上限（*state TTL*）兜底
</section>
<section class="en" markdown="1">
Qingxi Landing sits where the Yuan River bends.

There is a tally house at the landing, and in the tally house sits an old clerk named Shen. His work is simple: record the grain that eleven villages send down from upriver into a ledger, and hand it to the provincial granary boat before dusk.

Here is the rule. Every boat of grain carries a slip, and the slip bears the hour the grain **left the village** — loaded at dawn, loaded at noon, loaded at dusk. What the provincial office wants is not when the boat reached the landing. It wants when the grain left. Harvest has a flood season; grain taken before it and grain taken after it do not fetch the same price.

So each day, Clerk Shen's job is to sort the boats that arrive into three night-watches according to the hour on their slips: the dawn watch, the noon watch, the night watch.

— —

The first year, he did it stupidly.

He reasoned that the boats must eventually all be in. So he sat at the landing and waited, and only when there was no longer a single boat on the river did he begin to write.

Then came the storm upriver.

Rain fell in the mountains above Shangping, Xiaping, and Shamuao, and washed out two plank roads. The boats did not fail to come. They were slow. Slow was bearable. What was not bearable was that the provincial granary boat did not wait. The third watch drum sounded, the warehouse gate shut, and that night's ledger was worthless.

His first wait-for-everyone finished at the fourth watch. The ledger arrived a day late, the dawn grain was recorded as noon grain, and a batch harvested before the flood season was taxed as a batch harvested after it.

That winter, the provincial granary fined him half a year of rice.

— —

From the second year, he travelled upriver and looked at the post stations.

He noticed one thing: boats run late, post stations do not. Each station has a flag. One wooden pole, one bolt of blue cloth. When a boat from that station's district has been loaded and has departed, the station hoists the flag high. While any village is still waiting, the flag stays at half-mast.

The flags do not rise together. The fog clears early at Shangping, so its flag goes up early. Shamuao is behind the mountain and rises two hours late.

Clerk Shen drew a line on the whitewashed wall of the landing.

He did not look at the highest flag. He looked at the **lowest** one.

Three stations: two flags up at noon, one still pressed down at dawn. He drew the red line at dawn. Because only that pressed-down flag is saying one sentence: **every boat loaded before dawn has already gone.**

Once the red line passed an hour, Clerk Shen held it settled: grain loaded at that hour had either arrived, or would never arrive.

He called that line the water line.

— —

The first night he sealed the ledger by the line, and the ledger made the third watch drum.

But the line is a lie.

Not a malicious lie. A human one. Shamuao's flag being down only means that flag has not risen. Perhaps the post rider is sick. Perhaps the fog is too thick to see the road. Perhaps a boat turned over halfway and the grain went to the fish. The line passing an hour does not mean there was no boat.

On the ninth night, a boat did cross the line.

A small boat, its slip marked dawn, arriving at daybreak. Clerk Shen's ledger was sealed. The clay was already stamped.

He thought all night: burn the slip, or break the seal and rewrite?

Break the seal, and the provincial office would receive two dawn ledgers — a smaller one, and an amended one. He did not know what the provincial office would make of that. But he did know the grain was real. He broke the seal.

From then on he had a second rule: **after the line passes, every ledger stays unsealed for one more watch.**

That extra watch is called the slow seal. A boat arriving during the slow seal is folded back into whichever ledger its slip names, with a small line added: *amended*.

A boat too far past the line is refused even by the slow seal. Those go into a separate book called the leakage account, settled at the end of the month. The leakage account never enters the day's three ledgers.

— —

In the third year's rains, Shamuao's flag did not rise for three days.

For three days the red line did not move, and Clerk Shen's three ledgers stayed blank. Grain on the bank piled to the roof of the shed. Riders from Shangping and Xiaping came on time to ask: why are you not sealing the ledger?

He said: one flag is still pressed down.

What he did not say was that he could not tell whether that flag was standing still on its own, or had been tangled by wind.

On the fourth day the flag went up, the red line leapt from dawn to mid-afternoon in one motion, and all three ledgers caught up within the hour. In the margin Clerk Shen wrote four characters: *one line, jammed dead.*

— —

By the fourth year, Clerk Shen's ledger had become the most important object at Qingxi Landing.

The provincial grain officer came once, turned the pages, and asked: how do you know you drew the line in the right place?

Clerk Shen said: I don't. The line never guarantees anything.

The officer said: then you are gambling.

Clerk Shen said: I am. But what I am gambling on is when to concede. Draw the line too early, and I lose the boats that come late. Draw it too late, and I lose the third watch drum. That one watch of slow seal is what lets me concede without conceding absolutely.

The officer asked: sealing watch by watch — isn't that needlessly chopped up?

Clerk Shen said: It isn't chopped up. Dawn is dawn and noon is noon. The third watch drum wants three numbers, not one total. Stir the whole day into a single pot and nobody can tell where the flood season turned.

The officer was quiet for a while, then added a requirement: not only the day's three watches. He wanted, at any moment, the total for the three watches counting back from now. He wanted to watch the flood season move.

So Clerk Shen kept a second abacus. Each time a boat came, he slid the oldest watch's grain out and the newest watch's grain in. The window slid along the river without ever stopping.

— —

In the year Clerk Shen died, he was teaching an apprentice named He.

The apprentice asked: master, how far does the line have to be drawn before today is finished?

Clerk Shen, from his bed, said: until the last post station raises its flag.

The apprentice said: isn't that the same as waiting for everyone?

Clerk Shen said: no. Waiting for everyone is waiting for boats. Drawing the line is waiting for news. Boats capsize, sink, get half their cargo stolen. News does not. News says exactly one thing — above me, there are no boats.

The apprentice said: but that could be false.

Clerk Shen smiled a little: which is why I never seal it shut. I keep one watch.

— —

By now you've probably recognized it: this is the **event-time watermark** of stream processing.

### What it is

In stream processing, every **event** carries two times:

- *Event time*: when it actually happened — the loading hour written on the slip.
- *Processing time*: when the system saw it — the hour the boat reached the landing.

These are never equal. Networks reorder, sources back up, partition read rates diverge. Windowing by processing time lets the speed of the machine decide the answer, and replaying the same data twice yields two different results. So the correct choice is to window by event time.

But windowing by event time runs straight into Clerk Shen's problem: nothing can wait indefinitely for an event that may never come.

A **watermark** is the system's assertion about completeness: **no more events with event time ≤ T will arrive.** It is a monotonically advancing scalar, derived as the **minimum** across upstream partition progress (*low watermark*), and it propagates downstream with the data. It is not a guarantee. It is a wager, and the part you lose is called **late data**.

Five mechanisms orbit that line:

- **Windows**: slice the stream by event time — fixed windows (dawn, noon, night), sliding windows (the three watches counting back from now), session windows (break when the gap between adjacent events exceeds a threshold).
- **Triggers**: when to emit. It can be "watermark passes the window end" (on-time, final), or "every five hundred piculs send a runner" or "every stick of incense report" (*early / speculative firing*), so downstream does not have to wait for the ledger to close before starting work.
- **Allowed lateness**: how long after the window end the system keeps accepting late data, amending results it already emitted. This is the slow seal.
- **Late side output**: events beyond allowed lateness no longer enter the window; they are routed to a separate stream. The leakage account.
- **Accumulating vs. discarding**: when an amendment arrives, do you re-emit the complete new total, or only a delta? Clerk Shen chose to re-emit, because the provincial office only accepts whole ledgers.

### Why it matters

Aggregating by processing time is the most common default mistake in stream processing. It is always correct in a demo, because demo data is never late. In production, one backed-up source, one consumer restart, one cross-region link blip, and replaying the same history twice produces two different numbers — while billing, alerting, metering, and reporting are all built on that number.

The watermark turns "when is this result complete" from something implicit and machine-speed-determined into something explicit: tunable, observable, arguable. It forces three questions to be answered at once:

- Which clock are you windowing on?
- How long are you willing to wait? The longer you wait, the higher the latency and the less late data you lose.
- Where does the part you could not wait for go? Dropped, side-output, or kept as a correction you can merge?

It is also the bridge that makes unified stream-and-batch possible: batch processing is just the special case where **the watermark is pushed to infinity**. The same windowing, triggering, and allowed-lateness semantics, run over an unbounded stream, give a bounded approximation; run over bounded data, they give an exact answer. Google's Dataflow model (Akidau et al., 2015) split this into four orthogonal pieces — windowing, triggering, accumulation, watermarks — and Apache Flink, Beam, and Spark Structured Streaming all build on that decomposition.

The cost sits in the slowest pipe. A watermark takes a global minimum, so one stuck partition pins the whole stream's watermark, windows never fire, and state grows without bound behind the backlog. This is not an implementation bug; it is intrinsic to the design — to be "complete," you must let the slowest one speak. Production systems therefore add watermark **idleness** detection to drop empty partitions out of the minimum, **state TTL** as a backstop for late data, and monitoring for watermark skew across partitions. The four characters Clerk Shen wrote in the margin are that metric.

_Metaphor mapping_

- The loading hour on the slip → event time
- The hour the boat reaches the landing → processing time
- Eleven upriver villages whose boats arrive out of loading order → events arriving out of order across partitions
- The three ledgers for dawn, noon, and night → tumbling windows
- The second abacus counting back three watches at any moment → sliding windows
- A post station's blue flag raised high → a single partition's progress, its per-partition watermark
- Clerk Shen looking only at the lowest flag → the low watermark, the global minimum
- The red line drawn on the whitewashed wall → watermark T: "no more events with event time ≤ T"
- Waiting for the final flag before sealing → watermark pushed to infinity, which is batch processing
- The small boat crossing the line → late data
- Breaking the seal and adding a line marked *amended* → allowed lateness and result revision
- Keeping each ledger unsealed one extra watch → retaining a window past its end for allowed lateness
- Sending a runner every five hundred piculs → early / speculative trigger
- The last ledger before the third watch drum → the window's final trigger
- Re-sending the whole ledger rather than only the difference → accumulating mode
- The leakage account, settled monthly → late side output
- One pot-stirred total for the whole day → the question you cannot answer once window boundaries are gone
- A sick post rider, thick fog, a capsized boat → the watermark is an assertion, not a guarantee; root causes of late data
- The third watch drum closing the warehouse gate → downstream's hard latency constraint
- Shamuao's flag down for three days, the red line jammed → watermark skew: one lagging partition pins global progress
- Dropping an empty post station out of "the lowest flag" → partition idleness detection
- Boats still too late for the slow seal → eviction beyond allowed lateness, backstopped by state TTL
</section>
