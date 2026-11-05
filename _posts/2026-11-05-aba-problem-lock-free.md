---
layout: fable
title: "行李房的旧木牌 · The Old Wooden Tag"
title_zh: "行李房的旧木牌"
title_en: "The Old Wooden Tag"
concept: "The ABA Problem in Lock-Free Data Structures"
tags: [runtime, memory, performance]
illustration: /assets/art/2026-11-05-aba-problem-lock-free.jpg
youtube_id: "OTTHlwAC60g"
---
<section class="zh" markdown="1">
渡口镇有个行李房，专给过江的人存箱子。

行李房不大，一面墙上钉着八十个木钩，编号从一到八十。每个木钩上前一个箱子压着后一个箱子，箱子侧面都别着一张小纸条，写着"我下面挂的是几号钩"。八十个钩子串成一条链，掌柜只认最上面那一个。

墙根放着一本簿子，簿子第一页只有一行字，写着"顶钩：七号"。这一行是整间房的全部账目。要取箱子，看顶钩在哪；要存箱子，把顶钩改掉。除此以外，房里一个字都不写。

掌柜姓何，做事极稳，稳到有点慢。他取箱子的手法，镇上没人挑得出错：

第一，看簿子，顶钩是七号。
第二，看七号钩上那只箱子的纸条，写着"下面挂的是十二号"。
第三，把七号箱子解下来交给客人。
第四，在簿子上把顶钩改成十二号。

中间那一步——看纸条——他得把箱子抱到窗边的光底下，因为纸条常被雨水泡糊。这一抱，就要抱一会儿。

何掌柜一个人守了二十年，从没错过。真正让这间房出事的，是他收了个徒弟。

徒弟姓邵，手快，恨不得一只手解三个箱子。何掌柜起先不放心，后来想通了：两个人，一个解箱子，一个看纸条，各干一步，谁也不碰谁的活，这不就是快了一倍？

错了。两个人的速度，是把一件必须连着做的事拆成了两个人各做一半，而中间那段时间，谁都没在看。

出事那天是赶集。

何掌柜正抱着一只箱子在窗边看纸条。七号箱，纸条上写着"下面挂的是十二号"。

邵徒弟那边同时在干活：西街的老太来取七号箱，邵徒弟利索地解下来递给她；她下面那只十二号箱，主人也来了，邵徒弟又解下来递出去。两个箱子走了，墙上七号钩和十二号钩都空了。

紧跟着，东街一个挑担的客人要存箱子。八十个钩子空了两个，按房里的老规矩，空出来的钩子立刻回收，重新编号——挑担客人的箱子，就挂在了刚空出来的七号钩上，成了墙上最上面那一个。它下面挂的是三号钩。

何掌柜这时候从窗边抬起头。他把纸条看明白了：下面挂十二号。

他先看簿子——这是他的第二步和第四步之间的那个念头——簿子上顶钩还是"七号"。

他心里一松：没变，没人动过。

于是他把墙上那只七号钩上的箱子解下来，交给了手里拿着领条的那位客人，然后在簿子上郑重地把顶钩改成"十二号"。

三件事同时发生，而当时没人知道是哪三件：

一，那位客人拿走的不是他的箱子，是挑担客人的。领条对得上——领条上写的就是"七号"。
二，簿子上的顶钩现在指着十二号，而十二号钩是空的。下一个来取箱子的人，会被领到一面空墙前面。
三，那条链，从七号开始往下，已经和簿子上写的那条链没有关系了。

到下午，房里彻底乱掉：有人拿到空钩，有人拿到别人的箱，有人的箱子从房顶上掉下来——因为有个学徒照着簿子去解一只"存在的"箱子，手一滑，把三只箱子全拽了下来。

何掌柜坐在墙根，簿子摊在膝盖上，看了一整夜。

