---
layout: fable
title: "码头仓库的储物柜 · The Lockers at the Cargo Dock"
title_zh: "码头仓库的储物柜"
title_en: "The Lockers at the Cargo Dock"
concept: "CSI VolumeAttachment and attach/detach protection"
tags: [kubernetes, storage]
---
<section class="zh" markdown="1">
云港码头有一排贴着编号的铁皮储物柜，专门存放临时要用的货物工具——扳手、绳索、防水布。每个搬运班组要用哪个柜子，得先去总务处登记。

总务处有一条铁律：_每个柜子同一时刻只能发出一张钥匙牌_，除非柜子门上贴着"共享"的绿标——那种柜子可以同时发好几张钥匙牌，谁都能开。大多数柜子没有绿标，只能一张牌走天下：你拿着牌，就等于宣告"眼下只有我能碰这柜子里的东西"。

某天，三号仓库的搬运工老赵领了 8 号柜的钥匙牌，正忙着往外搬防水布。忽然调度室一声令下：8 号柜的活儿要挪去五号仓库，换成搬运工小林来接手。小林兴冲冲跑去总务处要钥匙牌，却被拦下——总务处的规矩写得明明白白：_新牌子发出之前，必须先收回旧牌子，并确认旧柜门已经真正锁好_。

麻烦来了：老赵所在的三号仓库那天风浪大，通讯的信号旗全打不出去，总务处联系不上老赵，不知道他到底有没有把柜门锁好、钥匙牌到底还在不在他手里。总务处的老规矩这时候显得格外死板——它宁可让 8 号柜的活儿在那儿_空转等着_，也不肯冒险给小林发一张新牌子。要是老赵那头其实还没锁好、还在里面翻东西，这时候小林在五号仓库那头也拿到牌子同时动手，两个人同一时刻伸手进同一个柜子，轻则货物混在一起分不清，重则两人手都夹在铰链里。

等了大半天，三号仓库的信号旗终于重新打出来——是空的，老赵确实早就锁好了柜子，只是信号断了没能通知。总务处这才把旧牌一笔勾销，新牌子交到小林手上。

也有过更凶险的一次：某回三号仓库的信号旗彻底烧毁，怎么等都等不来确认。调度室最后拍板：_不等了，强行作废老赵那张牌子_，就当他已经松手，重新发牌给小林。这么做等于把宝压在"老赵大概率已经放手"这个猜测上——万一猜错，两人真的同时动了手，那就是总务处心知肚明、却也没有更好办法时才肯认下的风险。

——到这儿你大概已经认出来了：这些编号柜子、钥匙牌和总务处的登记规矩，讲的其实是 Kubernetes 里的 _CSI（Container Storage Interface）卷生命周期_ 管理——_VolumeAttachment_ 与它的 attach/detach 保护机制。

_概念解释_
Kubernetes 里一块持久化存储（PersistentVolume）想要被某个节点上的 Pod 使用，中间要经过 attach-detach controller 的严格把关。它会为每次"某节点要用某卷"的请求创建一个 _VolumeAttachment_ 对象——相当于那张钥匙牌。对于访问模式是 _ReadWriteOnce（RWO）_ 的卷（没贴绿标的柜子），同一时刻只能有一个节点持有对它的 attachment；_ReadWriteMany（RWX）_（贴绿标的柜子，通常是 NFS、CephFS 这类支持并发挂载的存储）才允许多节点同时挂载。

当 Pod 因为节点故障、驱逐或调度被挪到新节点时（8 号柜的活儿从三号仓移到五号仓），controller 必须先确认旧节点上的卷已经 _detach_ 完成，才会给新节点创建新的 VolumeAttachment——这是为了避免同一块块存储被两个节点同时挂载写入，造成文件系统损坏（这也是为什么很多 CSI driver 干脆不支持给 RWO 卷做多点挂载）。

如果旧节点失联（_NotReady_）、迟迟无法确认 detach 是否完成，Pod 就会一直卡在 _Pending_，新副本起不来——这是 Kubernetes 有意为之的保守设计，宁可服务暂时不可用，也不冒数据损坏的风险。运维如果等不及、确认旧节点确实已经彻底下线（比如硬件已经报废、绝无可能再复活写入），可以手动执行 _force detach_，强制解除旧的 VolumeAttachment——但这本质上是在没有确认的情况下赌一把，官方文档也明确警告这可能导致数据损坏，只应在万不得已、且已确认旧节点不会再访问该卷时使用。

_隐喻对应表_

- 编号储物柜 → PersistentVolume（持久化存储卷）
- 贴绿标的共享柜 vs 只能一张牌的柜子 → ReadWriteMany（RWX）vs ReadWriteOnce（RWO）访问模式
- 钥匙牌 → VolumeAttachment 对象
- 总务处 → attach-detach controller
- "发新牌前必须先收回旧牌、确认柜门锁好" → 新节点 attach 前必须确认旧节点已完成 detach
- 8 号柜的活从三号仓挪到五号仓 → Pod 因故障/驱逐/重调度从旧节点迁移到新节点
- 三号仓信号旗打不出去 → 旧节点 NotReady / 与控制面失联
- 活儿空转等着，新牌迟迟发不出 → Pod 卡在 Pending，等待 detach 确认
- "两人同时伸手进同一柜子，货物混乱" → 同一块存储被两节点同时挂载写入，文件系统损坏（split-brain 式的数据损坏）
- 调度室拍板强行作废老牌子 → 手动 force detach VolumeAttachment（有风险，需谨慎）
</section>
<section class="en" markdown="1">
Port Yunhai had a row of numbered steel lockers along the dock, used to stash tools for cargo handling — wrenches, rope, tarps. Whenever a work crew needed a locker, they had to check in with the dispatch office first.

