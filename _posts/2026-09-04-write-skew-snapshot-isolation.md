---
layout: fable
title: "云岭的更鼓 · The Two Watchmen of Cloud Ridge"
title_zh: "云岭的更鼓"
title_en: "The Two Watchmen of Cloud Ridge"
concept: "Write skew anomaly under Snapshot Isolation"
tags: [databases, distributed-systems]
illustration: /assets/art/2026-09-04-write-skew-snapshot-isolation.jpg
youtube_id: "az2jZZEhRM8"
---
<section class="zh" markdown="1">
云岭孤峰之上有一座烽燧望楼，终年立在风雨和乱云里。岭下是一道险峻的关口，常有游骑和匪寇借着夜雾摸黑过山。

望楼里只有两名更夫：老赵和小鲁。两人一老一少，守着城守府立下的铁律：“**岭上风急，烽燧不可空悬；二人之中，无论何时必须至少留一人清醒当值。**”

值更屋的石壁上钉着两枚木牌，一枚刻着“赵”，一枚刻着“鲁”。牌子下各有两格凹槽，插着刻有“醒”和“眠”的青铜签子。为了怕两人同时伸手挪牌子起争执，营里早年定过规矩：每逢正更的钟声一响，杂役便会用炭墨把当时墙上的名牌拓下一份纸样，分别塞进两人的怀里。

谁想下值去歇息，必须先看怀里的拓纸：若拓纸上另一人写着“醒”，便说明守更的铁律没有破，他才准把自己的签子换成“眠”，投进确认槽里。只要槽里没有别人同时争抢同一根属于自己的签子，更鼓就算交接妥当。多年来，这道法子管得极好，从没出过差错。

八月尽头的一个雨夜，冷风刮得像刀子。两人都在暴雨里顶了整整十个时辰，眼皮沉得像灌了铅。

子时三刻，交更的铜钟“当”地敲响。杂役像往常一样，将墨迹未干的炭拓纸递给老赵和小鲁。

老赵在望楼顶层的石栏边冻得瑟瑟发抖。他掏出怀里的炭拓纸就着风灯看了一眼：老赵为“醒”，小鲁为“醒”。

老赵心里盘算：规矩要的是“至少一人清醒”。小鲁既然醒着在守夜，那么即便自己下去睡上两个时辰，岭上也依然有人看着。“规矩没破，合情合理。”老赵欣慰地在自己的签子上换成了“眠”，顺着石缝滑进确认槽，转身钻进热乎的炭盆旁，倒头沉沉睡去。

几乎就在同一瞬间，在望楼下层避雨的小鲁也摸出了自己的炭拓纸。纸是同一刻印下的，上面的字迹一模一样：老赵为“醒”，小鲁为“醒”。

年轻的小鲁鞋袜早已湿透，寒气直往骨头里钻。他看着纸面，也暗自松了一口气：“老赵经验老到，既然他在顶楼守着，我便回耳房烤干衣服、歇一歇脚。只要老赵在，规矩便没有破。”小鲁同样把自己的签子换成“眠”，推进了确认槽。

铁铸的确认槽发出清脆的机关合拢声。老赵改的是“赵”字签，小鲁动的是“鲁”字签。两人没有碰同一根木牌，没有任何人在争抢同一道槽子，两道签子全都顺顺当当地落了锁。

可烽燧顶上，风把最后一盏残灯吹灭了。

大雨如注，黑魆魆的望楼上空无一人。四更天，一队黑巾蒙面的骑卒悄无声息地穿过了云岭隘口，直奔后方的县城而去。

天亮时，巡防按察使带着兵马赶到，只见更鼓冰凉，两人都还裹在被子里鼾声正酣。老赵和小鲁被揪出被窝时又惊又屈，双双掏出怀里的炭拓纸和落锁回执大喊：“大人明查！小的交签之前看得清清楚楚，对方明明醒着！每一道规矩都是按律办的，怎么会无人当值？！”