他想不明白的不是"谁错了"。两个人都没错：他按规矩看了簿子，看了纸条，改了簿子；邵徒弟按规矩解了客人的箱子，回收了空钩。错的是那个空隙——**从他看簿子、到他改簿子，中间隔着他抱箱子看纸条那一段时间。在这段时间里，墙被人改过，又改回了原样。**

他第二天一早，把全房的人都叫到墙根，说了三句话。这三句话后来刻在了门口的木牌上。

**第一句：只看"还是七号"是不够的，要看"第几次的七号"。**

他让木匠在每个钩子上多刻一道浅槽，每换一次箱，槽就加深一分，从一到正字，一笔一笔往上记。刻痕不能擦，只能加。以后看簿子，不能只写"顶钩：七号"，要写"顶钩：七号，第四十一笔"。

这样再看一遍：他抱箱子看纸条的时候，簿子上是"七号，第四十一笔"；等他回来，簿子上已经是"七号，第四十七笔"——同样是七号，但七号被换过六轮。他一眼就能看见：这段时间里墙不是我看到的那个墙。

镇上的人把这道槽叫"加深一笔"。何掌柜说，这一笔比钩子本身金贵。

**第二句：你正在看的东西，不许被别人拿走。**

何掌柜在墙上加了一排木夹子，谁把某个箱子抱到窗边看，就先把夹子夹在那只箱子和它下面那只箱子上。夹子夹住的钩子，回收时不许用。规矩很简单：**当着人的面念的账，不许改。**

起初有人说这太浪费——赶集那天八十个钩子，被夹住的有十七只，后面来的客人只能往高处挂。何掌柜只说了一句：留着空钩，客人多走两步；空钩被人拿走，客人拿错箱子。多走两步的账，我认；拿错箱子的账，谁认？

**第三句：空钩不立刻回收，隔一轮再用。**

这是最省事也最难让人接受的一条。空出来的钩子，当天不再编号，等墙上的客人都走完一轮，才回到可用的一堆里重新编号。这样，任何一个人手上的"七号"，在他手上的这一轮里不会变成别人的七号。

代价是墙上总有几只钩子闲着。何掌柜说，闲着的钩子是租金，不是浪费。

后来行李房又添了新麻烦。

墙上不止八十个钩子——赶集的日子临时加钩，钩子的编号是从一堆旧木牌里抓的，抓哪块算哪块。抓出来的木牌，有的已经被刻满了正字，翻过来一看，还是七号。何掌柜一开始觉得无所谓：编号一样，钩子一样。直到有一年，一只箱子挂上了"七号，第四十一笔"——和四年前一块退役木牌上的刻痕一模一样。他抱到窗边看纸条，回来对簿子，对得严丝合缝，把别人的箱子交了出去。

那天他把所有退役木牌收进一只樟木箱，箱上贴了一张纸：**刻满的牌，不再出手。**

镇上的人后来总结这间房的三件事，说得比何掌柜还准：

一、账上的号不算数，账上的号加刻痕才算数。
二、你手上拿着的号，在你放手之前不能变。
三、变不了的号，不要拿出去。

到这儿，你大概已经认出来了：这就是 *无锁数据结构*（*lock-free data structure*）里的 *ABA 问题*。

### 这是什么

先看机器。所有 *CAS*（compare-and-swap，比较并交换）指令——x86 的 `LOCK CMPXCHG`、ARM 的 `LDXR/STXR`、C++ 的 `std::atomic<T>::compare_exchange_weak`——都只做一件事：**"这个地址上的值如果还是 X，就写成 Y，返回成功；否则什么都不做，返回失败。"** 它比较的是**值**，不是**历史**。

问题就出在这里。值可以被改掉，也可以被改回来。

经典现场，一个无锁栈：

```c
// 栈顶指针 top；每个节点有自己的 next
Node *pop(void) {
    Node *old_top = top;          // ① 读到 top = A
    Node *next    = old_top->next; // ② 读到 A->next = B
    // ...这里可能被抢占，或者只是变慢（缓存未命中、页错误、调试器断点）
    return CAS(&top, old_top, next) ? old_top : retry(); // ③
}
```

