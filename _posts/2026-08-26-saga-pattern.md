---
layout: fable
title: "红娘的规矩 · The Matchmaker's Rule"
title_zh: "红娘的规矩"
title_en: "The Matchmaker's Rule"
concept: "Saga pattern"
tags: [distributed-systems, microservices]
illustration: /assets/art/2026-08-26-saga-pattern.jpg
---
<section class="zh" markdown="1">
镇上有个出了名的红娘，叫阿绣。

阿绣说过一句话，人人都记得："我只接一种婚事——**退得干净的**。"

别的红娘谈亲，只管往前推：先说通男方家，再说通女方家，请了媒人，定了聘礼，选了日子，订了酒楼，雇了花轿，最后拜堂成亲。一环接一环，热热闹闹往前赶。万一中间哪一环出了岔子——比如酒楼突然着了火——前面谈好的全部陷在那里：聘礼退不回来，花轿退不掉，两家人面子挂不住，一桩婚事变成一桩烂账。

阿绣不这么干。

她有一本账本。每谈成一步，她就在账本上写两行：第一行写做了什么，第二行写"如果要退，怎么退"。

比如：

- 说通男方家 → 如果要退，亲自登门致歉，退还信物
- 定下聘礼 → 如果要退，按约定退还聘金，外加赔礼一份
- 订好酒楼 → 如果要退，提前三日退订，付定金一成作为补偿
- 雇好花轿 → 如果要退，通知轿行取消，退还预付银两

每一步往前走之前，她先确认退路写好了。退路没写清楚的事，她不做。

镇上人笑她麻烦："婚事哪有那么多意外？直接往前推不就行了？"

阿绣不理。

直到有一天，一桩大婚出了事。两家都是镇上体面人家，排场极大。阿绣已经推到第五步——酒楼订好，花轿雇好，喜帖发出，聘礼过了，连戏班子都请好了。

然后女方家突然来信：女儿不愿意，婚事取消。

换了别的红娘，这时候就慌了。五步全卡在那里，每一步都牵着钱和面子，退一步都要吵架，退五步等于打仗。

阿绣翻开账本，从最后一步开始退：

第五步：通知戏班子取消，按约定付半价定金——退。
第四步：收回喜帖，给每家送一份点心致歉——退。
第三步：花轿行取消，退还预付——退。
第二步：酒楼退订，付定金一成——退。
第一步：登门男方家致歉，退还聘金加赔礼——退。

五步，三天，退得干干净净。两家人虽然扫兴，但没人吃亏，没有烂账。男方家甚至说："虽然婚没成，但阿绣这人，下次有事还找她。"

后来有人问阿绣："你每桩婚事都要花双倍功夫写退路，值吗？"

阿绣说："我做一百桩婚事，九十九桩顺顺当当走完。但那一桩出事的，如果退不干净，毁的不只是一桩婚，是两个家。写退路不是因为我悲观——是因为我知道，世上的事，越大越不能只有一条路。"

还有人问："为什么你不像老钱那样做？老钱谈婚事，先让两家都把银子锁在柜里不许动，等所有事都敲定了再一起拿出来。这样不是更稳？"

阿绣摇头："老钱的法子，一桩小婚事可以。排场大了，你让两家和酒楼和轿行和戏班子都把银子锁着不动，等你一个人把所有环节都谈好？没人等得起。我的法子不锁银子——每一步谈完就付，就做，就往前走。只要退路在手，出了事随时收得回来。"

——到这儿你大概已经认出来了：这就是 *Saga pattern*。

### 这是什么

*Saga pattern* 是一种管理**长事务**（*long-running transaction*）的方法。与 *two-phase commit*（2PC）将所有参与者锁定直到统一提交不同，Saga 把一个大事务拆成一系列**本地事务**（*local transactions*），每个本地事务完成后立即提交。每个本地事务都对应一个预定义的**补偿事务**（*compensating transaction*），如果后续某一步失败，系统按**逆序**执行已完成步骤的补偿事务，把状态回滚到初始状态。

Saga 有两种协调方式：**编排式**（*choreography*），每个服务完成后触发下一个，像接力赛；**指挥式**（*orchestration*），由一个中央协调器按顺序调用各服务。阿绣就是指挥式——她拿着账本一步步推。

### 为什么重要

在微服务架构中，一个业务操作（下单、支付、发货）往往跨越多个服务和数据库，无法用单一数据库事务保证一致性。2PC 理论上可行，但需要所有参与者长时间持锁，在高并发分布式系统中会严重影响吞吐和可用性。Saga 放弃全局锁，换取每一步立即提交的灵活性，代价是必须为每一步设计补偿逻辑。理解 Saga 直接影响你设计订单系统、支付流程、跨服务数据迁移等长链路操作时的可靠性和可恢复性。

_隐喻对应表_

- 红娘阿绣 → Saga 协调器（*orchestrator*）
- 一桩大婚 → 长事务（*long-running transaction*）
- 每一步（说通男方、定聘礼、订酒楼……） → 本地事务（*local transaction*）
- 账本上的"如果要退" → 补偿事务（*compensating transaction*）
- 从最后一步开始倒着退 → 逆序执行补偿事务
- 每一步谈完就付、就做 → 本地事务立即提交，无全局锁
- 老钱的做法（把银子锁起来等全部谈好） → *two-phase commit*（2PC），所有参与者加锁等待统一提交
- 没人等得起 → 2PC 在高并发/长事务场景下阻塞严重
- "退路没写清楚的事，她不做" → 每个本地事务必须有对应的补偿事务才能纳入 Saga
- "九十九桩顺顺当当，一桩出事退得干净" → Saga 的设计理念：正常路径高效，异常路径可恢复
</section>
<section class="en" markdown="1">
There was a matchmaker in town named Ah Xiu, famous for one rule she never broke.

