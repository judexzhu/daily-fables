---
layout: fable
title: "钥匙就挂在箱子上 · The Key Hangs on the Crate"
title_zh: "钥匙就挂在箱子上"
title_en: "The Key Hangs on the Crate"
concept: "Envelope encryption (DEK / KEK)"
tags: [security, aws, storage]
illustration: /assets/art/2026-08-06-envelope-encryption.jpg
---
<section class="zh" markdown="1">
海关码头西头有一排货仓，管货仓的是老麦。

三十年前他刚接手的时候，货仓只有一把锁。一把很好的锁——黄铜的，六道栓，钥匙他贴身带着，睡觉都不摘。全埠的箱子都堆在这一道门后面。

出事是在第七年。有天夜里，钥匙从他挂在墙上的外衣口袋里被摸走了。摸走的人只得手了那一夜，可那一夜他能打开的，是整座货仓。

老麦从此换了做法。

如今你把一只箱子交给他，他先不收钱。他当着你的面从墙上取下一把**全新的锁**——每只箱子一把，两百只箱子两百把，没有哪两只共用。锁扣上，钥匙就在他手里，小小一根，铜色发亮。

接下来是外人看着奇怪的部分。他不把钥匙交给你，不挂在自己腰上，也不扔进抽屉。他把钥匙塞进一根小铜管，拧上盖，然后拎着这根管子——只有管子，箱子留在原处——穿过院子，走到尽头那扇又厚又圆的铁门前。

铁门后面坐着封印师。整个码头只有她一个人，一辈子不出那道门。她从窗口接过铜管，在管盖上压下一道**大印**。印模就在她脚边的柜子里，从来没有离开过那间屋子；没有副本，没有拓片，谁也没见过它长什么样——包括老麦。压好的管子从窗口递回来。

老麦把这根封好的铜管，用麻绳系在箱子的提手上。就系在箱子上，明晃晃地挂着，谁都看得见。

新来的伙计头一天上工就忍不住问：钥匙就挂在箱子上，那还锁个什么劲？

老麦让他自己试。伙计拧不开铜管——盖被印死了；拿刀撬，管子裂开，里面的钥匙也跟着断了。转头去撬箱上那把锁，六道栓，撬到天亮也没撬开。

要取货的时候，老麦解下铜管，走那三十步，从窗口递进去。封印师验印、开管，把钥匙从窗口递回来。开箱、点货、重新上锁；钥匙塞回管里，再封一次，再系回箱子上。她从头到尾没碰过箱子，也不知道里面装的是茶叶还是火药——她只认管子。窗台上摊着一本册子：哪一根管、什么时辰、谁递进来的，一根一行。

去年冬天真出了事。一伙人翻墙进院，撬松了三只箱子拖走——铜管还系在提手上，也一并拖走了。半个月后货在邻埠被起获，箱子还锁着，铜管还封着，一根都没打开。他们什么也没拿到，因为他们拖不走那扇铁门。

也有过一回，某只箱子的锁真被人照着配出了钥匙。丢的就是那一只。旁边两百只箱子，各是各的锁，那把配出来的钥匙插进第二只箱子的锁孔，连一半都进不去。

最见功夫的是另一桩。封印师每年换一次大印——旧印模熔了，重铸一枚新的。你大概以为那天要把全埠两百只箱子统统拆开、重锁、重编号？老麦那天只干了一件事：把两百根铜管拎进院子，一根一根递进窗口，验旧印、换新印、递出来、系回原处。半天完事。箱子一只没动，锁一把没换，货一件没搬——重新封过的，只是那两百根小管子。

伙计后来问过一句：万一那枚大印毁了呢？老麦说，那两百只箱子就再也开不了了。不是丢了，是永远打不开。他说这话的时候没有笑。

——到这儿你大概已经认出来了：老麦这一套"一箱一锁、钥匙封进小管、小管明着挂在箱子上、大印永不出那道铁门"，就是 *envelope encryption*。

**概念解释。** Envelope encryption 是所有主流云上静态加密的默认结构。数据不直接用主密钥加密，而是每个对象（或每次写入）随机生成一把一次性的对称密钥 *DEK*（data encryption key），用它在本地做 AES-GCM 之类的批量加密；然后把这把 DEK 送进 KMS/HSM，用永不导出的 *KEK*（key encryption key，也叫 master key、CMK）加密一次，得到一小段密文——*wrapped DEK*——原样存在密文对象旁边。读的时候流程反过来：取出 wrapped DEK，调 `kms:Decrypt` 换回明文 DEK，在本地解开数据，用完立刻从内存里抹掉。KEK 自始至终没有出过 HSM 的边界，KMS 也自始至终没有见过你的数据。