在 ② 和 ③ 之间，另一个线程做了三件事：`pop()` 掉 A，`pop()` 掉 B，然后**因为 B 被释放并复用**，又把 A（或者一块正好落在 A 地址上的新节点）`push()` 回了栈顶。

现在 `top` 的值又等于 A。③ 的 CAS 比较成功——它看到的就是 A。于是代码把栈顶设成 `next`，也就是**已经被弹出、可能已经释放的 B**。

两个后果，都很严重：

- **丢数据。** 栈顶被指回一个已经不在栈里的节点，从那一刻起链上其余元素全部失去引用，永久泄漏。
- **悬空指针。** 那个内存如果已经还给分配器（并被别处复用），整个链就是一堆野指针，读写它属于 *use-after-free*。

注意这里不是"竞态导致读到脏值"。每一次读到的值都是当时真实的。**错的是把"值相同"当成了"状态相同"。** 一个值的三次变化 A→B→A，在 *CAS* 眼里和"没变过"不可区分——这就是 ABA。

三条标准解法，对应故事里的三句话：

1. **给值加计数器（tagged pointer / 版本号）。** 让指针和版本号一起做比较，即 *double-width CAS*（`cmpxchg16b`、`cmpxchg8b`）或者把版本号塞进指针高位。A 回来时版本号已经变了，CAS 就失败。这就是"加深一笔"。
   - 关键细节：版本号会**回绕**（wrap）。32 位记数配 64 位指针，理论要 2³² 次修改才绕回同一 (指针, 版本) 组合，工程上够用；但别在 8 位版本号上赌这个，那不叫安全，叫概率。
2. **不让人拿走你正在看的东西（safe memory reclamation）。** 版本号解决的是栈顶指针，**节点本身被释放复用**才是更深的坑。三种工程做法：
   - *Hazard pointers*：线程发布"我正在看这个指针"，回收者看到有 hazard 就跳过。这就是墙上的木夹子。
   - *Epoch / QSBR（quiescent-state-based reclamation）*：推迟回收，等所有线程都经过一次静止点。这就是"隔一轮再用"。
   - *Garbage collection*（如 Java 的 `AtomicReference`、Go 的 GC）：内存不由你释放，ABA 只剩指针层的问题，`AtomicStampedReference` 这类带戳的引用可以覆盖。
3. **不把可能翻新过的编号发出去。** 复用内存时要让"旧值"不再等于新值——物理上做不到时，就用上面两种机制绕开，而不是假装它不会发生。

真实世界里的踩点：Java `AtomicInteger` 的 `compareAndSet` 在无锁栈里就是教科书级 ABA（所以有 `AtomicStampedReference`）；C 里 `free()` 后 `malloc()` 拿回同一地址极其常见（glibc 的 *tcache* 几乎保证这一点）；内核里 slab 回收走的是 *RCU* + 引用计数，绕开了 ABA；数据库的 MVCC 版本号设计本质上是同一件事的另一个方向。

### 为什么重要

因为这是那种**测试永远测不出来、压测一遍就过、上线三个月才第一次崩**的问题。

- **它需要精确的时序，而且窗口极窄。** ② 到 ③ 之间通常只有几条指令；要触发，得让线程恰好在那个位置被抢占，并且另一条线程恰好在窗口内完成 push/pop/push 三个动作。本机跑一万次都不会出现，压测机上一次都不出现，只有生产上三千万次里出现那一次。
- **它不报错，只损坏数据。** CAS 返回 `true`。日志里全是成功。栈看起来在工作——`push` 之后 `size` 加了，`pop` 也返回了东西，只是返回的东西不对，或者链后半截不见了。表现是"偶发内存缓慢增长"和"偶发数据缺失"，两个最难归因的症状。
- **越"安全"的复现越不可靠。** Address Sanitizer、TSan 能抓到一部分 UAF，但它们改变时序，也可能让 ABA 消失（这也是为什么无锁代码要用专门的模型检查工具：CDSChecker、GenMC、Loom for Rust/Go）。
- **它藏在性能动机里。** 从互斥锁改到无锁，通常是在压力下做的决定——而压力下做的重构，最容易漏掉内存回收策略这一层。锁的保护范围是显式的（`lock` 到 `unlock` 之间），无锁的保护范围是隐式的（每一个"读—算—改"三步之间）。

