---
layout: fable
title: "正本斋的三支笔 · Three Brushes in the Scriptorium"
title_zh: "正本斋的三支笔"
title_en: "Three Brushes in the Scriptorium"
concept: "Kafka Replication (ISR, High Watermark, Leader Epoch)"
tags: [distributed-systems, databases]
illustration: /assets/art/2026-11-10-kafka-isr-high-watermark-leader-epoch.jpg
youtube_id: "n7mV4d0hqS0"
---
<section class="zh" markdown="1">
盐课司后院有一间没有窗的屋子，叫正本斋。

屋里只有一卷长纸，叫**正本**。纸从东墙的轴子上放出来，铺过三张长案，最后垂进西墙的铜槽。盐引、拨兑、罚没、补录，全都只在这卷纸上写，从尾巴往上写，一笔接一笔。不能插队，不能改前面的字。

写字的人只有一个，姓韩，人叫韩先生。他一天写四万笔。

案子后面隔一道纱屏，屏后坐着三个誊录手：陈五、阿泰，和一个不到二十的吕七。他们不写新账，只做一件事——**顺着正本，一笔一笔往自己的抄本上誊。** 每人一支笔、一盏油灯、一枚小铜镇纸。镇纸压在"我已经誊到哪儿了"的那一行上。那一行，斋里的人叫**笔头**。

查账的人不进这间屋子。他们在前院柜台上查，但柜台上有一条铁规矩：**只能查到封印线以上。**

封印线是许掌印手里那枚章盖的位置。许掌印不誊账，不写账，一辈子只做一件事：盖印。规矩是他父亲传下来的——**印，盖在三个誊录手笔头里最低的那一行的下一行。**

为什么盖在那儿？

因为笔头以下是三个人手里都确实有的账；笔头以上，只有韩先生一个人有。韩先生若今夜暴毙，斋里要从屏后挑一个人来接他的笔，而接的人只接得住自己抄到的地方。印盖在最低的那个笔头下面，等于对天下人宣布：**这行以上的账，换谁接笔都还在。**

盐商交引要回执。回执也照这条线开：韩先生写完不算数，三个誊录手的笔头都过了那一行，才盖印、才开回执。

那年八月，吕七的油灯漏了。

他不肯喊人来换，怕耽误抄账，就着半盏灯抄。灯不亮，抄得慢。韩先生写十行，他抄六行。三天下来，他的笔头落后了四千行。

封印线立刻不动了。

不是许掌印不肯盖，是那条规矩把印死死按住——最低的那个笔头就是吕七的笔头。韩先生照写，纸照长，可印一行也上不去。前院柜台上，六百个盐商排着队等回执，队伍从柜台一直排到大门外的槐树下。

许掌印没有先换灯。他先做的是另一件事：**把吕七的名字，从屏后那块"在位名册"上划掉了。**

名册上的规矩更冷：一个誊录手，若**一炷香之内笔头没往前挪过一行**，就不算在位。吕七的灯暗了三天，早就不算。名册上剩下陈五和阿泰，封印线随即跳到阿泰的笔头，印重新动起来，柜台重开。

被划出名册的人不许走。吕七换了新灯，人还坐在屏后，接着抄。他抄得比以前还整齐，一路追到韩先生的笔尾，然后一笔一笔跟着往下抄。可许掌印的印，不等他。

他抄下的那些行，**印一次也没盖过**。

第二年开春，韩先生死在案上，笔还压在最后一行。

许掌印当场要挑一个接笔的人。规矩第三条：**接笔的人只能从在位名册里挑，挑笔头最长的那一个。**

名册上是陈五和阿泰。陈五的笔头长，陈五接了笔。

这条规矩不只为"最长"两个字。名册里的人，笔头一定在封印线以上。所以不管挑谁，**封印线都不会往后退一行**，前一天开出去的回执也不会第二天变成假账。

新韩先生上任头一件事，是把正本卷首那行小字改了。原来写着"第六任，起于第八千一百一十二行"，他添成"第七任，起于第八千一百一十二行"。字稍大，用朱砂。

