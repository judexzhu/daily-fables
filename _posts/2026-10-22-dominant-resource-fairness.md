---
layout: fable
title: "谁先靠岸 · Who Berths First"
title_zh: "谁先靠岸"
title_en: "Who Berths First"
concept: "Dominant Resource Fairness"
tags: [scheduling, distributed-systems]
illustration: /assets/art/2026-10-22-dominant-resource-fairness.jpg
youtube_id: "SPjyEtgSEp0"
---
<section class="zh" markdown="1">
雾散之前，港口是最安静的。

七号码头的账房叫阿棠。她面前摊着两本簿子：一本记岸线，十丈，多一寸也没有；一本记吊机，十个时辰，过了晌午就再也借不出来。

每天天没亮，船就在口外排队。

渔船瘦，靠岸只要一丈。可它自己有网有人手，吊机用不到两刻——船老大宁可自己扛。

石驳船沉，要吃两丈岸线，吊机只要一个时辰：石头吊上去，就不再动了。

铁壳货轮最大，三丈岸线，六个时辰的吊机，一卸就是一整天。

阿棠的难处不在算账，在于"公平"这两个字没有唯一的写法。

起初她按先来后到。三天之后，海面上只剩货轮——渔船来得早，可来得早换不来岸线；货轮占了两天岸线，渔船就再也不来了。

后来她改成一船一份。十一条船，每船只许用十一个中的一个份额。结果更糟：货轮那一份岸线不够停，只好空着；渔船那一份吊机用不上，也空着。港口装了一半的货，看上去像只装了小半。

— —

那个冬天最冷的一夜，阿棠把两本簿子叠在一起看，忽然看见一件她之前没看见的事。

原来每条船都带着一个"最紧的数"。

渔船：岸线要一丈，是十分中的一分；吊机要两刻，十个时辰里的三十分之一。它最紧的是岸线，一分。

石驳船：岸线两丈，二分；吊机一个时辰，十分之一。最紧的也是岸线，二分。

铁壳货轮：岸线三丈，三分；吊机六个时辰，十分之六。它最紧的是吊机，六分。

阿棠把规则改成一句话：谁的最紧的数最小，谁先靠岸；靠完之后，把他用掉的份额写回他自己的账簿，再从头看一遍。

第二天清晨她照着念。渔船只用一分，第一个进；石驳船二分，跟上；货轮背着六分，只能排在最后，看着小船一条条卸完。等到货轮终于靠岸，岸线用了三分，吊机用了六分——这一天两本簿子同时写满，一寸岸线不剩，一个时辰吊机不剩。

这还不是最妙的。

最妙的是那个想耍聪明的船长。他以为把岸线多报一丈，就能多占一丈。阿棠照他自己的簿子算，那个"最紧的数"自己先涨了上去，于是他排得更靠后，还得多付报上去的那一丈的账。他第二次来，报得一字不差。

还有嫉妒。石驳船的老船主起初不服气：凭什么货轮一次占三丈？他抱着两本簿子算了半天，最后把簿子推回来，说，我要是换成它的位置，我落下的石头比现在还少。他不再说话。

到这里你可能已经认出来了：这就是 *Dominant Resource Fairness*（DRF，主资源公平）。

### 这是什么

*Dominant Resource Fairness* 是多资源调度器里关于"公平"的一个定义，以及实现这个定义的一套算法。它出自 2011 年 Mesos 的论文（Ghodsi 等）。

一台机器或一个集群从来不只有一种资源：*CPU*、内存、*GPU*、带宽、*ephemeral storage*。不同的任务吃不同的资源——推理服务把 *GPU* 吃满却几乎不碰 *CPU*，内存数据库把内存吃满却几乎不碰 *CPU*，编译任务两样都要。如果按"槽位"（*slot*）分配，每个任务占一个固定大小的格子，那么每次分配都在浪费它不需要的那一半。

DRF 的做法是这样：对每个用户（或队列），算出他手上每种资源占集群总量的比例；其中最大的那个比例，就是他的 *dominant share*（主份额），对应的资源就是他的 *dominant resource*。调度器每一次都把资源给 **dominant share 最小** 的那个人。

论文证明这一条规则同时满足四条性质：

- *sharing incentive*（共享激励）：没有用户愿意退出去独占自己那一份，因为在共享池里他至少能拿到自己主资源的 1/n。
- *strategy-proofness*（防策略性）：虚报需求只会抬高自己的 dominant share，把自己排到后面，还要为报出去的份额付账。
- *envy-freeness*（无嫉妒）：没有任何用户愿意和另一个用户交换位置。
- *Pareto efficiency*（帕累托效率）：再也找不到一种改法能让某个用户变好而没人变坏。