工程上的取舍，按收益排序：

1. **先问：真的需要无锁吗？** 无锁赢得的是"没有线程被挂起"，输掉的是可读性和可验证性。通道（channel）、刺信号量（`sync.Mutex`）、每线程局部结构、分片加锁，能解决问题就别写 CAS 循环。这不是保守，这是把风险换成了确定性。
2. **要写 CAS 循环，就同时写回收策略。** 两者是同一件事的两半，不能先写一半、上线、以后再说。
3. **别手写，先用语言的标准容器。** Go 的 `sync.Map`/`atomic.Pointer`（依赖 GC）、Java 的 `java.util.concurrent`、Rust 的 `crossbeam`（自带 epoch 回收，本质是故事里那张木牌）。这些库存在的唯一理由，就是有人已经替你踩过这一排坑。

最后是这个故事的隐喻本身。何掌柜不是不细心，他是**把"还是七号"当成了"还是那只箱子"**。所有的并发错误里，这一种最难防，因为它不来自粗心，而来自一个原则上很合理、只在概率极小的时刻不成立的假设。

木牌上的第三句最值钱：**变不了的号，不要拿出去。**

_隐喻对应表_

- 八十个木钩串成的链 → 无锁栈（*lock-free stack*）的节点链
- 簿子第一页的"顶钩：七号" → 栈顶原子指针 `top`
- 箱子上别着"下面挂的是几号钩" → 每个节点的 `next` 指针
- 何掌柜的四步取箱法 → `pop()` 的读 top → 读 next → CAS → 重试循环
- 抱箱子到窗边看纸条 → 第二次读与 CAS 之间的时间窗口（抢占、缓存未命中、页错误）
- 邵徒弟同时解走七号和十二号两箱 → 另一线程把 A 和 B 都弹出
- 钩子立刻回收、重新编号 → 被弹出的内存立刻 `free()` 并被分配器复用
- 挑担客人的箱子重新挂在七号 → 同一地址上重新出现同一个值 A
- 簿子上顶钩仍是"七号"，何掌柜因此放心 → CAS 看到值没变，比较成功
- 顶钩被改成十二号，而十二号是空钩 → 栈顶指向已被弹出/释放的 B，链断裂
- 客人拿到别人的箱子，领条却对得上 → 程序拿到正确类型的错误对象，无异常、无告警
- 有人拿到空钩、箱子从房顶掉落 → `next` 已成悬空指针，后续遍历读到垃圾
- 何掌柜"两个人各干一步就快一倍"的判断 → 把临界区拆给两个线程，中间出现无人保护的空隙
- "只看还是七号不够，要看第几次的七号" → *tagged pointer* / 版本号，用双字 CAS 比较（指针, 版本），A 回来时版本已变
- 钩子上刻的浅槽只能加不能擦 → 版本号单调递增，不因业务状态回退而回退
- 版本号会刻满、会翻回来 → 版本号回绕（wrap）问题：位宽决定安全边界，不是逻辑决定
- 刻满的牌不再出手 → 复用内存时不能让旧值等于新值；做不到就靠回收机制绕开
- 墙上的木夹子，夹住的钩子不许回收 → *hazard pointers*：线程发布"我正在访问这个指针"
- "当着人的面念的账不许改" → 被 hazard 标记的节点不可释放，即使它已不在链上
- 空钩隔一轮才回收 → *epoch* / *QSBR* 延迟回收，等所有线程经过静止点
- 留空钩是租金不是浪费 → 回收延迟换取正确性，内存占用上升是可接受的代价
- 赶集日临时加钩，木牌从旧牌堆里抓 → *tcache* / slab 复用几乎必然返回刚 `free()` 的同一地址
- 那只"七号，第四十一笔"的退役木牌被再次发出 → 版本号回绕后 (指针, 版本) 组合真正重复，CAS 被彻底骗过
- 樟木箱上"刻满的牌不再出手" → 位宽不足的版本号不能当安全机制用，只能当概率机制
- 镇上三句总结：号加刻痕才算数、手上的号不能变、变不了的号不要拿出去 → 带版本比较、安全回收、不发出可复用的旧值
- 何掌柜二十年不出错，换徒弟当天出事 → 单线程永无 ABA；它只在真正并发时出现，因此压测与单机复现极难命中
- "两个人各干一步，谁也不碰谁的活" → 无锁的直觉吸引力，以及它隐藏的代价：锁的保护范围是显式的，CAS 循环的保护范围是隐式的
</section>
<section class="en" markdown="1">
At the ferry town there was a left-luggage hut for travellers crossing the river.

