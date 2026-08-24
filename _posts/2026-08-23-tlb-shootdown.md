---
layout: fable
title: "铃响之后，人人划掉一行 · When the Bell Rings, Everyone Crosses Out a Line"
title_zh: "铃响之后，人人划掉一行"
title_en: "When the Bell Rings, Everyone Crosses Out a Line"
concept: "TLB shootdown"
tags: [linux, performance]
illustration: /assets/art/2026-08-23-tlb-shootdown.jpg
youtube_id: "2I38fOPw8DA"
---
<section class="zh" markdown="1">
码头边的调度大厅，每天早上七点开门。

大厅最里面有一间账房，一排排大账簿顶到天花板，记着哪家货主的货堆在哪个仓位。全城只有这一份权威记录，要改，只能进去改。

问题是账房在最里面。一个办事员要给货主开单，走进去、爬梯子、翻到那一页、抄下仓位号、再走回自己的桌子——一趟三分钟。而大厅里坐着十二个办事员，每人一天开几百张单。

所以他们各自都有个小本子。第一次替某位货主跑过一趟账房之后，就把"陈记米行 → 七号仓"抄在自己本子上。下次这位货主再来，扫一眼本子，十秒出单。十二个人，十二个小本子，各抄各的，谁也不知道别人本子上写了什么，也没人有义务告诉别人。

多数日子这样很好。直到有一天，账房把陈记米行从七号仓挪到了十九号仓——七号仓要腾出来给别人。

改账簿只花了十秒。麻烦的是，此刻大厅里可能有五六个办事员的本子上，还写着"陈记米行 → 七号仓"。他们不会知道账簿变了。小本子不会自己更新。要是有人照着那一行开了单，苦力就会把货扛进一个已经不属于陈记的仓位——或者更糟，扛进一个正在交给别人的空仓。

于是大厅有条规矩，很粗暴，但有效：

改完账簿的那个人，站起来，摇一次铜铃。跑腿的小孩立刻散开，冲到每一张桌子前，把一张纸条拍在办事员手边。办事员不管手上正写着什么，必须停笔，翻开本子，把"陈记米行"那一行划掉——只划这一行，其余几百行不动——然后朝铃声的方向喊一声"划了"。

摇铃的人站在原地不动，数着回音。喊声没齐之前，他不许把七号仓的钥匙交给新货主。哪怕只差一个人没喊，七号仓就还不算空。

老办事员都懂这套规矩的代价。

铃一响，整个大厅停摆一瞬，十二个人同时停笔。挪一个仓位本身只要十秒，摇这一次铃比改账簿贵得多。而且大厅越大、桌子越多，铃响的代价越高——二十四个办事员就要数二十四声回音。所以有经验的管事会把一天要挪的仓位攒起来，下午一次挪完，只摇一次铃。

后来他们又学乖了两件事。一是账房门口挂了块牌子，记着哪个办事员曾经为哪家货主跑过账房；摇铃时只叫那几个人，从没碰过陈记米行的桌子不必打扰。二是小本子换成了带编号的活页：整本作废时不再一行行划，换个编号就行，旧编号下的所有行自动失效，而且改天还能翻回去接着用。

——到这儿你大概已经认出来了：这就是 *TLB shootdown*。

### 这是什么

CPU 每访问一次内存，都要把虚拟地址翻译成物理地址，翻译规则存在内存里的 *page table* 里。完整走一遍（*page table walk*）要好几次访存，太慢，所以每个 CPU core 内部有一小块硬件缓存 *TLB*（Translation Lookaside Buffer），存最近用过的翻译结果。

关键在于：**TLB 不是由硬件保持一致的。** 数据 cache 有 MESI 之类的 coherence 协议，一处改动别处自动失效；TLB 没有这个待遇。当内核修改或撤销一个 page table entry——`munmap`、`madvise(MADV_DONTNEED)`、page migration、COW 断开写保护、NUMA balancing——别的 core 的 TLB 里可能还缓存着旧翻译，而它们无从知晓。

