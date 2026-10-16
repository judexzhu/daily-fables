---
layout: fable
title: "篓底的麦捆 · The Sheaf at the Bottom of the Basket"
title_zh: "篓底的麦捆"
title_en: "The Sheaf at the Bottom of the Basket"
concept: "Work Stealing Scheduler"
tags: [scheduling, concurrency]
illustration: /assets/art/2026-10-16-work-stealing-scheduler.jpg
youtube_id: "1mipaMC2410"
---
<section class="zh" markdown="1">
九月的云溪村，千亩麦田翻涌如金海。

麦收是全村头等大事：九支割麦队，一条田垄一队，从村东排到村西。今年年成极好，麦穗沉得压弯了腰——可村口晒谷场上，大总管钱伯却愁白了头。

老法子里，全村的活计都攥在钱伯一个人手上。每支队伍割完一片，就得扛着麦捆走三里地回晒谷场，排长队等钱伯从签筒里抽出一张新路签，再指着下一块田。九支队伍，九条长龙，全绕着那支签筒打转。日头越毒，钱伯的手越赶不上九把镰刀：割得快的队伍杵在场边干等签；黑土垄里活又深又密，一支队伍陷在里头，旁人却不敢越界搭手。明明人人手里有镰，大半功夫却都耗在了排队上。

禾娘子接管晒谷场那天，只看了一炷香，便把钱伯的签筒收进了库房。

"割麦的人，自己最清楚麦在哪儿。"她给每支队伍发了一只崭新的高背竹篓，立下三条规矩。

第一条：**各家的活，装各家的篓。**
开镰前，每队把自家田垄拆成几大捆活计，像麦束一样捆好，码进自己的背篓。从此路签作废，不必再回村领活——篓里有活，地头就是活。

第二条：**自家取活，只从篓顶拿。**
割着割着，一垄麦常会岔成两片——比如割到溪湾，田分成了溪南与溪北。规矩是：把两片新活各自捆好，**摞上篓顶**，再伸手取下最上面那一捆，接着干。

老镰刀手起初不服："这不是舍近求远？"禾娘子只笑：刚捆上的活，必然挨着方才割过的地——镰口还热着，田埂还认得，顺手就能续上。压在篓底的老活，是整片没动过的生地，自有用处，先别碰。

第三条，也是最惊人的一条：**篓空了，去别人篓底"偷"一捆。**
哪支队伍手脚快，自家篓里见了底，不必回村，不必禀报，信步走向**随便哪一家**的田头。若那家篓里只剩一捆，那是人家的口粮，看也不看，转身另寻；若篓里还摞着两捆以上，就从**最底下**抽走一捆，扛回自家地头，闷头就割。

"这……这不是偷吗？"有后生咋舌。

"偷的是活，不是麦。"禾娘子淡淡道，"记住三小条：只偷篓底，一次一捆，只剩一捆的人家一捆也不许动。"

后生们更糊涂了：偷，为什么偏偷篓底？

禾娘子掰着手指："其一，主人取活永远从篓顶伸手，客人只从篓底抽活——两只手永远碰不到同一捆麦，谁也不跟谁抢。其二，篓底那捆最老、最大：顶上的活越分越碎，篓底压着的才是整片生地。你跑了三里地，就该扛回一捆够割一下午的，才不亏脚程。至于不许挑人家——若人人都挤去最忙的田头，最忙的那家反倒被讨活的围满，谁也偷不成。信步走，走到谁家算谁家。"

奇的是，整个麦收下来，"偷"竟没发生几回。

晒谷场立了记功牌：九支队伍整整七天，偷活统共不到二十回。道理藏在第二条规矩里——活是越割越碎的。起初人人篓里都是整片生地，越往后，摞上篓顶的净是巴掌大的支垄；九支队伍各自埋头细分，篓里的活自然越压越薄。信步走到谁家，十有八九人家篓里只剩一捆口粮，转身便走。只有谁家真积压了成片的活，"偷"才真的发生——而且一偷，就是一整片。

