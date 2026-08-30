---
layout: fable
title: "澡堂里的火与窗 · The Stove and the Window"
title_zh: "澡堂里的火与窗"
title_en: "The Stove and the Window"
concept: "Control-Loop Oscillation and Hysteresis"
tags: [kubernetes, distributed-systems, linux]
illustration: /assets/art/2026-08-19-control-loop-hysteresis.jpg
youtube_id: "FJW8S58NgCA"
---
<section class="zh" markdown="1">
天还没亮，城南澡堂的新伙计小满就接了班。

老板临走只交代一句："池子的水，要让客人舒服。太凉了会骂，太烫了也会骂。"

墙角一只铁炉，炉膛通到池底。添煤，水就热；开窗，热气就散。听上去简单得很。

卯时三刻，头一批客人下了池，齐声叫冷。小满赶紧铲了三锹煤进去，炉火轰地一声起来了。

他等着。水还是凉的。

又等了一会儿，还是凉的。客人开始拍着池沿抱怨。小满心一横，又添了三锹。

一炷香之后，池子开了。真的开了——白汽把房梁都遮住了，客人红着脸从水里跳出来，骂声比刚才还难听。

小满慌了，抄起竹竿把四扇窗全推开，冷风灌进来。半炷香，池面浮起一层薄雾，客人又开始喊冷。他又铲煤。又开窗。又铲煤。

到了晌午，小满已经跑了十七趟，浑身是汗，池子却没有一刻是舒服的。它要么滚烫，要么冰凉，像一只被两只手轮流推的秋千——越推，晃得越大。

老师傅这时候才从后院慢悠悠进来，看了一眼，坐下了。

"你有没有发现，"老师傅说，"你添的煤，要过一炷香才到水里。"

"我知道啊，所以我等不及。"

"你等不及，是因为你在跟**此刻**的水说话。可水回答的，是你**一炷香之前**说的那句。你们俩根本不在同一个时辰里。"

老师傅让人在池边挂了一支温度木牌，然后立下三条规矩，写在墙上：

**第一，别看这一瞬的水。看一炷香里的平均。** 有人刚跳进来带起一股凉，那不算数。

**第二，冷了不一定要添煤，烫了不一定要开窗。** 低于三十八度才添煤，高于四十二度才开窗。三十八到四十二之间——什么都不做，不管你的手觉得多不对劲。中间这一段，是留给炉子自己走完那一炷香的路的。

**第三，每次只添一锹。添完，坐下，数满一炷香再决定下一步。** 哪怕客人正在骂你。

小满照做了。头两个时辰，他觉得自己像个懒汉，坐在那儿什么也不干。可到了下午，怪事发生了：他一整天只添了四锹煤，只开过一次窗，池子却从早到晚都是温的。

那支木牌上的针，几乎没怎么动过。

——到这儿你大概已经认出来了：这不是澡堂的故事，是一个 *feedback control loop* 的故事。小满那十七趟，就是每个 SRE 都见过的 autoscaler *flapping*：因为动作和效果之间隔着延迟，控制器不停地在过冲和欠冲之间来回震荡。老师傅的三条规矩，正是 *metric smoothing*、*hysteresis* 和 *stabilization window*。

### 这是什么

任何"测量 → 决策 → 执行"的闭环，只要**执行到测量之间存在延迟**，就有震荡的风险。控制理论里这叫 *dead time*：延迟越长、增益越高，系统越容易从收敛滑向自激振荡。

Kubernetes 的 HorizontalPodAutoscaler 是最典型的例子。它读到 CPU 高，扩容；但新 Pod 要拉镜像、启动、通过 readiness probe、被收进 Service endpoints——中间可能是一两分钟。这段时间里指标依然高，于是控制器接着扩，等所有 Pod 终于都起来，容量已经远远超了。指标随即暴跌，控制器开始缩容，缩完流量一来，又不够了。这就是小满的秋千。

治它的办法不是"让控制器更聪明"，而是**让控制器更迟钝**——而且是有讲究的迟钝：

