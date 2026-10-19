---
layout: fable
title: "三枚玉符 · Three Jade Tallies"
title_zh: "三枚玉符"
title_en: "Three Jade Tallies"
concept: "Shamir's Secret Sharing"
tags: [security, distributed-systems]
illustration: /assets/art/2026-10-18-shamirs-secret-sharing.jpg
youtube_id: "KUL-w4tHUoM"
---
<section class="zh" markdown="1">
青崖镇只有一家钱庄，叫通汇庄，三代人的营生。镇上人的银子、地契、商队托付的汇票，全锁在后院地下的银窖里。窖门是整块青石凿的，锁孔深藏；钥匙只有一把，乌铁打的，蟋蟀大小，三代掌柜都把它贴身挂在腰上。

如今老掌柜秦老爷子老了，膝下五个儿子。这把乌铁钥匙传给谁，成了他夜里睡不着的心病。

传给老大一个人？邻镇裕丰庄就是这么办的：钥匙传了长子，长子夜里贪杯，钥匙被人从腰上摸了去——一夜之间，整窖搬空，裕丰庄至今没缓过来。**一把钥匙，就是一条等着被摸走的腰带。**

五个儿子一人配一把？更不行。老三好赌，老五耳根软。哪家的钥匙丢了、被偷了、被人灌醉后拓了模，整窖就完了。五把钥匙，五条腰带。**锁再硬，也只防得住最小心的人。**

把钥匙铰成五片，兄弟每人一片？谁也别想独吞了，可开一回窖得五兄弟凑齐——老二常年跟着商队走西口，一去大半年；那年山道塌方，西口的商队折了一半的人。人若回不来，那片钥匙就永远埋在山里，**整窖的银子，陪着一片铁，一同锁死。**

三条路都堵死了，老爷子也把自己堵死在了里面。

那年秋天，镇上来了位云游的先生，姓沙。人们只知他画沙为算：一根木棍、一片平沙，天下的账都能画出来。

沙先生在通汇庄喝了三天茶，听完老爷子的心病，笑了："你何必要有一把钥匙？"

"不开窖了？"

"照开。但钥匙不必'存在'。"沙先生用木棍在院中沙地上画了一道弧，像一张拉满的弓，"我心里有一条弧线，谁也不知道——连你也不必知道。它从银窖正下方那眼暗泉里升起，弯上天去。我在这条弧上取五个点，把每个点的'里程'和'高度'刻进五枚玉符。五个儿子，一人一枚。"

"玉符怎么开锁？"

"凑齐**任意三枚**，往窖门前的石台上一放，机簧自会描出穿过三点的唯一一条弧，顺着弧摸回它的源头——暗泉的位置，就是锁心里那把唯一的钥匙。三枚足矣，不必五个都在：老二在西口，剩下四枚任取三枚，窖照样开。"

"两枚呢？"

沙先生抹平沙盘，重新点下两个点："你瞧，过这两点，能画出多少条弧？"

老爷子盯着看了半晌，背上一层细汗。过两点，弧有千千万万条，每一条都通向一眼不同的泉。**两枚玉符在手，那眼真泉混在千千万万眼假泉里，一丝一毫也不多露。**偷符的人攥着的不是半个秘密——是零。

老爷子拍案：就这么办。

铸符那夜，玉坊的火亮到三更。五枚青玉符，每枚刻着两个小数字。沙先生临走留了话："玉符丢了不必慌：凑齐三枚，照着原弧再铸一枚，给谁补谁。只是记牢，三枚一凑，弧线就在那三个人眼里了。真到疑心兄弟的那一天，请人来**另画一条弧**——旧符尽数作废，钥匙无声无息换了芯。"

头两年太平。第三年冬天，出了事。

腊月里，老三在赌坊输红了眼，怀里的玉符抵给了做局的人；又有人翻进老五的院子，摸走了他压在枕下的那枚。做局的人攒齐两枚玉符，夜里带着匠人来试窖。他们把两枚摆上石台，又照赌徒间道听途说的零碎，胡乱猜了第三个点。机簧一声不响地描弧——弧描得出来，源头却指向七里外的枯井、山阴的乱石滩、从来不存在的泉。猜一夜，错一夜；猜一年，错一年。**两点定不了弧；猜出来的每一条弧，都通向别人家的泉。**窖门纹丝不动。

