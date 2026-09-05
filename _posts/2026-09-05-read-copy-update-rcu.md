---
layout: fable
title: "摘星楼的旧星图 · The Star Charts of the Stargazing Tower"
title_zh: "摘星楼的旧星图"
title_en: "The Star Charts of the Stargazing Tower"
concept: "Read-Copy-Update (RCU)"
tags: [linux, memory, performance]
illustration: /assets/art/2026-09-05-read-copy-update-rcu.jpg
youtube_id: "rjwN2JBuA6Q"
---
<section class="zh" markdown="1">
古都北郊的摘星楼高九层，楼顶立着司天监最紧要的物什：一幅悬在正厅中央的巨幅青绢星图。

天下各地的航船水手、钦天监生、行商游侠，每天都有数百人络绎不绝地登楼。他们围着星图抄录星宿经纬、核验航向。楼里从不设门禁，也没有门官收牌查验。谁来谁看，走到图前摊开纸笔便能提笔推演，既不互斥，也无需排队。

但这幅图并不是死物。

岁差更迭，流星经天，行星时有逆行。每隔数日，观星博士便要勘校天象，修正星图上的几十处刻度。

早年间刚来的一位副监官，曾试着用老规矩办事：每逢要改图，便命两名武卒敲响铜锣，大喝“闲人回避，闭阁修谱”，把正在抄录的几十名学者生生撵出楼外，将朱红大门反锁两个时辰。

结果惹出了大祸。南海运送贡丝的船队因迟迟拿不到最新的昏旦星位，在暗礁处搁了浅；算历的生员被堵在门外，算错了一季的节气。老监正闻讯大怒，把那把大铜锁狠狠扔下了山崖：“星图是给天下人看的。看图的人一息不能停，谁给你的胆子把门闭上？！”

后来接替掌案的，是一位白发老史官。老史官从不关门，甚至从不在大厅里大声说话。

他改图的法子极有意思。

每逢天象有变，老史官既不当众擦拭旧绢，也不在旧图上涂抹红墨。他独自坐在后阁的案几前，铺开一张全新的素白青绢，照着旧图细细临摹，只在有变动的星宿处画上最新勘定的赤纬与距度。这叫**静处造新**。

新图画就、印鉴盖毕，老史官端着新卷穿过大厅。他趁着两名生员低头研墨的工夫，脚步轻捷地走上中堂台座，左手解下系着旧星图的铜钩，右手同时将新星图挂上横梁。整个过程不过一瞬，轻若飞絮。

下一刻，新迈进大厅的书生抬头，看到的就是正午刚刚勘定的新星宿；他们依着新图推演，分毫不差。

然而，大厅里仍坐着七八位早在老史官挂图之前就已入座的学者。有的在核算前夜的客星轨迹，有的正就着手里的半卷旧抄本苦苦思索。老史官既没有催促他们抬头，更没有强行夺走旧图投入火盆。

他只是静静地靠在廊柱旁，手里捧着一盏热茶，看着大厅里的人。

他看的是每个人的“**离席**”。

半个时辰后，东案的算学先生推开砚台，长舒一口气，收拾竹简走出了大门——他看完了。
又过了一炷香，西侧伏案沉睡的年轻人揉了揉眼，打着哈欠跨过门槛，去庭院里打水洗脸——他也离席了。
到了申时初刻，最后一位年迈的航海老舵工终于合上罗盘手抄本，向星图拜了三拜，拄着拐杖缓缓迈下了九层高楼。

大厅的青石板上，先前在座的所有学者，已经全部走出过这间屋子。哪怕后来大厅里又涌进了一批批新读者，老史官也心里透亮：凡是曾经目睹过旧图的那批眼睛，此刻已经全部从旧图的推演中抽身了。

这时，老史官才放下茶盏，走回案前，将那张退下来的旧青绢卷起，投进炭火盆中焚化。

从始至终，没有一个人等过门锁，没有一笔推演被打断，新旧图谱交替如四时流转。

——到这儿你大概已经认出来了：老史官用的，正是 Linux 内核中至关重要的 **Read-Copy-Update (RCU)**。