It was small: eighty wooden pegs on one wall, numbered one to eighty. Each peg held one trunk on top of another, and every trunk had a slip of paper pinned to its side reading "the peg below me is number such-and-such." Eighty pegs strung into a single chain, and the keeper only ever cared about the top one.

On the floor against the wall lay a ledger. Its first page held one line: "Top peg: seven." That line was the hut's entire bookkeeping. To fetch a trunk, look at the top peg. To store one, change the top peg. Nothing else in the room was written down anywhere.

The keeper was named He, and he was careful — careful nearly to the point of slowness. Nobody in town could find a flaw in how he retrieved a trunk:

First, look at the ledger. Top peg is seven.
Second, look at the slip on the trunk hanging on peg seven. It says: below me is peg twelve.
Third, lift trunk seven down and hand it to the customer.
Fourth, write the top peg in the ledger as twelve.

That middle step — reading the slip — required carrying the trunk to the light by the window, because the slips were often rain-blurred. Carrying it took a while.

Keeper He ran the hut alone for twenty years without a single mistake. What finally broke the hut was that he took on an apprentice.

The apprentice was named Shao: quick-handed, and he'd have liked to unhook three trunks with one hand if he could. He had worried about it at first, then decided it was fine — two people, one lifting trunks and one reading slips, each doing a separate step, neither touching the other's work. Surely that was twice as fast.

It was not. Two people's speed meant splitting one thing that had to happen in one piece into two halves done by two people, with a stretch in the middle that nobody was watching.

The day it happened was market day.

Keeper He was at the window with a trunk in his arms reading its slip. Trunk seven, slip says: below me is peg twelve.

Shao was working at the same time. The old woman from West Street came for trunk seven, and Shao unhooked it and handed it over, neat as you like. The owner of trunk twelve — the one beneath it — arrived too, and Shao unhooked that one and handed it over as well. Both trunks gone; pegs seven and twelve now empty.

Immediately after, a porter with a carrying pole wanted to store a trunk. Two pegs out of eighty stood empty, and by the hut's old rule, freed pegs were recycled at once and renumbered on the spot — so the porter's trunk went onto that freshly emptied peg seven, and became the topmost thing on the wall. Below it hung peg three.

That was when Keeper He looked up from the window. He had the slip clear in his mind: below me is peg twelve.

He checked the ledger — that thought between his second step and his fourth — and the ledger still read "top peg: seven."

He relaxed. No change. Nobody had touched a thing.

So he lifted the trunk off peg seven, handed it to the customer holding a claim ticket, and solemnly wrote the top peg down as twelve.

Three things happened at once, and at the time nobody knew which three:

One: the customer did not walk out with his own trunk, but with the porter's. The claim ticket matched — the ticket said "seven."
Two: the ledger's top peg now pointed at twelve, and peg twelve was empty. The next person to fetch a trunk would be led to a bare wall.
Three: the chain starting at peg seven no longer had anything to do with the chain written in the ledger.

