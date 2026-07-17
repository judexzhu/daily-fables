---
layout: fable
title: "断桥之后的清源寺 · Qingyuan Temple After the Broken Bridge"
title_zh: "断桥之后的清源寺"
title_en: "Qingyuan Temple After the Broken Bridge"
concept: "quorum and split-brain"
tags: [distributed-systems]
---
<section class="zh" markdown="1">
清源寺住着九位长老。寺里有一条铁律：任何一件大事——收徒、动用粮仓、修改戒律——都要写进那本唯一的《总账》里，而且*必须有超过一半的长老当场按下手印*，这一笔才算数。八九不离十的多数不行，正好一半也不行，必须是"过半"，也就是至少五个手印。

平日里这规矩显得囉嗦。有长老抱怨：九个人凑齐五个手印，光是找人就得半天。老方丈只淡淡回一句："慢一点没关系，怕的是两本账。"

那年夏天山洪冲垮了寺前的石桥。桥一断，寺子被生生劈成两半：东院留下五位长老，西院困住四位，隔着暴涨的河水,谁也喊不到谁。

西院这边急坏了。有人说："粮仓在我们这边，先开仓放粮吧！"另一位却拦住他："我们只有四个人。按铁律，四个按不了这一笔。"他们数了又数，怎么也凑不出过半。于是西院做了一个看似窝囊的决定——*什么大事都不办*，只照旧念经、扫地、烧饭,把《总账》原封不动地锁进柜子。他们宁可"暂时不能办事"，也不敢自己另起一本账。

东院这边有五位。五，正好过半。于是东院照常收徒、开仓、议事，每一笔都凑齐了手印，《总账》一页页往下写。

一个月后洪水退去，桥修好了。两院长老碰头,翻开各自的账本——西院那本停在洪水那天,一字未添;东院那本记满了这一个月的事。两本一合,毫无冲突,天衣无缝地接了起来。老方丈捻着胡须说:"你们看,幸亏当初西院忍住了。要是他们也自作主张记了账,今天我们就有两本谁也不认谁的《总账》,这寺就散了。"

——到这儿你大概已经认出来了：这座清源寺讲的,其实是分布式系统里的 _quorum_（法定多数）机制,以及它如何防止 _split-brain_（脑裂）。

_概念解释_
在 _etcd_、_Raft_、_ZooKeeper_ 这类需要强一致的系统里,一个由 N 个节点组成的集群,要提交任何一次写入,都必须得到 _quorum_（严格过半,即 ⌊N/2⌋+1 个节点）的确认。这条规则的精妙之处在于:一个集群无论怎么被网络分区切开,*最多只有一个分区能同时握有过半节点*。握有多数的那一半可以继续写;不足多数的那一半会主动停止写入,进入只读/不可用状态。这样就保证了永远不会出现两个分区各写各的、事后无法合并的"两本账"——也就是 _split-brain_。这本质上是 _CAP_ 里的取舍:宁可牺牲少数派的 _availability_,也要守住 _consistency_。这也是为什么集群节点数一般取奇数(3、5、7):5 个节点能容忍 2 个故障,6 个节点同样只能容忍 2 个,多出来的那个节点白白增加成本却不提升容错。

_隐喻对应表_

- 九位长老 → 集群里的 N 个节点(如 etcd 的 5 个成员)
- 唯一的《总账》 → 复制状态机的 _replicated log_ / 一致的数据
- "必须过半按手印" → _quorum_ 写入规则(⌊N/2⌋+1)
- 山洪冲断石桥 → _network partition_(网络分区)
- 东院五人,继续记账 → 握有 _quorum_ 的多数派分区,可继续接受写入
- 西院四人,锁账只读 → 少数派分区主动降级,牺牲 _availability_ 保 _consistency_
- 两本账合并无冲突 → 分区愈合后日志一致,无 _split-brain_
- 老方丈"怕的是两本账" → 系统设计者对 _split-brain_ 的核心恐惧
- 取奇数长老更省事 → 集群用奇数节点以最优化容错成本
</section>
<section class="en" markdown="1">
Nine elders lived at Qingyuan Temple. The temple had one iron rule: any weighty matter — admitting a disciple, opening the grain store, amending the precepts — had to be written into the single _Ledger_, and it counted only if *more than half of the elders pressed their handprint on it in person*. Almost-half would not do. Exactly half would not do. It had to be _more_ than half — at least five prints.

In ordinary times the rule seemed tedious. One elder grumbled that just rounding up five prints out of nine could eat half a day. The old abbot only replied, "Slow is fine. What I fear is two ledgers."

That summer a flood tore out the stone bridge in front of the temple. With the bridge gone, the temple was split clean in two: five elders on the east side, four trapped on the west, the swollen river between them too loud to shout across.

The west side panicked. "The grain store is on our side — let's open it now!" one urged. But another stopped him: "There are only four of us. By the rule, four cannot press this entry." They counted and recounted; they could never reach more than half. So the west side made what looked like a spineless decision — *they transacted nothing at all*. They simply chanted, swept, and cooked as before, and locked the _Ledger_ away untouched. They would rather "be unable to act for now" than dare start a second ledger of their own.

The east side had five. Five is more than half. So the east carried on — admitting disciples, opening the store, holding council — every entry gathering its full set of prints, the _Ledger_ filling page by page.

A month later the water fell and the bridge was rebuilt. The elders of both sides met and opened their books. The west's book stopped on the day of the flood, not a word added; the east's was full of a month's affairs. Laid together, the two joined seamlessly, without a single conflict. Stroking his beard, the abbot said, "You see — thank heaven the west held back. Had they kept their own ledger too, we would now have two _Ledgers_ that refuse to recognize each other, and this temple would be finished."

— By now you've probably recognized it: Qingyuan Temple is really about _quorum_ in distributed systems, and how it prevents _split-brain_.

_Concept_
In strongly-consistent systems like _etcd_, _Raft_, and _ZooKeeper_, a cluster of N nodes can commit any write only with confirmation from a _quorum_ — a strict majority, ⌊N/2⌋+1 nodes. The elegance is this: no matter how a _network partition_ slices the cluster, *at most one partition can hold a majority at the same time*. The side holding the majority keeps writing; the side short of a majority voluntarily stops accepting writes and goes read-only / unavailable. That guarantees you never get two partitions each writing their own history that can't later be merged — i.e. _split-brain_. It is fundamentally the _CAP_ trade-off: sacrifice the minority's _availability_ to preserve _consistency_. It's also why cluster sizes are usually odd (3, 5, 7): 5 nodes tolerate 2 failures, and 6 nodes also tolerate only 2 — the extra node adds cost without adding fault tolerance.

_Metaphor mapping_

- Nine elders → the N nodes in the cluster (e.g. etcd's 5 members)
- The single _Ledger_ → the _replicated log_ / consistent data of the state machine
- "must have more than half of the prints" → the _quorum_ write rule (⌊N/2⌋+1)
- Flood breaking the bridge → a _network partition_
- East side of five, still writing → the majority partition holding _quorum_, still accepting writes
- West side of four, locked and read-only → the minority partition self-demoting, trading _availability_ for _consistency_
- The two ledgers merging without conflict → logs consistent after the partition heals, no _split-brain_
- Abbot's fear of "two ledgers" → the designer's core fear of _split-brain_
- Preferring an odd number of elders → clusters using odd node counts to optimize fault-tolerance cost
</section>