所以失效必须由软件亲手做：发起修改的 CPU 用 *IPI*（inter-processor interrupt）打断所有可能持有旧 entry 的 CPU，让它们各自执行 `invlpg` 踢掉那一条，然后回 ack。发起方必须**等所有 ack 到齐**，才能安全地复用那个 physical frame。这整套动作就叫 TLB shootdown。

### 为什么重要

它是一次同步的、跨核的、打断所有人的操作，成本随 core 数线性增长。在高核心数机器上，频繁 `munmap`、allocator 归还内存、容器内存反复伸缩的 workload，会被 shootdown 的 IPI 风暴拖垮——现象是莫名其妙的高 `system` CPU 和尾延迟抖动，而 perf 上只看到一坨 `smp_call_function_many` 和 `flush_tlb_mm_range`。Linux 的对策正是故事里那两条：用 `mm_cpumask` 只打断真正跑过这个进程的 CPU；用 *PCID / ASID* 给地址空间打标签，让 context switch 不必整表清空。

_隐喻对应表_

- 账房里的大账簿 → 内存里的 *page table*
- 走进账房翻一页 → *page table walk*，慢
- 办事员的小本子 → 每个 CPU core 私有的 *TLB*
- 扫一眼本子就出单 → TLB hit
- 十二个本子各抄各的，互不知情 → TLB **不**由硬件维持 coherence
- 把陈记米行挪出七号仓 → 修改或撤销一个 *PTE*（`munmap`、page migration、COW 断开）
- 摇铜铃、小孩拍纸条 → *IPI*，inter-processor interrupt
- 停笔、划掉那一行 → 被中断的 CPU 执行 `invlpg`，无效化单条 entry
- 只划一行，不撕整本 → 单页失效 vs. full TLB flush
- 站着数回音，喊齐才交钥匙 → 等所有 CPU ack 完成，才可复用 physical frame
- 铃一响全厅停摆 → shootdown 的同步开销，随 core 数放大
- 攒到下午一次挪完 → 批量失效，`flush_tlb_mm_range` 合并
- 门口那块"谁跑过账房"的牌子 → `mm_cpumask`，只 IPI 相关 CPU
- 带编号的活页本 → *PCID / ASID*，地址空间标签，避免整表清空
</section>
<section class="en" markdown="1">
The dispatch hall by the docks opens at seven every morning.

At the very back sits the ledger room: rows of enormous bound books reaching the ceiling, recording which merchant's goods sit in which warehouse bay. It is the only authoritative record in the city, and the only place a change can be made.

The trouble is that the ledger room is at the very back. For a clerk to write a claim ticket, he must walk in, climb the ladder, find the page, copy down the bay number, and walk back to his desk — three minutes a trip. And there are twelve clerks in the hall, each writing hundreds of tickets a day.

So they each keep a little notebook. After making the trip once for a given merchant, a clerk copies "Chen's Rice House → Bay 7" into his own notebook. Next time that merchant comes, a glance at the notebook, ten seconds, ticket written. Twelve clerks, twelve notebooks, each copied privately. Nobody knows what's in anyone else's, and nobody is obliged to tell.

Most days this works beautifully. Until the morning the ledger room moves Chen's Rice House out of Bay 7 and into Bay 19 — because Bay 7 has been promised to someone else.

Amending the ledger takes ten seconds. The problem is that right now, five or six notebooks out in the hall still say "Chen's Rice House → Bay 7." Those clerks have no way of knowing the ledger changed. Notebooks do not update themselves. If anyone writes a ticket off that stale line, the porters will haul goods into a bay that is no longer Chen's — or worse, into a bay being handed to a stranger.

So the hall has a rule. It is brutal, and it works:

Whoever amends the ledger stands up and rings the brass handbell once. The messenger boys scatter instantly, running to every desk and slapping a folded slip down beside each clerk. Whatever he is in the middle of writing, the clerk must stop, open his notebook, and strike out the Chen's Rice House line — that line only, the other several hundred untouched — then shout back toward the bell: "Struck."