By afternoon the hut was in ruins. Someone was handed an empty peg. Someone got another man's trunk. Somebody's trunks fell off the wall — because an apprentice followed the ledger to unhook a trunk that "existed," slipped, and brought three of them down together.

Keeper He sat against the wall with the ledger on his knees and read it all night.

What he could not work out was not who was wrong. Neither was wrong. He had followed the rules: read the ledger, read the slip, changed the ledger. Shao had followed the rules: unhooked the customer's trunk, recycled the free pegs. What was wrong was the gap — **between the moment he read the ledger and the moment he wrote it, he spent time carrying a trunk to the window. During that time the wall was changed, and changed back to the way it looked.**

The next morning he called everyone to the wall and said three things. Those three were later carved on a board above the door.

**One: "still peg seven" is not enough. You need "which peg seven."**

He had the carpenter cut a shallow notch into every peg, deepened each time the peg took a new trunk, tallied in strokes like the character for five. A notch cannot be erased, only added. From then on the ledger would not read "top peg: seven," but "top peg: seven, forty-first stroke."

Look again with that in place. While he carried the trunk to the window, the ledger read "seven, forty-first stroke." When he came back it read "seven, forty-seventh stroke" — the same seven, but seven had turned over six times. He could see at a glance: the wall I looked at is not the wall this is.

The town called the notch "one more stroke." Keeper He said that stroke was worth more than the peg.

**Two: what you are looking at may not be taken away.**

He added a row of wooden clips to the wall. Whoever carried a trunk to the window clipped the trunk, and the trunk under it, first. A clipped peg could not be reused. The rule was simple: **an account being read aloud may not be edited.**

At first people said it was wasteful. On market day, seventeen of eighty pegs were clipped at once, and later customers had to hang their trunks higher. Keeper He said only this: leave the pegs free, and a customer walks a few extra steps. Let a free peg be taken, and a customer walks off with the wrong trunk. The extra steps, I will answer for. The wrong trunk, who will answer for?

**Three: free pegs are not recycled at once. Wait one round.**

This was the easiest to do and the hardest to accept. A freed peg would not be renumbered that day. Only after every customer on the wall had gone through a full round would it return to the usable pile and take a new number. That way, any "seven" in somebody's hand would not become somebody else's seven while he held it.

The cost was pegs standing idle on the wall. Keeper He said an idle peg is rent, not waste.

Later the hut ran into fresh trouble.

The wall held more than eighty pegs — on market days extra ones were added, and their numbers were grabbed from a heap of old wooden tags, whichever came up. Some tags dragged out of that heap had their tallies fully carved, and turned over, still read seven. Keeper He thought little of it at first: same number, same kind of peg. Then one year a trunk went out on "seven, forty-first stroke" — identical to a tag retired four years earlier. He carried it to the window, read the slip, checked the ledger, and it matched perfectly. He handed over another man's trunk.

That day he collected every retired tag into a camphor chest and pasted a paper to its lid: **a tag whose tally is full does not go out again.**

The town's own summary of the hut was sharper than Keeper He's:

One: a number in the ledger does not count. A number plus its tally counts.
Two: a number in your hand may not change before you let go of it.
Three: a number that cannot change must not be handed out.

By now you have probably recognized it: this is the *ABA problem* in *lock-free data structures*.

### What it is

Start with the instruction. Every *CAS* (compare-and-swap) — x86's `LOCK CMPXCHG`, ARM's `LDXR/STXR`, C++'s `std::atomic<T>::compare_exchange_weak` — does exactly one thing: **"if the value at this address is still X, write Y and report success; otherwise do nothing and report failure."** It compares a **value**, not a **history**.

That is the whole problem. A value can change, and it can change back.

The classic site, a lock-free stack:

```c
// top is the stack head; each node carries its own next
Node *pop(void) {
    Node *old_top = top;           // 1: read top == A
    Node *next    = old_top->next; // 2: read A->next == B
    // ...here we may be preempted, or merely slowed (cache miss, page fault, a debugger breakpoint)
    return CAS(&top, old_top, next) ? old_top : retry(); // 3
}
```