"I only take weddings," she said, "that can be **cleanly unwound**."

Other matchmakers charged forward: persuade the groom's family, persuade the bride's family, arrange the betrothal gifts, book the banquet hall, hire the sedan chair, and march straight to the ceremony. Step after step, momentum carrying everyone along. If something went wrong midway — say the banquet hall caught fire — everything that came before was stuck: betrothal gifts couldn't be returned, the sedan chair couldn't be un-hired, both families lost face, and one wedding became one mess.

Ah Xiu worked differently.

She kept a ledger. For every step she completed, she wrote two lines: the first recorded what was done, the second recorded how to undo it.

For example:

- Persuade groom's family → To undo: visit in person to apologize, return the token of engagement
- Settle betrothal gifts → To undo: return the gift money, plus a consolation fee
- Book banquet hall → To undo: cancel three days in advance, forfeit ten percent deposit
- Hire sedan chair → To undo: notify the carrier to cancel, refund the advance payment

Before taking each step forward, she made sure the way back was written down. If she couldn't define the retreat, she wouldn't proceed.

The townspeople laughed. "Weddings don't go wrong that often. Just push forward!"

Ah Xiu ignored them.

Then one day, a grand wedding fell apart. Both families were prominent, the scale enormous. Ah Xiu had pushed through five steps — banquet hall booked, sedan chair hired, invitations sent, betrothal gifts delivered, even a theater troupe engaged.

Then word came from the bride's family: the daughter refused. The wedding was off.

Any other matchmaker would have panicked. Five steps, each tangled in money and reputation. Undoing one meant arguments; undoing all five meant war.

Ah Xiu opened her ledger and started from the last step:

Step five: notify the theater troupe, pay the half-price cancellation fee — done.
Step four: recall invitations, send pastries to each household as apology — done.
Step three: cancel the sedan chair, process the refund — done.
Step two: cancel the banquet hall, forfeit the ten percent — done.
Step one: visit the groom's family in person, return the betrothal gifts plus consolation — done.

Five steps. Three days. Cleanly unwound. Both families were disappointed but not ruined. The groom's family even said, "The wedding didn't happen, but next time we need something arranged, we'll call Ah Xiu."

Someone asked her later: "You spend double the effort writing undo steps for every wedding. Is it worth it?"

Ah Xiu said: "I arrange a hundred weddings, and ninety-nine go through without a hitch. But the one that fails — if it can't be unwound cleanly, it doesn't just ruin a wedding. It ruins two families. I write undo steps not because I'm pessimistic — but because I know that the bigger the stakes, the more you need a way back."

Someone else asked: "Why don't you work like Old Qian? He makes both families lock their money in a chest until every single detail is finalized, then everyone acts at once. Isn't that safer?"

Ah Xiu shook her head. "Old Qian's method works for small weddings. But for a grand affair, you're asking two families, a banquet hall, a sedan carrier, and a theater troupe all to freeze their money and wait while one person negotiates every detail? Nobody can wait that long. My way doesn't lock anything — each step is paid and done the moment it's settled. As long as I have the way back written down, I can recover from anything."

By now you've probably recognized it: this is the *Saga pattern*.

### What it is

The *Saga pattern* is a method for managing *long-running transactions*. Unlike *two-phase commit* (2PC), which locks all participants until a unified commit, a Saga splits one large transaction into a series of *local transactions*, each committed immediately upon completion. Each local transaction has a predefined *compensating transaction* — if a later step fails, the system executes the compensating transactions for all completed steps *in reverse order*, rolling the state back to the beginning.

Sagas come in two coordination styles: *choreography*, where each service triggers the next upon completion (like a relay race), and *orchestration*, where a central coordinator drives the sequence. Ah Xiu is the orchestrator — she holds the ledger and drives each step.

### Why it matters

In microservice architectures, a single business operation (place order, charge payment, ship goods) often spans multiple services and databases, making a single database transaction impossible. 2PC is theoretically sound but requires all participants to hold locks for the entire duration, severely impacting throughput and availability in high-concurrency distributed systems. Sagas trade the global lock for the flexibility of immediate per-step commits, at the cost of requiring compensating logic for every step. Understanding Sagas directly affects how you design order systems, payment flows, cross-service data migrations, and any long-chain operation that must be both reliable and recoverable.

_Metaphor mapping_

- Matchmaker Ah Xiu → Saga orchestrator
- A grand wedding → long-running transaction
- Each step (persuade family, settle gifts, book hall…) → local transaction
- "How to undo" in the ledger → compensating transaction
- Undoing from the last step backward → reverse-order compensation execution
- Each step paid and done immediately → local transactions commit immediately, no global lock
- Old Qian's method (lock money until everything is finalized) → *two-phase commit* (2PC), all participants lock and wait for unified commit
- "Nobody can wait that long" → 2PC blocks severely under high concurrency / long transactions
- "If she couldn't define the retreat, she wouldn't proceed" → every local transaction must have a compensating transaction to be part of a Saga
- "Ninety-nine go through, one unwinds cleanly" → Saga's design philosophy: efficient happy path, recoverable failure path
</section>
