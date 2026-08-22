---
layout: fable
title: "趁着满座，把茶楼搬过街 · Moving the Teahouse While the Guests Are Still Drinking"
title_zh: "趁着满座，把茶楼搬过街"
title_en: "Moving the Teahouse While the Guests Are Still Drinking"
concept: "Live migration (pre-copy with dirty page tracking)"
tags: [virtualization, linux, kubernetes]
illustration: /assets/art/2026-08-22-live-migration-pre-copy.jpg
---
<section class="zh" markdown="1">
德兴茶楼开在城南的窄街上，四十年没歇过一天。街对面新起了一座楼，梁更粗、地基更稳、后院还能拴马。掌柜老雷决定搬过去。

难的不是搬。难的是：不许关门。

老雷立过一条死规矩——客人手里那杯茶不能凉，说到一半的书不能断，账台上的算盘不能停。所以不能等打烊了再搬。得在满座的时候搬。

他先做了一件怪事。给楼里每一样东西——每张桌子、每口坛子、每本账册、后厨每个抽屉——都配了一块巴掌大的铜片，一面刷红，一面刷白。伙计只要动过那样东西，随手把铜片翻成红面。每天四更，老雷派个小徒弟绕楼一圈，把红面的记下来，再统统翻回白。

第一夜，挑夫照着单子把整座楼抄了一遍：一样一样搬到对面新楼，摆在一模一样的位置。整整一夜。天亮时，街对面立着一座分毫不差的德兴茶楼——只不过，它是昨夜那一座。

而这一夜里，老楼照常营业。小徒弟绕完一圈，手里记了三百多样红的。

第二夜，挑夫只搬这三百多样。快得多。天亮时，红的剩八十来样。

第三夜，只剩二十来样。第四夜，十一样。

每一轮都比上一轮短，而一轮越短，客人来得及弄乱的东西就越少——这个雪球是往下滚的。

可滚到第七夜，就再也不往下滚了。九样、十样、九样，卡住了。老雷去看那九样究竟是什么：账台上的流水簿、灶上那口一直在响的高汤锅、门口的号牌筒、伙计围裙口袋里的零钱包……全是整座楼里一刻不停被摸的东西。它们不是搬不动，是**一搬过街就旧了**。

老雷这才明白：剩下这一小撮，永远追不上。

他有三个法子，那晚全用上了。

其一，多雇两队挑夫，一夜能搬的翻倍。

其二，跟伙计交代：这几夜手脚放慢些，上茶慢半拍，客人催也慢半拍。生意稍稍难受，但红铜片一夜比一夜少。

其三，也是最后一手——认。

三更时分，老雷站到堂中间，拍了拍手，喊了一声"稍候"。

灯没灭，但一切停住了：说书的收了扇子，算盘按住不动，锅从火上端下来。挑夫冲进来，把最后剩下的九样抱起，跑过街，摆好。老雷把流水簿最后一行写完，合上，也跟着过了街。

前后不到半盏茶。

然后他做了最后一件事：把老楼门口那块"德兴"的匾摘下来，挂到对面新楼门口，又在老门板上贴了一张纸——**茶楼在对面**。街上送水的、送炭的、跑腿的，从此都往新楼走。

第二天早上，没有一个客人发现自己换了楼。有人只隐约记得，昨夜说书说到一半，好像顿了一下。

后来老雷讲起这事，总要补一句：他其实还想过另一条路——干脆先把空楼开起来，客人直接坐进新楼，缺什么再打发人跑回老楼取。头几刻钟手忙脚乱，但停业时间几乎是零。他没敢用。因为那样一来，老楼要是在这当口塌了，两边的茶楼就都没了——一半在这边，一半在那边，凑不出一座完整的。

——到这儿你大概已经认出来了：这不是茶楼的故事。铜片是内存页的 *dirty bit*，一夜一夜的搬运是 *pre-copy* 的迭代轮次，那半盏茶的"稍候"是 *stop-and-copy* 的 downtime window，门上那张纸是 *gratuitous ARP*。这是虚拟机 *live migration* 的故事。

### 这是什么

*Live migration* 要做的事听上去不讲道理：把一台**正在运行**的虚拟机从一台物理机挪到另一台，客人（guest OS 和它上面的应用）全程不掉线，中断只有几百毫秒。

