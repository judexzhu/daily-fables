---
layout: fable
title: "借灯的规矩 · The Lantern Lease"
title_zh: "借灯的规矩"
title_en: "The Lantern Lease"
concept: "Lease-based distributed locking"
tags: [distributed-systems, kubernetes, sre]
illustration: /assets/art/2026-08-31-lease-based-locking.jpg
---
<section class="zh" markdown="1">
南塘镇只有一盏夜灯。

铁灯挂在镇口的石柱上，是全镇最亮的那盏，能照出整条青石巷的影子。夜里要走暗巷、要查水渠、要巡仓房的人，都得用这盏灯。可灯只有一盏，争来争去，有人没灯也硬走，摔进过水渠；有人一借不还，别人干等到天亮。

镇长想了个办法，叫"借灯牌"。

规矩是这样的：灯旁挂一块木牌，正面刻着"空"。谁要借灯，先翻牌子看——写着"空"，就把自己的名字写上去，注明"还灯时辰"，然后取灯走人。别人来了，看见牌上有名字，就知道灯被借走了，只能等。

关键在"还灯时辰"这四个字。

镇长说："你借灯，必须写一个时辰。到了时辰，不管你回没回来，牌子自动算作'空'，别人可以来取。你要是还没用完，中途回来续一次时辰也行——但你得亲自回来翻牌。"

有人问："要是我忘了回来续，灯还在我手上，别人把牌翻了怎么办？"

镇长说："那灯就不算你的了。你手上的灯，到时辰就灭——灯芯里有根引线，按时辰烧到头，灯自己灭。你拿着一盏黑灯，也做不了什么。新的人取了新灯芯，点的是新灯。"

又有人问："要是两个人同时看到牌子写着'空'，同时写名字呢？"

镇长指着牌子下面的铜锁说："牌子一次只能翻一个人。你要写名字，先扣铜锁。锁扣上了，只有你能写；写完放锁，下一个人才能扣。两个人同时来，一个扣到了，另一个扣不上，就知道晚了一步。"

这套规矩用了三年，没人摔进水渠，也没人等到天亮。

最精妙的是那个"时辰"。有了它，就算借灯的人出了事——比如查水渠时崴了脚回不来——灯也不会被一个人永远占着。时辰一到，锁自动开，灯自动腾出来给下一个需要的人。

镇上的教书先生说："这不是借灯。这是**租**灯。有期限的借，叫租。"

——到这儿你大概已经认出来了：那盏灯是 *shared resource*，木牌是 *lock state*，铜锁是 *compare-and-swap*（CAS），写名字是 *acquire*，还灯时辰是 *TTL*（*time-to-live*），中途续时辰是 *lease renewal*，到期自动作废是 *lease expiration*，灯芯烧尽是 *fencing* 机制。这是 **lease-based distributed locking** 的故事。

### 这是什么

*Lease*（租约）是一种带有效期的锁。持锁方获得对共享资源的独占访问权，但这份权利有一个明确的过期时间（*TTL*）。在 TTL 到期之前，持锁方必须主动续约（*renew*）；如果续约失败——进程崩溃、网络分区、GC 暂停——锁在 TTL 到期后自动释放，其他竞争者可以获取。

核心机制：

1. **获取**（*acquire*）：原子地（CAS）将锁状态从"空"写为"我的 + TTL"。
2. **续约**（*renew*）：在 TTL 到期前，持锁方延长有效期。通常续约间隔设为 TTL 的 1/3。
3. **过期**（*expire*）：TTL 到期后，锁自动释放。持锁方若在过期后仍持有旧锁，凭过期的 *fencing token* 无法写入被保护的资源。
4. **释放**（*release*）：正常完成后主动释放，不必等 TTL 到期。

Kubernetes 中的 *Leader Election* 就是基于 lease 的——`coordination.k8s.io/v1` 的 `Lease` 对象存储在 etcd 中，controller-manager、scheduler、自定义 operator 用它来选主。etcd 本身用 *lease* 管理 TTL key。Redis 的 `SET key value EX seconds NX` 也是 lease 语义。ZooKeeper 的 *ephemeral node* 加 *session timeout* 实现了类似效果。

### 为什么重要

分布式系统中最危险的锁是**永远不释放的锁**。一个进程崩溃了，它持有的锁如果没有过期机制，就成了"死锁"——所有等待者永远等下去，系统停摆。

Lease 用时间换安全：即使持锁方完全失联，系统最多等一个 TTL 就能恢复。代价是 TTL 太短则续约频繁、锁抖动；TTL 太长则故障恢复慢。这是一个经典的 *liveness vs. safety* 权衡。

在 Kubernetes 里，这个权衡是具体的：controller-manager 的 lease 默认 15 秒 TTL、10 秒续约。如果主节点网络中断超过 15 秒，备节点就会接管。太短会导致脑裂风险（旧主还没完全退出，新主已经开始工作）；太长会导致 15+ 秒的控制平面空窗。*Fencing token*（单调递增的 generation 计数器）解决脑裂：旧主的写入因为 token 过期被 etcd 拒绝。

_隐喻对应表_

- 南塘镇唯一的夜灯 —— 共享资源（只允许一个持有者）
- 木牌（空/已借） —— 锁状态（lock state）
- 铜锁（一次只能一人扣） —— compare-and-swap（CAS）原子操作
- 写名字 —— acquire lock（获取锁）
- "还灯时辰" —— TTL（time-to-live，锁的有效期）
- 中途回来续时辰 —— lease renewal（续约）
- 到期不续则作废 —— lease expiration（过期自动释放）
- 灯芯按时辰烧尽 —— fencing mechanism（防止过期持锁方继续操作）
- 借灯人崴脚回不来 —— 进程崩溃 / 网络分区
- 两人同时翻牌，一人扣到锁 —— CAS 竞争，只有一个成功
- "有期限的借叫租" —— lease 的本质：有 TTL 的锁
</section>
<section class="en" markdown="1">
Nantang town had only one night lantern.