Dispatch had one unbending rule: _any given locker could have at most one key tag issued at a time_, unless its door carried a green "shared" sticker — those lockers could hand out several key tags at once, and anyone holding one could open it. Most lockers had no sticker. One tag ruled them all: holding it meant declaring "right now, nobody but me touches what's inside."

One day, dockhand Lao Zhao from Warehouse Three checked out the key tag for Locker 8 and was busy hauling tarps out of it. Then dispatch issued a new order: Locker 8's job was moving to Warehouse Five, to be picked up by dockhand Xiao Lin. Xiao Lin hurried over to get the key tag — and was turned away. Dispatch's rule was explicit: _before a new tag can be issued, the old tag must be returned, and the locker confirmed properly locked._

Trouble was, Warehouse Three was getting hammered by rough seas that day, and every signal flag they raised went unseen. Dispatch couldn't reach Lao Zhao — no way to know whether he'd actually locked the door, or whether the old tag was still in his hand. Dispatch's rule turned unforgivingly rigid right when it mattered most: it would rather let Locker 8's job _sit idle and wait_ than risk handing Xiao Lin a new tag. If Lao Zhao hadn't actually locked up yet — still rummaging inside — and Xiao Lin got a tag and reached in at the same moment over in Warehouse Five, at best the cargo would get jumbled beyond sorting; at worst, two hands would end up jammed in the same hinge.

After a long wait, Warehouse Three's signal flag finally went up again — empty, meaning all-clear. Lao Zhao had locked up ages ago; it was just the signal that had failed. Only then did dispatch cancel the old tag and hand the new one to Xiao Lin.

There was a worse case once, too. Warehouse Three's signal flag burned up entirely — no confirmation was ever coming, no matter how long they waited. Dispatch finally made the call: _stop waiting, forcibly void Lao Zhao's tag_, treat him as having let go, and issue a fresh one to Xiao Lin. That decision was a bet on the assumption that Lao Zhao had probably already released his grip — and if the guess was wrong and both hands really did end up in the locker at once, that was a risk dispatch knowingly accepted because there was no better option left.

——By now you've probably recognized it: those numbered lockers, key tags, and dispatch's check-in rules are describing Kubernetes' _CSI (Container Storage Interface) volume lifecycle_ management — the _VolumeAttachment_ object and its attach/detach protection.

_What it is_
In Kubernetes, before a PersistentVolume can be used by a Pod on some node, it has to clear the attach-detach controller's strict gatekeeping. For every "this node wants this volume" request, the controller creates a _VolumeAttachment_ object — the equivalent of that key tag. For volumes with access mode _ReadWriteOnce (RWO)_ (lockers without the green sticker), only one node can hold an attachment to it at a time; _ReadWriteMany (RWX)_ (the stickered, shared lockers — typically backed by storage like NFS or CephFS that supports concurrent mounts) allows multiple nodes to mount it simultaneously.

When a Pod moves to a new node — due to node failure, eviction, or rescheduling (Locker 8's job moving from Warehouse Three to Warehouse Five) — the controller must first confirm the volume has finished _detaching_ from the old node before it will create a new VolumeAttachment on the new one. This prevents the same block storage from being mounted for writes by two nodes at once, which would corrupt the filesystem — which is also why many CSI drivers simply refuse to allow multi-attach on RWO volumes at all.

If the old node goes unreachable (_NotReady_) and detach can't be confirmed for a long time, the Pod just sits stuck in _Pending_ and the new replica never comes up. This is a deliberately conservative design choice in Kubernetes: it would rather leave the service temporarily unavailable than risk data corruption. If an operator can't wait any longer and has confirmed the old node is genuinely gone for good (hardware decommissioned, no possibility it comes back and writes again), they can manually run a _force detach_ to forcibly remove the old VolumeAttachment — but this is fundamentally a bet made without confirmation. The official docs explicitly warn it can cause data corruption, and it should only be used as a last resort, once you're certain the old node will never touch that volume again.

_Metaphor mapping_

- Numbered storage lockers → PersistentVolume
- Green-stickered shared locker vs. single-tag locker → ReadWriteMany (RWX) vs. ReadWriteOnce (RWO) access mode
- Key tag → VolumeAttachment object
- Dispatch office → attach-detach controller
- "New tag only after the old tag is returned and the locker confirmed locked" → new-node attach only after old-node detach is confirmed
- Locker 8's job moving from Warehouse Three to Warehouse Five → Pod migrating to a new node after failure/eviction/rescheduling
- Warehouse Three's signal flag going unseen → old node NotReady / unreachable from the control plane
- The job sitting idle, no new tag issued → Pod stuck in Pending, waiting on detach confirmation
- "Two hands reaching into the same locker at once, cargo jumbled" → the same volume mounted for writes by two nodes simultaneously, filesystem corruption (split-brain–style data corruption)
- Dispatch forcibly voiding the old tag → manual force detach of a VolumeAttachment (risky, use with caution)
</section>
