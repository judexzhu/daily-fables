---
layout: fable
title: "落笔那一刻才誊抄 · The Page That Copies Only When You Write"
title_zh: "落笔那一刻才誊抄"
title_en: "The Page That Copies Only When You Write"
concept: "copy-on-write"
tags: [linux, storage, kubernetes]
illustration: /assets/art/2026-07-26-copy-on-write.jpg
---
<section class="zh" markdown="1">
山上的抄经院里只有一部《大典》，摊在中央的经台上。院里有二十位读经人，每人手里都捧着一本"自己的"典籍——可翻开看，页页都指向经台上那同一部书。老院丞早年立下规矩：给新来的人发一本典籍，不必真去誊抄整部大部头，那要耗上半年。发下去的只是一张薄薄的"目录",目录上每一条都写着"此页见经台第几番"。于是二十个人"各持一本",实际共读着同一部《大典》。发书快得像递一张纸条。

每一番纸页的角上，都刻着一个小小的_计数_:此刻有几本目录指着我。经台上那页《大典》若被十七本目录引着，角上就是"十七"。

这天午后，一位读经人读到一段律条，觉得抄错了一个字，想在页边补注。他起身走向老院丞。院丞并不慌，也绝不允许他直接在经台的公用页上落笔——那一动，二十个人读到的就全变了。院丞的做法是:取来一张空白_新页_，把那一整番原样誊抄下来，只在页边补上他要的注，然后把这张新页装进_这一个人_的目录里，让他这一条从"见经台第几番"改成"见我私藏的这一张"。经台上的原页，一根毫毛都没动;另外十九人翻开,读到的仍是旧样。原页角上的计数,从"二十"悄悄减成"十九";新页角上,则刻上"一"。

要紧的是那份"迟到的代价":发书时不抄，翻看时不抄，只有在_真正落笔要改_的那一刻，才付誊抄一页的工。而且只誊那一页,不动整部书。二十人里若始终无人动笔,这部《大典》就永远只有一份,二十本目录清清爽爽地共指着它。

院里还有一条不起眼的收尾规矩:哪一番纸页,角上的计数减到了"零"——再没有任何一本目录指着它——院丞便把它收进废纸篓,腾出地方。

——到这儿你大概已经认出来了:发下去的不是真副本,而是共享同一份底本的_引用_;只有当某人要写,才为他单独复制那一小块,别人不受影响。这就是 _copy-on-write_(写时复制,COW)。

**这是什么、为什么重要。** copy-on-write 是一种"先共享、推迟复制"的策略:多个使用者共同引用同一份底层数据,读取时大家共享、零成本;只有当某个使用者要_修改_时,系统才在那一刻为它单独复制出被改动的那一小块(通常是一个 page),其余部分继续共享。它把昂贵的复制从"创建的那一刻"推迟到"第一次写入的那一刻",而且只复制被写的粒度。这让 `fork()` 一个进程、给容器叠一层镜像、给卷打一个快照都能瞬间完成——因为一开始根本没搬数据。Linux 的 `fork` 与 overlayfs、容器镜像的分层、etcd/数据库的快照、ZFS/Btrfs 快照,底下都是这一个念头。代价是"第一次写"会比平时慢一拍(要现场复制),这就是所谓的 COW fault。

**隐喻对应表:**

- 中央经台上的那部《大典》:被共享的底层数据(物理 page)
- 二十本"目录"、发书快如递纸条:多个进程/容器各持一个廉价的_引用_,`fork`/快照瞬间完成
- 目录条目"见经台第几番":指向共享底本的引用(页表映射)
- 页角的_计数_:引用计数(reference count),记录有几方指着这一页
- 读经人想在页边补注:一次_写操作_
- 院丞不许直接在公用页落笔:共享页只读,写会触发保护
- 取空白新页、原样誊抄那一番、装进他自己的目录:写时把被改的那一页复制一份,只给写者(COW fault)
- 别的十九人翻开仍读旧样:其余使用者不受影响,隔离
- 计数减到零便收进废纸篓:引用归零时释放/回收该页
</section>
<section class="en" markdown="1">
The mountain scriptorium owns a single Great Book, laid open on a central lectern. Twenty readers live there, and each holds "his own" volume — yet open any of them and every page merely points back to that one book on the lectern. Long ago the old warden made a rule: when a newcomer arrives, don't actually copy out the whole tome (that would take half a year). Hand him only a thin _index_, each line reading "for this page, see leaf number so-and-so on the lectern." So twenty people each "hold a volume," while in truth all read the one Great Book. Handing someone a volume is as quick as passing a slip of paper.

In the corner of every leaf a small _tally_ is carved: how many indexes point at me right now. If seventeen indexes reference a given leaf of the Great Book, its corner reads "seventeen."

One afternoon a reader, poring over a passage of law, decides a character was mis-copied and wants a marginal note. He rises and goes to the warden. The warden is unhurried — but he will _never_ let the reader write on the shared lectern page directly, for that one stroke would change what all twenty people read. Instead the warden takes a blank _new leaf_, copies that whole leaf faithfully, adds the marginal note the reader wanted, and binds this new leaf into _this one person's_ index — changing his line from "see leaf so-and-so on the lectern" to "see this private leaf of mine." The original leaf on the lectern is untouched to a hair; the other nineteen still open to the old version. The original leaf's tally quietly drops from "twenty" to "nineteen"; the new leaf's corner is carved "one."

The whole point is the _deferred cost_: nothing is copied when a volume is handed out, nothing is copied when it's merely read — only at the moment someone _actually sets pen to page to change it_ is the labor of copying one leaf paid. And only that one leaf, never the whole book. If no one in the twenty ever writes, the Great Book stays a single copy forever, twenty clean indexes all pointing at it.

There is one quiet closing rule: any leaf whose tally falls to "zero" — no index points at it anymore — the warden drops into the scrap bin to reclaim the space.

— By now you have probably recognized it: what's handed out isn't a true duplicate but a _reference_ to one shared original; only when someone writes is that small block copied just for them, leaving everyone else unaffected. This is _copy-on-write_ (COW).

**What it is, and why it matters.** Copy-on-write is a "share first, copy later" strategy: many users reference the same underlying data and share it for free on reads; only when a user wants to _modify_ does the system, at that instant, copy out just the changed block (usually one page) for that user, while the rest stays shared. It defers the expensive copy from "the moment of creation" to "the moment of first write," and copies only at the granularity actually written. That's why `fork()`-ing a process, stacking an image layer for a container, or snapshotting a volume can all be instantaneous — no data was moved up front. Linux `fork` plus overlayfs, layered container images, etcd/database snapshots, ZFS/Btrfs snapshots — all rest on this one idea. The price is that the _first write_ runs a beat slower (it must copy on the spot): the so-called COW fault.

**Metaphor mapping:**

- The Great Book on the central lectern: the shared underlying data (a physical page)
- Twenty indexes, handed out as fast as passing a slip: multiple processes/containers each holding a cheap _reference_; `fork`/snapshot completes instantly
- An index line "see leaf so-and-so on the lectern": a reference/mapping to the shared original (the page-table entry)
- The _tally_ in the leaf's corner: the reference count — how many parties point at this page
- A reader wanting a marginal note: a _write_ operation
- The warden forbidding writing on the shared page: shared pages are read-only; a write traps
- Taking a blank leaf, copying that one leaf, binding it into his own index: on write, copy the changed page for the writer alone (the COW fault)
- The other nineteen still reading the old version: other users are unaffected — isolation
- A tally falling to zero, then into the scrap bin: when the reference count hits zero, the page is freed/reclaimed
</section>