开春，兄弟们凑齐三枚好符（老三、老五涉险的两枚作废另铸），又照沙先生的旧话，请人另画了一条弧。五枚新符下来，旧符当着全家的面，砸了。

通汇庄又开了三代。镇上人只记得一条规矩：**秦家的窖，三枚玉符开；两枚，神仙也开不了。**

讲到这儿，你多半已经认出来了：这就是密码学里的 *Shamir's Secret Sharing*——沙米尔秘密共享。

---

### 这是什么

*Shamir's Secret Sharing*（沙米尔秘密共享）是密码学家 Adi Shamir 1979 年提出的 **(k, n) 门限方案**（*threshold scheme*）：把一个秘密——通常是一把主密钥——"拆"成 n 份份额（*shares*），**任意 k 份能完整复原秘密；少于 k 份，得不到关于秘密的任何信息**。不是"算力上难破"，是信息论意义上的**零泄露**。

构造一句话讲完：**把秘密藏进一条曲线的起点**。随机构造一条 k−1 次多项式曲线 f(x)，令秘密 S = f(0)；第 i 份份额就是曲线上的一个点 (i, f(i))，发给 n 位持有者。k 个点唯一确定一条 k−1 次曲线（*Lagrange interpolation*，拉格朗日插值），代入 x = 0 即还原 S；而 k−1 个点能过的曲线有无数条，每条在起点的取值各不相同且等可能——攻击者手里的信息量恰好为零。**份额不是"半个秘密"**：单独看，一枚玉符只是曲线上一个无关紧要的点。

### 为什么重要

1. **单钥匙是单点故障**（裕丰庄一夜搬空），**复制钥匙是最弱环节安全**（五条腰带任人挑）。门限方案两头都赢：至多 k−1 份被偷，秘密不伤；至多 n−k 份缺失，门照常开。**安全性与可用性同时变好**，而非此消彼长。
2. **守护现实系统的根秘密**：HashiCorp Vault 的 *Shamir unseal*（五份解封钥，凑三份复活封印）；区块链分布式验证者的*门限签名*与 *DKG*——质押私钥从头到尾没人完整持有过；冷钱包多方共管；DNSSEC 应急密钥流程；云厂商根密钥的分片托管。
3. **更大拼图的基石**：*secure multi-party computation*（安全多方计算）与分布式密钥生成均以秘密共享为地基；*verifiable secret sharing* 再加一层"份额彼此一致"的可验证证明。
4. **弧可以重画**（*proactive resharing*，主动重共享）：周期性重新分发份额，攻击者就必须在**同一个窗口期内**偷齐 k 份——慢功夫、长线渗透，永远凑不齐。

_隐喻对应表_

- 乌铁钥匙贴身挂在腰上 → 被保护的秘密（*secret*，如主密钥）
- 沙先生心里那条弧 → 随机构造的 k−1 次多项式 f(x)（*random polynomial*）
- 弧线起点的暗泉 → 秘密本身，f(0)（*secret = f(0)*）
- 五枚青玉符 → n 份份额（*shares*，n = 5）
- 玉符上刻的里程与高度 → 曲线上的一个点 (i, f(i))，一份份额
- 任意三枚即可开窖 → 门限 k = 3（*threshold*）
- 三点唯一定一弧 → k 个点唯一确定 k−1 次多项式（*Lagrange interpolation*）
- 窖门石台的描弧机簧 → 持有者执行的复原算法（插值过程）
- 两枚玉符一丝不露 → 少于 k 份份额信息论零泄露（*information-theoretic security*）
- 赌徒猜第三点、弧弧皆错 → 伪造份额只会还原出错误的秘密（*forged shares*）
- 老二走西口、四枚任取三枚 → 容忍至多 n−k 份缺失（*availability*）
- 凑齐三枚补铸新符 → 用 k 份份额为新持有者再生成（*share re-provisioning*）
- 另画一条弧、旧符尽废 → 周期性重共享与秘密轮换（*proactive resharing / secret rotation*）
- 裕丰庄的钥匙被摸走 → 单点故障（*single point of failure*）
- 五人五把钥匙 → 复制密钥的最弱环节（*weakest-link security*）
</section>
<section class="en" markdown="1">
Green-Cliff Town had exactly one money house: Confluence Hall, three generations in the trade. The town's silver, its land deeds, the drafts entrusted by passing caravans — all locked in the cellar vault behind the courtyard. The vault door was carved from a single slab of blue stone, its keyhole deep-hidden; the key was singular, a crow-iron thing the size of a cricket, worn at the belt of each generation's master.