主流做法叫 *pre-copy*，就是老雷那套：

1. **全量轮**：VM 照常跑，把它全部内存页通过网络发到目的端。
2. **脏页追踪**：这一轮里 guest 改过的页要记下来。硬件替你记——EPT/NPT 页表项里的 dirty bit；KVM 把它收集成 *dirty bitmap*（新内核用 *dirty ring*，减少扫描开销）。
3. **迭代轮**：只发上一轮变脏的页。轮次越短，新产生的脏页越少，理论上指数收敛。
4. **stop-and-copy**：当剩余脏页少到"能在目标 downtime 内传完"（QEMU 默认 `downtime-limit` 300ms），暂停 vCPU，传最后一批脏页 + CPU 寄存器 + 设备状态，在目的端恢复执行。
5. **切流量**：目的端发 *gratuitous ARP / RARP*，交换机重新学习 MAC，网络包改道新宿主。

收敛的唯一条件是：**脏页速率 < 传输带宽**。追不上的那一小撮，就是 *writable working set*——进程时时刻刻在写的那几页（栈、堆顶、日志缓冲、锁），它们永远是脏的。

追不上怎么办？三条路，和老雷一样：加带宽（multifd 多连接并行）、*auto-converge*（主动 throttle vCPU，让 guest 慢下来，脏得慢一点）、或者压缩 delta（XBZRLE 只传页内变化的字节）。

再不行，就换 *post-copy*：立刻在目的端启动 VM，内存留空；guest 一碰到没搬过来的页就触发 page fault，通过 userfaultfd 向源端**按需拉取**。downtime 几乎为零，代价是——迁移中途源端或网络一挂，这台 VM 就彻底没了：完整的内存镜像哪一边都没有。这就是老雷不敢走的那条路。

### 为什么重要

这是"宿主机维护对用户不可见"这件事的物理基础。打内核补丁、换固件、腾空一台机器下架，全靠它。

放到 OpenShift 上就非常具体：OpenShift Virtualization / KubeVirt 在 node drain 时会为每个 VMI 创建 `VirtualMachineInstanceMigration`，走的正是 QEMU 的 pre-copy。`KubeVirt` CR 里的 `migrationConfiguration` 就是老雷那三个法子的旋钮：`bandwidthPerMigration`（雇多少挑夫）、`allowAutoConverge`（要不要让伙计放慢手脚）、`allowPostCopy`（敢不敢走那条险路）、`completionTimeoutPerGiB`（多久还不收敛就放弃）。

而**不收敛**是真实的运维症状，不是理论问题：一台内存写得很凶的 VM（数据库、JVM 堆很大的应用）会一轮一轮迁不完，最后超时被取消，VM 留在原节点上——于是 node drain 卡住，整个 maintenance window 停在那里。此时你要做的判断是：加带宽？允许 auto-converge（等于主动给 guest 降速）？还是干脆和业务方约个窗口，冷迁移？

顺便，同一个形状会在很多地方复现：数据库主从切换的 log shipping 追赶阶段、存储卷在线迁移、大表在线重建。都是同一句话——**先复制一个旧的全量，再一轮一轮追增量，最后停一小下，把最后的差补齐。**

### 隐喻对应表

- 德兴茶楼 —— 运行中的虚拟机（guest）
- 老楼 / 对面新楼 —— source host / destination host
- 楼里的桌子坛子账册 —— guest 的内存页（memory pages）
- 挑夫过街 —— 迁移网络的传输带宽
- 红白铜片 —— page table 里的 *dirty bit*
- 四更绕楼一圈记红的 —— 收集 *dirty bitmap* / dirty ring
- 第一夜整楼照抄 —— 全量 pre-copy 轮
- 第二、三、四夜越搬越少 —— 迭代轮次与指数收敛
- 卡在九样十样 —— *writable working set*，收敛不下去的下界
- 流水簿、高汤锅、零钱包 —— 高频写入页（日志缓冲、堆顶、锁）
- 多雇两队挑夫 —— 提高迁移带宽 / multifd
- 让伙计放慢手脚 —— *auto-converge*，throttle vCPU
- 那句"稍候" —— *stop-and-copy*，暂停 vCPU
- 最后九样 + 合上的流水簿 —— 最后一批脏页 + CPU/设备状态
- 半盏茶 —— downtime window（默认目标 ~300ms）
- 摘匾、贴"茶楼在对面" —— *gratuitous ARP / RARP*，交换机重学 MAC
- 想过但没敢用的"先开张、缺啥回去拿" —— *post-copy* 与 userfaultfd 按需取页
- 老楼这时候塌了 —— post-copy 期间源端失效，VM 不可恢复
</section>
<section class="en" markdown="1">
The Dexing Teahouse stood on a narrow street in the south of the city and had not closed for a single day in forty years. Across the street a new building had gone up — thicker beams, a firmer foundation, room in the back to tie up horses. Old Lei, the proprietor, decided to move into it.