The bell-ringer stands where he is, counting the shouts. Until all of them have come back, he may not hand the Bay 7 key to the new merchant. One clerk still silent means Bay 7 is not yet empty.

The old hands know what this rule costs.

When the bell rings, the whole hall stalls for a beat; twelve pens stop at once. Moving a bay takes ten seconds, but ringing the bell costs far more than amending the ledger did. And the bigger the hall, the more it costs — twenty-four clerks means twenty-four shouts to count. So an experienced foreman saves up all the day's bay moves and does them together in the afternoon, ringing the bell once.

In time they learned two more tricks. They hung a board by the ledger-room door recording which clerk had ever made a trip for which merchant, so the bell only summons those few; desks that never touched Chen's Rice House needn't be disturbed. And they replaced the notebooks with numbered loose-leaf ones: when a whole book must be voided, you don't strike out lines one by one — you just change the number, every line under the old number goes dead at once, and you can flip back to it another day.

— By now you've probably recognized it: this is *TLB shootdown*.

### What it is

Every memory access a CPU makes requires translating a virtual address to a physical one, and the translation rules live in the *page table* in memory. Walking the full table (a *page table walk*) costs several memory accesses — far too slow — so each CPU core keeps a small hardware cache, the *TLB* (Translation Lookaside Buffer), holding recently used translations.

The crucial point: **the TLB is not kept coherent by hardware.** Data caches have MESI and friends, where a write in one place automatically invalidates copies elsewhere. TLBs get no such service. When the kernel modifies or tears down a page table entry — `munmap`, `madvise(MADV_DONTNEED)`, page migration, breaking a COW write-protect, NUMA balancing — other cores may still hold the old translation, and they have no way to find out.

So invalidation must be done by software, by hand: the CPU making the change sends an *IPI* (inter-processor interrupt) to every CPU that might hold the stale entry, each executes `invlpg` to evict that one line, and acknowledges. The initiator must **wait for every acknowledgment** before it may safely reuse the physical frame. That whole dance is the shootdown.

### Why it matters

It is a synchronous, cross-core, everybody-stop operation whose cost grows with core count. On high-core-count machines, workloads that `munmap` frequently, return memory to the OS aggressively, or repeatedly resize container memory get dragged down by IPI storms — showing up as inexplicable `system` CPU time and tail-latency jitter, with nothing in the profile but a pile of `smp_call_function_many` and `flush_tlb_mm_range`. Linux's mitigations are exactly the two tricks from the story: `mm_cpumask`, to interrupt only the CPUs that have actually run this process, and *PCID / ASID*, tagging address spaces so a context switch needn't flush the whole thing.

_Metaphor mapping_

- the great ledger books in the back room → the *page table* in memory
- walking in and finding the page → a *page table walk*, slow
- each clerk's private notebook → the per-core *TLB*
- a glance at the notebook, ticket written → a TLB hit
- twelve notebooks copied privately, unaware of each other → the TLB is **not** kept coherent by hardware
- moving Chen's Rice House out of Bay 7 → modifying or tearing down a *PTE* (`munmap`, page migration, COW break)
- ringing the bell, boys slapping slips on desks → the *IPI*, inter-processor interrupt
- stopping mid-word to strike out that line → the interrupted CPU running `invlpg` on a single entry
- striking one line, not tearing up the book → single-page invalidation vs. a full TLB flush
- counting shouts before handing over the key → waiting for all acks before reusing the physical frame
- the whole hall stalling on one bell → the synchronous cost of a shootdown, amplified by core count
- saving the moves up for one afternoon bell → batched invalidation, `flush_tlb_mm_range`
- the board listing who made trips for whom → `mm_cpumask`, IPI only the relevant CPUs
- numbered loose-leaf notebooks → *PCID / ASID*, address-space tags that avoid full flushes
</section>