它同时是 *work-conserving* 的：只要还有资源能凑出一个任务，机器就不会空转。对不可分割的大任务，论文给了 *progressive filling*；*H-DRF* 则把同一本账簿延伸到层级队列。

### 为什么重要

单一资源的公平是容易的；多资源的公平首先是定义问题，然后才是算法问题。你得先回答"公平是什么意思"，才谈得上实现它。DRF 的价值一半在算法，一半在那四条性质——它们把一个模糊的词变成了一句可以被反驳的话。

你日常用的东西早就建在这上面：Mesos 的 *resource allocation*、YARN 的 *Capacity Scheduler* 与 *Fair Scheduler* 的多资源形式、Kubernetes 的 *extended resources* 配额与 *kube-scheduler* 里的 `NodeResourcesFit` 打分，以及各种 *gang scheduling* 和数据仓库的队列管理，都是 DRF 这本账簿换了身衣服。集群里永远会有某一种资源先被吃光，DRF 告诉你该盯哪一根指针。

它也解释了为什么"平均分"经常是最差的分配方式：平均分沿一种资源度量公平，其余的就在空转；DRF 沿最紧的那一种度量公平，浪费被压到接近零。

它同时画出了自己的边界：DRF 处理的是可分割资源（*divisible*）上的稳态公平，不处理按优先级的抢占（*preemption*）、来自 *affinity* 与拓扑的放置约束，也不处理真实负载的突发性。真实的调度器是把 DRF 的账簿垫在这些规则下面用的。

_隐喻对应表_

- 港口 → 一个集群或一台机器
- 十丈岸线、十个时辰吊机 → 两种可分割资源（CPU、内存、GPU……）
- 一条船要的份额 → 一个任务的资源请求向量（*resource vector*）
- 阿棠的两本簿子 → 调度器的分配状态（*allocation state*）
- 每条船"最紧的那个数" → *dominant share*，对应的资源即 *dominant resource*
- "谁的最紧的数最小，谁先靠岸" → DRF 的选择规则
- 货轮卡在吊机而不是岸线 → 不同任务的瓶颈资源不同，这正是 DRF 存在的理由
- 靠完把用量写回他自己的账簿 → 更新用户的分配，进入下一轮
- 两本簿子同时写满 → *work-conserving*，帕累托效率
- 多报一丈的船长反而排得更后 → *strategy-proofness*
- 石驳船老船主算完账不再说话 → *envy-freeness*
- 渔船永远排得进来 → *sharing incentive*，至少拿到主资源的 1/n
- 先来后到饿死渔船 → FIFO 在异构多资源下不公平
- 一船一份，一半资源空着 → *slot-based* 分配的浪费
</section>
<section class="en" markdown="1">
Before the fog lifts, the harbor is at its quietest.

The accountant at Pier Seven is named Tang. Two ledgers lie open in front of her. One counts the quay: ten zhang, not one inch more. One counts the cranes: ten hours, and after midday nothing can be borrowed.

Boats queue outside the mouth before dawn.

A fishing boat is thin. It needs one zhang of quay. It has hands and nets of its own, and asks the crane for a quarter of an hour — the boatman would rather carry the catch himself.

A stone barge is heavy. Two zhang of quay. One hour of crane: the stone goes up, and then it does not move.

The iron-hulled freighter is the biggest. Three zhang of quay, six hours of crane. One unloading takes it a whole day.

Tang's difficulty is not arithmetic. It is that the word "fair" has no single spelling.

At first she took boats in arrival order. Three days later there was nothing left on the water but freighters. The fishing boats had come early, but arriving early bought them no quay; the freighters held the quay two days running, and the fishing boats stopped coming.

Then she gave every boat an equal slot. Eleven boats, each allowed one eleventh of everything. That was worse. The freighter's eleventh was too little quay to berth, so it sat empty. The fishing boat's eleventh was more crane than it could use, so that sat empty too. The harbor loaded half its cargo while looking as if it had loaded a third.

— —

On the coldest night of that winter, Tang laid the two ledgers on top of each other, and saw something she had not seen before.

Every boat, it turned out, carried one *tightest number*.

Fishing boat: one zhang of quay, one part in ten. A quarter hour of crane, one part in thirty. Its tightest number is quay — one part.

Stone barge: two zhang of quay, two parts in ten. One hour of crane, one part in ten. Its tightest number is also quay — two parts.

Freighter: three zhang of quay, three parts in ten. Six hours of crane, six parts in ten. Its tightest number is the *crane* — six parts.

