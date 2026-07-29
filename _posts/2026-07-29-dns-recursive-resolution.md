---
layout: fable
title: "问路的邮差 · The Postman Who Never Knew the Address"
title_zh: "问路的邮差"
title_en: "The Postman Who Never Knew the Address"
concept: "DNS recursive resolution"
tags: [networking, distributed-systems]
illustration: /assets/art/2026-07-29-dns-recursive-resolution.jpg
---
<section class="zh" markdown="1">
从前有个小村子，村口有一间小小的邮局。一天清早，一位村民攥着信来找年轻的邮差阿禾，信封上只写了一个名字——"给远山那头的柳家老三"——却没有门牌号。

"我也不知道他住哪，"阿禾说，"但你别自己一路问过去了。把信交给我，我去替你把地址跑回来，最后只把准确门牌交到你手上。"于是这趟腿就全落在阿禾一个人身上。

阿禾先奔到镇口十字路那位最年长的老图书管理员跟前。老人家一个门牌都不记，可她记得一件更要紧的事：天下姓氏各归哪个"大区"打理。"柳家啊，"她眯眼一指东边，"我不知道老三住哪间屋，但我知道该去问谁——去东边的柳氏总登记处。"

阿禾又赶到柳氏总登记处。这里的书记同样不知道老三的具体门户，却知道再往下该找谁："柳家自己有位管家，专管本族门户。你去找他，那儿才有准信。"

最后阿禾寻到柳家的老管家。管家翻开自家册子，一口报出："远山第七户，青瓦房。"——这才是真正、说了算的答案。

回程路上，阿禾把这条记进了随身的小本子："柳家老三 = 远山第七户"，旁边还添了一行小字："此条到黄昏为止有效。"往后半天里，谁再来问同一个人，他都能张口就答，不必再跑一趟；等黄昏一过，这条就作废，下次得重新去问。他甚至连"查无此人"都记下来，省得同一个空名字被人反复来问。

——到这儿你大概已经认出来了：这套"我不知道，但我知道该问谁"、一层层被指引下去、最后由掌册人给出准确门牌、再把答案暂存一段时间的流程，就是 *DNS 递归解析（recursive resolution）*。

**这是什么。** 当你在浏览器里敲下一个域名，机器拿到的是名字，不是地址。一台 *recursive resolver*（递归解析器，通常由你的 ISP 或 `8.8.8.8` 这类公共服务提供）替你把整段"问路"包办到底：它先问 *root name server*（根服务器）——根不给最终答案，只告诉它该找哪个 *TLD name server*（如 `.com`）；TLD 再把它指向那个域名的 *authoritative name server*（权威服务器）；权威服务器手里才有真正的 *A/AAAA record*（IP 地址）。这种"根 → TLD → 权威"的层层 *delegation*（委派）让全世界的名字无需一本总册就能各自管辖。

**为什么重要。** 每次点击都重跑一遍三段问路太贵，所以 resolver 会按各条记录自带的 *TTL* 把答案缓存起来，到期才失效；连"查无此域名"（*NXDOMAIN*）也会做 *negative caching*。缓存让绝大多数解析快到无感，但也解释了为什么改了 DNS 记录后"要等一会儿才生效"——旧答案还在别人的小本子上没到黄昏。

**隐喻对应表**

- 信封上的名字（柳家老三）→ 你要访问的 *域名*
- 拿着信来托付的村民 → 发起查询的客户端（stub resolver）
- 一个人把全程腿跑完的邮差阿禾 → *recursive resolver*
- 十字路口的老图书管理员（只指路，不给门牌）→ *root name server*
- 按姓氏分区的柳氏总登记处 → *TLD name server*（如 `.com`）
- 手握自家册子的柳家老管家 → *authoritative name server*
- "远山第七户，青瓦房" → *A/AAAA record*（IP 地址）
- "我不知道，但该去问谁" 的层层指引 → *delegation*（NS 记录 / referral）
- 小本子上"到黄昏为止有效"的记录 → 带 *TTL* 的缓存
- 连"查无此人"也记下 → *negative caching*（NXDOMAIN）
</section>
<section class="en" markdown="1">
Once there was a small village with a tiny post office by its gate. One morning a villager came clutching a letter to Ah-He, the young postman. On the envelope was only a name — "To the third Liu child, over on Far Mountain" — and no house number.

"I don't know where he lives either," said Ah-He, "but don't go asking your way door to door. Give me the letter. I'll go fetch the address for you and put nothing in your hand but the exact house number."

So the whole errand fell on Ah-He alone. He ran first to the crossroads, to the oldest librarian in town. She remembered not a single house number, but she remembered something more useful: which broad district each family name belonged to. "The Lius?" She squinted and pointed east. "I don't know which room the third child keeps, but I know who to ask — go to the Liu Family Grand Registry, out east."

Ah-He hurried to the Grand Registry. Its clerk didn't know the third child's door either, but knew who to send him to next. "The Lius keep their own steward for their household. Go to him — that's where you'll get the real answer."

At last Ah-He found the old Liu steward. The steward opened his household book and read it straight off: "Seventh house on Far Mountain, the one with the blue-grey tiled roof." That, finally, was the true and final answer.

On the walk home Ah-He jotted it into the little notebook he carried: "Third Liu child = seventh house, Far Mountain," and beside it a small note: "Good until dusk." For the rest of the day, anyone asking after that same person got an instant answer — no second trip. Once dusk passed the note expired and he'd have to ask again. He even wrote down "no such person," so the same empty name wouldn't send him running twice.

— By now you've probably recognized it: this whole routine of "I don't know, but I know who to ask," being handed down level by level, the final house number given by whoever holds the book, and the answer kept on file for a while — that is *DNS recursive resolution*.

**What it is.** When you type a domain name, your machine has a name, not an address. A *recursive resolver* (usually run by your ISP or a public service like `8.8.8.8`) takes on the whole errand of "asking the way" end to end. It asks a *root name server* first — the root gives no final answer, only which *TLD name server* (like `.com`) to try. The TLD points it to that domain's *authoritative name server*, and only the authoritative server holds the real *A/AAAA record* (the IP address). This "root → TLD → authoritative" chain of *delegation* lets the world's names be governed locally, with no single master ledger.

**Why it matters.** Re-running all three hops on every click would be expensive, so the resolver *caches* each answer for as long as its record's *TTL* allows, expiring only then — and even a "no such domain" (*NXDOMAIN*) gets *negative caching*. Caching makes most lookups feel instant, and it also explains why a changed DNS record "takes a while to take effect": the old answer is still sitting in someone's notebook, not yet past dusk.

**Metaphor mapping**

- The name on the envelope (the third Liu child) → the *domain name* you want to reach
- The villager handing over the letter → the client that starts the query (stub resolver)
- Ah-He, who runs the whole errand himself → the *recursive resolver*
- The crossroads librarian (points the way, gives no house number) → the *root name server*
- The Grand Registry, sorted by family name → the *TLD name server* (like `.com`)
- The old Liu steward with his household book → the *authoritative name server*
- "Seventh house, blue-grey tiled roof" → the *A/AAAA record* (the IP address)
- The "I don't know, but here's who to ask" hand-offs → *delegation* (NS records / referrals)
- The notebook entry marked "good until dusk" → a cache with a *TTL*
- Writing down even "no such person" → *negative caching* (NXDOMAIN)
</section>