按察使把两张拓纸摔在地上：“你们每一个人看的世界都是真的，你们每一个人改的签子都没犯错。可正是因为你们各看各的真、各改各的签，云岭才成了空城。”

到这里，你大概已经认出来了：这就是数据库并发控制中的 **write skew anomaly under Snapshot Isolation**（快照隔离下的写倾斜异常）。

### 这是什么

在数据库的世界里，*Snapshot Isolation*（快照隔离）是一种被广泛采用的高性能并发控制级别。在快照隔离下，每个事务启动时，看到的都是数据库的一个一致性快照（如同那张“炭拓纸”）。只要两个并发事务没有尝试修改**同一行数据**，它们就不会触发传统的写写冲突（*first-committer-wins*），两个事务都能顺利提交。

然而，当系统的完整性依赖于**跨越多个不同记录的全局约束**时，快照隔离就会失效。

典型的写倾斜场景就像云岭望楼：
1. 约束条件是：`count(awake_guards) >= 1`。
2. 事务 A（老赵）读取快照，发现有两个守更人醒着，判断可以将自己更新为休眠（更新赵的记录），约束仍然满足。
3. 事务 B（小鲁）并发读取同一个快照，也发现有两个守更人醒着，判断可以将自己更新为休眠（更新鲁的记录），约束同样满足。
4. 两个事务各自提交。因为事务 A 修改的是记录 A，事务 B 修改的是记录 B，两者在行级别**没有交集**，快照隔离判定没有任何写冲突，双方皆提交成功。
5. 最终结果：两张记录全变成了休眠，`count(awake_guards) = 0`，全局约束被彻底打破。

因为两个并发写入互相“倾斜”地错开了各自修改的目标，避开了行锁检测，所以被称为 **写倾斜（Write Skew）**。

### 为什么重要

许多工程师误以为只要开启了“快照隔离”或关系型数据库中的“可重复读（*Repeatable Read*）”，就能免除所有并发竞争问题。但写倾斜恰恰是快照隔离无法防御的盲区：

- **医疗排班系统**：医院要求“同一科室至少有一位值班医生在岗”。两位医生同时在手机上申请请假，各自看到对方在岗，系统同时批准两份申请，导致整个科室瞬间空岗。
- **账户透支风控**：用户有两个关联账户（主账户与副账户），规则允许合并余额支付，但“主账户余额 + 副账户余额 >= 0”。若两笔并发扣费分别针对主账户和副账户，各自读取旧的合并总额，最终导致总透支。
- **会议室预约与名单去重**：读取当前时间段确认无冲突后插入记录，两个并发事务各自向不同行写入预约，导致同一个物理资源被重叠预订。

防范写倾斜通常有三种经典手段：
1. **可串行化隔离（Serializable Snapshot Isolation / SSI）**：如 PostgreSQL 的 `SERIALIZABLE` 级别，通过追踪读写依赖图（*rw-antidependencies*），在检测到潜在写倾斜环路时主动中止其中一个事务。
2. **显式悲观锁（Pessimistic Locking）**：在读取时使用 `SELECT ... FOR UPDATE`，强制锁住所有涉及该约束的相关行，让并发事务排队执行。
3. **物化冲突（Materializing Conflicts）**：人为引入一行代表全局约束的“锁记录”（例如一面专门的更鼓槽位），迫使所有涉及该约束的事务都去修改这同一行，从而把跨行逻辑冲突转化为明确的行级写冲突。

_隐喻对应表_

- 望楼守更铁律（至少一人清醒） → 跨多行的全局一致性约束（Invariant）
- 子时三刻拓下的炭拓纸 → 事务开启时获取的一致性快照视图（Snapshot View）
- 老赵与小鲁各自查看拓纸 → 两个事务并发读取重叠数据范围
- 老赵改赵签、小鲁改鲁签 → 并发事务写入互不重叠的不同行（Disjoint Writes）
- 确认槽顺利落锁无争抢 → 行级写冲突检测通过（No Write-Write Conflict）
- 望楼无人守更、隘口失守 → 约束被打破的写倾斜异常（Write Skew Anomaly）
- 必须争抢唯一的更鼓机关槽 → 显式锁（`SELECT FOR UPDATE`）或物化冲突方案
</section>
<section class="en" markdown="1">
High upon the isolated crag of Cloud Ridge stood a lone beacon tower, weathered by endless gale and cloaked in drifting mountain mist. Below the ridge lay a treacherous pass where bandits and enemy scouts routinely crept through the foggy defiles under cover of night.