### 这是什么

在现代操作系统并发控制中，*Read-Copy-Update (RCU)* 是一种为“多读极少写”场景量身打造的高性能同步机制。

传统的读写锁（*rwlock*）虽然允许多个读取者并发，但读取者之间仍需要通过原子指令修改锁的引用计数或状态位，这会在高并发 CPU 缓存之间引发剧烈的缓存行颠簸（*cacheline bouncing*）；且当写者尝试获取写锁时，要么阻塞所有读者，要么等待所有读者离开。

RCU 彻底颠覆了这个逻辑，它将一次数据更新拆分为三个独立阶段：

1. **读取端免锁访问（Read-side critical section）**：
   读取者进入临界区时无需获取任何锁，不执行任何原子总线指令（在非抢占内核中甚至只需暂时关闭抢占），直接通过指针读取共享数据。开销极低，完全零等待。
2. **拷贝并发布（Copy and Publish）**：
   写者想要修改数据结构（如链表中的一个节点）时，先将旧数据拷贝一份，在私有副本上进行修改。修改完成后，通过一次原子的指针替换（如内核中的 `rcu_assign_pointer`），将全局指针直接指向新数据。自这一刻起，所有新来的读取者都会直接访问新版本，而此时旧版本依然留在内存中供旧读者使用。
3. **宽限期与安全回收（Grace Period and Reclamation）**：
   写者不能立刻释放旧对象的内存，因为可能仍有读者正停留在旧指针上。写者必须等待一段**宽限期（Grace Period）**。在这段宽限期内，系统内每一个 CPU（或线程）都必须至少经历一次**静止状态（Quiescent State）**——例如发生一次上下文切换（说明旧读者已经退出了临界区）。当所有可能引用旧数据的读取者全部离席后，写者便可安全地将旧对象内存释放（如通过 `synchronize_rcu` 同步等待或 `call_rcu` 异步回调）。

### 为什么重要

在操作系统内核、核心路由网关与微服务基础架构中，大量数据结构呈现出“百万次读取、仅偶尔一次更新”的特征：
- **Linux 内核网络路由表与连接跟踪**：每秒钟数百万个网络数据包要查路由表转发，但路由规则几天才变一次。如果用普通锁，多核 CPU 性能会彻底锁死在总线竞争上。
- **虚拟文件系统（VFS）目录项缓存（dcache）**：高并发查找文件路径，绝大多数只需读目录树，RCU 保证了海量路径遍历的线性扩展能力。
- **分布式配置中心与服务发现（Local Cache）**：本地内存缓存微服务节点列表，上万个 RPC 请求并发读列表，后台监听线程收到变更通知后替换不可变指针，旧列表等待在途请求处理完毕后垃圾回收。

RCU 的本质是**用时钟与内存换取无锁的极致读取吞吐量**：
- 它接受在极短的时间窗口内，旧读者读到的是刚刚被替换的“旧版本”；
- 它通过将内存释放延迟到“确认无人使用”的宽限期之后，化解了读写冲突；
- 它让读取操作真正实现了与 CPU 核数完全成正比的零损耗扩展。

_隐喻对应表_

- 摘星楼中堂悬挂的青绢星图 → 被全局指针引用的共享数据结构
- 各路学者不排队自由抄写星图 → 读取端临界区（无需持锁、零开销读）
- 闭阁修谱导致南海船队搁浅 → 传统读写锁/排他锁造成的停顿与性能灾难
- 后阁铺素绢临摹并修正刻度 → 写者的“Copy”阶段（在副本上修改）
- 一瞬之间挂新星图换下旧图 → 指针原子替换与发布（`rcu_assign_pointer`）
- 新进书生直接看新星图 → 新发起的读取操作直接指向最新数据
- 挂图前已在大厅里参验的老学者 → 跨越指针替换时刻的在途旧读取事务
- 学者算完收拾竹简跨出大门 → CPU/线程经历一次静止状态（Quiescent State）
- 先前在座的所有人全部走出大厅 → 宽限期（Grace Period）结束
- 老史官将退下来的旧绢投入火盆焚化 → 延迟内存回收与释放（Reclamation / `kfree`）
</section>
<section class="en" markdown="1">
On the northern outskirts of the ancient capital stood the nine-story Stargazing Tower. At its pinnacle, within the central pavilion, hung the most sacred possession of the Astronomical Bureau: a colossal star chart painted on cerulean silk.