这是许家唯一一条被外人笑话过的规矩：**每一个接笔的人，都要在任期开头留一道任签**，写清自己是第几任、从哪一行起。老许掌印的原话是："将来总有人拿着尾巴来问你，那时候你别跟他算印，让他自己看签。"

吕七就是那年三月回来的。抄本誊到了第九千四百行——比新韩先生的笔尾长出二百八十行。

那二百八十行是老韩先生临死前写的：盐引三十七张，罚没两笔。印从没盖过。陈五和阿泰的抄本上没有。新韩先生的笔尾，就停在那二百八十行之前。

吕七抱着抄本去找许掌印："这二百八十行是真的。字是老韩先生写的，我亲眼看他落笔。"

许掌印说："是真的，也不算。印没盖过，名册上没有第二个人有这二百八十行。老韩先生写完那三十七张引，当场就去了——那三十七张引的盐，一担都没出过库。留在册上，就是让后人对着一笔从未发生的兑付查账。"

于是他让吕七做一件事：**把抄本上多出来的部分，撕掉。**

撕到哪儿？

第一个办法，是照自己抄本上的封印线撕。这个办法会把好账撕坏。

因为印是许掌印盖的，传到吕七手里有时差。他被划出名册那三天，印已经往前跳了一大截；而他抄本上记着的印，还停在老地方。照它撕，就撕得不够——剩下的那截账跟新正本对不上，明天还得再撕一次。若碰上印传得过快，又会撕得太多：一笔已经开过回执的账被他亲手撕了，柜台查得到，抄本上没了。

那半个月，正本斋一天来回撕抄十几遍，越撕越乱。陈五说，屋子里像有两个人各拿一把刀，谁也不肯先停手。

第二个办法，是许掌印后来才立成文的一条：**不比印，比任签。**

两个人的抄本上都抄着每卷开头那道朱砂签。吕七翻到自己抄本上最后一处和新正本对得上的签——"第六任，起于第八千一百一十二行"。新韩先生身上也有这一签，位置一样。

那就什么也不用算了。**这一签之后的所有行，全撕。**

撕完正正好好停在新韩先生笔尾的前一行。一行不多，一行不少。撕一次，永不回头。

这套办法能一次了结，是因为任签是**接笔人自己写的、只能往上加的号**，不是谁的观察记录。号是几，就从第几任之后撕，答案只有一个。

那年冬天，还出过一桩事。

连下半月雨，库房漏水，屏后三张案全泡了。名册上的人一个都誊不了。柜台上压着六百张回执，商会的人在外面砸门。

许掌印等了一夜，等到天亮，三个笔头一个也没动。

他开了祖上传下来的一条口子——**从已经划出名册的人里，挑一个来接笔。**

他挑的是当时笔头最长的那一个，恰恰是当年被划出去的人。理由是急：隔着屏的人已经十天没抄过账，可他有几百行没盖过印的账，全须全尾地揣在怀里。

新正本一立，封印线往后退了。

第二天，前院那些领了回执的盐商来查账，自己那一笔没了。不是被人涂掉，是从没在官册上存在过。盐已经出了库，回执上是朱红印，官册上是一片空白。

档册里管这件事叫"三百张回执案"。许掌印后来把一个铜楔钉进了那条口子的门框上，谁都拔不下来。他的说法很短：

**宁可柜台停三天，不要官册后退一行。**

到这里你大概已经认出来了：这就是 **Kafka 的副本复制——ISR、high watermark 和 leader epoch**。

### 这是什么

Kafka 的一个 partition 就是那卷正本：**只能追加，不能改写，每一笔有一个严格递增的行号（offset）。**

同一时刻只有一个副本接受写，叫 *leader*（韩先生）。其余副本叫 *follower*（陈五、阿泰、吕七），它们发 `Fetch` 请求顺着 leader 的 log 往前拉，**只读不写**。