Now Old Master Qin was aging, with five sons beneath him. To whom the iron key should pass was the ache that kept him awake at night.

To the eldest alone? That was the road Abundance Hall in the next valley took: the key passed to the firstborn, the firstborn drank deep one night, and someone lifted the key from his belt — by dawn the vault stood empty, and Abundance Hall never recovered. **One key is one belt, waiting to be picked.**

One copy per son, five keys? Worse. The third son gambled; the fifth was easily led. Let any key be lost, stolen, or moulded off a drunkard in his sleep, and the vault was done. Five keys, five belts. **However hard the lock, it protects only as much as the most careless holder.**

Cut the key into five shards, one per brother? No one could rob the vault alone anymore — but every opening needed all five gathered. The second son rode the western caravan routes half the year, and the year the mountain road collapsed, the western caravans lost half their men. If a man never came home, his shard stayed buried in the mountains with him — **and a vault full of silver sat locked beside a sliver of iron, forever.**

Three roads, all blocked; and the old master had bricked himself in with them.

That autumn a wandering scholar came through town — Master Sha. People knew only that he reckoned by drawing in sand: one stick, one smooth patch, and any account under heaven could be drawn out.

Master Sha drank three days of tea at Confluence Hall, heard the old man's ache, and laughed. "Why must there be a key at all?"

"Never open the vault again?"

"Open it as always. But the key need not *exist*." With his stick he drew an arc in the courtyard sand, like a drawn bow. "I hold one curve in my mind — no one else knows it, and you needn't either. It rises from the hidden spring directly beneath your vault and bends up into the sky. On this curve I take five points, and carve each point's *distance and height* into a jade tally. Five sons, one tally each."

"And how does a tally open a lock?"

"Gather **any three** and set them on the stone table before the vault. The mechanism traces the one arc that passes through all three points, and follows it back to its origin — the hidden spring. The spring's position is the one true key the lock has ever known. Three suffice; all five need not be present: while your second son rides the western routes, any three of the remaining four open the vault as usual."

"And two?"

Master Sha smoothed the sand flat and set down two points. "Look — how many arcs pass through these two?"

The old master stared, and a fine sweat rose on his back. Through two points run ten thousand arcs, each rising from a different spring. **Holding two tallies, the true spring hides among ten thousand false ones and gives up nothing — not a hair's worth.** What the thief holds is not half a secret. It is zero.

The old master struck the table: so be it.

The night the tallies were cut, the jade-cutter's fire burned past the third watch: five jade tallies, each carved with two small numbers. Before leaving, Master Sha left one more instruction: "If a tally is lost, don't panic — three gathered can cast a fresh one on the same curve for whoever needs it. Only remember: the moment three gather, the curve lies bare before those three pairs of eyes. On the day you distrust your own brothers, send for someone to **draw a new curve** — void every old tally, and the key silently changes its heart."

Two years of peace. In the third winter, trouble.

In the twelfth month the third son, losing badly at the gambling house, pledged his tally to the men running the table; and someone climbed the fifth son's wall and lifted the tally from under his pillow. The gamesters gathered their two tallies and brought a craftsman to the vault by night. They set both on the stone table and, following scraps the gamblers had overheard, guessed wildly at a third point. The mechanism traced its arc in silence — an arc existed, but its origin pointed to a dry well seven li away, to the scree on the mountain's shaded side, to springs that had never existed. A night of guessing, a night of error; a year of guessing, a year of error. **Two points cannot fix an arc, and every guessed arc runs to someone else's spring.** The vault door never moved.

