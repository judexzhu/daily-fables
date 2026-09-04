---
layout: fable
title: "系丝带的管家 · The Steward Who Tied Ribbons"
title_zh: "系丝带的管家"
title_en: "The Steward Who Tied Ribbons"
concept: "Tri-color mark-and-sweep garbage collection"
tags: [runtime, memory]
illustration: /assets/art/2026-08-14-tri-color-mark-and-sweep.jpg
youtube_id: "17fSvopmqE4"
---
<section class="zh" markdown="1">
老宅有四十七个房间，住了三代人。谁也说不清楼上东厢那只樟木箱是谁的、还有没有人要。管家默里先生每年秋天清一次屋子。

他不问"这东西还有人要吗"——问了没人答得上来。他只做一件事：从前厅出发。

前厅的墙上挂着一排钥匙，那是老爷、夫人、厨房、账房每天真正在用的几把。默里先生的规矩是：**凡是从这排钥匙出发、一道门一道门走得到的东西，就是这家还要的**。走不到的，天亮就装车拉走。

清屋那夜他提着一盏油灯，口袋里装满红丝带。

他从前厅的第一把钥匙开始。开门，进屋，给屋里每一样东西系上丝带——但只是系上，先不细看。系过丝带、还没被翻查过的房间，他在门框上画一道浅浅的粉笔印，意思是"记着，回头还得进来一趟"。等他真的把这屋里所有抽屉都拉开、所有钥匙都跟着走了一遍，他就把粉笔印擦掉：这间屋子彻底办完了，再不必回来。

于是整夜里，宅子被分成三种东西：**擦掉粉笔印的**（办完了），**留着粉笔印的**（系了带子但还没查完），和**光着的**（还没人碰过）。默里先生只盯住中间那一堆——有粉笔印的房间还剩一间，他的活儿就没干完；粉笔印全没了，剩下所有光着的东西，就是没人要的。

麻烦在于，这家人晚上并不睡。

厨娘会在半夜把一只银盘从餐室端进办完了的储藏室；小少爷会把书房的一把钥匙摘下来揣进兜里。默里先生已经擦掉粉笔印的房间，本来说好不再回去了——要是有人往里塞进一件光着的东西，那件东西天亮就要被当作废物拉走，可它明明还有人在用。

所以每一道门口都站着一个听差。听差整夜什么也不干，只做一件事：**任何东西被搬进已经办完的房间，或者任何钥匙被挪了地方，听差先给那件东西系上一根丝带，再放行**。宁可多系，绝不漏系。这就是全部的秘密——办完的房间里，永远不许出现光着的东西。

天快亮时默里先生站在前厅数了数：没有粉笔印了。他推开院门，车夫把所有没系丝带的东西装上车拉走——包括阁楼上那两只互相锁着对方钥匙的旧柜子。两只柜子多年来一直互相"证明"着对方有用，可谁也不是从前厅走得到的，一起走。

也有系了丝带却其实早没人要的：厨娘半夜端进储藏室、天亮前又不想要的那只银盘，安安稳稳留到了明年秋天。默里先生不介意。**多留一年不要紧，错拉走一件正在用的东西才要命。**

——到这儿你大概已经认出来了：这位管家做的，是 tri-color mark-and-sweep 垃圾回收。

_概念_

追踪式垃圾回收（tracing GC）不去问"这块内存还有人引用吗"，而是从 _GC roots_（栈、全局变量、寄存器）出发遍历对象图：可达的活，不可达的回收。三色抽象把对象分成 white（未访问）、gray（已标记但引用未扫完）、black（已标记且引用全扫完）三类，gray 集合为空即标记完成，剩下的 white 全部清扫。

关键难点是**并发**：Go、Java ZGC 这类回收器要在应用线程还在跑的时候标记，否则 stop-the-world 停顿会长到没法接受。可程序一边跑一边改指针，就可能把一个 white 对象藏进一个 black 对象里——black 已经扫完不会再看它，这个还活着的对象就会被误回收。解决办法是 _write barrier_：赋值指针时顺手把对象染灰（Dijkstra 式）或把被覆盖的旧值染灰（Yuasa 式），维持"black 绝不指向 white"这条不变式。代价是保守：标记完成后才死掉的对象这一轮不会被回收（_floating garbage_），下一轮再说。

这也是它比引用计数强的地方：互相引用成环的垃圾，refcount 永远降不到零，可达性遍历一趟就识破了。工程上你会在 Go 服务的 `GOGC`、GC 停顿曲线、以及"为什么内存降得比预期慢"这些地方反复遇见它。

_隐喻对应表_