**为什么重要。** 第一是**规模**：KMS 是网络服务，有速率限制，单次能加密的载荷通常只有几 KB，你不可能把一个 5 TB 的卷送进去；把批量加密留在本地、只让 KMS 处理几十字节的密钥，性能和成本才成立。第二是**爆炸半径**：一把 DEK 只保护一个对象，泄漏一把并不危及第二个对象；同时也避开了单密钥的密码学磨损（AES-GCM 在同一密钥下的 nonce 空间是有限的）。第三是**轮转的经济性**：换 KEK 只需要把那些几十字节的 wrapped DEK 重新包一遍，数据一个字节都不用重写——这就是 S3 SSE-KMS、EBS、GCP CMEK、Vault transit 敢承诺"随时轮转主密钥"的原因；反过来，如果要真正轮转 DEK，就必须重写数据本身。第四是**权限与审计的分离**：拿到存储桶的读权限而拿不到 `kms:Decrypt`，看到的只是密文；每一次解包都会落一条 CloudTrail，谁在什么时候开了哪只箱子有据可查。第五是**加密性销毁**（crypto-shredding）：销毁 KEK，等于同时销毁了所有依赖它的数据，不用去物理擦除分布在几十块盘上的副本——这是很多合规删除要求的实现方式，也意味着 KEK 的备份和权限策略是整条链上最脆弱的一环。最后一个实践细节：那"三十步"是有代价的，每次 unwrap 都是一次 KMS 往返，所以 S3 Bucket Keys、Kubernetes KMS provider v2 这类设计都会在一定范围内**缓存或复用 DEK**——用一点点爆炸半径去换掉大量 KMS 调用。

**隐喻对应表：**

- 箱子里的货 → 明文数据 / 对象本身
- 每箱一把新锁及其钥匙 → 每个对象一把随机的 *DEK*
- 塞进铜管、被印死的钥匙 → *wrapped DEK*（被 KEK 加密后的数据密钥）
- 铜管明着系在箱子提手上 → wrapped DEK 与密文存放在一起，公开可见也无妨
- 铁门后柜子里的大印，一辈子不出门 → *KEK* / master key，永不离开 KMS/HSM 边界
- 封印师本人 → KMS：只做 `Encrypt`/`Decrypt`，从不接触箱子里的货
- 老麦走的那三十步 → 一次 KMS API 往返（延迟成本，所以才有 DEK 缓存 / S3 Bucket Keys）
- 撬管钥匙断、撬锁撬不开 → 只拿到密文和 wrapped DEK 毫无用处
- 翻墙拖走的三只箱子 → 存储介质失窃 / 对象泄漏，静态加密仍然成立
- 配出一把钥匙只丢一只箱子 → 一对象一密钥，把爆炸半径限制在单个对象
- 每年换大印只重封两百根管子 → KEK 轮转 = 重新包裹 DEK，数据零重写
- 窗台上那本册子 → 审计日志（CloudTrail 里的 `kms:Decrypt`）
- 大印毁了，箱子永远打不开 → *crypto-shredding*：销毁 KEK 即销毁全部数据
- 三十年前那一把锁一把钥匙 → 单一共享密钥：一次泄漏，全埠沦陷
</section>
<section class="en" markdown="1">
At the west end of the customs pier stands a row of warehouses, and old Mai keeps them.

Thirty years ago, when he first took the job, the warehouse had exactly one lock. A fine lock — brass, six bolts, and he carried the key against his skin, even in his sleep. Every crate in the port sat behind that one door.

The trouble came in the seventh year. One night the key was lifted from the coat he had hung on the wall. The thief had only that one night — but for that one night, what he could open was the entire warehouse.

Old Mai changed his method after that.

These days, when you hand him a crate, he doesn't take your money first. In front of you he lifts a **brand-new lock** off the wall — one per crate, two hundred crates and two hundred locks, no two of them alike. He snaps it shut, and the key is in his hand: a small thing, bright as a new coin.

Then comes the part that puzzles strangers. He doesn't give you the key. He doesn't hang it on his belt, and he doesn't drop it in a drawer. He slides it into a small brass tube, screws the cap down, and carries the tube — only the tube; the crate stays where it is — across the yard to the thick round iron door at the far end.

Behind that door sits the sealkeeper. There is one of her in the whole port, and she never comes out. She takes the tube through the window slot and presses a **great seal** into the cap. The die for that seal lives in a cabinet at her feet and has never once left the room: no copy, no rubbing, and nobody has ever seen what it looks like — old Mai included. The sealed tube comes back out through the slot.

Old Mai ties the sealed tube to the crate's handle with hemp twine. Right there on the crate, hanging in plain sight, where anyone can see it.

A new hand asked on his first morning: if the key hangs on the crate, what is the lock even for?

Old Mai told him to try. The boy couldn't unscrew the tube — the cap was sealed shut. He pried at it with a knife; the tube split, and the key inside snapped with it. He turned to the lock on the crate — six bolts — and was still at it when the sun came up.

When goods are collected, old Mai unties the tube, walks the thirty paces, and hands it through the slot. The sealkeeper checks the seal, opens the tube, passes the key back out. He opens the crate, counts the goods, locks it again; the key goes back in the tube, gets sealed again, gets tied back on. She never touches a crate from beginning to end, and she has no idea whether it holds tea or gunpowder. She knows only tubes. On her sill lies a ledger: which tube, at what hour, handed in by whom — one line each.