麦收结束那晚，禾娘子在祠堂黑板上写下一行字：

**全村收麦的时辰 ≈ 麦捆总数 ÷ 队数 + 最深那条岔垄的层数。**

九支队伍齐动手，总活摊成九份；可黑土垄那条最深的岔路——支垄套支垄，足足岔了十二层——却定下了全村再快也得割满的底数。人多，摊得薄的是总活；摊不平的，是那口最深的岔。

讲到这里，你多半已经认出来了：这就是并行计算中的 *work stealing*——工作窃取调度。

---

### 这是什么

*Work stealing*（工作窃取）是现代并行运行时最核心的调度策略之一：系统不再由一个中央队列统一派活，而是给**每个工作线程配一条专属的双端队列**（*deque*）。线程自己派生的子任务压入队头，自己也从队头取活（*LIFO*，后进先出）；而**空闲线程则从随机选中的"受害者"队列尾部偷走任务**（*FIFO*，先进先出）。

1999 年，Robert Blumofe 与 Charles Leiserson 证明了它的性能界：用 $P$ 个处理器执行总工作量为 $T_1$、关键路径深度为 $T_\infty$（*span*）的程序，期望运行时间满足 $T_P \le T_1/P + O(T_\infty)$——任务充足时加速比近乎线性，任务不足时优雅退化为关键路径长度。这正是黑板上那行"总捆数÷队数＋最深岔垄层数"的来历。

今天，它驱动着几乎所有主流并发运行时：Go 的 *goroutine* 调度器（每线程本地运行队列，偷时连半条队列一并端走）、Java 的 *ForkJoinPool* 与并行流、Rust 的 *Tokio*、.NET 线程池与 Intel TBB——你刷到的每一次请求背后，几乎都有线程在悄悄偷活。

### 为什么重要

1. **消灭中央瓶颈**：派活完全去中心化，$P$ 个线程各守各的队列，不存在被九条长龙围住的钱伯。争用只发生在罕见的"偷"上，且偷者随机分散，不会围堵同一家。
2. **缓存友好**：自家从队头拿刚派生的子任务，正接着方才的工作继续——数据还热在缓存里，镰口未冷。
3. **有硬保证的近线性加速**：$T_P \le T_1/P + O(T_\infty)$ 意味着只要活够多，加机器就近似等比例提速；而"最深岔垄"（*span*）给出了任何调度器都无法击穿的下界，工作窃取能逼近它。
4. **天然对抗不规则负载**：递归分治、动态派生的大量细粒度任务（编译器、并行流、*future* 组合子）忽深忽浅，静态分配必然失衡；工作窃取让失衡被一次次的"偷"自动抹平。

_隐喻对应表_

- 九支割麦队 → 并行运行时的工作线程（*worker threads*）
- 大总管钱伯与签筒 → 中央任务队列及其派活瓶颈（*central task queue*）
- 每队的高背竹篓 → 每线程专属双端队列（*per-worker deque*）
- 溪湾岔出的溪南溪北两片新活 → 任务派生子任务（*task forking*）
- 自家取活只从篓顶拿（新捆摞顶、取最上） → 队头 LIFO 压入/弹出，保住缓存局部性（*locality*）
- 空篓队去别家篓底抽一捆 → 空闲线程从受害者队尾 FIFO 偷任务（*steal from tail*）
- 只偷最老最大的篓底那捆 → 偷粗粒度任务，摊薄偷取成本（*coarse-grained steal*）
- 主人从顶拿、客人从底抽，永不抢同一捆 → 双端访问天然低争用（*low contention*）
- 信步走向随便哪一家 → 随机选择受害者，避免围堵热点（*randomized victim*）
- 只剩一捆的人家一捆不许动 → 队列长度 ≤ 1 时不偷（*steal only if len > 1*）
- 活越割越碎、偷越来越少 → *work-first* 原则，期望偷取次数仅 $O(P)$
- 最深岔垄的层数 → 关键路径深度（*span*，$T_\infty$）
- 祠堂黑板上的公式 → Blumofe–Leiserson 界 $T_P \le T_1/P + O(T_\infty)$
</section>
<section class="en" markdown="1">
September in Cloud-Creek Village: a thousand mu of wheat rolling like a golden sea.