Between 2 and 3, another thread does three things: `pop()`s A, `pop()`s B, and then — because B was freed and its memory reused — `push()`es A back on top (or a new node that happens to land at A's address).

Now `top` equals A again. The CAS at 3 succeeds, because what it sees is A. So the code sets the head to `next` — which is B, already popped and possibly already freed.

Two consequences, both bad:

- **Lost data.** The head now points to a node no longer in the stack, and every element below it loses its last reference. Permanent leak.
- **Dangling pointers.** If that memory went back to the allocator and was reused elsewhere, the chain is a row of wild pointers, and walking it is a *use-after-free*.

Note what this is not: it is not a race that produced a stale read. Every read returned the true value of its moment. **What broke was treating "same value" as "same state."** A value that goes A→B→A is indistinguishable from an unchanged one to a CAS. That is ABA.

Three standard fixes, matching the hut's three sentences:

1. **Attach a counter to the value (tagged pointer / version).** Compare pointer and version together, via *double-width CAS* (`cmpxchg16b`, `cmpxchg8b`) or by packing a version into high pointer bits. When A comes back the version has moved, and the CAS fails. That is the notch.
   - The key detail: the version **wraps**. A 32-bit counter with a 64-bit pointer needs 2³² modifications to reproduce the same (pointer, version) pair — fine in practice. Do not gamble on an 8-bit version. That is not safety, it is a probability.
2. **Do not let what you are looking at be taken away (safe memory reclamation).** The version fixes the head pointer; **the node being freed and reused** is the deeper hole. Three engineering answers:
   - *Hazard pointers*: a thread publishes "I am looking at this pointer," and reclaimers skip anything hazarded. That is the wooden clip.
   - *Epoch / QSBR (quiescent-state-based reclamation)*: defer reclamation until every thread has passed a quiescent point. That is "wait one round."
   - *Garbage collection* (Java's `AtomicReference`, Go's GC): memory is not yours to free, so ABA reduces to the pointer layer, and stamped references like `AtomicStampedReference` cover it.
3. **Do not hand out identifiers that can come back renovated.** When reusing memory, make the old value not equal the new one — and where that is physically impossible, route around it with the two mechanisms above instead of pretending it cannot happen.

Where this bites in the real world: Java's `AtomicInteger.compareAndSet` in a lock-free stack is the textbook ABA (hence `AtomicStampedReference`); in C, `malloc()` returning the address you just `free()`d is the common case, not the exception (glibc's *tcache* all but guarantees it); in the kernel, slab reclamation goes through *RCU* plus refcounts and dodges ABA entirely; and MVCC version numbers in databases are the same idea pointed the other way.

### Why it matters

Because this is the kind of bug that **tests never catch, a load test passes every time, and production hits three months in.**

- **It needs exact timing, and the window is tiny.** Step 2 to step 3 is usually a handful of instructions. Triggering it requires a preemption at precisely that spot *and* another thread completing push/pop/push inside the window. Ten thousand runs locally: nothing. A full load test: nothing. Production, once in thirty million operations.
- **It reports no error, only damage.** CAS returns `true`. The logs are all successes. The stack looks like it works — `push` increments the size, `pop` returns something, it is just the wrong something, or the back half of the chain is gone. It surfaces as "memory grows slowly" and "data occasionally missing," the two hardest symptoms to attribute.
- **Better tooling can make it disappear.** AddressSanitizer and TSan catch some of the UAF, but they change timing and may make the ABA unobservable. This is why lock-free code gets model checkers: CDSChecker, GenMC, Loom for Rust and Go.
- **It hides inside a performance motive.** Moving from a mutex to lock-free is usually a decision made under pressure, and refactors made under pressure most often skip the reclamation layer. A lock's protected range is explicit (`lock` to `unlock`). A lock-free loop's protected range is implicit — it is the gap inside every read-compute-write triple.

