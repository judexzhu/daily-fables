---
layout: fable
title: "渔港的旗语交接 · The Harbor's Flag Relay"
title_zh: "渔港的旗语交接"
title_en: "The Harbor's Flag Relay"
concept: "lease-based leader election and fencing tokens"
tags: [distributed-systems, kubernetes]
illustration: /assets/art/2026-07-04-leader-election-fencing-token.jpg
---
<section class="zh" markdown="1">
海边有一座小渔港，港口窄，进出港的水道只容得下一条船通过。为了不让两条船同时抢道相撞，渔港立下规矩：_任何时候，只能有一位"掌旗人"站在防波堤尽头指挥进港_——他挥旗，船看旗行事，谁也不许自作主张。

掌旗人不是终身的，大家轮流当值。领班在船坞入口挂着一块_木牌_，上面刻着当值掌旗人的名字，还有一个_只会往上跳、从不回退的编号_。每次换人，编号就往上跳一位。木牌旁边挂着一只_沙漏_：掌旗人上任那一刻翻转沙漏，沙漏漏完之前，他必须赶回来，在木牌旁的墙上重重划一道，向领班证明"我还醒着，我还是这一班的掌旗人"。划完这一道，沙漏重新翻转，继续计时。要是沙漏漏尽了，谁也没来划这一道，领班就认定这人已经不能视事——不管是回家了、睡着了、还是出了什么意外——立刻把木牌上的名字换掉，派新的掌旗人顶上，编号再往上跳一位。

有一天当值的老陈，被海风迷了眼，一头栽倒在防波堤的背风角里，昏睡了过去——他的沙漏悄悄漏尽，谁也没通知他。领班照老规矩办事：名字划掉，派了年轻的小林顶班，木牌上的编号从 7 跳到了 8。

没过多久老陈悠悠转醒，浑然不知自己早已被换下，抄起旗子对着一条正要进港的渔船就挥了起来。恰好小林也在同一时刻对同一条船打着旗语——两套旗号几乎同时出现，船上的舵手一时竟分不清该听谁的，险些一头撞上暗礁。

好在渔港还有最后一道保险：_验旗人_不是光看旗语标不标准，而是要求每一次挥旗都必须报出当值编号，他手里死死记着一句话——"眼下只认编号 8"。老陈挥的旗号，不管姿势多么熟练、多么斩钉截铁，报的编号却是那个已经作废的 7——验旗人看也不看，直接当废旗处理，视而不见；唯独编号 8 的小林，才是船只肯听命的那个人。渔船最终有惊无险地靠了岸。

——到这儿你大概已经认出来了：这渔港讲的，其实是分布式系统里的 _leader election via lease_（基于租约的选主），以及配套的救命稻草 _fencing token_（围栏令牌）。

_概念解释_
在 Kubernetes 的 controller-manager、scheduler，或任何需要"同一时刻只能有一个实例真正干活"的系统里，常见做法是靠一个 _Lease_ 对象来选主：当选者把自己的身份写进 `holderIdentity`，并且必须在 `leaseDurationSeconds`（沙漏）到期前不断刷新 `renewTime`（划道）。一旦续约超时，别的候选者就会抢占这把 lease，成为新的 leader，同时——这是关键——把一个只增不减的编号（_term / fencing token_，在 etcd 里体现为 lease 的 revision 或专门维护的计数器）往上跳一位。真正的坑在于：_持有 lease 不等于真的还活着_。GC 长时间停顿、进程被系统挂起、网络短暂失联，都可能让一个"过期"的 leader 在毫不知情的情况下醒来，继续以为自己还是 leader，继续对外发号施令——这就是老陈昏睡又醒来的那一幕，业内常叫它 _zombie leader_ / _stale leader_ 问题。仅凭续约机制本身无法杜绝这个风险，真正兑底的是下游资源（数据库、存储卷、外部 API）在执行任何写入前，都强制核对随请求带来的编号是否等于"当前已知最新的"那个编号——编号旧了，哪怕请求本身完全合法、格式正确，也一律拒绝。这正是 Martin Kleppmann 在批评"仅凭时间续约的分布式锁不安全"时反复强调的 _fencing token_ 方案，也是 Chubby、ZooKeeper、etcd 一类系统在实现分布式锁时的标准补丁。

_隐喻对应表_

- 渔港、狭窄水道 → 需要"同一时刻只能一个实例生效"的资源/临界区
- 掌旗人 → _leader_（当选的那个实例）
- 木牌上刻的编号（只增不减） → _fencing token / term_（每次换届单调递增）
- 沙漏 → `leaseDurationSeconds`（租约有效期 / TTL）
- 定期划道证明"还醒着" → `renewTime` 心跳续约（renew/heartbeat）
- 沙漏漏尽没人划，领班换人 → lease 过期，触发重新选主（re-election）
- 老陈昏睡后悠悠转醒还继续挥旗 → _zombie / stale leader_：GC 停顿、进程假死后，旧 leader 不自知地继续行动
- 验旗人核对编号，只认编号 8 → 下游系统执行前核对 _fencing token_，拒绝携带旧编号的请求
- 编号 7 的旗令被当场作废 → stale writes 被 fencing token 机制挡下，即使旗语本身完全合法
- 险些撞上暗礁 → 若无 fencing token，split-brain 式的双 leader 可能造成的实际数据/操作冲突
</section>
<section class="en" markdown="1">
By the sea sat a small fishing harbor with a channel so narrow that only one boat could pass through at a time. To keep two boats from racing for the channel and colliding, the harbor had one rule: at any moment, only one "flag-bearer" may stand at the end of the breakwater directing traffic — he waves the flag, boats obey the flag, and no one acts on their own judgment.