Last winter it was put to the test. A gang came over the wall, worked three crates loose and dragged them off — brass tubes still tied to the handles, dragged off along with them. Two weeks later the goods turned up in the next port, crates still locked, tubes still sealed, not one of them opened. They got nothing, because the one thing they could not drag off was the iron door.

There was also the time someone did manage to copy a key, working from one particular lock. That one crate was lost. The two hundred beside it each had their own lock; the copied key wouldn't go halfway into the second keyhole.

But the finest bit of craft was something else. The sealkeeper changes the great seal once a year — the old die is melted down and a new one cast. You might imagine that day means opening two hundred crates, re-locking every one, re-tagging the lot. What old Mai actually did was this: carry two hundred brass tubes into the yard, hand them through the slot one at a time, old seal checked, new seal pressed, back out, tied back on. Done by midday. Not a crate moved, not a lock changed, not a bale of goods lifted — all that was resealed were two hundred little tubes.

The boy asked once: and if the great seal itself were destroyed? Then those two hundred crates never open again, old Mai said. Not lost — simply shut forever. He wasn't smiling when he said it.

— By now you have probably recognized it: old Mai's arrangement — one lock per crate, the key sealed inside a small tube, the tube hanging openly on the crate, the great seal that never leaves the iron door — is _envelope encryption_.

**The concept.** Envelope encryption is the default shape of encryption at rest on every major cloud. Data is never encrypted directly with the master key. Instead, each object (or each write) gets a freshly generated one-time symmetric _DEK_ (data encryption key), used locally for bulk encryption such as AES-GCM. That DEK is then handed to a KMS/HSM and encrypted once under a non-exportable _KEK_ (key encryption key, also called a master key or CMK), producing a short ciphertext — the _wrapped DEK_ — which is stored right next to the encrypted object. Reading reverses the flow: fetch the wrapped DEK, call `kms:Decrypt` to get the plaintext DEK back, decrypt the data locally, and wipe the DEK from memory immediately. The KEK never crosses the HSM boundary, and the KMS never sees your data.

**Why it matters.** First, **scale**: a KMS is a network service with rate limits and a payload ceiling of a few kilobytes; you cannot feed it a 5 TB volume. Keeping bulk encryption local and letting the KMS handle a few dozen bytes of key material is what makes the economics work at all. Second, **blast radius**: one DEK protects one object, so leaking one compromises nothing else — and it sidesteps cryptographic wear on a single key, since AES-GCM has a finite safe nonce space per key. Third, **cheap rotation**: rotating the KEK means re-wrapping those few-dozen-byte blobs, with not one byte of data rewritten. That is why S3 SSE-KMS, EBS, GCP CMEK, and Vault transit can promise master-key rotation on demand; rotating the DEK itself, by contrast, requires rewriting the data. Fourth, **separation of privilege and audit**: read access to the bucket without `kms:Decrypt` yields nothing but ciphertext, and every unwrap leaves a CloudTrail entry recording who opened which crate and when. Fifth, **crypto-shredding**: destroy the KEK and you have destroyed every object that depended on it, with no need to physically scrub replicas spread across dozens of disks — which is how a great deal of compliance-grade deletion is actually implemented, and also why KEK backup and key policy are the most fragile link in the chain. One last practical note: those thirty paces cost something. Every unwrap is a KMS round trip, which is why designs like S3 Bucket Keys and the Kubernetes KMS provider v2 **cache or reuse a DEK** across a bounded scope — trading a little blast radius for a great many fewer KMS calls.

**Metaphor mapping:**

- The goods inside the crate → the plaintext data / the object itself
- A brand-new lock and its key per crate → a random per-object _DEK_
- The key sealed inside the brass tube → the _wrapped DEK_ (data key encrypted under the KEK)
- The tube tied openly to the crate handle → wrapped DEK stored alongside the ciphertext, safe in plain sight
- The great seal in the cabinet that never leaves the room → the _KEK_ / master key, never leaving the KMS/HSM boundary
- The sealkeeper herself → the KMS: `Encrypt`/`Decrypt` only, never touching the goods
- The thirty paces across the yard → a KMS API round trip (latency, hence DEK caching / S3 Bucket Keys)
- Snapped key, unpickable lock → ciphertext plus wrapped DEK is useless on its own
- The three crates dragged over the wall → stolen media or leaked objects; encryption at rest still holds
- A copied key losing only one crate → per-object keys bound the blast radius
- Resealing two hundred tubes each year → KEK rotation = re-wrapping DEKs, zero data rewritten
- The ledger on the windowsill → the audit log (`kms:Decrypt` in CloudTrail)
- A destroyed seal die, crates shut forever → _crypto-shredding_: destroying the KEK destroys the data
- The single lock of thirty years ago → one shared key: one leak, and the whole port is open
</section>
