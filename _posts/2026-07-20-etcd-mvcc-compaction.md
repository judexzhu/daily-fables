---
layout: fable
title: "守册人的号码 · The Keeper Who Never Crossed Anything Out"
title_zh: "守册人的号码"
title_en: "The Keeper Who Never Crossed Anything Out"
concept: "etcd MVCC and compaction"
tags: [distributed-systems, kubernetes, storage]
illustration: /assets/art/2026-07-20-etcd-mvcc-compaction.jpg
---
<section class="zh" markdown="1">
湖边的小镇有一位守册人。镇上每一桩变动——谁家添了一头牛、谁把船卖给了谁、井水位涨了还是落了——都要来告诉他。可这守册人有个奇怪的规矩：他从不涂改。

他手里那本册子，每记一笔就翻到新的一行，行头写一个只增不减的号码：第 4001 号、第 4002 号、第 4003 号……哪怕是同一口井的水位，昨天记过、今天又变了，他也不去擦掉旧的那行，而是在新的一行、用新的号码，把新水位再写一遍。旧的仍在，新的也在，只是号码更大。

于是奇妙的事情发生了。有个走商回来，问："我离开那天——大约第 3500 号那会儿——镇上是什么光景？"守册人不慌不忙，顺着号码往回翻，把每口井、每条船在第 3500 号那一刻的样子，一字不差地念给他听。仿佛时间被钉在了那一页上，之后的变动都不算数。

还有个信差，天天来只问一句："从我上次听到的第 3500 号往后，都发生了什么？"守册人便从 3501 号一路念下去，一笔一笔、按号码顺序，绝不跳、绝不乱。信差听完，记下自己听到的最后一个号码，明早再来接着往后听。

只是册子越写越厚。守册人的架子终有堆不下的一天。于是每隔一阵，他会挑一个号码——比方说第 3800 号——把这之前的旧行整段裁掉、烧作引火，只留下"每样东西在 3800 号时的最新模样"，以及之后的全部新行。当下的账目一分不差，架子却轻了大半。

麻烦的是那走商。他若再回来，说"给我念第 3500 号那天的光景"，守册人只能摊手："对不住，那些页我已经裁了——你要的号码，太旧了。"那个天天来的信差也一样：他若偷了懒、隔太久没来，等再上门时，上次那个号码早被裁进灰里，守册人只能说："断了。你从头把现在的账目重抄一遍吧，我们从今天的号码重新开始。"

——到这儿你大概已经认出来了：这位从不涂改、只按号码往上叠的守册人，就是 etcd 的 *MVCC*（多版本并发控制）；而他定期裁掉旧页、给架子腾地方的那一刀，就是 *compaction*。

**这是什么。** etcd 是 Kubernetes / OpenShift 的后端存储。它对每一次写入都不做"原地覆盖"，而是给整个键空间分配一个全局单调递增的 *revision* 号，把新值作为一个新版本追加进去。这带来三个好处：你可以在某个 revision 上做一致的快照读，看到那一刻的完整世界；你可以从某个 revision 起 *watch*，按顺序补齐之后的所有变更——这正是 Kubernetes 里各控制器 list-then-watch 的地基；历史也天然可追溯。代价是旧版本会不断堆积、撑大数据库。*compaction* 就是定期丢弃某个 revision 之前的历史版本（只保留每个键在该点的最新值），把空间还回来。但被压实的历史找不回来了：向 etcd 请求一个早于 compaction 点的 revision，会收到 `mvcc: required revision has been compacted`；一个落后太多的 watcher 也会被断线，必须重新 list 拿到当前 revision、再从那里接着 watch。

**隐喻对应表：**

