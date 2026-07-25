---
layout: fable
title: "灯墙上的记性 · The Wall That Remembers Faces"
title_zh: "灯墙上的记性"
title_en: "The Wall That Remembers Faces"
concept: "Bloom filter"
tags: [distributed-systems, storage]
illustration: /assets/art/2026-07-25-bloom-filter.jpg
---
<section class="zh" markdown="1">
夜市的木门后，坐着一位守门的老妇人。她管着一整面灯墙——上百盏小纸灯，密密麻麻钉在木格子里，天黑时全是暗的。

她的记性早就不中用了，认不得每一张脸。可她有个法子。

每个进夜市的人，手里都攥着一块刻花的木牌。老妇人面前坐着三位蒙眼的老读牌人，谁也看不见灯墙。客人把木牌递过去，三个人各自摸一遍那些刻痕，然后各自抬手，往灯墙上一指——第一位指东北角一盏，第二位指正中一盏，第三位指最下一排一盏。

规矩是死的：**同一块木牌，同一位读牌人，永远指同一盏灯**。三个人的手法各不相同，所以三根手指落在三个不同的角落。

放人进去的时候，老妇人就照着三根手指，把那三盏灯点亮。

到了要问"这人从前来过没有"，她不翻名册——她根本没有名册。她只让三位读牌人再摸一遍木牌，再指一次。然后她抬眼看那三盏灯：

- 若三盏**全亮**，她就笑着说："你怕是来过的，进吧。"
- 若哪怕**有一盏还暗着**，她斩钉截铁："你这张脸，绝没进过这道门。"

这后半句她从不会错。因为读牌人手法固定，你若真进来过，你那三盏灯当年必被点亮，绝不会自己灭。**说"没来过"，就一定没来过。**

可前半句会骗人。夜市越热闹，点亮的灯越多。偶尔来个头一回的生客，命也真巧——他那三盏灯，早被三个素不相识的先客一人点了一盏，凑巧全亮了。老妇人便热络地招呼："回来啦。"其实两人从未谋面。

灯越点越密，这种认错的次数就越多。

有人劝她：夜深了，把没用的灯吹灭几盏吧。老妇人摇头——她吹不得。任何一盏灯，说不定正记着另一位老主顾的三分之一张脸。灭一盏，就抹掉了别人的记性。灯只进不出。

——到这儿你大概已经认出来了：这面只肯说"来过"或"绝没来过"、宁可错认也绝不漏认、点了就再吹不灭的灯墙，是一只 *Bloom filter*。

**这是什么。** Bloom filter 是一种极省空间的概率型集合，用来快速回答"某个元素在不在集合里"。它不存元素本身，只留一排比特（灯的亮暗）。加入元素时，用 k 个 *hash function*（三位读牌人）把它映射到 k 个位置并置 1；查询时看这 k 位是否全为 1。它的关键性质是**单边错误**：答"不在"永远可信（no false negatives），答"在"则可能是假警报（false positives）。装的元素越多、置 1 的位越密，误判率越高。标准 Bloom filter 还**不能删除**——因为一个比特可能被多个元素共享。

**为什么重要。** 当"完整名册"太大、放不进内存或查一次太慢时，Bloom filter 用一丁点空间先挡掉绝大多数"一定不在"的查询：数据库用它跳过不必存在的磁盘读（LSM-tree 的每层 *SSTable* 都带一个），CDN 和缓存用它判断"这东西我肯定没存过"，分布式系统用它在网络两端便宜地比对集合。答错的代价只是白跑一趟去查真表，答对省下的却是海量无谓的查找。

**隐喻对应表：**

- 灯墙（上百盏灯，起初全暗）→ 一排比特位，初始全 0
- 一盏灯亮/暗 → 一个 bit 为 1/0
- 三位蒙眼读牌人 → k 个 hash function
- 同一木牌、同一读牌人永远指同一盏 → hash 的确定性
- 放人时点亮三盏灯 → 插入元素：把 k 个位置置 1
- 三盏全亮 = "怕是来过" → 查询命中：可能存在
- 有一盏暗 = "绝没进过" → 查询未命中：一定不存在（no false negatives）
- 生客的三盏恰被别人凑齐点亮、被错认 → 哈希碰撞导致的 false positive
- 灯越点越密、错认越多 → 填充率上升，误判率升高
- 吹不灭任何一盏灯（可能记着别人）→ 标准 Bloom filter 无法删除元素
</section>
<section class="en" markdown="1">
Behind the wooden gate of the night market sits an old gatekeeper woman. She tends an entire wall of lamps — a hundred-odd little paper lanterns nailed into a wooden grid, all dark when night falls.