Harvest was the village's weightiest season. Nine reaping crews, one to a strip of field, lined up from the village's east end to its west. The year had been bountiful—ears of wheat bowing under their own weight—yet at the threshing ground, Steward Qian, the grand dispatcher, was graying with worry.

Under the old rules, every task in the village passed through Steward Qian's hands. Whenever a crew finished a plot, they shouldered their sheaves, trudged three li back to the threshing ground, and queued for a fresh route slip drawn from Qian's bamboo tally-tube—only then would he point them at the next field. Nine crews, nine long lines, all circling that one tally-tube. The hotter the sun, the further Qian's hand fell behind nine sickles: the quick crews stood idle waiting for slips, while in the black-soil strip the work ran deep and dense—one crew mired in it, and no other dared step across the boundary to help. Every hand held a sickle; most of the day was spent standing in line.

The day Lady He took over the threshing ground, she watched for the burn of a single incense stick, then locked Qian's tally-tube in the storehouse.

"Reapers know best where the wheat is." She handed each crew a brand-new high-backed bamboo basket and laid down three rules.

Rule one: **every crew's work lives in that crew's basket.**
Before the first cut, each crew broke its strip into a few great bundles of work, tied them like sheaves, and stacked them in its own basket. The route slips were abolished—no more trudging back to the village for assignments. As long as your basket held work, the edge of your field was the assignment.

Rule two: **take your own work from the top of the basket.**
As you cut, a strip of wheat often forked into two—reach the creek bend, and the field split into south bank and north bank. The rule: tie each new piece of work into its own bundle, **stack both on top of the basket**, then reach up, take the topmost bundle, and keep cutting.

The old hands grumbled at first. "Isn't that the long way round?" Lady He only smiled: the freshest bundle always lies beside the field you just cut—your sickle's edge is still warm, your feet still know the furrows; work continues hand to hand. The old bundles pressed at the bottom of the basket are whole unploughed tracts. Leave them be; they have their own use.

Rule three, the most startling: **when your basket runs empty, go "steal" one bundle from the bottom of someone else's.**
A crew quick enough to empty its own basket need not return to the village, need not report to anyone—simply stroll toward the field of **whichever crew you happen upon**. If their basket holds a single bundle, that is their rations: don't even look at it, turn and try the next field. If it holds two or more, draw out **the bottom-most bundle**, carry it home to your own strip, and cut in silence.

"Th-that's stealing, isn't it?" gasped a young reaper.

"You steal work, not wheat," Lady He said mildly. "Remember the three small clauses: only from the bottom, one bundle at a time, and a basket down to its last bundle is untouchable."

The young ones were more confused than ever. Stealing—why the bottom?

Lady He counted on her fingers. "First: the owner always reaches in from the top, a guest only draws from the bottom—two hands can never seize the same bundle, so nobody fights anybody. Second: the bottom bundle is the oldest and the largest—the work near the top has been split finer and finer, but what's pressed at the bottom is a whole unploughed tract. If you've walked three li, you should carry back a bundle that cuts all afternoon, or the walk isn't worth your legs. As for not choosing your victim—if everyone crowded into the busiest field, the busiest crew would be mobbed by thieves and nobody would steal anything. Stroll where you please; whoever you reach is whoever you rob."

The strange thing: through the entire harvest, "stealing" barely happened.

The threshing ground kept a merit board: nine crews, seven full days, fewer than twenty thefts in all. The secret hid inside rule two—work cuts itself finer as it goes. At first every basket held whole unploughed tracts; as the days passed, what got stacked on top were ever smaller forks of a fork. Nine crews each buried in their own dividing, baskets pressed thinner and thinner. Stroll to any field, and nine times out of ten that basket was down to its last ration—turn and go. Only when a crew had genuinely hoarded a whole tract did a theft truly occur—and one theft carried off an entire field.

