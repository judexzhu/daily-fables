---
layout: fable
title: "最后一道菜 · The Last Dish"
title_zh: "最后一道菜"
title_en: "The Last Dish"
concept: "Tail latency amplification and hedged requests"
tags: [distributed-systems, performance]
illustration: /assets/art/2026-08-07-tail-latency-hedged-requests.jpg
---
<section class="zh" markdown="1">
喜宴摆在祠堂前的院子里，四十桌。管调度的是老周。

院子东边一溜四十只小灶，一灶一菜，一菜一厨。传菜的伙计十来个，端着圆托盘在灶和桌之间穿来穿去。规矩是老规矩：一桌四十道菜，**得齐了才开席**。少一道，满桌人就干坐着，看着先到的热菜一点点凉下去。

老周头一年接这活儿的时候很得意。他算过账：一个厨子出一道菜，快的时候一炷香，慢的时候两炷香，一百回里顶多有一回会拖到三炷香。三炷香那种事，一百回才一回啊——他跟东家拍胸脯，稳得很。

结果头一场宴，二十桌里有十七桌开席晚了半个时辰。

东家来问，他挨个去查。四十个厨子，一个个问过来，每个都说：我那道菜没耽误啊。他们说的都是实话。那天绝大多数菜确实是一炷香出的锅。可一桌四十道菜，只要有**一道**恰好落在那"一百回里的一回"上，整桌人就得等那一道。四十个厨子，每人一百回里错一回——一桌能全须全尾躲过去的机会，其实连三分之二都不到。

老周这才看明白账错在哪儿：他一直在算"厨子平均多快"，可开席的时辰从来不由平均那位决定，只由**最慢那位**决定。桌上菜越多，最慢那位就越躲不开。

第二年他换了法子。

先是不再问厨子"你今天快不快"，改成每场宴派个小徒弟蹲在院口，只记一件事：每一道菜从下单到落桌，花了多久。一场宴下来一千多条。他不看那个平均数，他把条子按快慢一字排开，专看**排在最末尾那几十条**。看得多了就摸出门道：拖后腿的那些，多半不是厨子懒。有的是四十只灶共用院子当中那一口井，打水排了长队；有的是厨子做满两道菜就得停下来刮锅、添炭——那活儿不干不行，可一干就是小半炷香；有的是前头压着一桌九人的大席，后头的小菜只能干等。慢从来不是随机的坏运气，是**那些共用的东西**在某个时刻恰好挤到了一起。

然后是他那招最出名的。

如今菜单送出去，老周并不盯着厨子看。他只在托盘架上摆一排竹签，一道菜一根。等到某根签子插过去、迟迟不见收回——**不是刚插上就急，而是等到它明显比同场的其他签子都慢了**——他才抬手叫一个闲着的伙计：西头那口备灶，同一道菜，再来一份。

于是同一道菜，两个厨子在做。哪边先出锅哪边上桌，另一边一挥手，撤。

新来的伙计觉得这是白扔柴火。老周让他自己数：一场宴一千多道菜，真被他叫过"再来一份"的，不到五十道——因为他从不一上来就派双份，只在那道菜已经慢过了绝大多数同伴之后才派。多烧的柴不到一成，可十七桌迟开变成了一两桌。

后来他又添了两条。一是让备灶和原灶彼此知道：谁先动第二勺，就朝对面吼一声，另一个立刻撂下不做，省得两口锅都白占着。二是记账——哪个厨子连着三场都排在末尾那几十条里，就先把他从名册上摘下来歇几天，只派些不进席面的闲菜给他，试几回稳当了再放回去。

——到这儿你大概已经认出来了：老周对付的，是 *tail latency*（尾延迟）在 *fan-out* 之下的放大；而他那招"再来一份"，就是 Google《The Tail at Scale》里讲的 *hedged request*。

那年冬天东家想再稳一点，问老周：干脆每道菜都做两份，行不行？老周说不行。四十只灶就那些炭、就那一口井，你把活儿翻一倍，井边的队也翻一倍，人人都慢——原先只有一道菜拖后腿，到时候是道道拖后腿。

对付最慢那一道的法子，绝不能是让所有道都变慢。

### 这是什么

