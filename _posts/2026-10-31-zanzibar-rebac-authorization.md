---
layout: fable
title: "不存名册的档案馆 · The Archive That Keeps No Roster"
title_zh: "不存名册的档案馆"
title_en: "The Archive That Keeps No Roster"
concept: "Zanzibar-style relationship-based authorization (ReBAC)"
tags: [security, distributed-systems]
illustration: /assets/art/2026-10-31-zanzibar-rebac-authorization.jpg
youtube_id: "N-sDcKjrhJE"
---
<section class="zh" markdown="1">
南都的档案馆在城北，是一座七层石塔。塔顶常年有海风，把檐下的铜铃吹得整夜在响。

塔里存着全城的地契、婚书、船契、遗嘱。普通人一辈子进塔一次，用掉一天。

管塔的老馆正姓沈，人称沈七层。

他守一条被历任城主骂过无数次的规矩：**塔里不存名册。**

没有一页纸写着"谁可以进哪一间库房"。没有总册，没有按姓氏排的准入簿，没有一份能拿去烧掉的名单。

新来的书吏阿棠第一天就问：客人来了，我怎么知道让不让他进？

沈七层说：你去问。

—

问谁？

问那些只记得一件事的人。

城南的每一处行会、船帮、作坊、祠堂，各守一本薄册。册上只写两样东西之间的关系，一行一条，绝不写第二层。

——这份船契，归船帮会看。

——阿棠，去年秋入的船帮会。

——船帮会，听漕运司的令。

——漕运司，归城北档案馆管。

一行只连两处。船帮会不必知道漕运司上头还有谁，也不必知道阿棠的师父是谁。它只承认自己亲眼见过的、那一条线上的两个名字。

于是阿棠要回答"这个渔人能不能看这份祖上的船契"，她不能查，只能走。

第一站，船帮会。这份契谁能看？——会里的人，和漕运司的人。

第二站。这个渔人是不是会里的人？——是。册上一行：秋入。

走完了。两站。开门。

沈七层给她的头一件东西不是钥匙，是一根竹杖，杖上刻七个缺口。

**最多走七站。第八站的答案，一律是不许。**

第二件是一条规矩：同一处门，一趟里不许进两次。人一多，册子会绕回自家头上——甲会的人属乙会，乙会册上又写着甲会。走走就转圈，转到天黑也出不来。

册子的扉页上还写着这一处准用哪几种笔法：

- 任一即可。
- 全都须是。
- 除他之外。

前两种准顺着链条一直带下去。第三种不行——"除他之外"只许写在本处扉页，不许带进下一家的册里。沈七层这么定，不是出于讲究：一笔减法藏在链条深处，你算不出它有多大。

塔西侧有一间小屋，里头坐着一位老妇人，面前摊着几百片扇形的竹牌。

她把"走"这件事提前走完了。一个会一片牌，牌上写着这个会底下所有人。阿棠一推门，她已经把牌递过来，牌角带着水钟的刻。

若阿棠要的刻比牌上的新，老妇人把牌收回去，只说两个字：去走。

—

那年秋天出了一件事。

正午，铁匠行会划掉了一行字。一个人被逐出师门，从册上消失。

而阿棠在上午十一点给他做过一次答案。那个答案盖了章，章就挂在封库的门上。

他一整天进出封库，无人拦。傍晚才发现，三卷兵器图样不在了。

老馆正把章摔在案上。他没有骂阿棠说谎。

他说：**你没有说谎。你回答了一个已经变了的问题。**

—

答案不是此刻的真相。答案只是你读册子的那一刻的真相。

于是塔里改了规矩：每一份答案都要带水钟的刻。刻不是时辰，是"我读册子时，册子正处在哪一刻"。

封库的门上另有一行小字，是后来刻上去的：

此门只认不早于它最后一次改动的刻。

撤销因此变得极便宜：划掉一行。划掉那一刻，所有比它旧的答案同时作废——不必追回任何一张纸，不必去找任何一个盖过章的人。

城门各处的门吏，各备一小叠抄片，只答最常见的三五问。抄片也带刻。刻一变，塔里差人一处处换。这是塔里每天唯一非做不可的事。

那年冬天，那个渔人进了塔。他没有介绍信，阿棠也不认识他。