The flag-bearer's post wasn't held for life; people took turns on duty. The dockmaster hung a wooden plaque at the harbor entrance bearing the name of the current flag-bearer and a number that only ever climbed, never fell. Each time the post changed hands, the number ticked up by one. Beside the plaque hung an hourglass: the moment a flag-bearer took the post, the hourglass was flipped, and before the sand ran out, he had to return and carve a fresh mark on the wall beside the plaque, proving to the dockmaster "I'm still awake, I'm still on duty." Once the mark was carved, the hourglass flipped again and the countdown restarted. If the sand ran out with no mark carved, the dockmaster assumed the man could no longer serve — whether he'd gone home, fallen asleep, or met some accident — and immediately swapped the name on the plaque, put a new flag-bearer on duty, and bumped the number up again.

One day, Old Chen, on duty, got sand in his eyes from a gust off the sea and collapsed into a sheltered corner of the breakwater, falling into a deep sleep — his hourglass quietly ran dry, and no one told him. The dockmaster followed the old rule: struck his name, put young Lin on duty, and the number on the plaque jumped from 7 to 8.

Not long after, Old Chen groggily woke up, with no idea he'd already been replaced, grabbed his flag, and began signaling a boat that was about to enter the harbor. At that very moment, Lin was signaling the same boat too — two sets of flag signals appearing almost simultaneously left the helmsman unable to tell whom to obey, nearly running the boat onto the rocks.

Fortunately the harbor had one last safeguard: the flag-verifier didn't just check whether the signaling looked correct — every wave of the flag had to carry the current duty number, and he held onto one fact firmly: "right now, only number 8 counts." No matter how practiced or decisive Old Chen's signaling was, the number it carried was the already-invalid 7 — the verifier didn't even look twice, treating it as void and ignoring it outright. Only Lin, carrying number 8, was the one the boat would obey. The boat docked safely, if narrowly.

——By now you've probably recognized it: this harbor is really about _leader election via lease_ in distributed systems, along with its lifesaving companion, the _fencing token_.

_The concept, and why it matters_
In Kubernetes' controller-manager, scheduler, or any system where "only one instance may actually be doing the work at any given moment" matters, the common approach is to elect a leader through a _Lease_ object: the winner writes its identity into `holderIdentity` and must keep refreshing `renewTime` before `leaseDurationSeconds` (the hourglass) expires. Once a renewal is missed, another candidate seizes the lease and becomes the new leader — and, critically, bumps up a monotonically increasing number (the _term / fencing token_, realized in etcd as a lease revision or a dedicated counter). The real trap is that _holding the lease is not the same as actually still being alive_. A long GC pause, a process suspended by the OS, or a brief network blip can let an "expired" leader wake up with no idea anything happened, still believing it's the leader, still issuing commands — that's Old Chen dozing off and waking up again, what the industry calls the _zombie leader_ / _stale leader_ problem. The renewal mechanism alone can't rule this out; the real backstop is that downstream resources (databases, storage volumes, external APIs) must, before executing any write, verify that the number attached to the request matches the latest known number — if the number is stale, the request is rejected outright, no matter how well-formed or legitimate it otherwise looks. This is exactly the _fencing token_ approach Martin Kleppmann champions when arguing that time-based distributed locks alone are unsafe, and it's the standard patch used by Chubby, ZooKeeper, and etcd-based distributed locks.

_Metaphor mapping_

- The harbor, the narrow channel → the resource / critical section where only one instance may act at a time
- The flag-bearer → the _leader_ (the currently elected instance)
- The ever-climbing number on the plaque → the _fencing token / term_ (monotonically increasing per election)
- The hourglass → `leaseDurationSeconds` (the lease TTL)
- Carving a fresh mark to prove "still awake" → `renewTime` heartbeat/renewal
- Sand runs out, no mark carved, dockmaster replaces the man → lease expiry triggering re-election
- Old Chen waking up and signaling again, unaware → the _zombie / stale leader_: a GC pause or process freeze leaving an old leader unknowingly still acting
- The flag-verifier checking the number, only trusting 8 → downstream systems checking the _fencing token_ before acting
- Old Chen's number-7 signal voided on the spot → stale writes blocked by the fencing token, even when the signal itself is perfectly valid
- The boat nearly hitting the rocks → the real conflict/damage a split-brain-style dual leader could cause without a fencing token
</section>
