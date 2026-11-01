---
layout: fable
title: "第七个挑夫 · The Seventh Porter"
title_zh: "第七个挑夫"
title_en: "The Seventh Porter"
concept: "Universal Scalability Law"
tags: [performance, distributed-systems, sre]
illustration: /assets/art/2026-11-01-universal-scalability-law.jpg
youtube_id: "VTQGmkgLCh0"
---
<section class="zh" markdown="1">
沈家盐栈在南岸，盐市在北岸。中间那道涧太深，架不起石桥，只有一列铁索，索上铺着窄板，一次只容得下一副担子。风起时，桥摆得像一条晾在竹竿上的白布。

头一年，栈里只有一个挑夫，阿七。一天走十趟，一趟一担，日落交账：十担。

第二年盐市要货凶起来。沈砚招人。

两个挑夫，一天十八担——不是二十，因为过桥要等，但她认了。四个，二十九担。八个，三十二担。招到第十二个，账上只剩二十九担，比八个人的时候还少三担。

挑夫们不懒。桥头有一杆官秤，归老秤管。每副担子出栈之前，都得在秤上过一遍：老秤看秤星、记数、在盐引上盖一枚朱印。这一杆秤、这一枚印，全栈只有一副。人再多，秤不会自己生出第二杆来。于是八个挑夫里，总有一个人站在秤边等，剩下的在桥上走。

沈砚在墙上画了两道线：一道记人手，一道记担数。人手一路往右上走，担数先跟着走，然后弯了，像被谁按住了头。

她算过：那杆秤吃掉的时间，添多少人都追不回来。这一节，后来人叫它 *contention*——争用。争用吃出来的那个天花板是个常数：秤一天能盖多少印，栈里一天最多就出多少担。若只有这一节，十二个人也够爬到天花板底下，不必再招。

可还有第二节。

铁索桥一次只容一副担子，为了不在桥心撞上，规矩是：每个挑夫出发前，得跟当天当班的所有挑夫对一次——你走第几段、我走第几段、谁先谁后。两个人对一次，一句话；十个人对，四十五句话；二十个人对，一百九十句话。对到后来，常常一整个上午都耗在桥头点头。

这一节不成常数，也不成直线——它成双成对地长：人手翻一倍，互相对答的次数翻四倍。后来人叫它 *coherency*——一致性开销，也叫串扰。

于是那道弯不只是弯了，它开始往下走。

招到第十六个挑夫那天，交了二十四担。第二十个，二十一担。第二十四个，十八担。沈砚把新来的挑夫叫到墙前，指着那条往下溜的后半截：“你看，多一个人，少一担半。不是他们不卖力，是他们在互相问路。”

那天夜里她在账本背面记下一个式子，记了一辈子：

**能跑的担数 = 人手 ÷〔1 + 争用 ×（人手 − 1） + 串扰 × 人手 ×（人手 − 1）〕**

它分三段。人手少的时候，括号里几乎还是一，担数跟着人手一起长——那是好日子。争用那一节把人按住，你就只能贴着天花板往上蹭，这是阿姆达尔（*Amdahl*）说过的话。可串扰那一节扛的是“人手乘人手”，越往后越凶，总有一天它压过前两节，担数开始往回退。

沈砚做了两件事。

第一件，把秤搬开。她不再叫每副担子都在桥头过秤，只叫挑夫在南岸仓里十担码成一垛，老秤一天上垛三次，一次盖一垛的印。争用没消失——秤还在，印还是那一枚——可每一次争用摊到了十担身上，那一节常数薄了一大半，天花板抬高一截。

第二件，拆掉“两两对答”。她在桥头立一块黑板，挂一面号旗。每天清早，她把桥分作六段；谁走哪一段，自己上去写一行；写满了，旗一挂，谁也不许再问第二个人。**从“每两个人对一次”，变成“每人看一次板”**——人数翻一倍，板上的字只翻一倍，不再翻四倍。

串扰那一节，几近被一刀削平。

那年秋天，栈里二十七个人，交了一百二十二担。沈砚知道这不是最好的年景，这是她手里最好的年景。她也知道黑板上第五段还是常常堵——一节新的争用，正在那里悄悄长出来。

