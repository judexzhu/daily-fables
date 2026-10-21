---
layout: fable
title: "双巢之谷 · The Valley of Two Nests"
title_zh: "双巢之谷"
title_en: "The Valley of Two Nests"
concept: "Cuckoo Hashing"
tags: [memory, performance]
youtube_id: "nVWSthAsKtY"
illustration: /assets/art/2026-10-21-cuckoo-hashing.jpg
---

<section class="zh" markdown="1">

深秋，雁声掠过云雾山的脊线。山坳里有一条谷，南来北往的候鸟都认得它——双巢谷。

谷里有东、西两片树林，树干上藏着上千个树洞，是历代驿长经营的鸟驿。管谷的是灰伯，一个提灯在林间走了四十年的老驿长。

双巢谷的规矩，从灰伯的师父那辈就立下了：每只落脚的鸟，登记时领两张巢牌——一张指向东林的一个洞，一张指向西林的一个洞。巢牌由两位老登记人各自按鸟的羽纹掐算，一人一套手法，谁也算不出第三个洞。而铁律是：**每只鸟，任一时刻，只住在两个洞之中的一个。**

规矩换来一样别的山谷没有的东西。灰伯找鸟，从不翻册子，也从不喊人满山搜：先看第一张牌的洞；不在，再看第二张。两步之内，鸟必在。别谷的驿站翻遍整片林子也未必寻得着一只夜宿的雀；双巢谷的灯笼，照两个洞口就够。

——这就是双巢谷的名声：**找一只鸟，最多看两个洞。**

名声的代价，藏在夜里。

新客到来，先去第一张牌的洞。洞若空着，皆大欢喜。可秋迁正盛，洞常常是满的。于是便动用谷里那门不好看的旧俗——**让巢**：新鸟进洞，把原主挤出去。

被挤出去的鸟不吵不闹（好吧，其实吵醒了半座山），抖抖羽毛，飞往自己第二张牌的洞。若那里也满着呢？那就再让一次——把那一家的原主挤出去，自己住下。被挤的再飞往自己的另一个洞……一环扣一环，鸟赶鸟，洞换洞，整条山脊半夜都是扑翅声。

大多数夜晚，链条十几跳之后就平息了——总有一只鸟，发现自己的另一个洞恰好空着。链条的尾巴落进空洞里，山谷重新安静。

可有那么一夜，链条绕回来了。

那是秋迁最盛的一个晚上。新客让巢，原主挪洞，一环、两环、十几环——最后被挤出的那只鸟，兜兜转转，翅膀一斜，落回了链条开头的那个洞。洞里坐着的，正是一切起因的那位新客。挤它？它一挪，又得去挤链条上的第二家……山谷炸了锅：鸟赶鸟赶成一圈，谁也安顿不下来，扑翅声从半夜响到天明。

灰伯提灯出屋，看了半炷香，转身取来三样东西。

第一样是旧规矩，这回钉在了谷口的木牌上：**一次让巢，链条最多跳一百跳。**一百跳之内安顿不下来的，不许再挤——链条那么长，必定是在绕圈；绕圈，是没有天亮的。

第二样在谷中央。那里立着一棵五人合抱的老槐，树腰上凿着一排客洞，平日封着。灰伯揭了封条：链条跳到头、实在无处可去的鸟，今夜宿老槐。客洞不多，统共几个，却专收这种"两洞皆满、链条到顶"的鸟。册子上添一笔：查它，先看两张巢牌，再看老槐。

第三样，是灰伯早写好、一直压在箱底的一纸告示：**迁巢日。**客洞若也满了——秋迁年年更盛，这一天总会来——全谷搬去后山那片更密的林子，树洞多出一倍，所有鸟重新领牌、重新安巢。迁巢日不收新客，鸡飞狗跳一整天。可一年摊不上几回；摊到每只鸟头上，搬家不过是每季抖一回翅膀的功夫。

从此双巢谷有了后半句名声：挤是挤了点，可**册子上从来没有一只鸟是找不到的**。鸟要走，更是干脆——巢空着就空着，册子一笔都不用改。

——到这儿你大概已经认出来了：这就是*布谷鸟哈希（cuckoo hashing）*。而那门"挤走原主"的旧俗，正是布谷鸟的天性：把蛋下在别人的巢里，把原主挤出去。算法的名字，就是这么来的。