- 前厅墙上那排每天在用的钥匙 → GC roots（栈、全局变量、寄存器）
- 走得到 / 走不到 → 可达性（reachability），而非"有没有人引用"
- 红丝带 → mark bit，已标记为存活
- 门框上的粉笔印 → gray 集合：已标记、引用尚未扫完
- 擦掉粉笔印 → 变为 black：该对象的所有引用都已跟踪
- 光着的东西 → white：本轮未被标记
- 粉笔印全部擦光 → gray 集合为空，标记阶段结束
- 天亮装车拉走 → sweep 阶段回收 white 对象
- 全家人整夜照常活动 → 并发标记，应用线程不停
- 门口的听差 → write barrier
- "办完的房间里不许有光着的东西" → 三色不变式：black 不指向 white
- 宁可多系一根丝带 → 屏障是保守的，只会误判为存活
- 阁楼上互锁钥匙的两只旧柜 → 引用环，reference counting 回收不掉
- 留到明年秋天的那只银盘 → floating garbage，下一轮才清
</section>
<section class="en" markdown="1">
The old house had forty-seven rooms and three generations living in it. Nobody could say who owned the camphor chest in the upstairs east wing, or whether anyone still wanted it. Every autumn, Mr. Murray the steward cleared the house out.

He never asked "does anyone still want this?" — ask that and no one can answer. He did one thing only: he started from the front hall.

On the front hall wall hung a short row of keys: the ones the master, the mistress, the kitchen and the counting-room actually used each day. Murray's rule was simple. **Whatever you can reach from that row of keys, door after door, the household still wants. Whatever you cannot reach goes out on the cart at dawn.**

That night he walked with an oil lamp and pockets full of red ribbon.

He began with the first key in the front hall. Open the door, step in, tie a ribbon on every object inside — just tie it, don't study it yet. On the doorframe of a room he had ribboned but not yet searched, he drew a faint chalk mark: _remember, you owe this room another visit_. Only when he had pulled open every drawer and followed every key inside did he rub the chalk mark off. That room was finished. He need never come back.

So all night the house sorted itself into three kinds of thing: the **chalk rubbed off** (finished), the **chalk still there** (ribboned, not yet searched), and the **bare** (untouched). Murray watched only the middle pile. One chalk mark left anywhere and his night's work wasn't done; no chalk marks left, and everything still bare was, by definition, wanted by no one.

The trouble was that this family did not sleep.

The cook would carry a silver dish from the dining room into a pantry Murray had already finished. The young master would lift a key off the study wall and pocket it. Murray had promised himself never to re-enter a finished room — so if someone slipped a bare object into one, that object would ride out on the dawn cart, even though somebody was plainly still using it.

Which is why a footman stood in every doorway. All night the footmen did nothing at all except this: **when anything was carried into a finished room, or any key was moved from one place to another, the footman tied a ribbon on it first and only then let it pass.** Tie too many, never miss one. That was the whole secret — a finished room must never contain a bare thing.

Near dawn Murray stood in the front hall and counted. No chalk marks. He opened the courtyard gate and the carter loaded everything without a ribbon — including the two old cabinets in the attic, each holding the other's key. For years those two had vouched for one another's usefulness, but neither could be reached from the front hall, so out they went together.

Some ribboned things were dead too. The silver dish the cook carried into the pantry at midnight and lost interest in by morning sat safe until next autumn. Murray didn't mind. **A year's delay costs nothing; hauling away something still in use costs everything.**

— by now you've probably recognized him: the steward is doing tri-color mark-and-sweep garbage collection.

_The concept_

A tracing garbage collector never asks "is this memory still referenced?" It walks the object graph outward from the _GC roots_ — stack, globals, registers — and keeps whatever it can reach. The tri-color abstraction partitions objects into white (unvisited), gray (marked, references not yet scanned) and black (marked, all references scanned). When the gray set empties, marking is done and every remaining white object is swept.

The hard part is doing this **concurrently**. Collectors like Go's and Java's ZGC must mark while application threads keep running, because a full stop-the-world pause would be unacceptably long. But a running program mutates pointers, and it can hide a white object inside a black one — black is finished and will never be rescanned, so a live object gets collected. The fix is a _write barrier_: on every pointer write, shade the new referent gray (Dijkstra style) or the overwritten value gray (Yuasa style), preserving the invariant that no black object points to a white one. The barrier is deliberately conservative: objects that die after being marked survive the cycle as _floating garbage_ and are collected next time.

This is also why tracing beats reference counting — a cycle of mutually referencing garbage never drops its refcount to zero, but a reachability walk sees straight through it. In practice you meet this concept in `GOGC` tuning, GC pause histograms, and every "why is memory coming down slower than I expected" investigation.

_Metaphor mapping_

- the row of daily-use keys in the front hall → GC roots (stack, globals, registers)
- reachable / unreachable by walking → reachability, not "is anyone referencing it"
- the red ribbon → the mark bit: proven live
- the chalk mark on the doorframe → the gray set: marked, references not yet scanned
- rubbing the chalk off → turning black: all outgoing references traced
- bare objects → white: unmarked this cycle
- no chalk marks left anywhere → gray set empty, marking phase complete
- the dawn cart → the sweep phase reclaiming white objects
- the family moving about all night → concurrent marking with mutator threads running
- the footmen in the doorways → the write barrier
- "no bare thing in a finished room" → the tri-color invariant: no black-to-white pointer
- tie too many ribbons rather than miss one → the barrier is conservative; it only over-approximates liveness
- the two attic cabinets holding each other's keys → a reference cycle, uncollectable by reference counting
- the silver dish that survives till next autumn → floating garbage, collected in a later cycle
</section>