In spring the brothers gathered three good tallies (the two compromised ones were voided and recast) and, by Master Sha's old instruction, had a new curve drawn. When the five new tallies came down, the old ones were smashed before the whole family.

Confluence Hall traded on for three more generations. The town remembered one rule: **the Qin vault opens for three tallies; for two, not even the immortals can open it.**

By now you've probably recognized it: this is *Shamir's Secret Sharing*.

---

### What it is

*Shamir's Secret Sharing* is a *(k, n) threshold scheme* published by Adi Shamir in 1979: split a secret — typically a master key — into n *shares* such that **any k shares reconstruct the secret exactly, while fewer than k shares reveal nothing about it**. Not "computationally hard to crack" — *information-theoretically* zero leakage.

The construction in one sentence: **hide the secret at the origin of a curve.** Choose a random polynomial f(x) of degree k−1 with the secret S as f(0); the i-th share is simply the point (i, f(i)), handed to one of n holders. Any k points determine a degree-(k−1) polynomial uniquely (*Lagrange interpolation*); evaluate at x = 0 and S falls out. With only k−1 points, infinitely many polynomials fit, each taking a different, equally likely value at the origin — the attacker's information is exactly zero. **A share is not "half a secret"**: alone, a tally is just an unremarkable point on a curve.

### Why it matters

1. **A single key is a single point of failure** (Abundance Hall emptied overnight); replicated keys are weakest-link security (five belts to pick). A threshold scheme wins on both ends: up to k−1 shares stolen, the secret is unharmed; up to n−k shares missing, the door still opens. **Security and availability improve together** rather than trading off.
2. **It guards root secrets in real systems**: HashiCorp Vault's *Shamir unseal* (five unseal keys, any three revive the seal); *threshold signatures* and *DKG* for distributed blockchain validators, where a staked private key is never held whole by anyone; multi-party cold-wallet custody; DNSSEC emergency key procedures; sharded custody of cloud providers' root keys.
3. **It is the foundation of a larger puzzle**: *secure multi-party computation* and distributed key generation both rest on secret sharing; *verifiable secret sharing* adds proofs that shares are mutually consistent.
4. **The curve can be redrawn** (*proactive resharing*): periodically re-deal the shares, and an attacker must steal k of them **within a single window** — a slow, patient theft never adds up.

_Metaphor mapping_

- the crow-iron key worn at the belt → the protected secret, e.g. a master key (*secret*)
- the curve in Master Sha's mind → the randomly constructed degree-(k−1) polynomial f(x) (*random polynomial*)
- the hidden spring where the curve begins → the secret itself, f(0) (*secret = f(0)*)
- the five jade tallies → n shares (*shares*, n = 5)
- distance and height carved on a tally → one point (i, f(i)) on the curve — one share
- any three tallies open the vault → the threshold k = 3 (*threshold*)
- three points fix exactly one arc → k points uniquely determine the degree-(k−1) polynomial (*Lagrange interpolation*)
- the stone table's arc-tracing mechanism → the reconstruction algorithm run by the holders (interpolation)
- two tallies give up nothing → fewer than k shares leak zero information, information-theoretically (*information-theoretic security*)
- the gamblers' guessed third point, always wrong → forged shares reconstruct a wrong secret (*forged shares*)
- the second son away west, any three of four suffice → tolerance of up to n−k missing shares (*availability*)
- three gathered recast a lost tally → re-provisioning a share for a new holder (*share re-provisioning*)
- drawing a new curve, smashing the old tallies → periodic resharing and secret rotation (*proactive resharing / secret rotation*)
- Abundance Hall's lifted key → single point of failure (*single point of failure*)
- five copied keys, five belts → weakest-link key replication (*weakest-link security*)
</section>