Every day, hundreds of seafarers, court astronomers, imperial students, and roaming caravaneers ascended the tower. They gathered beneath the great silk canopy, copying coordinate degrees and verifying navigation tracks. There was no guard at the entrance and no register to sign. Whoever arrived stepped forward, dipped a brush in ink, and began calculating at once—no gatekeeper, no queue, and no contention.

Yet the sky is never frozen.

Equinoxes shift, shooting stars blaze through the void, and wandering planets periodically retrograde. Every few days, the imperial astronomers had to re-verify celestial alignments and correct dozens of coordinate lines upon the chart.

Years ago, a newly appointed junior minister attempted to handle revisions by the old administrative manual. Whenever the chart required correction, he ordered guards to strike a bronze gong, shouting, "Clear the hall! The tower is barred for revision!" They expelled dozens of calculating scholars into the courtyard and latched the great vermillion doors shut for four continuous hours.

Disaster struck swiftly. A merchant fleet laden with imperial silk ran aground on shallow shoals because their navigators could not obtain the dusk celestial bearings; imperial students locked outside miscalculated the solar terms for an entire agricultural season. The Chief Astrologer was incandescent with fury. He took the great iron padlock and cast it down the cliffside: "The star charts belong to the realm! Those who read them must never be halted for a heartbeat! Who gave you the audacity to bar the doors?!"

The elder archivist who succeeded him never locked a door, nor did he ever raise his voice within the hall.

His manner of updating the chart was fascinating to behold.

Whenever the heavens shifted, the elder neither scraped the old silk in front of the scholars nor defaced it with crimson blotches. Instead, he retired quietly to the inner study, rolled out a pristine scroll of fresh cerulean silk, and meticulously traced the constellations from the old draft, amending only the coordinate lines for the shifted bodies. This was known as **crafting anew in stillness**.

When the new scroll was complete and stamped with the bureau seal, the elder carried the roll into the great rotunda. Seizing a moment when two seated scholars dipped their brushes into inkstones, he lightly stepped onto the central dais. With his left hand he unhooked the brass latch of the old chart, and with his right hand slid the new chart into the overhead timber groove. The entire exchange took less than a breath—silent as drifting willow catkins.

In the very next instant, scholars stepping through the courtyard archway looked up and immediately beheld the freshly calibrated constellations. Their subsequent calculations were accurate to the minute.

Yet within the hall still sat eight scholars who had taken their seats before the elder swapped the scrolls. Some were verifying the trajectory of a guest star observed the night before; others were deeply absorbed in calculations based on the half-copied notes in their hands. The elder archivist neither urged them to look up nor snatched the old scroll from their sight to cast it into the brazier.

He simply leaned against a wooden pillar with a steaming cup of tea in his palms, quietly watching the room.

What he watched for was each scholar's **departure**.

Half an hour later, a mathematician at the eastern table pushed his inkstone aside, let out a satisfied breath, gathered his bamboo slips, and walked out the door—his inquiry was finished.
A quarter-hour later, a young apprentice who had fallen asleep at the western desk rubbed his bleary eyes, yawned, and crossed the threshold to wash his face in the courtyard fountain—he too had stepped away.
By late afternoon, the last weathered sea-captain folded his compass parchment, bowed three times toward the canopy, and descended the tower stairs with his walking staff.

Every single scholar who had been in the hall when the elder swapped the charts had now passed through the courtyard gate. Even though waves of new visitors had entered in the meantime, the elder knew with absolute clarity: every set of eyes that had ever beheld the old chart was now completely disengaged from it.

Only then did the elder set down his teacup, step behind the partition, roll up the retired silk scroll, and consign it to the charcoal hearth.

From beginning to end, not a single person had waited at a locked gate, not a single calculation had been interrupted, and the constellations transitioned as seamlessly as the turning of the seasons.