她把那条曲线又画了一遍，在那个弯最高的地方点了一个点：七个人，三十三担。再往后，八个人三十二担，十二个人二十九担——人手在加，担子在减。书上说，那个点是

**√〔（1 − 争用）÷ 串扰〕**

人手过了这一点，再招人就是往回走。招人不是往火里添柴，是加税——而且这税按人头的平方收。

讲到这儿，你多半已经认出来了：这就是 *Universal Scalability Law*——通用可扩展性定律。

### 这是什么

*Universal Scalability Law*（USL，通用可扩展性定律）由 Neil J. Gunther 提出，用来描述 N 个执行单元（线程、进程、节点、实例）时的相对吞吐：

**C(N) = N ÷ (1 + α(N − 1) + βN(N − 1))**

- **α 是争用**（*contention*，又称串行化）：任务里必须排他串行的那一小段。若只有 α，曲线只能趋近 1/α，这正是 *Amdahl's Law* 的渐近天花板。
- **β 是一致性开销**（*coherency*，又称串扰）：任意两个单元之间必须交换信息、对齐状态的那部分。它的对数是 N(N − 1)/2，所以这一项是二次的。
- **两者并存时，曲线先升、到顶、再下降**——这叫 *retrograde scaling*（回退式扩展）：多一个单元，总产出反而更少。
- **顶点在 N\* = √((1 − α) ÷ β)**。越过它，加机器就是亏本。

因为 Amdahl 定律只是 USL 在 β = 0 时的特例，USL 比它更贴近真实：真实系统里，单元之间没有不说话的。

### 为什么重要

1. **它解释了“加机器反而变慢”**：许多团队以为是运气、是云厂商、是框架有 bug。USL 说这是算术：只要存在两两同步，就必然有顶点，过了顶点就是回退。分布式锁、成员心跳、缓存行失效、共识协议的 O(N²) 消息，都是 β。
2. **它把调优分成两个完全不同的方向**：削 α（批量化、预计算、缩短临界区、把印盖在垛上）抬天花板；削 β（用单一公告板、单向广播、分片代替两两对账）把顶点右移。方向错了，加班再多也只是更快地撞墙。
3. **它给容量规划一条硬边界**：用几个实测点拟合出 α 与 β，就能算出一套系统最多该开到多少并发、扩到哪儿必须换架构。比“再压一轮试试”便宜得多。
4. **它是拆分与单元化的判据**：β 与“谁必须跟谁保持一致”成正比。把强一致的范围切小（分区、单元化、按租户切），β 自然下来。这就是为什么规模大到一定程度，所有系统最后都走向单元化。
5. **它解释了为什么自动扩容会在峰值上帮倒忙**：弹性伸缩按负载加实例；若负载正好落在顶点右侧，加实例会拉长协调链、加剧串扰，把峰值推成悬崖。

_隐喻对应表_

- 挑夫人数 → 执行单元数 N（线程／节点／实例）
- 一天交的担数 → 相对吞吐 C(N)
- 桥头那一杆唯一的官秤与朱印 → 必须串行的临界区，α 的来源
- 秤一天能盖多少印 → 天花板 1/α，即 *Amdahl* 渐近上限
- 每副担子出栈都要过秤 → 逐次争用（每次都付的串行代价）
- 十担一垛、一天上垛三次 → 批量化（*batching*），把 α 摊薄
- 铁索桥一次只容一副担子 → 共享排他资源（*shared exclusive resource*）
- 出发前与当天所有挑夫两两对答 → 一致性开销 β，次数 N(N − 1)/2
- 二十人一百九十句话 → 二次增长（人手翻倍，对答翻四倍）
- 十六人二十四担、二十四人十八担 → *retrograde scaling*（回退式扩展）
- 墙上那个点了点的弯顶：七个人三十三担 → N\* = √((1 − α) ÷ β)
- 桥头一块黑板、一面号旗，各写一行 → 单向广播／公告板，把 O(N²) 压成 O(N)
- 把桥分作六段，各写各段 → 分片／缩小一致性范围（*sharding*）
- 黑板上第五段又常常堵 → 分片后新生的争用点，α 会漂移
- 阿七一人十担 → N = 1 的基线，曲线以它为分母
- “招人不是添柴，是加税，税按人头平方收” → 同步开销的二次性
- 争用那一节追不回来 → 串行段的不可并行部分
- 削去两节之后一人近四担半 → 调 β 把顶点右移，调 α 把天花板抬高

