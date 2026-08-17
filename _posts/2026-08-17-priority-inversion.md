---
layout: fable
title: "井台上的那只木桶 · The Only Bucket at the Well"
title_zh: "井台上的那只木桶"
title_en: "The Only Bucket at the Well"
concept: "Priority Inversion (and Priority Inheritance)"
tags: [linux, distributed-systems]
illustration: /assets/art/2026-08-17-priority-inversion.jpg
---
<section class="zh" markdown="1">
天刚亮，山村的井台上还没人。小徒弟阿禾第一个到，把那只木桶挂上绳钩，一圈一圈往下放。

村里只有这一口井，井上只有这一只桶。规矩很简单：**桶在谁手上，就归谁用完再交出来**。别人急不急，桶不认。

阿禾放到一半，老院长匆匆赶来了。后山药炉上的水快熬干了，火不能停——全村最要紧的事，此刻就压在他身上。按司水人的名册，老院长的名分最高，理应第一个取水。

可老院长走到井台，只能停下。桶在阿禾手上。名分再高，也没法把别人手里正在用的桶抢过来——名册管的是**排队的次序**，不是**桶的归属**。他只能站着等阿禾摇上来。

按理说，这一等不算久：阿禾摇几十下就好。

坏在下一刻。集市的商贩们挑着担子涌到井边，要给驮马饮水。司水人翻开名册：商贩的名分比一个小徒弟高。于是他一挥手："让商队先。"阿禾被叫下井台，站到一旁——**但桶绳还系在他腕上，他没打完，也就没交出去**。

商贩一队接一队。每来一个，名分都压过阿禾，阿禾就一次又一次被叫下井台。他手里攥着那只桶，却永远轮不到把它用完。

于是井台上出现了一件荒唐事：全村最急的老院长，被一群商贩挡在了后面。而按名册，这些商贩本来一个都排不到他前面。挡住他的不是他们的名分，是那只卡在阿禾手上、永远交不出来的桶。而商队一整个上午都不会断——这一等，没有上限。

——到这儿你大概已经认出来了：这就是 *priority inversion*。

司水人的老规矩里，其实有一条压箱底的：**当高名分的人在等某人手上的桶时，就把他的名分暂借给持桶的那个人。** 阿禾借来老院长的名分，立刻被请回井台；商贩再来也得让他。他摇上一桶水，把桶交出，名分随即归还，他重新变回队尾那个小徒弟。

更严的村子还有另一条：**凡碰这只桶的人，一律先升到"所有会用这只桶的人当中最高的那个名分"**，碰之前就升，用完就降。

### 这是什么

*Priority inversion* 出现在同时具备两件事的系统里：**基于优先级的抢占式调度**，和**互斥锁（mutex）**。调度器按优先级决定谁跑，但锁的所有权不按优先级转移——锁在谁手上就在谁手上。

于是：低优先级任务 L 持有锁，高优先级任务 H 阻塞在同一把锁上，此时中优先级任务 M（根本不需要这把锁）不断抢占 L。L 跑不完，锁不释放，H 就一直等。H 实际上被 M 阻塞了，尽管 H 的优先级高于 M。这叫 *unbounded priority inversion*：只要 M 这类任务源源不断，H 的等待时间没有理论上界。

经典事故是 1997 年的 Mars Pathfinder：火星车着陆后反复重启，根因是一个低优先级的气象数据任务持有 VxWorks 的 mutex，被中优先级的通信任务反复抢占，高优先级的 bus management 任务超时，watchdog 于是复位整机。NASA 远程打开了 VxWorks 的 *priority inheritance*，问题消失。

两种标准解法：*priority inheritance*（持锁者临时继承所有阻塞者中的最高优先级，释放后恢复）和 *priority ceiling*（每把锁预先标注一个"天花板"优先级，取锁瞬间就把持有者提上去）。Linux 的 `PTHREAD_PRIO_INHERIT` 和 RT-mutex 就是前者。

工程上的味道：任何"最急的那件事被一堆不重要的事间接饿死"的现象都是它的亲戚——`SCHED_FIFO` 的实时线程等一把被普通线程持有的 mutex；critical pod 等 kubelet 里一把被低优先级 reconcile 循环占着的锁；一条紧急请求排在共享连接池后面，而池子被一堆低价值查询轮流占满。规律都一样：**优先级只作用于"跑不跑"，不作用于"锁在谁手上"。**

_隐喻对应表_

- 井上唯一的木桶 → *mutex*：不可抢占的独占资源
- 小徒弟阿禾 → 低优先级任务 L，锁的持有者
- 老院长和熬干的药炉 → 高优先级任务 H，阻塞在同一把锁上
- 集市商贩 → 中优先级任务 M，可运行且不需要这把锁
- 司水人和他的名册 → 基于优先级的抢占式 scheduler
- "桶在谁手上归谁用完" → 锁的所有权不随优先级转移
- 老院长站着干等 → *blocking*，正当的锁等待
- 商贩一队接一队地插到阿禾前面 → M 反复抢占 L，锁迟迟不释放
- 商队一上午不断 → *unbounded* priority inversion：等待没有上界
- 把名分暂借给持桶的人 → *priority inheritance*
- 碰桶就先升到最高名分 → *priority ceiling protocol*
- 交出桶后名分归还 → 释放锁即恢复原优先级
</section>
<section class="en" markdown="1">
At first light the well platform is still empty. Ahe, the youngest apprentice, gets there first, hooks the wooden bucket onto the rope, and begins letting it down, coil by coil.