- 守册人那本只增不改的册子 → etcd 的 *MVCC* 键值存储
- 每行行头只增不减的号码 → *revision*（全局单调递增的版本号）
- 同一口井的水位在新行重写、旧行仍在 → 写入不覆盖，而是追加一个新版本
- 走商问"第 3500 号那天的光景" → 在指定 revision 上的一致快照读
- 天天来、从上次号码往后接着听的信差 → 从某个 revision 起的 *watch*，按序补齐增量
- 信差记下"最后听到的号码" → watcher 保存的 *resourceVersion*
- 守册人裁掉某号之前的旧页、烧作引火 → *compaction*，回收旧版本占用的空间
- 走商要的号码"太旧、页已裁掉" → `required revision has been compacted`
- 信差隔太久、上次号码已被裁，只能重抄现账 → watcher 落后于 compaction 点被断线，必须重新 list-then-watch
</section>
<section class="en" markdown="1">
A small town by the lake kept a Keeper of the Register. Every change in town — a household gaining a cow, a boat changing hands, a well rising or falling — was brought to him. But the Keeper had a strange rule: he never crossed anything out.

For each thing he was told, he turned to a fresh line, and at the head of that line he wrote a number that only ever climbed: entry 4001, entry 4002, entry 4003. Even when it was the same well whose level he had recorded yesterday and that had changed again today, he did not erase the old line. He wrote the new level on a new line, under a new number. The old stayed; the new stayed; only the number was larger.

And so something wonderful became possible. A travelling merchant returned and asked, "The day I left — around entry 3500 — what did the town look like?" Unhurried, the Keeper walked the numbers backward and read out, exactly, how every well and every boat had stood at the moment of entry 3500. It was as if time had been pinned to that page, and nothing after it counted.

There was also a courier who came each day with a single question: "Since the last number I heard, entry 3500, what has happened?" The Keeper read on from 3501, one line at a time, in strict order of the numbers, never skipping, never scrambling. When the courier had heard it all, he wrote down the last number he had reached, and came back the next morning to carry on from there.

But the Register grew thicker. The Keeper's shelves would one day have no more room. So every so often he chose a number — say entry 3800 — and cut away the whole run of older lines before it, burning them for kindling, keeping only "the latest shape of each thing as of 3800" and every newer line after. The present accounts were untouched to the letter, yet the shelves were lighter by half.

The trouble was the merchant. If he came back and said, "Read me the day of entry 3500," the Keeper could only spread his hands: "I'm sorry — those pages I have already cut. The number you want is too old." And the same for the daily courier: if he grew lazy and stayed away too long, the number he had last heard would be ash by his return, and the Keeper could only say, "The thread is broken. Copy out the present accounts from scratch, and we will begin again from today's number."

— By now you have probably recognised him: the Keeper who never crosses anything out, only stacking numbers upward, is etcd's *MVCC* (multi-version concurrency control); and the periodic cut that clears the shelves is *compaction*.

**What it is.** etcd is the backing store of Kubernetes / OpenShift. It never overwrites a write in place. Instead it assigns the whole keyspace a globally monotonically increasing *revision* number and appends the new value as a new version. This buys three things: you can do a consistent snapshot read at a given revision, seeing the whole world as of that instant; you can *watch* from a given revision and receive every later change in order — the very foundation of the list-then-watch pattern every Kubernetes controller relies on; and history is naturally auditable. The price is that old versions pile up and bloat the database. *compaction* periodically discards historical versions older than a chosen revision (keeping only each key's latest value as of that point) and hands the space back. But compacted history is gone: ask etcd for a revision older than the compaction point and you get `mvcc: required revision has been compacted`; a watcher that has fallen too far behind is likewise cut off and must re-list to obtain the current revision, then watch again from there.

**Metaphor mapping:**

- The Keeper's append-only, never-erased Register → etcd's *MVCC* key-value store
- The ever-climbing number at the head of each line → the *revision* (globally monotonic version number)
- The same well rewritten on a new line while the old line survives → a write appends a new version rather than overwriting
- The merchant asking "what did entry 3500 look like" → a consistent snapshot read at a specified revision
- The daily courier resuming from the last number he heard → a *watch* from a given revision, replaying changes in order
- The courier writing down "the last number heard" → the *resourceVersion* a watcher stores
- The Keeper cutting and burning lines older than a chosen number → *compaction* reclaiming space from old versions
- The merchant's number "too old, those pages are cut" → `required revision has been compacted`
- The courier who stayed away too long and must recopy the present → a watcher that fell behind the compaction point, forced to re-list-then-watch
</section>