</section>
<section class="en" markdown="1">
The Shen family salt depot stood on the south bank; the salt market on the north. The gorge between them was too deep to bridge with stone, so there was only a chain span with narrow planks laid across it—wide enough for one carrying pole at a time. When the wind rose, the bridge swayed like a white cloth hung on a bamboo pole.

The first year, the depot had one porter, A-Qi. Ten trips a day, one load a trip, and at sunset he handed over the tally: ten loads.

The second year, the salt market grew hungry. Shen Yan hired.

Two porters: eighteen loads a day—not twenty, because crossing involved waiting, but she accepted it. Four porters: twenty-nine. Eight: thirty-two. When she reached the twelfth, the tally read twenty-nine—three loads fewer than with eight.

The porters were not lazy. At the bridgehead stood a single official scale, kept by Old Cheng. Before any load left the depot it had to pass over that scale: Old Cheng would read the beam, note the figure, and stamp the salt licence with a vermillion seal. One scale, one seal, for the entire depot. Hire more men and the scale does not sprout a second beam. So of every eight porters, one stood at the scale waiting, and the rest walked the bridge.

Shen Yan drew two lines on the wall: one for hands, one for loads. The hands climbed steadily to the right. The loads followed—then bent, as if someone had pressed a hand on their head.

She worked it out: the time the scale ate could never be chased back by adding men. This slice, later people would call *contention*. The ceiling contention imposes is a constant: however many seals the scale can stamp in a day, that is the most loads the depot can ship. If contention were all, twelve men would have been enough to creep up under the ceiling. No need to hire another.

But there was a second slice.

The chain bridge took one pole at a time. To keep two porters from meeting at mid-span, the rule was: before setting out, each porter had to check in with every other porter on shift that day—which span will you take, which will I, who goes first. Two men: one sentence. Ten men: forty-five sentences. Twenty men: one hundred ninety sentences. By the end, whole mornings went into nodding at the bridgehead.

This slice was neither constant nor straight—it grew in pairs: double the hands, quadruple the exchanges. Later people would call it *coherency*, or crosstalk.

So the bend did not merely bend. It began to fall.

The day the sixteenth porter joined, they shipped twenty-four loads. The twentieth: twenty-one. The twenty-fourth: eighteen. Shen Yan called the newest porter to the wall and pointed at the sliding tail of the line: "Look. One more man, one and a half fewer loads. Not because they won't work—because they are busy asking each other the way."

That night, on the back of the ledger, she wrote down a formula she would keep for life:

**loads shipped = hands ÷ [1 + contention × (hands − 1) + crosstalk × hands × (hands − 1)]**

It has three parts. With few hands, the bracket is still nearly one, and loads climb with hands—those are the good years. The contention term holds you down, so you can only creep along under the ceiling; that is what Amdahl said. But the crosstalk term carries "hands times hands," and it grows crueller the further you go, until one day it outweighs the other two and the loads start walking backwards.

Shen Yan did two things.

First, she moved the scale off the path. No longer would every pole be weighed at the bridgehead; porters stacked loads ten to a pallet in the south warehouse, and Old Cheng went to the pallets three times a day, stamping a whole pallet with one seal. Contention had not vanished—the scale remained, the seal was still one—but each contention was now spread across ten loads. That constant thinned by more than half, and the ceiling rose.

Second, she tore down the pairwise check-in. At the bridgehead she set up a blackboard and hung a signal flag. Each morning she divided the bridge into six spans; whoever took a span wrote one line for himself; when the board was full, the flag went up, and no one was permitted to ask a second person anything. **From "every pair exchanges a word" to "every man reads the board once"**—double the men, double the writing, no longer fourfold.

The crosstalk term was cut nearly flat.

That autumn, twenty-seven men shipped one hundred twenty-two loads. Shen Yan knew it was not the best year available in the world. It was the best year available in her hands. She also knew span five on that blackboard still jammed most days—a new slice of contention was quietly growing there.