Tang rewrote the rule in one sentence: whoever's tightest number is smallest berths first; when they are done, write what they used back into their own ledger, and look again from the top.

She read it out the next morning. The fishing boat spent one part and went in first. The stone barge, two parts, followed. The freighter, carrying six parts, could only wait at the back and watch the small boats unload one after another. By the time it finally berthed it had taken three parts of quay and six parts of crane — and that day both ledgers closed full. Not an inch of quay left. Not an hour of crane.

That was not the best part.

The best part was the captain who thought he was clever. He believed that if he declared one zhang more quay than he needed, he would be given one zhang more. Tang did the arithmetic from his own ledger: his tightest number rose first, so he queued further back, and he still paid for the zhang he had declared. He came back the second time and declared it exactly.

Then there was envy. The old master of the stone barge objected at first — by what right does the freighter take three zhang at once? He worked the two ledgers for a long while, then pushed them back and said: if I took its place, I would land less stone than I do now. He said nothing more.

By now you've probably recognized it: this is *Dominant Resource Fairness* (DRF).

### What it is

*Dominant Resource Fairness* is a definition of fairness in multi-resource schedulers, together with an algorithm that achieves it. It comes from the 2011 Mesos paper (Ghodsi et al.).

A machine or a cluster never offers only one kind of resource: *CPU*, memory, *GPU*, bandwidth, *ephemeral storage*. Different workloads consume different resources. An inference service saturates the GPU and barely touches the CPU. An in-memory database saturates memory and barely touches the CPU. A compile job wants both. If you allocate by *slot* — one fixed-size box per task — then every allocation wastes the half of the box the task does not need.

DRF works like this. For each user or queue, compute the fraction it holds of every resource the cluster offers. The largest of those fractions is that user's *dominant share*, and the resource behind it is its *dominant resource*. The scheduler always gives the next allocation to the user with the **smallest dominant share**.

The paper proves that this single rule satisfies four properties at once:

- *Sharing incentive*: no user prefers to leave and take a private slice, because inside the shared pool it receives at least 1/n of its own dominant resource.
- *Strategy-proofness*: overstating demand only raises your own dominant share, pushes you back in the queue, and still charges you for what you declared.
- *Envy-freeness*: no user wants to swap places with another user.
- *Pareto efficiency*: no change improves one user without hurting another.

It is also *work-conserving*: as long as some task fits, the machine does not idle. For large indivisible tasks the paper adds *progressive filling*; *H-DRF* carries the same ledger into hierarchical queues.

### Why it matters

Single-resource fairness is easy. Multi-resource fairness is a definition problem before it is an algorithm. You have to answer "what does fair mean" before you can implement anything. Half of DRF's value is the algorithm; the other half is those four properties, which turn a vague word into a claim someone can argue with.

You already run on this. Mesos *resource allocation*, YARN's *Capacity Scheduler* and *Fair Scheduler* in their multi-resource forms, Kubernetes *extended resources* quotas and the `NodeResourcesFit` scoring in *kube-scheduler*, and most *gang scheduling* and warehouse queue managers are DRF's ledger wearing different clothes. In any cluster one specific resource always runs out first; DRF tells you which needle to watch.

It also explains why "equal shares" is often the worst allocation. Equal shares measure fairness along one resource and idle the rest. DRF measures fairness along the tightest one, which drives waste toward zero.

And it draws its own boundary. DRF handles steady-state fairness over *divisible* resources. It does not handle *preemption* by priority, placement constraints from *affinity* or topology, or the burstiness of real traffic. A production scheduler layers DRF's ledger underneath all of those rules.

_Metaphor mapping_

- The harbor → a cluster or a single machine
- Ten zhang of quay, ten hours of crane → two divisible resources (CPU, memory, GPU…)
- What one boat asks for → a task's request vector
- Tang's two ledgers → the scheduler's allocation state
- Each boat's tightest number → its *dominant share*; the resource behind it is its *dominant resource*
- "Smallest tightest number berths first" → DRF's choice rule
- The freighter bottlenecked on cranes, not quay → different workloads have different bottleneck resources, which is why DRF exists
- Writing usage back into the boat's own ledger → updating that user's allocation and looping
- Both ledgers closing full → *work-conserving*, Pareto-efficient
- The captain who over-declares and queues further back → *strategy-proofness*
- The barge master doing the arithmetic and falling silent → *envy-freeness*
- The fishing boat always fitting in → *sharing incentive*, at least 1/n of the dominant resource
- Arrival order starving the fishing boats → FIFO is unfair across heterogeneous resources
- Equal slots leaving half the harbor empty → the waste of *slot-based* allocation
</section>