The village has one well, and the well has one bucket. The rule is simple: **whoever holds the bucket keeps it until they are done, then hands it over**. The bucket does not care how urgent anyone else's errand is.

Halfway down, the old abbot arrives in a hurry. The medicine pot on the mountain fire is nearly dry and the fire cannot be let out — the most urgent business in the whole village rests on him this morning. By the water steward's ledger, the abbot's rank is the highest here. He should draw first.

But when he reaches the platform he simply has to stop. The bucket is in Ahe's hands. Rank, however high, cannot pull a bucket out of someone else's grip — the ledger governs **the order of the queue**, not **the ownership of the bucket**. All he can do is stand and wait for Ahe to haul it up.

By any reasonable accounting, that wait should be short. A few dozen turns of the rope.

Then the next moment ruins it. Merchants from the market crowd in with their shoulder poles, wanting water for the pack mules. The steward consults his ledger: a merchant outranks a boy apprentice. He waves them through — "the caravan first." Ahe is called down off the platform and stands aside — **but the rope is still looped around his wrist. He never finished, so he never handed it over.**

Caravan after caravan. Every arrival outranks Ahe, so every arrival calls him down off the platform again. He is clutching the bucket and never once gets his turn to finish with it.

And so something absurd stands at the well: the abbot, the most urgent person in the village, is stuck behind a crowd of merchants — not one of whom, by the ledger, ranks above him. What blocks him is not their rank. It is a bucket wedged in the hands of a boy who is never allowed to give it back. And the caravans will keep coming all morning — so there is no telling how long the wait runs.

— By now you have probably recognized it: this is *priority inversion*.

Buried in the steward's old rules there is one he rarely needs: **when a high-ranking person is waiting on a bucket in someone's hands, lend that person the high rank for the moment.** Ahe borrows the abbot's rank and is invited straight back to the platform; arriving merchants now yield to him. He hauls up one bucket, hands it over, gives the rank back, and turns into the last boy in the queue again.

Stricter villages keep another rule: **anyone who touches this bucket is raised, at the moment of touching, to the highest rank among everyone who ever uses it** — raised on grasping, lowered on releasing.

### What this is

*Priority inversion* appears in any system with two properties at once: **priority-based preemptive scheduling** and **mutual exclusion locks**. The scheduler decides who runs by priority, but lock ownership does not transfer by priority — whoever holds the lock holds it.

So: low-priority task L holds a lock; high-priority task H blocks on that same lock; and medium-priority task M — which does not want the lock at all — repeatedly preempts L. L cannot finish, the lock is not released, and H keeps waiting. H is effectively blocked by M, even though H outranks M. Where the supply of M-like work is unbounded, so is the delay — hence *unbounded priority inversion*.

The canonical accident is Mars Pathfinder, 1997. After landing, the rover kept resetting. The root cause: a low-priority meteorological data task held a VxWorks mutex and was repeatedly preempted by a medium-priority communications task; the high-priority bus management task timed out waiting on the same mutex, and the watchdog reset the spacecraft. NASA remotely enabled VxWorks' *priority inheritance* and the resets stopped.

Two standard remedies: *priority inheritance* (the lock holder temporarily inherits the highest priority among its blocked waiters, and drops back on release) and *priority ceiling* (each lock carries a precomputed ceiling priority, and any holder is raised to it the instant it acquires). Linux's `PTHREAD_PRIO_INHERIT` and its RT-mutexes implement the former.

The engineering smell: anything where *the most urgent work gets indirectly starved by a pile of unimportant work* is a relative. A `SCHED_FIFO` realtime thread waiting on a mutex held by an ordinary thread. A critical pod waiting on a kubelet lock held by a low-priority reconcile loop. An urgent request queued behind a shared connection pool that low-value queries keep fully occupied. The pattern is always the same: **priority governs who runs, not who holds the lock.**

_Metaphor mapping_

- the single wooden bucket → the *mutex*: a non-preemptible exclusive resource
- Ahe the apprentice → low-priority task L, the lock holder
- the abbot and the drying medicine pot → high-priority task H, blocked on the same lock
- the market merchants → medium-priority tasks M, runnable and not needing the lock
- the water steward and his ledger → the priority-based preemptive scheduler
- "whoever holds the bucket keeps it until done" → lock ownership does not transfer by priority
- the abbot standing and waiting → *blocking*: legitimate lock contention
- caravans cutting ahead of Ahe again and again → M preempting L, delaying release
- caravans arriving all morning → *unbounded* priority inversion: no ceiling on the wait
- lending the high rank to whoever holds the bucket → *priority inheritance*
- raised to the top rank on touching the bucket → the *priority ceiling protocol*
- rank returned once the bucket is handed over → priority restored on lock release
</section>