By now, you have probably recognized the elder's wisdom: this is **Read-Copy-Update (RCU)** in the Linux kernel.

### What it is

In modern operating systems and concurrent architecture, *Read-Copy-Update (RCU)* is a high-performance synchronization mechanism specifically designed for read-heavy, write-rare data structures.

Traditional reader-writer locks (*rwlocks*) permit multiple concurrent readers, but readers must still execute atomic operations to increment and decrement lock reference counters. Under massive CPU core counts, this triggers brutal cacheline bouncing across processor sockets. Furthermore, when a writer arrives, it must either block all incoming readers or wait for active readers to clear, causing severe latency spikes.

RCU fundamentally upends this design by splitting updates into three clean phases:

1. **Lock-free Reader Critical Sections**:
   Readers entering an RCU critical section do not acquire locks and execute zero atomic bus instructions (in non-preemptible kernels, it merely requires disabling preemption). They traverse shared data structures via pointers with zero wait time and minimal overhead.
2. **Copy and Publish**:
   When a writer wishes to modify a data structure (such as updating a node in a linked list), it first allocates a private copy, applies the modifications to the copy, and then atomically overwrites the global pointer (e.g., via `rcu_assign_pointer`). From that exact microsecond forward, all newly arriving readers observe the new version, while existing readers safely continue traversing the old version undisturbed.
3. **Grace Period and Deferred Reclamation**:
   The writer cannot immediately reclaim or free the old memory object because concurrent readers may still hold references to it. The writer enters a **Grace Period**. During this grace period, every CPU or worker thread must pass through at least one **Quiescent State**—such as a context switch or idle loop, which guarantees that any pre-existing reader critical section has concluded. Once all CPUs have reported a quiescent state, the writer knows that no thread holds a reference to the old memory, and safely reclaims it (via `synchronize_rcu` or asynchronous `call_rcu`).

### Why it matters

In operating system kernels, routing engines, and high-throughput microservice caches, read-to-write ratios frequently exceed 100,000 to 1:
- **Linux Kernel IP Routing & Netfilter Connection Tracking**: Packets arrive at millions of frames per second, each requiring a routing table lookup. The routing table itself changes only when network topology shifts. RCU allows packet forwarding to scale linearly with CPU core counts without encountering lock contention.
- **Virtual Filesystem (VFS) Dentry Cache**: High-concurrency directory path resolution relies on RCU-walk mode to traverse file trees without locking inodes, giving Linux unmatched filesystem lookup throughput.
- **Service Discovery & Config Routing (Local Immutability)**: Local microservice proxies look up upstream service pools millions of times a minute. Background threads fetch endpoint updates, publish new immutable maps, and defer reclamation of old routing tables until in-flight requests drain.

The genius of RCU is **trading a delay in memory reclamation for uncompromised, wait-free read performance**:
- It tolerates a brief transitional window where concurrent readers may observe either the older or newer version;
- It eliminates reader-side lock contention and cache-invalidation penalties;
- It achieves near-ideal linear scalability across modern multi-core computing architectures.

_Metaphor mapping_

- The grand cerulean star chart hung in the central rotunda → The shared data structure referenced by a global pointer
- Scholars consulting and copying the chart freely without locks → Lockless read-side critical sections
- Barring the tower doors and halting trade fleets → Reader-writer lock contention and Stop-the-World latency spikes
- Painting the revised chart on fresh silk in the back room → The Copy phase (modifying an isolated duplicate)
- Unhooking the old chart and hanging the new chart in one breath → Atomic pointer replacement and publication (`rcu_assign_pointer`)
- Newly arriving scholars immediately calculating from the new chart → New readers observing the published pointer
- Scholars who were already calculating from the old chart before the swap → In-flight readers traversing the pre-existing version
- Scholars finishing their notes and stepping through the courtyard gate → CPUs passing through a Quiescent State (context switch)
- Every pre-existing scholar having departed the room → Completion of the Grace Period
- Consigning the old silk scroll to the charcoal brazier → Deferred memory reclamation (`synchronize_rcu` / `kfree`)
</section>