The outpost was manned by only two watchmen: seasoned Old Zhao and young Lu. Between them hung the ironclad edict of the garrison commander: "**The winds over the pass are treacherous and the beacon must never stand unguarded. Between the two of you, at least one must remain awake on the parapet at all times.**"

On the stone wall of the watchroom hung two carved wooden tablets: one inscribed with "Zhao" and the other with "Lu". Beneath each tablet sat slots fitted with bronze markers engraved with "Awake" or "Resting". To keep the men from quarreling or snatching markers at the same instant, the garrison had long ago established an orderly ritual: at each hourly chime of the bronze bell, a runner would take a fresh charcoal rubbing of the wall board and hand one copy to each watchman.

Whoever wished to step down from the windy platform to rest had to consult the rubbing in his palm first: if the rubbing showed the other watchman marked as "Awake", the sacred invariant held true, and he was permitted to swap his bronze marker to "Resting" and slide it into the iron confirmation chute. So long as no two men wrestled for the very same marker slot, the watch transitioned smoothly. For years, this practice had kept the frontier secure without a single hitch.

On a bitter night at the close of autumn, rain lashed the mountain peak like knives. Both men had endured ten grueling hours in the biting downpour, their eyelids heavy as wet stones.

At midnight, the bronze bell sounded its deep chime. The runner delivered the freshly inked charcoal rubbings into the trembling hands of Old Zhao and young Lu.

Huddled against the stone parapet at the top of the tower, Old Zhao pulled out the rubbing and inspected it by the dim light of his oil lantern: Zhao was "Awake"; Lu was "Awake".

Old Zhao reasoned carefully: the law demanded that *at least one guard remain awake*. Lu was awake on duty. If Zhao stepped down to sleep beside the brazier for two hours, the rule would still hold: Lu was watching the gorge. "The rule is satisfied. It is entirely safe." With a sigh of relief, Old Zhao swapped his marker to "Resting", slid it into his designated slot in the chute, and retreated to the warm quarters to collapse into deep slumber.

At that very same moment, on the sheltered lower walkway, young Lu examined his own rubbing from the exact same midnight bell. The black ink was identical: Zhao was "Awake"; Lu was "Awake".

Young Lu's boots were soaked through, and the chill bit deep into his marrow. Gazing at the slip, he too breathed a sigh of relief: "Old Zhao is veteran and wise. Since he is awake and watching the beacon on the top platform, I can return to the hearth room, dry my clothes, and rest my limbs. As long as Zhao is up there, the law is unbroken." Lu dutifully changed his own marker to "Resting" and dropped it into his slot.

The iron mechanisms inside the chute clicked smoothly into place. Zhao had modified only Zhao's marker; Lu had modified only Lu's marker. Neither man touched the other's slot; neither attempted to overwrite the same bronze piece. Both slips locked home without collision.

Yet on the parapet above, the lashing rain snuffed out the last dying embers of the watch fires.

In pitch blackness and deafening rain, the tower stood entirely abandoned. In the fourth watch, a troop of masked horsemen slipped through the unguarded pass without raising a whisper of alarm.

At dawn, the inspecting magistrate arrived with cavalry only to find the beacon drum stone-cold and both watchmen wrapped in heavy quilts, snoring peacefully. Dragged out into the morning fog, the two men cried out in disbelief, thrusting forward their charcoal rubbings and stamped receipts: "Excellency, look! When I checked the record before submitting my marker, the other man was wide awake! Every rule was observed to the letter! How could the tower be empty?!"