- **LEO（log end offset）**：每个副本自己写到/抄到哪一行，即笔头。
- **HW（high watermark）**：ISR 中所有副本 LEO 的最小值，即封印线。消费者只能读到 HW 之前的消息。
- **ISR（in-sync replicas）**：跟得上的副本集合。判据是 `replica.lag.time.max.ms`（默认 30000 ms）——follower 在这个窗口内没有把 fetch 推进到 leader 的 LEO，就被踢出 ISR。注意它是**按时间**判的，不是按落后多少条：吞吐不同的 partition 之间，"落后多少条"没有可比性。
- **leader epoch**：leader 每换届一次，新任 leader 在自己的 log 里落一个 `(epoch, startOffset)` 标记。这就是朱砂任签。KIP-101 之后，follower 重连时不再用自己本地的 HW 去截断，而是发 `OffsetForLeaderEpoch`，双方找出**最后一段 epoch 相同的区段**，一次性 `truncate` 到那个区段的边界。

**为什么消费者只能读到 HW。** 因为 leader 随时会死，而可见的消息必须满足"换 leader 也不会消失"。HW 恰好是这个下界：所有 ISR 成员都追过了的位置。所以 leader 只能从 ISR 里选（*clean leader election*），新 leader 的 LEO ≥ HW，HW 只增不减，已开的回执不会变成假账。

**ISR 的收缩与扩张不对称。** 收缩让 HW 继续前进（这是拿一致性换可用性，也是唯一让柜台重开的办法）；扩张要求 follower 先追上 HW——不是追上 LEO——所以一个副本重新回到 ISR，不会把 HW 拖回去。

**三个把这条机制接到业务上的旋钮**：
- `acks=all` + `min.insync.replicas=N`：producer 只有在至少 N 个 ISR 副本追加成功后才收到 ack；ISR 数小于 N 时**拒写**，报 `NotEnoughReplicas`。这是"名册少于两人，韩先生停笔"。
- `unclean.leader.election.enable`：默认 `false`。设成 `true`，允许从 ISR 之外选 leader——封印线后退，已 ack 的消息消失，就是"三百张回执案"。
- `isolation.level=read_committed`：消费者只读到 HW；`read_uncommitted` 读到 leader 的 LEO，就会看见那些明天可能被撕掉的行。

### 为什么重要

**一、"提交"不是抽象概念，是一个可以写下来的式子。** 在 Kafka 里它就是 `committed = min(LEO of every ISR member)`。任何多副本系统都得回答同一个问题：一条记录在什么时刻才算数？答案从来不是"写成功了"，而是"**哪些副本上已经有了，且换谁当家都不会没**"。把这句话变成公式，你才看得出成本在哪：HW 的前进速度等于**最慢的那个 ISR 成员**的前进速度。一块慢盘可以把一整个 partition 的读可见性按在地上——这就是 ISR 收缩在运维上属于"必要的残忍"的原因。

**二、分歧的裁定标准必须来自权威的单调序列，不能来自各方的本地观察。** 这一条超出 Kafka：

- Raft 用 *term*，Paxos 用 *ballot*，Kafka 用 *leader epoch*，etcd 用 *revision*。
- 让 follower 拿自己本地的 HW 去决定截多少（Kafka 0.11 之前的做法），会得到震荡：截少了不收敛，截多了撕掉已提交的数据，于是截断—重拉—再截断，即 *truncation storm*。
- 用 leader 的任期去决定，则一次截断、确定收敛。吕七撕一次就不再回头，原因不在他更聪明，在于他问的是一个**只能往上加的号**。

这条规律在别处一样有效：Kubernetes 的 `resourceVersion`、Terraform 的 state serial、数据库的 LSN，都是"用权威的单调数判定分歧，而不是用观察者的猜测判定分歧"。

**三、可用性与正确性在这里是一个显式的开关，而且它会静默失败。** `unclean.leader.election.enable=true` 不会报错，不会告警，它只是让历史被改写。反过来说，`min.insync.replicas=2` + `acks=all` 的代价是：只要 ISR 掉到 1，写入就会**停下来**。选哪一个不是技术问题，是业务问题——但你必须知道自己在选，因为默认值替你选了一次。

**四、该盯的指标很具体，而且能被翻译成一句话。**

