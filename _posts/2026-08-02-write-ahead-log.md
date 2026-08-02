---
layout: fable
title: "先落墨，后起锅 · The Book Before the Fire"
title_zh: "先落墨，后起锅"
title_en: "The Book Before the Fire"
concept: "Write-Ahead Log"
tags: [distributed-systems, storage, etcd]
illustration: /assets/art/2026-08-02-write-ahead-log.jpg
---
<section class="zh" markdown="1">
海港边有家吵闹的小馆子，生意好到夜里灶上八口锅同时冒烟。掌台的是位叫阿霜的女人，她定了一条铁规矩，全店没人敢破：**任何一道菜下锅之前，先把它写进那本账**。

那是一本厚重的账簿，用铁链拴在传菜口的木台上，谁也搬不走。客人报了菜，阿霜不吆喝、不动手，先蘸墨，在**下一条空行**上一笔一笔写清楚：第几桌、什么菜、几分熟。写完，等墨迹干透，她才扭头朝灶上喊。规矩听着笨——客人都催了还在写字——可她从不通融："墨没干，这道菜就不算数。"菜端出去、客人确实收下了，她再回来在那一行边上补一个小小的"讫"字。

那年秋末的一夜，一阵狂风灌进来把所有油灯全扑灭了，灶上还腾着火星，众人手忙脚乱冲进后院躲。等风过、重新点灯回到店里，一片狼藉：半熟的鱼、切到一半的葱、没人记得清刚才到底在做哪几道菜——**除了那本账**。阿霜翻开它，从最后一个"讫"字往下读，一行行照着重做：这条已上、跳过；这条没上、补齐。没有一道菜漏掉，也没有一道菜重复端给客人。天大的乱子，一本账就理清了。

也正因为她只往后添、从不涂改，这本账越写越厚。所以每天打烊、确认昨夜每一道菜都收了尾，她就利落地划一道横线，翻开崭新的一页从头记起——账簿因此永远不会厚到一千页翻不动。

——到这儿你大概已经认出来了：这本"先落墨、后起锅"的账簿，就是数据库与 etcd 里的 *write-ahead log*（WAL，预写日志）。

它是什么：在真正修改数据之前，先把"我打算做什么"这条记录**顺序追加**写进一个只增不改的日志，并确保它落到了稳定存储上（`fsync`），这一步做完，这笔操作才算 *committed*。真正的数据结构可以慢慢改；就算此刻断电，重启后只要**重放**（replay）日志，就能把状态恢复到崩溃前的一致点。

它为什么重要：磁盘写入随时可能被断电、宕机打断。如果你一边改数据一边指望它别出事，崩溃后就会留下一堆"改了一半"的烂摊子。WAL 用一个简单的次序保证换来了 *durability* 和 *crash recovery*——先记意图、再改数据，恢复时照着日志重做即可。etcd（也就是每个 Kubernetes/OpenShift 集群的心脏）正是靠 WAL，才敢承诺"确认写入的数据，断电也不丢"。

隐喻对应表：

- 那本铁链拴住的账簿 → *write-ahead log*（日志本身）
- 先落墨、后起锅的铁规矩 → 预写规则：改数据前，先把意图持久化到日志
- 只写下一条空行、从不涂改 → 顺序 append-only 写入
- 等墨迹干透才算数 → `fsync` / *durability*：日志落盘后，操作才算 committed
- 狂风扑灭油灯 → 崩溃 / 断电
- 从最后一个"讫"字往下重做 → 恢复时 *replay*（redo）日志
- 那个"讫"字 → *checkpoint*（检查点）
- 每天划线、翻开新页 → checkpoint 后 *truncate* / 压缩日志，控制体积
- 没有一道菜重复端出 → 重放的 *idempotency*（幂等）
</section>
<section class="en" markdown="1">
There was a loud little eatery down by the harbor, busy enough that on a good night eight pans smoked on the stove at once. The woman who ran the pass was named Ah-Shuang, and she kept one iron rule no one in the shop dared break: **before any dish touches a pan, it goes into the book first.**

It was a heavy ledger, chained to the wooden counter at the pass so no one could carry it off. When a guest ordered, Ah-Shuang did not shout and did not move — she dipped her brush and wrote it out, stroke by stroke, on the **next empty line**: which table, which dish, how well done. Only when the ink had dried did she turn and call it to the stove. The rule sounded foolish — guests grumbling while she wrote — but she never bent it: "If the ink isn't dry, the dish doesn't count." And when a plate went out and the guest had truly taken it, she came back and added a small mark — *done* — beside that line.

One night late that autumn a gust tore through and snuffed every oil lamp at once. Sparks still hissed on the stove; everyone scrambled out into the back yard. When the wind passed and the lamps were lit again, the kitchen was chaos: half-cooked fish, scallions cut halfway, no one able to recall which dishes had even been underway — **except the book.** Ah-Shuang opened it, read down from the last *done* mark, and remade each line in order: this one delivered, skip it; this one not, finish it. Not a dish was lost, and not a dish was sent to a guest twice. A calamity, untangled by a single ledger.

And because she only ever added lines and never scratched anything out, the book grew thicker and thicker. So each night at closing, once every dish from the evening was confirmed finished, she drew a clean line across the page and started fresh on a new one — and so the ledger never swelled to a thousand unturnable pages.

— By now you've probably recognized it: this "ink before the pan" ledger is the *write-ahead log* (WAL) of databases and etcd.

What it is: before actually modifying the data, you first **append**, in order, a record of *what you intend to do* into a log that only grows and is never rewritten, and you make sure it has reached stable storage (`fsync`). Only once that is done is the operation *committed*. The real data structures can be updated at leisure; even if the power fails this instant, on restart you simply **replay** the log to recover the state to a consistent point just before the crash.

Why it matters: a disk write can be interrupted at any moment by a power cut or a crash. If you mutate your data and merely hope nothing goes wrong, a crash leaves you with a pile of half-applied wreckage. The WAL trades one simple ordering guarantee for *durability* and *crash recovery* — record the intent, then change the data; on restart, redo from the log. etcd — the heart of every Kubernetes/OpenShift cluster — relies on exactly this to promise that acknowledged writes survive a power loss.

Metaphor map:

- The chained ledger → the *write-ahead log* itself
- The iron rule, ink before the pan → the write-ahead rule: persist the intent to the log before mutating data
- Writing only the next empty line, never scratching out → sequential, append-only writes
- Counting only once the ink is dry → `fsync` / *durability*: an op is committed once its record is on stable storage
- The gust snuffing the lamps → a crash / power loss
- Remaking from the last *done* mark down → *replay* (redo) of the log on recovery
- The *done* mark → a *checkpoint*
- Drawing a line, a fresh page each night → *truncating* / compacting the log after a checkpoint to bound its size
- No dish sent out twice → the *idempotency* of replay
</section>