- **Metric smoothing**：用一个时间窗口的聚合值，而不是瞬时采样点。HPA 读的是 metrics-server 上一个 scrape 窗口的平均值，正是为了不被单个毛刺牵着走。
- **Hysteresis / deadband**：上行和下行用不同的触发点，中间留一段死区。HPA 里对应 `--horizontal-pod-autoscaler-tolerance`（默认 10%）：期望副本数与当前副本数之比落在 0.9~1.1 之间，控制器一动不动。少了这段死区，指标只要在单一阈值上下抖动，就会反复触发。
- **Stabilization window**：`behavior.scaleDown.stabilizationWindowSeconds` 默认 300 秒——在这个窗口里取历史推荐值的**最大值**，等于要求"持续冷静五分钟"才允许缩容。而 scale-up 的窗口默认是 0，因为过载的代价通常远大于多开几个 Pod：**两个方向的谨慎程度本来就不该对称。**
- **Rate limit / step size**：`behavior` 里的 policies 限制每个周期最多加减多少 Pod 或百分比。一次走一小步，比一次跳到底安全。

这个道理远不止属于 HPA。Cluster Autoscaler 的 `scale-down-unneeded-time`、BGP 的 route flap damping、TCP 拥塞控制里加性增乘性减的不对称、恒温器上那两度回差、甚至你调整团队人手的节奏——都是同一个问题的同一个答案：**当反馈有延迟时，最好的反应速度不是最快的那个。**

还有一层更根本的启发：与其一味调参数去压制震荡，不如先**缩短延迟本身**。镜像更小、启动更快、readiness 更早、scrape 间隔更短——环路越短，你能安全地反应得越快。参数是在既定延迟下求最优；缩短延迟是把最优本身抬高。

_隐喻对应表_

- 池子的水温 → 被观测的 *metric*（CPU、QPS、queue depth）
- 小满 → 控制器（HPA、Cluster Autoscaler，任何 reconcile loop）
- 铲煤 / 开窗 → scale up / scale down
- 煤下去到水变热之间的那一炷香 → *dead time*：Pod 启动 + readiness + metrics scrape 间隔
- "你在跟此刻的水说话，水回答的是一炷香前那句" → 控制器看到的永远是**过去的**系统状态
- 十七趟来回、越推越大的秋千 → *oscillation* / flapping
- 客人被烫得跳出来 → overshoot：浪费的容量与被伤到的用户
- "看一炷香里的平均" → *metric smoothing*，聚合窗口
- 三十八度与四十二度两个阈值 → *hysteresis*，两个方向不同的触发点
- 三十八到四十二之间什么都不做 → *deadband* / HPA `tolerance`
- "添完坐下，数满一炷香" → *stabilization window*（scale-down 默认 300s）
- "每次只添一锹" → scaling policy 的 *rate limit* / step size
- 老师傅坐着不动的那份定力 → 在有延迟的环路里，克制本身就是一种正确的控制策略
- 一整天只添四锹煤，水却始终是温的 → 收敛：动作更少，结果反而更稳
</section>
<section class="en" markdown="1">
It was still dark when Man, the new hand at the bathhouse on the south side of town, took over the morning shift.

The owner left him with one instruction: "Keep the pool comfortable. They'll curse you if it's cold. They'll curse you if it's scalding."

In the corner stood an iron stove, its flue running under the floor of the pool. Add coal, the water heats. Open the windows, the heat escapes. Simple enough.

At a quarter past five, the first bathers climbed in and cried out in unison that it was freezing. Man hurried over and shoveled three loads of coal into the stove. The fire went up with a roar.

He waited. The water was still cold.

He waited a while longer. Still cold. The bathers began slapping the stone rim and complaining. Man set his jaw and shoveled in three more.

Half an hour later, the pool boiled. Truly boiled — steam swallowed the roof beams, red-faced men leapt out of the water, and the cursing was worse than before.

Man panicked, grabbed a bamboo pole, and threw all four shutters open. Cold air poured in. Fifteen minutes later a thin mist lay on the surface and the bathers were shouting about the cold again. So he shoveled coal. Then opened windows. Then shoveled coal.

By midday he had made seventeen trips, was soaked with sweat, and the pool had not been comfortable for a single moment. It was either scalding or freezing, like a swing shoved by two hands in turn — and the harder they shoved, the wider it swung.

Only then did the old master come strolling in from the back courtyard. He took one look, and sat down.

"Have you noticed," he said, "that the coal you shovel doesn't reach the water for half an hour?"

"I know. That's exactly why I can't wait."

"You can't wait because you're talking to the water as it is **now**. But the water is answering what you said **half an hour ago**. The two of you aren't even in the same hour."

The old master had a wooden temperature gauge hung by the pool, then laid down three rules and had them written on the wall.

**First: don't look at this instant's water. Look at the average over the last half hour.** Someone jumping in brings a swirl of cold with him. That doesn't count.

**Second: cold does not always mean coal, and hot does not always mean windows.** Add coal only below thirty-eight degrees. Open windows only above forty-two. Between thirty-eight and forty-two — do nothing at all, no matter how wrong it feels to your hand. That band belongs to the stove, so it can finish walking the half hour it's already walking.