On the night the harvest closed, Lady He wrote a single line on the ancestral hall's blackboard:

**The village's harvest hours ≈ the total bundles ÷ the number of crews + the depth of the deepest-forking strip.**

Nine crews together spread the total work into nine shares; but the deepest fork in the black-soil strip—fork within fork, twelve layers deep—set the floor no amount of speed could cut beneath. More hands spread the total thinner; what they cannot flatten is the deepest fork.

By now you've probably recognized it: this is *work stealing*—the scheduling strategy at the heart of parallel runtimes.

---

### What it is

*Work stealing* is the core scheduling strategy of modern parallel runtimes: instead of one central queue handing out tasks, **every worker thread owns a double-ended queue** (*deque*). A thread pushes its own newly spawned subtasks onto the head and pops its own work from the head (*LIFO*); **idle threads steal tasks from the tail of a randomly chosen victim's deque** (*FIFO*).

In 1999, Robert Blumofe and Charles Leiserson proved its performance bound: executing a program of total work $T_1$ and critical-path depth $T_\infty$ (*span*) on $P$ processors yields expected running time $T_P \le T_1/P + O(T_\infty)$—near-linear speedup whenever work is abundant, degrading gracefully to the critical path when it is not. That is the blackboard formula: total bundles ÷ crews + the deepest fork.

Today it powers virtually every mainstream concurrency runtime: Go's *goroutine* scheduler (per-thread local run queues, stealing half a victim's queue at a stroke), Java's *ForkJoinPool* and parallel streams, Rust's *Tokio*, the .NET thread pool, and Intel TBB—behind nearly every request you make, some thread is quietly stealing work.

### Why it matters

1. **No central bottleneck**: dispatch is fully decentralized—$P$ threads each guard their own queue; there is no Steward Qian ringed by nine long lines. Contention happens only on rare steals, and thieves scatter randomly instead of mobbing one victim.
2. **Cache-friendly**: a worker takes its own newest subtask from the head—continuing exactly where it left off, with data still warm in cache and the sickle's edge still hot.
3. **Near-linear speedup with a hard guarantee**: $T_P \le T_1/P + O(T_\infty)$ means more machines ≈ proportionally faster whenever work is plentiful, while the deepest fork (*span*) sets the floor no scheduler can beat—and work stealing approaches it.
4. **Thrives on irregular load**: recursive divide-and-conquer, dynamically spawned fine-grained tasks (compilers, parallel streams, *future* combinators) run deep in some places and shallow in others; static partitioning must fail, while stealing smooths the imbalance away on its own.

_Metaphor mapping_

- Nine reaping crews → worker threads in a parallel runtime (*worker threads*)
- Steward Qian and his tally-tube → the central task queue and its dispatch bottleneck (*central task queue*)
- Each crew's high-backed bamboo basket → per-worker deque (*per-worker deque*)
- The creek bend forking the field into south and north banks → a task forking into subtasks (*task forking*)
- Taking your own work from the top of the basket (newest bundle stacked on top) → LIFO push/pop at the head, preserving locality (*locality*)
- An empty-basket crew drawing one bundle from another's basket bottom → an idle thread FIFO-stealing from the victim's tail (*steal from tail*)
- Stealing only the oldest, largest bottom bundle → coarse-grained steals that amortize the cost (*coarse-grained steal*)
- Owner takes from the top, guest from the bottom—never the same bundle → naturally low-contention access from both ends (*low contention*)
- Strolling toward whichever crew you happen upon → randomized victim selection to avoid hotspot mobs (*randomized victim*)
- A basket down to its last bundle is untouchable → no stealing when queue length ≤ 1 (*steal only if len > 1*)
- Work cutting finer and finer, thefts rarer and rarer → the *work-first* principle—expected steals are only $O(P)$
- The depth of the deepest-forking strip → critical-path depth (*span*, $T_\infty$)
- The formula on the ancestral hall's blackboard → the Blumofe–Leiserson bound $T_P \le T_1/P + O(T_\infty)$
</section>