她只是走完了那两站。

到这儿你大概已经认出来了：这就是 **Zanzibar** 式的 *relationship-based authorization*——**关系型授权**，*ReBAC*。

### 这是什么

*Zanzibar* 是 Google 2019 年公开的全局授权系统，给 Drive、Docs、Calendar、YouTube 同做一套权限判定。它的核心选择是：**只存事实，不存结论。**

存的事实叫**关系元组**（*relation tuple*），形状是 `文档:年报#查看@用户:阿棠`，或者 `文档:年报#查看@组:船帮会#成员`。三段：**对象#关系@主体**。关键在最后一段：主体可以是人,也可以是**另一个对象上的某个关系**。这就是递归的入口——册子可以指向册子。

一条检查请求 `文档:年报#查看@用户:阿棠`，被翻译成一棵**求值树**：命中元组即返回，遇到用户集就展开、接着查。所以一次授权不是一次查表，而是一次**有界深度的图遍历**。"角色"不是一个能被授予的东西，而是图里的一条路径。

**用户集重写**（*userset rewrite*）让你把多条路径组合起来：`union`（任一即可）、`intersection`（全都须是）、`exclusion`（除他之外）。Zanzibar 对 `exclusion` 的嵌套位置有限制，正是为了让这次遍历的复杂度可以被界住。

**Zookie** 是那个水钟的刻：每次判定返回一个不透明的版本戳，客户端下次带上它，服务端保证"不给你比这更旧的答案"。它解决的是 **new enemy problem**——一个权限刚被撤销的人，能不能靠一份几分钟前的缓存答案继续进门。任何缓存过的授权结论，在被撤销的那一刻就是一句假话；版本戳让"旧"这件事变成可检测的，而不是靠猜 TTL。

**Leopard** 是那位老妇人：把图遍历的结果预先物化成索引树，命中就直答，未命中回源走图，并且带着新鲜度检查。索引让人快，也让人旧；旧到不能用的索引必须自己认输。

### 为什么重要

- **稀疏。** 权限矩阵是"所有主体 × 所有对象"，几乎全是空格。元组只画真实存在的边：一百万个用户乘一千万份文档，这个矩阵装不下，但真实的边只有几千万条。**存边不存格，是这套设计能成立的第一块石头。**
- **撤销是一行，不是一次广播。** 传统 ACL 里撤一个人要改几十份列表；这里删一条元组，所有下游判定立刻失效——因为没有任何地方存着"结论"，只有事实。
- **组合天然生效。** 一万人入一个组，是加一条元组；组本身再有上级，也不用去同步一万份文档的权限位。**扇出发生在读时，不在写时。**
- **它替代的是 RBAC 的角色爆炸。** 用角色描述"谁能看哪份文档"，角色数会随对象数乘关系数增长，最后没人管得过来。ReBAC 里没有角色表，只有路径——路径是算出来的，不是配出来的。
- **有界才能上关键路径。** 每次判定都挡在请求前面，所以遍历必须浅：深度上限、禁止循环、限制排除嵌套、p99 目标在毫秒级。**Zanzibar 的容量不是数据库容量，是遍历预算。**
- **缓存的正确性可以被证明。** 版本戳把"这份答案有多旧"变成可比较的量，于是缓存不再是赌博，而是一次带条件的复用。这是这一整套里最容易被忽略、但最贵的一块。
- **一致性是快照，不是线性。** 判定读的是某一刻的一致快照，不追求和最新写入同步。代价是必须显式携带"我读的是哪一刻",收益是可以在全球几十个副本上并行回答。
- **它被抄成了行业地基。** SpiceDB、OpenFGA、Auth0 FGA、Permify 都是这套模型的实现，Kubernetes 之外的多数 SaaS 权限问题，本质是"要不要一张关系图"。

_隐喻对应表_