**Third: one shovel at a time. Then sit down, count out a full half hour, and only then decide the next move.** Even while they're cursing you.

Man did as he was told. For the first two hours he felt like a loafer, sitting there doing nothing. But by the afternoon something strange had happened: he had added four shovels of coal the entire day, opened the windows once, and the pool had been warm from dawn to dusk.

The needle on that wooden gauge had barely moved.

— By now you've probably recognized it: this isn't a story about a bathhouse. It's a story about a *feedback control loop*. Man's seventeen trips are the autoscaler *flapping* that every SRE has watched happen: because there is a delay between action and effect, the controller oscillates endlessly between overshoot and undershoot. The old master's three rules are *metric smoothing*, *hysteresis*, and a *stabilization window*.

### What this is

Any closed loop of "measure → decide → act" risks oscillation the moment there is **delay between acting and observing the result**. Control theory calls it *dead time*: the longer the delay and the higher the gain, the more easily a system slides from convergence into self-sustained oscillation.

Kubernetes' HorizontalPodAutoscaler is the canonical example. It sees CPU high and scales up — but the new Pods must pull an image, start, pass readiness probes, and get admitted into the Service endpoints, which can take a minute or two. Throughout that window the metric is still high, so the controller keeps scaling; by the time all the Pods are finally up, capacity has badly overshot. The metric then collapses, the controller scales down, traffic arrives, and there isn't enough again. That is Man's swing.

The cure is not to make the controller *smarter*. It is to make the controller **slower** — deliberately, in four specific ways:

- **Metric smoothing.** Use an aggregate over a window, not an instantaneous sample. HPA reads an averaged value over the metrics-server scrape window precisely so that a single spike can't yank it around.
- **Hysteresis / deadband.** Use different trigger points going up and going down, with a dead zone between them. In HPA this is `--horizontal-pod-autoscaler-tolerance` (10% by default): if the ratio of desired to current replicas falls between 0.9 and 1.1, the controller does nothing. Without that band, a metric jittering around a single threshold fires the loop over and over.
- **Stabilization window.** `behavior.scaleDown.stabilizationWindowSeconds` defaults to 300 seconds, during which HPA takes the **maximum** of its recent recommendations — effectively demanding five straight minutes of calm before it will shrink. The scale-up window defaults to zero, because the cost of being overloaded usually dwarfs the cost of a few extra Pods: **the two directions were never meant to be equally cautious.**
- **Rate limit / step size.** The policies under `behavior` cap how many Pods (or what percentage) may be added or removed per period. One small step at a time is safer than jumping all the way.

None of this belongs to HPA alone. Cluster Autoscaler's `scale-down-unneeded-time`, BGP route flap damping, the deliberate asymmetry of additive-increase/multiplicative-decrease in TCP congestion control, the couple of degrees of swing built into your thermostat, even the pace at which you resize a team — all the same problem, all the same answer: **when feedback is delayed, the best reaction speed is not the fastest one.**

And there is a deeper lesson under it. Rather than endlessly tuning parameters to suppress the oscillation, first **shorten the delay itself**. Smaller images, faster startup, earlier readiness, tighter scrape intervals. The shorter the loop, the faster you can safely react. Tuning finds the optimum under a given delay; shortening the delay raises the optimum.

_Metaphor mapping_

- The water temperature → the observed *metric* (CPU, QPS, queue depth)
- Man → the controller (HPA, Cluster Autoscaler, any reconcile loop)
- Shoveling coal / opening windows → scale up / scale down
- The half hour between coal and warmth → *dead time*: Pod startup + readiness + metrics scrape interval
- "You talk to the water now, it answers what you said half an hour ago" → the controller always sees a **past** state of the system
- Seventeen trips, a swing widening with every shove → *oscillation* / flapping
- Bathers leaping out scalded → overshoot: wasted capacity and harmed users
- "Look at the average over the last half hour" → *metric smoothing*, the aggregation window
- Thirty-eight and forty-two as two separate thresholds → *hysteresis*, different trigger points per direction
- Doing nothing between thirty-eight and forty-two → *deadband* / HPA `tolerance`
- "Shovel, then sit and count out a full half hour" → *stabilization window* (300s by default on scale-down)
- "One shovel at a time" → *rate limit* / step size in scaling policies
- The old master's stillness → in a delayed loop, restraint is itself a correct control strategy
- Four shovels all day, and the water always warm → convergence: fewer actions, steadier results
</section>