- `UnderReplicatedPartitions` > 0：有副本被划出名册了。
- `UnderMinIsrPartitions` > 0：名册人数不够，**此刻写入正在被拒**。这个值持续不为 0，等于你的可用性是假的——客户端拿不到 ack。
- `MaxLag` 与 `isr-shrink`/`isr-expand` 事件率：一个副本反复进出 ISR，说明它在临界点上抖，而每一次收缩都在放大 HW 的延迟。
- 消费者侧看 `records-lag-max` 与 `log-end-offset - high-watermark`：这个差就是"已经写进 Kafka、但还看不到"的部分，它由最慢的副本决定，不由你决定。

_隐喻对应表_

- 正本斋只有一卷长纸，从尾巴往上写，不能插队、不能改前面的字 → 一个 partition 是一条只能追加的 log，消息按 offset 严格有序
- 一行账 = 一笔盐引 → 一条消息 + 一个 offset
- 韩先生一个人写新账 → 同一时刻只有一个 *leader* 副本接受写
- 屏后的陈五、阿泰、吕七只顺着正本抄，从不写新账 → *follower* 副本通过 `Fetch` 从 leader 拉取，只读不写
- 铜镇纸压着"我已经誊到哪儿了"的那一行 → 每个副本的 **LEO**（log end offset）
- 封印线 = 三个笔头里最低的那一行的下一行 → **HW** = ISR 中所有副本 LEO 的最小值
- 柜台只能查到封印线以上 → 消费者只能读到 HW 之前的消息
- 韩先生写完不算数，三个笔头都过了才开回执 → `acks=all`：所有 ISR 副本都追加后才向 producer 确认
- 印一旦盖下就不再收回，已开的回执不会变假账 → 只要 leader 从 ISR 中产生，HW 单调不减，已提交的消息不会消失
- 一炷香之内笔头没挪过一行，就划出在位名册 → `replica.lag.time.max.ms`（默认 30s）为判据的 ISR 收缩
- 判据是"一炷香"而不是"落后多少行" → ISR 判据基于时间而非消息条数，吞吐不同的 partition 之间才有可比性
- 吕七的灯漏了，三天抄得比人慢，封印线就此不动，柜台排到槐树下 → 一个慢副本把整个 partition 的 HW 拖住，读可见性随最慢的 ISR 成员前进
- 划掉吕七之后印立刻往前跳，柜台重开 → ISR 收缩解除 HW 的阻塞，用一致性换回可用性
- 被划掉的人可以回来接着抄，但印不等他 → 掉出 ISR 的副本仍在复制，但它的进度不影响 HW
- 重新回到名册要先追到封印线，而不是追到笔尾 → 一个副本重新进入 ISR 需先追上 HW，因此不会把 HW 拖回去
- 接笔的人只能从在位名册里挑 → *clean leader election*，新 leader 只从 ISR 产生
- 挑名册里笔头最长的那一个 → 新 leader 取 ISR 中 LEO 最靠前的副本，保证 LEO ≥ HW
- 卷首那道朱砂任签："第七任，起于第八千一百一十二行" → **leader epoch**，`(epoch, startOffset)` 标记，随每次换届单调递增
- 吕七抄本上比新正本多出二百八十行没盖过印的账 → follower 的 log 尾部超出 HW 的未提交消息
- 撕掉多出来的部分 → **log truncation**
- 照自己抄本上的封印线撕，撕少了不收敛、撕多了撕掉已提交的账，一天来回十几遍 → 旧协议按本地 HW 截断导致的 *truncation storm*
- 不比印，比任签：找到最后一处相同的朱砂签，之后全撕 → `OffsetForLeaderEpoch`：以双方共享的最大 epoch 边界做截断点
- 撕一次就永不回头 → epoch 单调递增，截断点唯一且幂等，一次收敛
- 任签由接笔的人自己写、只能往上加 → epoch 由 leader 的任期决定，不由 follower 的本地观察决定
- 库里漏水，名册上无人可誊，许掌印从划掉的人里挑一个接笔 → `unclean.leader.election.enable=true`
- 新正本一立，封印线往后退，三百张回执变成空白 → HW 后退，已 ack 的消息丢失，历史被静默改写
- 名册少于两人，韩先生停笔，门上挂"暂不收引" → `min.insync.replicas` 未满足时拒写，报 `NotEnoughReplicas`
- 柜台有两种查法：只查到印，或查到韩先生笔尾 → `isolation.level=read_committed` 与 `read_uncommitted`
- 急性子查到的账第二天可能没了 → 读到 HW 之后的消息，就是读到了可能被截断的部分
- 门框上钉死的铜楔："宁可柜台停三天，不要官册后退一行" → 默认 `unclean.leader.election.enable=false`，正确性优先于可用性
</section>
<section class="en" markdown="1">
Behind the salt bureau there is a windowless room called the Master Scriptorium.