- 塔里不存名册，只在各处存一行行的直接关系 → 元组存储；不物化"谁能访问什么"的结论
- 一行只连两处，绝不写第二层 → 元组只记录直接边，间接关系靠遍历求值
- "这份船契，归船帮会看" → `文档:船契#查看@组:船帮会#成员`，主体是一个用户集
- "阿棠，去年秋入的船帮会" → `组:船帮会#成员@用户:阿棠`
- 册子可以指向另一本册子 → 元组主体递归指向另一个对象的关系，遍历由此展开
- 阿棠不能查，只能走 → 授权判定是一次图遍历查询，不是一次索引匹配
- 七道缺口的竹杖 → 最大遍历深度；超深一律拒绝
- 同一处门一趟不许进两次 → 遍历中的环检测；关系图存在环
- 任一即可 / 全都须是 / 除他之外 → `union` / `intersection` / `exclusion` 用户集重写
- "除他之外"只许写在本处扉页 → `exclusion` 的嵌套位置受限，以保证复杂度可界
- 沈七层定这条规矩不是因为讲究，而是算不出减法有多深 → 排除规则嵌套会让求值代价不可控
- 老妇人把"走"提前走完，一片牌写尽一个会底下所有人 → 物化索引（*Leopard*），命中则跳过图遍历
- 阿棠一推门牌子已经递到手边 → 索引让常见判定变成一次点查
- 牌角带着水钟的刻 → 索引项携带新鲜度戳
- 要的刻比牌上新，就"去走" → 索引落后于请求所需版本时回源遍历
- 正午铁匠行会划掉一行 → 撤销只是一次元组删除，写时成本恒定
- 上午十一点盖了章的答案，一整天继续放人进门 → 缓存过的授权答案在撤销后变成假话
- 老馆正说"你没有说谎，你回答了一个已经变了的问题" → *new enemy problem*：不是判定逻辑错，是答案过期
- 答案必须带水钟的刻 → zookie：判定结果附带不透明版本戳
- "此门只认不早于它最后一次改动的刻" → 写端要求最低新鲜度，旧答案被显式拒绝而非静默接受
- 撤销那一刻，所有更旧的答案同时作废，不必追纸 → 版本区间失效；无需反查并作废每一份缓存
- 城门门吏各备一小叠抄片 → 服务层只读副本 / 本地缓存，承载绝大多数判定
- 刻一变，塔里差人一处处换抄片 → 副本按版本推进；缓存失效是版本驱动的，不是心跳驱动的
- 每天唯一非做不可的事就是换抄片 → 副本追赶是这套系统的主要运维负担
- 走两站就开门，渔人没有介绍信 → 判定只依赖图里的路径，不依赖预先授予的角色或凭证
- 一次判定要在门吏喊第二声之前答完 → 授权在请求关键路径上，遍历预算即容量预算
- 塔不追求"此刻最新"，只保证"不早于你要求的刻" → 快照一致性读，而非线性一致，换取跨副本并行

</section>
<section class="en" markdown="1">
The Southern Capital's archive stood in the north of the city: a seven-storey stone tower, with a sea wind that ran all night through the bronze bells under the eaves.

Inside were the city's land deeds, marriage contracts, boat indentures, wills. An ordinary person entered the tower once in a lifetime and spent a day there.

The keeper was surnamed Shen. People called him Shen of the Seven Storeys.

He kept one rule, and every lord mayor for six generations had cursed it: **the tower holds no roster.**

No page anywhere said who might enter which room. No master register. No admission book sorted by family name. No list that could be taken out and burned.

On her first day the new clerk, A-Tang, asked: when a visitor comes, how do I know whether to let him in?

Shen said: you go and ask.

—

Ask whom?

Ask the people who remember only one thing each.

Every guild hall, boat society, workshop and ancestral shrine in the south of the city kept one thin book. The book recorded relations between two things, one line at a time, and never a second layer.

— This boat indenture is read by the boat society.

— A-Tang entered the boat society last autumn.

— The boat society answers to the Canal Office.

— The Canal Office answers to the archive in the north.

One line joined two places. The boat society had no need to know who stood above the Canal Office, nor who A-Tang's master was. It acknowledged only the two names on that one line, the pair it had seen with its own eyes.

So when A-Tang had to answer whether this fisherman might see his grandfather's boat indenture, she could not look it up. She had to walk.

First stop, the boat society. Who may read this indenture? — Members of the society, and men of the Canal Office.

Second stop. Is this fisherman a member? — He is. One line: entered in autumn.

Done. Two stops. The door opens.

The first thing Shen gave her was not a key. It was a bamboo staff with seven notches cut into it.

**Seven stops at most. At the eighth, the answer is always no.**