She drew the curve once more and marked the highest point of the bend: seven men, thirty-three loads. Beyond that—eight men, thirty-two; twelve men, twenty-nine—hands rising, loads falling. The books say that point is

**√[(1 − contention) ÷ crosstalk]**

Past it, every new hire walks you backwards. Hiring is not adding fuel to a fire; it is adding a tax—and the tax is levied on the square of the headcount.

By now you've probably recognized it: this is the *Universal Scalability Law*.

### What it is

The *Universal Scalability Law* (USL), formulated by Neil J. Gunther, describes relative throughput with N execution units—threads, processes, nodes, instances:

**C(N) = N ÷ (1 + α(N − 1) + βN(N − 1))**

- **α is contention** (also called serialization): the small portion of work that must be done exclusively, one at a time. With α alone, the curve can only approach 1/α—exactly the asymptotic ceiling of *Amdahl's Law*.
- **β is coherency** (also called crosstalk): the portion where any two units must exchange information and reconcile state. Its count is N(N − 1)/2, which makes this term quadratic.
- **With both present, the curve rises, peaks, then falls**—*retrograde scaling*: one more unit produces less total output.
- **The peak sits at N\* = √((1 − α) ÷ β).** Cross it and adding machines loses money.

Because Amdahl's Law is only the β = 0 special case of the USL, the USL sits much closer to reality: in real systems, no two units are silent to each other.

### Why it matters

1. **It explains "more machines, slower service"**: teams blame luck, the cloud vendor, a framework bug. The USL says it is arithmetic. Any pairwise synchronization implies a peak, and past the peak is regression. Distributed locks, membership heartbeats, cache-line invalidation, the O(N²) messages of consensus protocols—all of these are β.
2. **It splits tuning into two entirely different directions**: cutting α (batching, precomputation, shorter critical sections, stamping the pallet instead of the load) raises the ceiling; cutting β (one bulletin board, one-way broadcast, sharding instead of pairwise reconciliation) moves the peak right. Pick the wrong direction and overtime only makes you hit the wall faster.
3. **It gives capacity planning a hard boundary**: fit α and β from a few measured points and you can compute how many concurrent units a system should ever run, and where expansion must give way to redesign. Far cheaper than "let's load-test again."
4. **It is the criterion for splitting and cell-based design**: β is proportional to "who must agree with whom." Shrink the scope of strong consistency—partitioning, cells, per-tenant silos—and β falls on its own. That is why every system eventually goes to cells at scale.
5. **It explains why autoscaling can hurt at the peak**: elastic scaling adds instances in proportion to load. If load sits just right of the peak, extra instances lengthen the coordination chain and intensify crosstalk, turning a peak into a cliff.

_Metaphor mapping_

- Porter headcount → number of execution units N (threads / nodes / instances)
- Loads shipped in a day → relative throughput C(N)
- The one official scale and vermillion seal at the bridgehead → the exclusive critical section that generates α
- How many seals the scale can stamp in a day → the ceiling 1/α, the Amdahl asymptote
- Every load weighed before leaving → per-operation contention (a serial cost paid each time)
- Ten loads to a pallet, three pallet trips a day → batching, which spreads α thin
- A chain bridge that takes one pole at a time → shared exclusive resource
- Checking in with every other porter before departure → coherency cost β, N(N − 1)/2 exchanges
- Twenty men, one hundred ninety sentences → quadratic growth (double the hands, quadruple the talk)
- Sixteen men and twenty-four loads, twenty-four men and eighteen loads → retrograde scaling
- The marked peak of the bend: seven men, thirty-three loads → N\* = √((1 − α) ÷ β)
- One blackboard and one flag, each man writing a line → one-way broadcast / bulletin board, O(N²) collapsed to O(N)
- Six spans, each written by its own porter → sharding / shrinking the scope of consistency
- Span five jamming again → a new contention point born of sharding; α drifts
- A-Qi alone, ten loads → the N = 1 baseline the curve divides by
- "Hiring is not adding fuel, it is adding a tax on the square of the headcount" → the quadratic nature of synchronization cost
- The contention slice that can never be chased back → the non-parallelizable serial fraction
- Nearly four and a half loads per man after both cuts → lowering β moves the peak right, lowering α raises the ceiling

</section>