In it there is exactly one long scroll, called **the Master**. The paper runs off a roller on the east wall, across three tables, and down into a bronze trough in the west wall. Salt warrants, transfers, fines, supplementary entries — all of it is written on that one scroll, from the tail upward, entry after entry. Nobody jumps the line. Nobody alters a character already written.

Only one man writes: his surname is Han, and everyone calls him Master Han. He writes forty thousand entries a day.

Behind a gauze screen sit three copyists: Chen the Fifth, a boy called Tai, and Lü the Seventh, not yet twenty. They write no new entries. They do one thing — **they follow the Master and transcribe it, entry by entry, onto their own copies.** Each has a brush, an oil lamp, and a small bronze paperweight. The weight rests on the line that says *this is how far I have copied*. In that room, that line is called the **brush-head**.

Auditors do not enter the room. They work at the counter in the front courtyard, from the copies. But the counter has one iron rule: **you may read only up to the seal line.**

The seal line is wherever Seal-Keeper Xu has pressed his stamp. Xu copies nothing and writes nothing; all his life he has done one thing: press the seal. The rule came from his father — **the seal goes on the line just below the lowest brush-head of the three copyists.**

Why there?

Because below the brush-heads, all three men genuinely hold the entry. Above them, only Master Han holds it. If Master Han dies tonight, the room must pick someone from behind the screen to take up his brush, and that person can only reach as far as he has copied. Pressing the seal below the lowest brush-head declares to the world: **every entry above this line survives whoever takes the brush.**

A salt merchant who presents a warrant needs a receipt. The receipt follows the same line: Master Han's writing does not count by itself. Only when all three brush-heads have passed that line is the seal pressed and the receipt issued.

In the eighth month of that year, Lü the Seventh's lamp sprang a leak.

He would not call anyone to change it — he was afraid of losing copying time — so he worked by half a flame. Half a flame is slow. For every ten lines Master Han wrote, Lü copied six. Within three days his brush-head had fallen four thousand lines behind.

The seal line stopped dead.

Not because Xu refused to press. The rule held the seal down: the lowest brush-head was Lü's. Master Han kept writing, the paper kept growing, and the seal would not move a single line. At the front counter, six hundred salt merchants queued for receipts, from the counter out the gate and under the locust tree.

Xu did not change the lamp first. He did something else: **he struck Lü's name off the roster of those in place** behind the screen.

The roster's rule is colder still: a copyist whose brush-head has not advanced by one line **within the time it takes to burn one stick of incense** is no longer in place. Lü's lamp had been dim for three days, so he had long since ceased to be. The roster now held Chen the Fifth and Tai. The seal line jumped immediately to Tai's brush-head, the seal moved again, and the counter reopened.

A man struck off the roster is not sent away. Lü changed his lamp and stayed behind the screen, copying. He copied more neatly than before, caught up to Master Han's tail, and then followed it down line by line. But Xu's seal did not wait for him.

Not one of the lines Lü copied ever carried a seal.

The following spring, Master Han died at his table, his brush still weighing down the last line.

Xu had to choose a successor that same hour. The third rule: **the successor must come from the roster of those in place, and must be the one with the longest brush-head.**

The roster held Chen the Fifth and Tai. Chen's brush-head was longer, so Chen took the brush.

The rule was not only about length. Everyone on the roster has a brush-head above the seal line, so whoever is chosen, **the seal line cannot move backward by one line**, and yesterday's receipts cannot become tomorrow's forgeries.

The new Master Han's first act was to amend the small line at the head of the scroll. It read *Sixth tenure, beginning at line eight thousand one hundred twelve*; he made it *Seventh tenure, beginning at line eight thousand one hundred twelve*. Slightly larger characters, in vermillion.