Her memory failed her long ago; she can't recognize a single face. But she has a trick.

Everyone entering the market carries a small carved wooden token. Before her sit three blindfolded token-readers, none of whom can see the wall. A guest hands over the token; each reader runs their fingers over the carvings and, on their own, raises an arm to point at the wall — the first at a lantern in the northeast corner, the second at one dead center, the third at one along the bottom row.

The rule is iron: **the same token, read by the same reader, always points at the same lantern.** Each reader's method differs, so the three fingers land in three different corners.

To admit a guest, the woman lights exactly those three lanterns the fingers name.

When she must ask "has this person been here before?", she opens no ledger — she keeps none. She simply has the three readers feel the token again and point again. Then she looks up at those three lanterns:

- If all three are **already lit**, she smiles: "You've likely been here. Come in."
- If even **one is still dark**, she says flatly: "This face has never crossed my gate."

That second sentence is never wrong. Because the readers are consistent, if you truly had entered, your three lanterns would have been lit back then and never go out on their own. **When she says "never been," it's certain.**

But the first sentence can lie. The busier the market, the more lanterns burn. Now and then a true first-timer arrives with unlucky timing — his three lanterns each happen to have been lit already, one apiece, by three earlier strangers he's never met. The woman greets him warmly: "Welcome back." They have never laid eyes on each other.

The denser the lit lanterns grow, the more often she mistakes a stranger for a regular.

Someone suggests: it's late, blow out a few useless lanterns. She shakes her head — she can't. Any one lantern might be holding a third of some other regular's face. Snuff it, and you erase another's memory. Lanterns go on, never off.

— By now you've probably recognized it: this wall that will only ever say "been here" or "never been," that would rather mistake than miss, whose lamps once lit can never be blown out, is a *Bloom filter*.

**What it is.** A Bloom filter is an extremely space-efficient probabilistic set for quickly answering "is this element in the set?" It stores no elements themselves, only a row of bits (lanterns lit or dark). To insert, k *hash functions* (the three readers) map the element to k positions and set them to 1; to query, you check whether all k bits are 1. Its defining property is **one-sided error**: a "no" is always trustworthy (no false negatives), while a "yes" may be a false alarm (false positives). The more elements you add and the denser the set bits, the higher the false-positive rate. A standard Bloom filter also **cannot delete** — because one bit may be shared by many elements.

**Why it matters.** When the "full ledger" is too large to hold in memory or too slow to consult, a Bloom filter uses a sliver of space to reject the vast majority of "definitely-not-here" queries up front: databases use it to skip disk reads for keys that can't exist (every *SSTable* in an LSM-tree carries one), CDNs and caches use it to decide "I've surely never stored this," and distributed systems use it to compare sets cheaply across a network. The cost of a wrong "yes" is only a wasted trip to the real table; the payoff of a right "no" is skipping an ocean of pointless lookups.

**Metaphor mapping:**

- The lantern wall (a hundred lamps, all dark at first) → a row of bits, initialized to all 0
- A lantern lit/dark → a single bit as 1/0
- The three blindfolded readers → the k hash functions
- Same token, same reader, always the same lantern → determinism of the hash
- Lighting three lanterns on admission → inserting an element: set k positions to 1
- All three lit = "likely been here" → query hit: possibly present
- One still dark = "never crossed" → query miss: definitely absent (no false negatives)
- A newcomer whose three lanterns were coincidentally lit by others, mistaken as a regular → a false positive from hash collisions
- Denser lanterns, more mistakes → rising fill ratio raises the false-positive rate
- Unable to blow out any lantern (it may hold another's memory) → a standard Bloom filter cannot delete elements
</section>