### 这是什么

*布谷鸟哈希*是 Rasmus Pagh 与 Flemming Friche Rodler 在 2001 年提出的开地址哈希表。每个键由两个哈希函数 *h₁*、*h₂* 指定两个候选桶，任一时刻只住在其中一个。插入时若两个桶都满，就把其中一个旧键踢出去、改住它的另一个桶——被踢的可能再踢下一位，形成*驱逐链（displacement chain）*。链可能绕成环、令插入失败，因此设有跳跃上限；链条到顶仍无处安身的键，收进小小的 *stash* 备用桶；连 stash 也满，就 *rehash* 扩容重建。它的招牌性质是：查找与删除最坏情形 *O(1)*——最多看两个桶（外加 stash），再无别处可看。

### 为什么重要

链式哈希表的平均查找是 O(1)，最坏却可能拖到 O(n)：一个桶排起长队，谁也说不准要数多远。*布谷鸟哈希*用"每个键最多两处"换来**确定性的最坏 O(1) 查找与删除**——对延迟敏感的路径（路由表、网络设备、包处理）这是硬保证，而且每键两处的内存布局缓存友好、利于硬件实现与并行查找。它的衍生*布谷鸟过滤器（cuckoo filter）*是 Bloom filter 的现代替代：空间更省，还支持删除。它也是一课教科书级的取舍：用更贵的插入（驱逐链、偶发 rehash）买确定的查找——读写比例偏向读时，这门买卖才划算。

_隐喻对应表_

- 每只鸟的两张巢牌 → 每个键的两个哈希桶 *h₁(x)* 与 *h₂(x)*
- 一鸟任一时刻只住一巢 → 每个键任一时刻只存在于一个桶
- 找一只鸟最多看两个洞 → 最坏情形 *O(1)* 的查找与删除
- 两位登记人各掐各的、算不出第三个洞 → 两个独立的哈希函数
- 新鸟挤走原主 → 插入冲突时的驱逐
- 被挤的鸟飞往另一洞、再挤下一位 → *驱逐链（displacement chain）*
- 链条绕回源头的那一夜 → 驱逐成环，插入失败
- 百跳之约 → 单次插入的最大跳跃上限
- 老槐客洞 → *stash* 备用桶
- 迁巢日：更大的树林、重新领牌 → *rehash* 扩容重建
- 迁巢日不收新客 → 扩容期间的写入停顿
- 鸟走巢空、册子不动 → 删除零成本，无需善后

</section>

<section class="en" markdown="1">

Deep autumn; goose-cries skim the ridgeline of the Cloud-Mist Mountains. In the fold of the hills lies a valley every migrating bird knows by name: Two-Nest Valley.

Two groves grow there, east and west, their trunks honeycombed with a thousand hollows — a waystation on the long southern road, run by stationmasters going back generations. The keeper now is Old Gray, a man who has walked between those trees with a lantern for forty years.

The valley's rules were laid down in his master's day. Every bird that stops over is registered and receives **two nest tokens** — one naming a hollow in the east grove, one in the west. The tokens are divined by two old registrars, each reading the bird's feather-pattern with a method of her own; neither method can be bent to name a third hollow. And the iron rule: **at any moment, a bird lives in exactly one of its two nests.**

The rule buys something no other valley has. When Old Gray needs a bird, he opens no ledger and calls no search party: the hollow on the first token; not there? the second. Two looks, and the bird is found. In other valleys a roosting sparrow can hide from torches all night; in Two-Nest Valley one lantern visits two doorways.

That is the valley's fame: **to find a bird, look in at most two hollows.**

The price of that fame comes out at night.

A newcomer flies in and goes first to the hollow on its first token. If it's empty, everyone is happy. But at the height of migration, hollows are usually full. Then the valley invokes an old custom it isn't proud of — **nest-yielding**: the newcomer moves in and shoves the sitting tenant out.

The displaced bird — after waking half the mountain in protest — shakes out its feathers and flies to the hollow on its second token. And if *that* one is full? It yields in turn: shoves *its* tenant out and settles. The newly evicted one flies to its own other hollow... link after link, bird displacing bird, hollow trading hollow, wingbeats down the whole ridge past midnight.

Most nights the chain quiets after a dozen hops — some bird finds its other hollow standing empty, the tail of the chain settles in, and the valley sleeps.