It is the one Xu family rule that outsiders have always laughed at: **every man who takes up the brush leaves a tenure mark at the start of his term**, stating which tenure he is and which line he begins at. Old Xu's own words: "Someday a man will come to you holding a tail and asking questions. Do not argue with him about the seal. Let him read the marks."

Lü came back in the third month of that year. His copy had reached line nine thousand four hundred — two hundred eighty lines beyond the new Master Han's tail.

Those two hundred eighty lines were written by the old Master Han in the hour before he died: thirty-seven salt warrants, two fines. No seal had ever touched them. Chen's copy did not have them. Tai's did not. The new Master Han's tail stopped just short of them.

Lü carried his copy to Xu: "These two hundred eighty lines are real. The old Master Han wrote them. I watched his brush move."

Xu said: "Real, and still not counted. No seal touched them, and no second name on the roster holds them. The old Master Han wrote those thirty-seven warrants and died where he sat — not one load of that salt ever left the granary. Keep them in the book and you make every future auditor reconcile against a payment that never happened."

So he gave Lü one task: **tear out the surplus from your copy.**

Tear back to where?

The first method was to tear back to the seal line recorded in his own copy. That method destroys good entries.

The seal is pressed by Xu, and word of it reaches Lü late. In the three days Lü had been off the roster, the seal had already jumped a long way forward, while the seal recorded on Lü's copy still sat at the old place. Tear there and you have not torn enough: the remainder will not match the new Master, and tomorrow you tear again. And when word of the seal arrives too fast, you tear too much — you tear out an entry whose receipt is already in a merchant's hand. The counter can prove it existed. The copy no longer can.

For a fortnight the Scriptorium tore and recopied a dozen times a day, and each round was worse than the last. Chen said the room felt like two men with knives, neither willing to set his down first.

The second method is the one Xu later wrote down: **do not compare seals. Compare tenure marks.**

Both copies carry the vermillion mark at the head of every span of the Master. Lü turned to the last mark in his own copy that matched the new Master — *Sixth tenure, beginning at line eight thousand one hundred twelve*. The new Master Han carried the same mark, in the same place.

Then there is nothing to calculate. **Everything after that mark gets torn out.**

What remains stops exactly one line short of the new Master Han's tail. Not a line too many, not a line too few. One tear, and no going back.

This method settles in a single pass because a tenure mark is **written by the man who takes the brush, and can only count upward.** It is not anyone's observation record. If the mark says sixth, you tear everything after the sixth. There is only one answer.

That winter, one more thing happened.

Rain fell for half a month, the storeroom leaked, and all three tables behind the screen were soaked. Not one man on the roster could copy. Six hundred receipts sat at the counter and the guild was hammering on the gate.

Xu waited one night. At dawn, not one of the three brush-heads had moved.

He opened a door his family had left in the rules: **choose a successor from among those already struck off the roster.**

He chose the man with the longest brush-head at that moment — the very man who had been struck off years before. The reason was urgency: the man behind the screen had not copied in ten days, but he carried several hundred unsealed lines, complete and intact.

The new Master was installed, and the seal line moved backward.

The next day, merchants who came to the counter with receipts in hand found their entry gone. Not struck out. Never present in the official book. The salt had left the granary, the receipt bore a vermillion seal, and the register was blank.

The archive calls it the Case of the Three Hundred Receipts. Xu drove a bronze wedge into the frame of that door afterward, and no one has been able to pull it out. His explanation was short:

**Better the counter closes for three days than the register moves back one line.**

By now you have probably recognized it: this is **Kafka replication — ISR, the high watermark, and leader epoch**.

### What it is

A Kafka partition is that scroll: **append-only, no rewriting, every entry carrying a strictly increasing line number (offset).**

At any moment exactly one replica accepts writes, the *leader* (Master Han). The others are *followers* (Chen, Tai, Lü). They issue `Fetch` requests and pull forward along the leader's log, **read-only, never writing**.