一次请求 fan out 到很多个后端（40 个 shard、40 次并行 RPC、40 个 pod），而响应必须等**全部**子请求返回才能合成——这时整体延迟等于其中**最慢**那一个的延迟。哪怕每个后端只有 1% 的概率走上慢路径，40 路并发下"至少有一路慢"的概率就接近 33%；100 路是 63%。这就是尾延迟放大（tail latency amplification）。

### 为什么重要

- 在 fan-out 场景里，平均延迟（mean）几乎没有诊断价值，必须看 p99 / p99.9。用户感受到的是尾巴，不是平均。
- 尾巴通常不是随机的，而是有结构的：共享资源竞争、后台任务（GC、compaction、日志轮转、CPU throttling）、队头阻塞、冷缓存。找结构比加机器有用。
- 主要缓解手段：*hedged request*（等到 p95 再补发第二份，取先到的、取消另一个）、*tied request*（两个副本互相通知，谁先开工谁通吃）、micro-partitioning 加选择性复制、把持续慢的副本放进 probation。
- Hedge 的关键在**阈值**：只对超过 p95 的那一小撮请求补发，额外负载就被压在个位数百分比；无条件双发会把系统整体推向饱和，尾巴反而更长。

_隐喻对应表_

- 一桌四十道菜、齐了才开席 → fan-out 请求，必须等所有子请求返回才能响应
- 每个厨子"一百回里慢一回" → 单个后端的 p99 慢路径
- 二十桌里十七桌迟开 → tail latency amplification，1 − 0.99⁴⁰ ≈ 33%
- 老周头一年算的"厨子平均多快" → mean latency，在 fan-out 下具有误导性的指标
- 蹲在院口一条条记时的小徒弟 → per-request latency instrumentation
- 只看排在末尾那几十条 → p99 / p99.9 分位数
- 共用的那口井、刮锅添炭、前头压着的大席 → 共享资源竞争、后台维护任务（GC / compaction）、队头阻塞
- 等签子明显慢过其他签子才叫"再来一份" → hedged request，阈值设在 p95 而非立即双发
- 哪边先出锅哪边上桌、另一边撤 → 取最先返回的结果，取消其余副本
- 多烧的柴不到一成 → 阈值把 hedge 的额外负载限制在个位数百分比
- 两口灶互相吼一声 → tied request，副本间的 cross-server cancellation
- 连着三场垫底就摘下名册歇几天 → latency-induced probation，把慢副本暂时移出服务
- "不能每道菜都做两份" → 无条件冗余耗尽共享容量，反而让整体延迟更差
</section>
<section class="en" markdown="1">
The wedding banquet was set up in the courtyard in front of the ancestral hall. Forty tables. Old Zhou ran the floor.

Along the east wall stood forty small stoves — one stove per dish, one cook per stove. A dozen runners wove between the stoves and the tables with round trays. The rule was the old rule: forty dishes to a table, and **nobody eats until all forty have landed**. One dish missing and the whole table sits there watching the ones that already arrived go cold.

Old Zhou was pleased with himself that first year. He had done the arithmetic. A cook turns out a dish in one stick of incense on a good run, two on a bad one, and maybe once in a hundred times something drags it out to three. Once in a hundred — he told the family it was nothing to worry about.

At the first banquet, seventeen of the twenty tables started half an hour late.

The family asked him why. He went down the line, cook by cook, all forty of them, and every one of them said the same thing: my dish wasn't the problem. They were all telling the truth. Nearly every dish that night came off the fire in one stick of incense. But a table needs forty dishes, and if **any one** of them lands on that once-in-a-hundred, the whole table waits for that one. Forty cooks, each missing once in a hundred — the odds of a table getting through clean weren't even two in three.

That was when Old Zhou saw where his arithmetic had gone wrong. He had been reckoning how fast a cook was *on average*, but the hour the table starts eating is never set by the average cook. It's set by **the slowest one**. And the more dishes on the table, the harder that slowest one is to dodge.

The second year he changed his method.

First, he stopped asking cooks whether they were having a good night. Instead he posted an apprentice at the courtyard gate with one job: write down how long every single dish took, from the order going out to the plate landing. A banquet produced more than a thousand lines. He didn't look at the average. He laid the lines out fastest to slowest and read only **the few dozen at the very bottom**. After enough banquets he started seeing the shape of it. The stragglers were rarely a lazy cook. Sometimes forty stoves were all drawing from the one well in the middle of the courtyard, and there was a queue at the rope. Sometimes a cook had to stop after two dishes to scrape the wok and bank the coals — necessary work, but it ate half a stick of incense. Sometimes a table of nine was jammed up ahead in the line and everything small behind it simply waited. Slowness was never random bad luck. It was **the shared things** all crowding into the same moment.