But one night, the chain came home.

It was the busiest evening of the season. A newcomer yielded, a tenant moved, one link, two, a dozen — until the last bird displaced, circling back, wings tilting down onto the very hollow where the chain began. Sitting in it: the newcomer who had started everything. Evict *it*, and it must go displace the second link of the chain... The valley exploded — bird chasing bird around a closed circle, nobody settling, wingbeats from midnight to dawn.

Old Gray came out with his lantern, watched for the time an incense stick takes to burn, and went inside for three things.

The first was an old rule, nailed that dawn to a board at the valley mouth: **one arrival may cause at most a hundred hops.** Past a hundred, no more yielding — a chain that long is circling, and a circle has no morning.

The second stood in the valley's center: an old locust tree five men couldn't link arms around, a row of guest hollows carved low on its trunk, sealed in ordinary years. Old Gray broke the seals. A bird whose chain has hit the limit with nowhere to go sleeps that night in the locust. The guest hollows are few — a handful — but they exist precisely for the both-nests-full, chain-spent bird. A line is added to the register: to find this one, check its two tokens, then the locust.

The third was a notice Old Gray had written long ago and kept pressed at the bottom of a chest: **Moving Day.** If even the locust fills — migration swells year after year; that day always comes — the whole valley relocates to the denser forest over the back hill, hollows doubled, every bird re-registered and re-nested. Moving Day takes no newcomers and costs one chaotic day. It comes rarely; spread across every bird and every season, it amounts to one shake of feathers per quarter.

And so the valley gained the second half of its fame: crowded, yes — but **no bird in the register is ever unfindable.** Departures are even cleaner: a bird leaves, its hollow sits empty, and the register changes by not one stroke.

— By now you've probably recognized it: this is *cuckoo hashing*. And the custom of shoving out the sitting tenant is the cuckoo's own nature — laying its egg in another bird's nest and evicting the original occupant. That is where the algorithm got its name.

### What it is

*Cuckoo hashing* is an open-addressed hash table proposed by Rasmus Pagh and Flemming Friche Rodler in 2001. Each key is assigned two candidate buckets by two hash functions, *h₁* and *h₂*, and lives in exactly one of them at any moment. On insert, if both buckets are full, one sitting key is kicked out to its alternate bucket — and the evicted key may evict the next, forming a *displacement chain*. A chain can loop into a cycle and fail, so inserts carry a hop limit; keys left homeless at the limit go into a small *stash*; and when even the stash fills, the table *rehashes* into a larger one. Its signature property: lookup and deletion are worst-case *O(1)* — two buckets (plus the stash); there is nowhere else to look.

### Why it matters

Chained hash tables are O(1) *on average*, but a worst-case lookup can drag to O(n): one bucket grows a long queue, and nobody can promise how far down it you will count. *Cuckoo hashing* trades that away — "at most two places per key" buys **deterministic worst-case O(1) lookup and deletion**, a hard guarantee for latency-sensitive paths (routing tables, network gear, packet processing), with a memory layout that is cache-friendly, hardware-friendly, and easy to parallelize. Its descendant, the *cuckoo filter*, is a modern replacement for the Bloom filter: tighter space, and it supports deletion. It is also a textbook trade: costlier inserts (displacement chains, the occasional rehash) in exchange for certain lookups — a bargain that pays when reads far outnumber writes.

_Metaphor mapping_

- Each bird's two nest tokens → each key's two hash buckets, *h₁(x)* and *h₂(x)*
- A bird lives in only one nest at a time → a key exists in exactly one bucket at any moment
- Find a bird in at most two hollows → worst-case *O(1)* lookup and deletion
- Two registrars, each with her own method, no third hollow possible → two independent hash functions
- A newcomer shoves out the sitting tenant → displacement on insert conflict
- The evicted bird flies to its other hollow and evicts the next → the *displacement chain*
- The night the chain came back to its starting hollow → a displacement cycle; the insert fails
- The hundred-hop rule → the per-insert maximum chain length
- The guest hollows on the old locust → the *stash*
- Moving Day: the bigger forest, new tokens for everyone → *rehash* into a larger table
- Moving Day takes no newcomers → the write pause during a rehash
- A bird departs, its hollow sits empty, the register unchanged → deletion costs nothing; no cleanup needed

</section>