The second thing was a rule: on one errand, never enter the same door twice. With enough people the books double back on themselves — a man of the first society belongs to the second, and the second's book names the first. Walk long enough and you circle until dark.

The first page of each book also said which strokes this office was permitted to use:

- Any one will do.
- All must hold.
- All but him.

The first two may be carried down the chain. The third may not. "All but him" is allowed only on the office's own first page; it may never be carried into the next book. Shen set that rule not out of fussiness: a subtraction buried deep in a chain is a subtraction whose size you cannot compute.

West of the tower stood a small room, and in it an old woman with several hundred fan-shaped bamboo tablets spread before her.

She had walked the walking in advance. One tablet to a society, and on each tablet the names of everyone beneath that society. A-Tang had only to push the door open and the tablet was already in her hand, its corner marked with the notch of the water clock.

If the notch A-Tang required was newer than the one on the tablet, the old woman took the tablet back and said two words: go walk.

—

That autumn something happened.

At noon, the smiths' guild struck out a line. A man was expelled from his master's house and vanished from the book.

But at eleven that morning A-Tang had answered a question about him. The answer carried a seal, and the seal hung on the door of the sealed vault.

All day he came and went through that door and nobody stopped him. By evening three scrolls of weapon drawings were gone.

The old keeper threw the seal down on the desk. He did not accuse A-Tang of lying.

He said: **you did not lie. You answered a question that had already changed.**

—

An answer is not the truth of this moment. An answer is the truth of the moment at which you read the books.

So the tower changed its rule: every answer must carry the notch of the water clock. The notch is not the hour. It is *which moment the books were in when I read them*.

A small line was later cut into the door of the sealed vault:

This door accepts no notch earlier than its own last change.

Revocation thus became very cheap: strike out one line. At the instant of the striking, every answer older than it is void at once — no paper to chase down, no man who once stamped a seal to go and find.

The gate clerks at each city gate kept a small stack of copied tablets between them, answering only the three or five questions asked most often. The copies carried notches too. When the notch moved, the tower sent men round to replace them, gate by gate. That was the only thing the tower had to do every single day.

That winter the fisherman entered the tower. He had no letter of introduction, and A-Tang did not know him.

She had simply walked the two stops.

By now you have probably recognized it: this is **Zanzibar**-style *relationship-based authorization* — *ReBAC*.

### What it is

*Zanzibar* is the global authorization system Google published in 2019, serving Drive, Docs, Calendar and YouTube from one permission decision service. Its central choice: **store facts, never conclusions.**

The facts are **relation tuples**, shaped like `document:annual-report#viewer@user:A-Tang`, or `document:annual-report#viewer@group:boat-society#member`. Three parts: **object#relation@subject**. The important part is the last one — the subject may be a person, or **a relation on another object**. That is the door recursion walks through: a book may point at a book.

A check for `document:annual-report#viewer@user:A-Tang` becomes an **evaluation tree**: hit a tuple and return, meet a userset and expand, keep going. Authorization is therefore not a lookup but a **bounded-depth graph walk**. A role is not something granted; it is a path in that graph.

**Userset rewrites** combine paths: `union` (any one will do), `intersection` (all must hold), `exclusion` (all but him). Zanzibar restricts where `exclusion` may nest, precisely so the cost of that walk can be bounded.

**Zookies** are the notch of the water clock: every decision returns an opaque version stamp, and a client presenting it is promised an answer no older than that. They exist for the **new enemy problem** — whether a man just stripped of a permission can keep walking in on a cached answer from five minutes ago. Any cached authorization conclusion becomes a lie the instant the underlying relation changes; a version stamp makes staleness *detectable* instead of something you guess with a TTL.

**Leopard** is the old woman: a materialized index that pre-computes portions of the walk, answering hits directly and falling back to the graph on misses, with a freshness check attached. The index makes you fast and it makes you old; an index too old for the request must admit it and walk.

### Why it matters