The magistrate cast the rubbings onto the stones: "Each of you saw a true picture of the world, and neither of you broke a rule on your own marker. Yet because each of you made decisions on your own snapshot and altered different markers, Cloud Ridge became an empty gate."

By now, you have probably recognized it: this is the **write skew anomaly under Snapshot Isolation** in database concurrency control.

### What it is

In database systems, *Snapshot Isolation* (SI) is a widely used, high-performance concurrency control model. Under Snapshot Isolation, every transaction reads data from a consistent snapshot taken when the transaction begins—just like the freshly inked charcoal rubbing. As long as concurrent transactions do not attempt to write to the **exact same row or data item**, no write-write conflict is triggered (*first-committer-wins*), and both transactions are permitted to commit.

However, whenever an application's integrity relies on a **global invariant spanning multiple distinct rows**, Snapshot Isolation breaks down.

The classic write skew pattern unfolds precisely like Cloud Ridge:
1. The invariant is: `count(awake_guards) >= 1`.
2. Transaction A (Old Zhao) reads the snapshot, observes that two guards are awake, and concludes it is valid to update his own status to resting (modifying row A), as the invariant will remain satisfied.
3. Transaction B (young Lu) concurrently reads the same snapshot, also sees that two guards are awake, and concludes it is valid to update his own status to resting (modifying row B).
4. Both transactions commit. Because transaction A touched only row A and transaction B touched only row B, their write sets are **disjoint**. Under standard Snapshot Isolation, no row-level write conflict exists, and both transactions commit cleanly.
5. Final state: both guards are resting, `count(awake_guards) = 0`, and the multi-row invariant is completely shattered.

Because the two concurrent writes "skew" away from each other onto different records, they slip right past row-level conflict detection—hence the name **Write Skew**.

### Why it matters

Many developers assume that "Snapshot Isolation" or SQL's "Repeatable Read" (as implemented in PostgreSQL, MySQL InnoDB, CockroachDB, or Spanner) protects against all race conditions. Write skew is the primary blind spot where that assumption fails:

- **On-call rotations**: A hospital schedule requires at least one physician on call per department. Two doctors concurrently request leave on their phones; both see the other active, both requests are approved automatically, leaving the department unstaffed.
- **Overdraft protection across accounts**: A user has linked checking and savings accounts with the rule that `checking_balance + savings_balance >= 0`. Concurrent debits against checking and savings, each checking the aggregate balance from an initial snapshot, will overdraw the total balance.
- **Meeting room & resource reservations**: Checking availability in a timeslot and then inserting a new booking row without a lock allows two concurrent transactions to book overlapping reservations for the same physical room.

There are three primary remedies for write skew:
1. **Serializable Snapshot Isolation (SSI)**: Implemented in modern engines such as PostgreSQL's `SERIALIZABLE` level. SSI tracks read-write dependency cycles (*rw-antidependencies*) during transaction execution and automatically aborts one of the conflicting transactions before a violation can occur.
2. **Explicit pessimistic locks (`SELECT ... FOR UPDATE`)**: Forcing a lock on all rows contributing to the invariant forces concurrent transactions to serialize, ensuring the second transaction sees the up-to-date state.
3. **Materialized conflicts**: Artificially creating a shared "guard lock" record that any transaction modifying the schedule must touch and update, converting a multi-row semantic conflict into a standard, detectable row-level write-write conflict.

_Metaphor mapping_

- Watchtower invariant (at least one guard awake) → Global integrity invariant spanning multiple rows
- Charcoal rubbings taken at the hourly bell → Transaction snapshot view taken at start time
- Zhao and Lu inspecting their rubbings → Concurrent transactions reading overlapping data
- Zhao modifying Zhao's marker, Lu modifying Lu's marker → Concurrent transactions writing to disjoint rows
- Bronze markers sliding into the chute without collision → Row-level conflict check passing (no write-write conflict)
- Unguarded parapet and captured pass → The invariant-violating Write Skew Anomaly
- Contending for a single shared bell mechanism → Explicit locks (`SELECT FOR UPDATE`) or materialized conflict records
</section>