- **LEO (log end offset)** — how far each replica has written or copied: the brush-head.
- **HW (high watermark)** — the minimum LEO across the ISR: the seal line. Consumers can read only up to the HW.
- **ISR (in-sync replicas)** — the set of replicas that keep up. The test is `replica.lag.time.max.ms` (default 30000 ms): a follower that has not advanced its fetch to the leader's LEO within that window is removed from the ISR. Note that it is **time-based**, not count-based: "how many records behind" is not comparable across partitions with different throughput.
- **leader epoch** — every time leadership changes, the new leader writes an `(epoch, startOffset)` marker into its log. That is the vermillion tenure mark. Since KIP-101, a reconnecting follower no longer truncates using its own local HW; it sends `OffsetForLeaderEpoch`, the two sides find **the last span whose epoch matches**, and the follower truncates once to that boundary.

**Why consumers stop at the HW.** Because the leader can die at any moment, and a visible message must satisfy "it survives a leader change." The HW is exactly that lower bound: the position every ISR member has passed. So a leader is chosen only from the ISR (*clean leader election*), the new leader's LEO is ≥ HW, the HW never decreases, and receipts already issued never become forgeries.

**Shrinking and expanding the ISR are not symmetric.** Shrinking lets the HW move again — trading consistency for availability, and the only way to reopen the counter. Expanding requires the follower to catch up to the HW, not to the LEO, so a replica rejoining the ISR cannot drag the HW backward.

**Three knobs that connect this mechanism to your business:**
- `acks=all` + `min.insync.replicas=N` — the producer is acknowledged only after at least N ISR replicas have appended; when the ISR falls below N, writes are **rejected** with `NotEnoughReplicas`. This is "fewer than two names on the roster, Master Han stops writing."
- `unclean.leader.election.enable` — `false` by default. Set it to `true` and you allow a leader from outside the ISR: the seal line moves backward, acknowledged messages vanish, and you get the Case of the Three Hundred Receipts.
- `isolation.level=read_committed` — consumers read only to the HW. `read_uncommitted` reads to the leader's LEO, which means reading lines that may be torn out tomorrow.

### Why it matters

**One. "Committed" is not an abstraction; it is a formula you can write down.** In Kafka it is `committed = min(LEO of every ISR member)`. Every replicated system must answer the same question: at what instant does a record count? The answer is never "the write succeeded." It is "**which replicas hold it, and would it still hold if the wrong man took the brush?**" Put that into a formula and the cost becomes visible: the HW advances at the speed of **the slowest ISR member**. One slow disk can pin an entire partition's read visibility to the floor. That is why ISR shrinking counts as necessary cruelty in operations.

**Two. The arbiter of divergence must be an authoritative monotonic sequence, never a local observation.** This rule reaches well beyond Kafka:

- Raft uses a *term*, Paxos a *ballot*, Kafka a *leader epoch*, etcd a *revision*.
- Let a follower truncate based on its own local HW — Kafka before 0.11 — and you get oscillation: truncate too little and it never converges, truncate too much and you delete committed data, so it truncates, refetches, and truncates again. A *truncation storm*.
- Decide with the leader's tenure and you truncate once, deterministically. Lü tears once and never looks back — not because he is cleverer, but because he asked about **a number that can only count upward**.

The same rule works elsewhere: Kubernetes `resourceVersion`, a Terraform state serial, a database LSN. All of them settle divergence with an authoritative monotonic number instead of an observer's guess.

**Three. Availability versus correctness is an explicit switch here, and it fails silently.** `unclean.leader.election.enable=true` throws no error and raises no alarm; it simply lets history be rewritten. Conversely, `min.insync.replicas=2` with `acks=all` means that the moment the ISR drops to one, writes **stop**. Choosing between them is a business decision, not a technical one — but you should know you are choosing, because a default already chose for you.

**Four. The metrics to watch are specific, and each translates into one sentence.**

- `UnderReplicatedPartitions` > 0 — some replica has been struck off the roster.
- `UnderMinIsrPartitions` > 0 — the roster is too thin and **writes are being rejected right now**. A sustained non-zero value means your availability is fictional: clients are not getting acks.
- `MaxLag`, plus the rate of `isr-shrink` / `isr-expand` — a replica bouncing in and out of the ISR is oscillating at the threshold, and every shrink lengthens the HW's delay.
- On the consumer side, `records-lag-max` and `log-end-offset - high-watermark`: that difference is what is already in Kafka but not yet visible, and it is set by the slowest replica, not by you.