Then came the trick he became known for.

These days, once the order goes out, Old Zhou doesn't watch the cooks at all. He keeps a row of bamboo tallies on the tray rack, one per dish. When a tally goes in and doesn't come back — **not the instant it goes in, but once it has clearly fallen behind the rest of the tallies on the rack** — he raises a hand and calls an idle runner: the spare stove at the west end, same dish, one more.

So two cooks are making the same dish. Whichever comes off the fire first goes to the table, and the other one gets waved off.

A new runner said it was firewood thrown away. Old Zhou told him to count it himself. Over a thousand dishes in a banquet, and fewer than fifty ever got a "one more" — because he never sends two from the start, only after that dish has already fallen behind almost all of its peers. Less than a tenth extra firewood. Seventeen late tables became one or two.

Later he added two more rules. One: the spare stove and the original stove each know about the other, so whichever picks up the second ladle first shouts across the courtyard, and the other drops it immediately — no sense tying up two woks. Two: he keeps a ledger. Any cook who lands in that bottom few dozen three banquets running comes off the roster for a few days and gets only the side dishes that never reach the banquet table, until a few quiet runs prove he's steady again.

— And by now you've probably recognized it: what Old Zhou is fighting is *tail latency* amplified by *fan-out*, and his "one more" is the *hedged request* from Google's **The Tail at Scale**.

That winter the family wanted to be extra safe and asked him: why not just cook every dish twice? Old Zhou said no. Forty stoves share the same coal and the same well. Double the work and you double the queue at the rope, and then everyone is slow — instead of one dish dragging the table, every dish drags.

The cure for the slowest one can never be making all of them slower.

**What this is**

A request fans out to many backends — forty shards, forty parallel RPCs, forty pods — and the response can't be assembled until **all** the sub-requests come back. Overall latency therefore equals the latency of the **slowest** one. Even if each backend takes a slow path only 1% of the time, the chance that at least one of forty is slow is close to 33%; at a hundred, it's 63%. That's tail latency amplification.

### Why it matters

- Under fan-out, mean latency has almost no diagnostic value. You need p99 / p99.9. Users feel the tail, not the average.
- The tail is usually structured, not random: shared-resource contention, background work (GC, compaction, log rotation, CPU throttling), head-of-line blocking, cold caches. Finding the structure beats adding machines.
- The main mitigations: *hedged requests* (wait until p95, then send a duplicate, take the first, cancel the other), *tied requests* (replicas notify each other; first to start wins), micro-partitioning with selective replication, and probation for persistently slow replicas.
- The whole trick of hedging is **the threshold**: duplicate only the small slice past p95 and the extra load stays in the single-digit percents. Unconditional duplication drives the system toward saturation and makes the tail worse.

_Metaphor mapping_

- forty dishes to a table, nobody eats until all arrive → fan-out request; the response waits on every sub-request
- each cook slow "once in a hundred" → a single backend's p99 slow path
- seventeen of twenty tables late → tail latency amplification, 1 − 0.99⁴⁰ ≈ 33%
- Old Zhou's first-year arithmetic about the average cook → mean latency, a misleading metric under fan-out
- the apprentice at the gate timing every dish → per-request latency instrumentation
- reading only the bottom few dozen lines → p99 / p99.9 percentiles
- the shared well, scraping the wok and banking coals, the table of nine jammed ahead → resource contention, background maintenance (GC / compaction), head-of-line blocking
- calling "one more" only after a tally clearly falls behind → hedged request with the threshold at p95, not immediate duplication
- first off the fire goes to the table, the other is waved off → take the first response, cancel the remaining replicas
- less than a tenth extra firewood → the threshold caps hedging overhead at single-digit percent
- the two stoves shouting across the courtyard → tied requests, cross-server cancellation
- three bad banquets and off the roster for a few days → latency-induced probation, pulling a slow replica out of service
- "we can't cook every dish twice" → unconditional redundancy exhausts shared capacity and worsens overall latency
</section>