- **It is sparse.** The permission matrix is all subjects × all objects and is almost entirely empty. Tuples draw only the edges that exist: a million users times ten million documents does not fit anywhere, but the real edges number in the tens of millions. **Storing edges instead of cells is the first stone this design stands on.**
- **Revocation is a line, not a broadcast.** An ACL revokes by editing dozens of lists. Here you delete one tuple and every downstream decision dies at once — because no conclusion was ever stored, only facts.
- **Composition costs nothing at write time.** Ten thousand people joining a group is one tuple; if that group has a parent, no one has to touch ten thousand documents. **Fan-out happens at read, not at write.**
- **It replaces RBAC's role explosion.** Describing "who may see which document" with roles makes the role count grow with objects times relations until nobody can administer it. ReBAC has no role table, only paths — paths are computed, not configured.
- **Bounded depth is what puts it on the hot path.** Every decision stands in front of a request, so the walk must stay shallow: depth limits, cycle refusal, restricted exclusion nesting, a p99 target in single-digit milliseconds. **Zanzibar's capacity limit is not database capacity; it is traversal budget.**
- **Cache correctness becomes provable.** A version stamp turns "how stale is this answer" into a comparable quantity, so caching stops being a gamble and becomes conditional reuse. This is the least glamorous and most expensive part of the whole system.
- **Consistency is a snapshot, not a line.** A decision reads one consistent snapshot and does not insist on matching the newest write. The price is carrying "which moment I read" explicitly; the reward is answering in parallel from dozens of replicas worldwide.
- **It became the industry's floor.** SpiceDB, OpenFGA, Auth0 FGA and Permify all implement this model, and most SaaS permission questions outside Kubernetes reduce to one question: do you need a relationship graph?

_Metaphor mapping_

- No roster in the tower; only one line of direct relation kept at each office → tuple storage; the "who can access what" conclusion is never materialized
- One line joins two places and never a second layer → tuples hold direct edges only; transitive relations are computed by traversal
- "This boat indenture is read by the boat society" → `document:indenture#viewer@group:boat-society#member`, the subject is a userset
- "A-Tang entered the boat society last autumn" → `group:boat-society#member@user:A-Tang`
- A book may point at another book → a tuple's subject recursively names another object's relation; the walk unfolds from there
- A-Tang cannot look it up, she must walk → a check is a graph traversal query, not an index match
- The staff with seven notches → maximum traversal depth; anything deeper is denied
- No door twice on one errand → cycle detection during traversal; the relation graph contains cycles
- Any one will do / all must hold / all but him → `union` / `intersection` / `exclusion` userset rewrites
- "All but him" may sit only on the office's own first page → `exclusion` is restricted in where it nests, to keep evaluation bounded
- Shen's rule is not fussiness; a deep subtraction has no computable size → nested exclusion makes evaluation cost uncontrollable
- The old woman walks the walking in advance; one tablet names everyone beneath a society → materialized index (*Leopard*), which skips graph traversal on a hit
- The tablet is in A-Tang's hand before she asks → the index turns common checks into a point lookup
- The tablet's corner carries a water-clock notch → index entries carry a freshness stamp
- A newer notch than the tablet's means "go walk" → index lags the version the request requires, so fall back to the graph
- At noon the smiths' guild strikes out a line → revocation is one tuple delete; write cost is constant
- The seal stamped at eleven keeps admitting the man all day → a cached authorization answer becomes false on revocation
- "You did not lie. You answered a question that had already changed." → the *new enemy problem*: the decision logic was right, the answer was stale
- Every answer must carry the water-clock notch → zookie: decisions return an opaque version stamp
- "This door accepts no notch earlier than its own last change" → the write side demands a minimum freshness; stale answers are refused, not silently accepted
- Older answers void at the instant of striking, with no paper to chase → version-range invalidation; no need to hunt down and delete each cached answer
- Gate clerks keep a small stack of copied tablets → read-only serving replicas and local caches absorbing most checks
- When the notch moves, men are sent to replace the copies gate by gate → replicas advance by version; invalidation is version-driven, not heartbeat-driven
- Replacing the copies is the only daily obligation the tower has → replica catch-up is this system's main operational burden
- Two stops and the door opens; the fisherman had no letter of introduction → decisions rest on a path in the graph, not on pre-granted roles or tickets
- Answer before the gate clerk calls twice → authorization sits on the request path, so traversal budget is capacity budget
- The tower does not promise "newest", only "no older than the notch you demanded" → snapshot-consistent reads rather than linearizability, buying cross-replica parallelism

</section>