_Metaphor mapping_

- One long scroll, written from the tail upward, no jumping the line, no altering earlier characters → a partition is an append-only log; records are strictly ordered by offset
- One line of the ledger = one salt warrant → one message plus one offset
- Master Han alone writes new entries → exactly one *leader* replica accepts writes at a time
- Chen, Tai, and Lü only follow the Master and never write new entries → *followers* pull from the leader via `Fetch`, read-only
- The bronze weight rests on "how far I have copied" → each replica's **LEO** (log end offset)
- The seal line sits just below the lowest of the three brush-heads → **HW** = minimum LEO across the ISR
- The counter may read only up to the seal line → consumers can read only up to the HW
- Master Han's writing does not count; all three brush-heads must pass the line before a receipt is issued → `acks=all`: confirm to the producer only after every ISR replica has appended
- A pressed seal is never retracted, and issued receipts never become forgeries → with leadership confined to the ISR, the HW is monotonic and committed messages never disappear
- A brush-head that does not advance within one stick of incense is struck off the roster → ISR shrink governed by `replica.lag.time.max.ms` (default 30s)
- The test is "one stick of incense," not "how many lines behind" → ISR membership is time-based rather than count-based, so it is comparable across partitions of different throughput
- Lü's leaking lamp makes him copy slowly for three days; the seal freezes and the queue reaches the locust tree → one slow replica pins the whole partition's HW; read visibility advances at the speed of the slowest ISR member
- Striking Lü off lets the seal jump forward and the counter reopen → ISR shrink unblocks the HW, trading consistency for availability
- The struck-off man may return and keep copying, but the seal does not wait for him → a replica outside the ISR keeps replicating, but its progress does not affect the HW
- Rejoining the roster requires catching up to the seal line, not to the tail → a replica re-enters the ISR only after reaching the HW, so it cannot drag the HW back
- The successor must come from the roster of those in place → *clean leader election*: only an ISR member can become leader
- Choose the roster member with the longest brush-head → the new leader is the ISR replica with the highest LEO, guaranteeing LEO ≥ HW
- The vermillion mark at the head: "Seventh tenure, beginning at line eight thousand one hundred twelve" → **leader epoch**, an `(epoch, startOffset)` marker that increments monotonically with each leadership change
- Lü's copy holds two hundred eighty unsealed lines beyond the new Master → a follower's log tail holding uncommitted messages past the HW
- Tearing out the surplus → **log truncation**
- Tearing back to the seal recorded in one's own copy under- or over-tears, a dozen rounds a day → the *truncation storm* caused by the old local-HW truncation protocol
- Do not compare seals; compare tenure marks — find the last shared vermillion mark and tear everything after it → `OffsetForLeaderEpoch`: truncate at the boundary of the greatest epoch the two sides share
- One tear, never revisited → epochs increase monotonically, making the truncation point unique, idempotent, and convergent in one pass
- The tenure mark is written by the incoming man and can only count upward → the epoch is set by the leader's tenure, not by a follower's local observation
- The storeroom floods, nobody on the roster can copy, and Xu appoints a struck-off man → `unclean.leader.election.enable=true`
- The new Master is installed, the seal line retreats, three hundred receipts turn blank → the HW moves backward, acknowledged messages are lost, and history is silently rewritten
- Fewer than two names on the roster and Master Han stops writing, hanging out a "not accepting warrants" sign → writes rejected with `NotEnoughReplicas` when `min.insync.replicas` is unmet
- Two ways to read at the counter: up to the seal, or up to Master Han's tail → `isolation.level=read_committed` versus `read_uncommitted`
- What the impatient reader sees today may be gone tomorrow → reading past the HW means reading content that may yet be truncated
- The bronze wedge nailed into the door frame: "better the counter closes for three days than the register moves back one line" → the default `unclean.leader.election.enable=false`; correctness before availability
</section>