The moving was not the hard part. The hard part was this: he would not close.

Old Lei had one iron rule — no guest's tea goes cold, no storyteller stops mid-tale, no abacus at the counter falls silent. So he could not wait for closing time to move. He would have to move the teahouse while every seat was full.

He began with something strange. To every object in the building — every table, every jar, every ledger, every drawer in the kitchen — he attached a copper token the size of a palm, painted red on one side and white on the other. Whenever a waiter touched a thing, he flipped its token to red. And at the fourth watch each night, Old Lei sent an apprentice round the whole house to write down every red token he found, and flip them all back to white.

The first night, the porters copied the entire teahouse from the inventory: object by object across the street, set down in exactly the same place. It took the whole night. By dawn, an identical Dexing Teahouse stood across the street — except that it was last night's teahouse.

And all through that night, the old house had been open for business. When the apprentice finished his round, he had written down three hundred-odd red tokens.

The second night, the porters carried only those three hundred. Much faster. By dawn, some eighty were red.

The third night, twenty. The fourth night, eleven.

Each round was shorter than the last, and the shorter the round, the less the guests had time to disturb — a snowball rolling downhill.

But by the seventh night, it stopped rolling. Nine. Ten. Nine. Stuck. Old Lei went to see what those nine things were: the running ledger at the counter, the stockpot muttering on the stove, the wooden tube of waiting-numbers by the door, the coin purse in the head waiter's apron. Every one of them a thing that was never, for a single moment, not being touched. They were not heavy. They simply **went stale the instant they crossed the street**.

And so Old Lei understood: this last handful would never be caught.

He had three remedies, and used all of them that night.

First, he hired two more teams of porters — twice as much carried per night.

Second, he told the staff: for these few nights, slow your hands. Bring the tea a beat late. Let the guests wait a beat longer. Business suffered a little — and the red tokens grew fewer each night.

Third, and last: he stopped trying.

At the third watch, Old Lei stood in the middle of the hall, clapped his hands twice, and called out: **"One moment, please."**

The lamps stayed lit, but everything stopped. The storyteller folded his fan. A hand came down flat on the abacus. The pot came off the fire. The porters rushed in, gathered up the last nine things, ran them across the street, and set them down. Old Lei finished the last line in the ledger, closed it, and crossed the street himself.

The whole thing took less time than it takes to drink half a cup of tea.

Then he did one last thing. He took the *Dexing* board down from above the old door and hung it above the new one, and pasted a sheet of paper on the old door: **the teahouse is across the street**. From then on the water carriers, the charcoal men and the errand boys all walked to the new building.

The next morning, not one guest noticed he was sitting in a different house. A few dimly recalled that the storyteller had seemed to pause, once, in the middle of a tale.

Whenever Old Lei told this story afterwards, he added a coda. He had considered another route entirely: open the empty new house immediately, seat the guests there, and send a boy running back to the old house for whatever turned out to be missing. The first quarter-hour would be chaos, but the closure would be essentially zero. He had not dared. Because if the old house happened to collapse in that window, both teahouses would be lost at once — half of it here, half of it there, and no complete one anywhere.

— By now you have probably recognized it: this is not a story about a teahouse. The copper tokens are the *dirty bits* on memory pages, the nightly rounds are the iterations of *pre-copy*, the half-cup "one moment please" is the *stop-and-copy* downtime window, and the sheet of paper on the door is a *gratuitous ARP*. This is the story of virtual machine *live migration*.

### What it is

Live migration attempts something that sounds unreasonable: move a **running** virtual machine from one physical host to another with the guest OS and its applications never going down — an interruption of only a few hundred milliseconds.

The dominant technique is *pre-copy*, which is exactly Old Lei's scheme:

1. **Full round.** The VM keeps running while every one of its memory pages is sent over the network to the destination.
2. **Dirty tracking.** Pages the guest modifies during that round must be recorded. The hardware does it for you — dirty bits in the EPT/NPT page table entries — and KVM gathers them into a *dirty bitmap* (newer kernels use a *dirty ring* to avoid the scan cost).
3. **Iterative rounds.** Send only the pages dirtied in the previous round. Shorter rounds mean fewer newly dirtied pages, so in theory the set shrinks geometrically.
4. **Stop-and-copy.** Once the remaining dirty set is small enough to ship within the target pause (QEMU's default `downtime-limit` is 300ms), pause the vCPUs, transfer the last batch of pages plus CPU registers and device state, and resume execution on the destination.
5. **Redirect traffic.** The destination sends a *gratuitous ARP / RARP*; switches relearn the MAC, and packets start flowing to the new host.

Convergence has exactly one condition: **dirty rate < transfer bandwidth**. The handful you can never catch is the *writable working set* — the pages a process is always writing (stack, heap top, log buffers, locks). They are permanently dirty.

When it won't converge, you have Old Lei's three remedies: add bandwidth (multifd parallel connections), *auto-converge* (deliberately throttle the vCPUs so the guest dirties memory more slowly), or compress the delta (XBZRLE ships only the bytes that changed within a page).

If that still fails, switch to *post-copy*: start the VM at the destination immediately with empty memory, and let every touch of a missing page raise a fault that pulls it on demand from the source via userfaultfd. Downtime approaches zero — at the price that if the source host or the network dies mid-migration, the VM is gone for good, because no complete memory image exists on either side. That is the road Old Lei did not dare take.

### Why it matters

This is the physical basis for "host maintenance the user never sees." Kernel patching, firmware updates, draining a machine for decommission — all of it rests here.

In OpenShift it gets very concrete. OpenShift Virtualization / KubeVirt creates a `VirtualMachineInstanceMigration` for each VMI when a node is drained, and underneath it is QEMU pre-copy. The `migrationConfiguration` block in the `KubeVirt` CR is literally Old Lei's set of dials: `bandwidthPerMigration` (how many porters), `allowAutoConverge` (whether the staff slow their hands), `allowPostCopy` (whether you dare take the risky road), and `completionTimeoutPerGiB` (how long before you give up).

And **non-convergence is an operational symptom, not a theoretical one**: a memory-hot VM — a database, a JVM with a large heap — will iterate forever, eventually time out, get cancelled, and stay on the original node. The drain stalls, and the whole maintenance window stalls with it. The judgement call at that moment is: raise the bandwidth? Allow auto-converge (that is, deliberately slow the guest down)? Or negotiate a window with the application owner and do a cold migration?

The same shape recurs elsewhere, incidentally: the catch-up phase of database log shipping before a failover, online storage volume migration, online rebuilds of a large table. All of it is one sentence — **copy a stale full image, chase the delta round after round, then stop briefly and close the last gap.**

### Metaphor mapping

- The Dexing Teahouse — the running virtual machine (the guest)
- Old house / new house across the street — source host / destination host
- Tables, jars and ledgers inside — the guest's memory pages
- Porters crossing the street — the migration network's bandwidth
- The red-and-white copper tokens — the *dirty bit* in the page table entry
- The fourth-watch round recording the red ones — collecting the *dirty bitmap* / dirty ring
- Copying the whole house on the first night — the full pre-copy round
- Fewer things each night after — iterative rounds and geometric convergence
- Stuck at nine or ten — the *writable working set*, the floor convergence cannot cross
- The ledger, the stockpot, the coin purse — hot pages: log buffers, heap top, locks
- Hiring two more teams of porters — raising migration bandwidth / multifd
- Telling the staff to slow their hands — *auto-converge*, throttling the vCPUs
- "One moment, please" — *stop-and-copy*, pausing the vCPUs
- The last nine things plus the closed ledger — the final dirty pages plus CPU and device state
- Half a cup of tea — the downtime window (default target ~300ms)
- Moving the sign, pasting the note on the old door — *gratuitous ARP / RARP*, switches relearning the MAC
- The route he considered but did not take — *post-copy* with userfaultfd demand paging
- The old house collapsing in that window — source failure during post-copy, leaving the VM unrecoverable
</section>