The iron lantern hung on a stone pillar at the town entrance, the brightest light in the whole settlement — bright enough to throw shadows the length of the flagstone lane. Anyone who needed to walk a dark alley at night, inspect the irrigation ditch, or patrol the granary needed that lantern. But there was only one. People fought over it. Some walked without it and fell into the ditch. Others borrowed it and never brought it back, leaving everyone else waiting until dawn.

The town elder devised a system: the Lantern Tablet.

The rules worked like this: a wooden tablet hung beside the lantern, its face carved with the character for "free." Whoever wanted the lantern checked the tablet first — if it read "free," they wrote their name on it, noted a "return hour," then took the lantern and left. Anyone arriving later saw a name on the tablet and knew the lantern was taken; they could only wait.

The key was those two words: "return hour."

The elder explained: "When you borrow the lantern, you must write down an hour. When that hour arrives, whether you have returned or not, the tablet automatically counts as 'free' and someone else may take it. If you are not finished, you may come back in person to renew for another hour — but you must come back yourself and flip the tablet."

Someone asked: "What if I forget to come back and renew, but I still have the lantern, and someone flips the tablet?"

The elder said: "Then the lantern is no longer yours. The wick has a timed fuse set to your hour; when time is up, the lantern goes dark on its own. You are holding a dead lantern and can do nothing with it. The next person gets a fresh wick and lights a new flame."

Another asked: "What if two people see 'free' at the same time and both try to write their name?"

The elder pointed to the brass clasp beneath the tablet. "Only one person can flip the tablet at a time. To write your name, you must first latch the clasp. Once latched, only you can write. When you finish, you release the clasp and the next person can try. If two arrive together, one latches it and the other finds it already held — they know they were a step too late."

The system ran for three years. Nobody fell into the ditch. Nobody waited until dawn.

The most elegant part was the "hour." Because of it, even if the borrower had an accident — say, twisted an ankle while inspecting the ditch and could not return — the lantern would never be held by one person forever. When the hour was up, the clasp opened automatically and the lantern became available to the next person who needed it.

The town's schoolteacher observed: "This is not borrowing. This is **leasing**. Borrowing with a time limit is a lease."

— By now you have probably recognized it: the lantern is a *shared resource*, the wooden tablet is the *lock state*, the brass clasp is *compare-and-swap* (CAS), writing your name is *acquire*, the return hour is the *TTL* (time-to-live), coming back to extend the hour is *lease renewal*, automatic expiry is *lease expiration*, and the timed wick is the *fencing* mechanism. This is the story of **lease-based distributed locking**.

### What it is

A *lease* is a lock with an expiration time. The holder gains exclusive access to a shared resource, but that right has an explicit TTL. Before the TTL expires, the holder must actively *renew*; if renewal fails — process crash, network partition, GC pause — the lock is automatically released after TTL, and other contenders can acquire it.

Core mechanism:

1. **Acquire**: atomically (CAS) write the lock state from "free" to "mine + TTL."
2. **Renew**: before TTL expires, the holder extends the deadline. Renewal interval is typically TTL / 3.
3. **Expire**: after TTL, the lock auto-releases. A holder still carrying the old lock cannot write to the protected resource because its *fencing token* is stale.
4. **Release**: on normal completion, the holder releases explicitly without waiting for TTL.

Kubernetes *Leader Election* is lease-based — the `Lease` object in `coordination.k8s.io/v1` is stored in etcd; controller-manager, scheduler, and custom operators use it for leader election. etcd itself uses *leases* to manage TTL keys. Redis's `SET key value EX seconds NX` is lease semantics. ZooKeeper's *ephemeral nodes* plus *session timeouts* achieve a similar effect.

### Why it matters

The most dangerous lock in a distributed system is **one that is never released**. If a process crashes while holding a lock that has no expiry, the result is a deadlock — all waiters block forever and the system freezes.

Leases trade time for safety: even if the holder vanishes completely, the system waits at most one TTL before recovering. The cost is a tuning tradeoff: too-short TTLs cause frequent renewals and lock churn; too-long TTLs mean slow failure recovery. This is a classic *liveness vs. safety* tension.

In Kubernetes this tradeoff is concrete: the controller-manager lease defaults to a 15-second TTL with 10-second renewal. If the leader's network drops for more than 15 seconds, a standby takes over. Too short risks split-brain (the old leader has not fully stepped down when the new one starts); too long leaves a 15+ second control-plane gap. *Fencing tokens* (monotonically increasing generation counters) solve split-brain: the old leader's writes are rejected by etcd because its token is stale.

### Metaphor mapping

- Nantang's single night lantern — shared resource (only one holder allowed)
- Wooden tablet (free / taken) — lock state
- Brass clasp (only one person at a time) — compare-and-swap (CAS) atomic operation
- Writing your name — acquire lock
- "Return hour" — TTL (time-to-live, the lock's expiration)
- Coming back to extend the hour — lease renewal
- Automatic expiry when not renewed — lease expiration (auto-release)
- Timed wick that burns out — fencing mechanism (prevents expired holder from operating)
- Borrower twists ankle and cannot return — process crash / network partition
- Two people see "free," one latches the clasp — CAS contention, only one succeeds
- "Borrowing with a time limit is a lease" — the essence of a lease: a lock with TTL
</section>