Engineering trade-offs, ordered by payoff:

1. **Ask first whether you need lock-free at all.** Lock-free buys "no thread is ever parked"; it pays with readability and verifiability. Channels, a plain mutex, per-thread local structures, and sharded locks may solve the problem; if they do, do not write a CAS loop. That is not conservatism, it is trading risk for certainty.
2. **If you write a CAS loop, write the reclamation policy in the same commit.** They are two halves of one thing. Do not ship half and think about the other later.
3. **Do not hand-roll; start from the standard containers.** Go's `sync.Map` / `atomic.Pointer` (GC-backed), Java's `java.util.concurrent`, Rust's `crossbeam` (epoch reclamation built in, which is exactly the wooden tag). Those libraries exist for one reason: somebody already stepped in this row of holes.

And the metaphor itself. Keeper He was not careless. He confused **"still peg seven"** with **"still the same trunk."** Of all the concurrency bugs, this one is hardest to prevent, because it does not come from inattention — it comes from a perfectly reasonable assumption that fails only at a vanishingly small fraction of moments.

The board's third line is the valuable one: **a number that cannot change must not be handed out.**

_Metaphor mapping_

- Eighty pegs strung into a chain → the node chain of a *lock-free stack*
- "Top peg: seven" on the ledger's first page → the atomic head pointer `top`
- The slip reading "below me is peg twelve" → each node's `next` pointer
- Keeper He's four-step retrieval → `pop()`: read top, read next, CAS, retry loop
- Carrying the trunk to the window to read the slip → the window between the second read and the CAS (preemption, cache miss, page fault)
- Shao unhooking both seven and twelve → another thread popping A and then B
- Pegs recycled and renumbered at once → popped memory `free()`d and immediately reused by the allocator
- The porter's trunk rehung on peg seven → the same value A reappearing at the same address
- The ledger still reading "seven," so He relaxed → CAS seeing an unchanged value and succeeding
- Top peg set to twelve, and twelve is empty → head set to an already-popped, possibly freed B; the chain breaks
- A customer getting the wrong trunk with a matching claim ticket → the program receives a wrong object of the right type, with no exception and no log
- Someone handed an empty peg; trunks falling off the wall → `next` is now dangling; traversal reads garbage
- "Two people, one step each, twice as fast" → splitting a critical section across two threads, leaving a stretch nobody guards
- "Not just which seven, but which peg seven" → *tagged pointer* / version, compared with double-width CAS on (pointer, version); when A returns, the version has moved
- Notches that can only be added, never erased → the version is monotonic and does not roll back with business state
- Tallies filling up and turning over → version wrap: bit width sets the safety boundary, not logic
- A full tag never going out again → on reuse, the old value must not equal the new one; where it must, route around it with reclamation
- Wooden clips forbidding a clipped peg's reuse → *hazard pointers*: a thread publishes the pointer it is accessing
- "An account being read aloud may not be edited" → a hazarded node cannot be reclaimed even after it leaves the chain
- Free pegs waiting one round → *epoch* / *QSBR* deferred reclamation after a quiescent point
- Idle pegs as rent, not waste → delaying reclamation buys correctness at the cost of memory
- Market-day tags grabbed from the old pile → *tcache* / slab reuse almost always returns the address just freed
- The retired "seven, forty-first stroke" tag going out again → the version wrapped, so (pointer, version) genuinely repeated and CAS was fully fooled
- The camphor chest marked "a full tag does not go out again" → an undersized version field is a probability mechanism, never a safety mechanism
- The town's three lines: number plus tally counts; a number in hand may not change; an unchanging number must not go out → versioned compare, safe reclamation, never expose a reusable old value
- Twenty flawless years, then a failure on the apprentice's first day → single-threaded code has no ABA; it appears only under real concurrency, which is why load tests and local repro rarely hit it
- "Two people, one step each, neither touching the other's work" → the intuitive pull of lock-free code, and its hidden cost: a lock's protected range is explicit, a CAS loop's is implicit
</section>
