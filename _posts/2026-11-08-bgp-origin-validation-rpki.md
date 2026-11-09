---
layout: fable
title: "谁都能立一块路牌 · Anyone May Plant a Signpost"
title_zh: "谁都能立一块路牌"
title_en: "Anyone May Plant a Signpost"
concept: "BGP Route Origin Validation (RPKI ROAs and ROV)"
tags: [networking, security]
illustration: /assets/art/2026-11-08-bgp-origin-validation-rpki.jpg
---
<section class="zh" markdown="1">
从悬河渡往西，翻三道岭，是一条走了一千年的驿道。

道上一共四十七家驿铺，铺子之间没有上下之分，谁也不管谁。规矩只有一条，是几百年里自己长出来的：**每个铺子在门口立一块木牌，写上"本铺可发往某地"；脚夫到了岔口，看哪块牌子写的地方跟信上的地址对得上，就把信交给哪一铺。**

这条规矩能成，靠的是一个补丁式的细节：**牌子写得越细，越算数。** 一块写"盐铁镇"的牌，和一块写"盐铁镇第七坊"的牌，同时出现在一个岔口，脚夫一定走后者。谁写得细，信就归谁。

一千年来，从来没有人问过一句话：**这块牌子，是谁准你立的？**

盐铁镇的盐引、汇通号的汇票、官府的火漆札子，全都走这条路。旺季一天四万封。

出事那年秋天，断崖口多了一家铺子。

铺主姓侯，排行第三，早年在盐铁镇做过账，认得盐铁镇的路数。他在断崖口租了半间土房，门口立了一块牌，用旧墨描了五个字：

**盐铁镇第七坊。**

岔口上原本那块牌写的是"盐铁镇"，粗。侯三的牌，细。于是那一天发往第七坊的全部信件——盐引、汇票、札子——一封不落，全进了侯三的土房。

侯三没抢，也没撕。他只是把信拆开，抄一遍，再原样封好，隔三天缓缓发出去。抄一遍，汇票上的数额、盐引上的批号、札子上的人名，全在他手里了。

官府来查，只查到一件事：**那块牌子，不假。**

因为这条路从来就没有"假牌"这回事。谁都可以立牌，立了就算。侯三甚至不需要撒谎，他只需要挑一个没人写过那么细的地方，写得更细一点。

那年秋天，盐铁镇丢了十一万两银子的汇票。

**第一个办法是派人守岔口。**

盐铁镇雇了二十个拳师，分守在通往镇上的五个岔口，见陌生牌子就砸。砸了一个月，砸了十一块，信照旧丢。

因为路是传递的。信从断崖口进来，侯三并不需要直接把信送到第七坊——他把信交给下家，下家再交给下下家。每个岔口守着的拳师只看得见自己眼前那一段路，而信在四十七家铺子之间转手六七道。田家的信从南岭来，先到中路驿，再到西关驿，最后从断崖口进镇。断崖口那块牌子，拳师砸了七次，侯三立了八次。

砸不动。这条路的问题不在岔口，在**每一家铺子都默认相信上家给它的牌**。

**第二个办法是总驿署的关防。**

总驿署在省城，管着全道的驿册。它想出的是另一条路：不靠砸，靠**备案**。

每家铺子每年到总驿署立一份**关防**。关防上写三样东西，缺一不可：

- 一是这个铺子**代收哪一段地方**，写到镇为止（比如"盐铁镇全域"）；
- 二是这块地方**只有哪一家铺子可以代收**，写上那家铺子的名字，一个名字，不许改；
- 三是**最细能到哪一层**：盐铁镇的关防上写着"至镇为限，坊巷不在其内"。

关防由总驿署用火漆封好，一份留在省城，一份抄成一册，叫**关防册**。册子由塘报发往全道每一个岔口的**关验吏**，每半个时辰补一次新条目。

关验吏手里只有这一本册子，和一句话：**你只验一块牌上的最后一个名字。**

从此岔口上多了一道工序。牌立起来，关验吏翻册子，只有三种结果：

- **有案可稽，且牌上的名字跟关防上写的对得上** —— 准。牌子挂起来，信优先往这条路上发。
- **有案可稽，但名字对不上** —— 斥。牌子当场拆掉，信退回去。这一条杀死了绝大多数伪造。
- **册子上查无此案** —— 依老规矩，放行。

侯三的牌子就是在这个秋天被拆的。关验吏翻开册子，盐铁镇的关防明明白白写着"至镇为限，坊巷不在其内"。侯三写的是"第七坊"，已经越过了关防准许的最细一层。**斥。**

牌子拆了，人也押走了。盐铁镇那一年冬天睡得踏实。

**但第二年春天，信又开始走了不该走的路。**

这一次没有假牌。这一次每一块牌都是真的。

事情出在中路驿。中路驿是西关驿的老客户，每年按季结一次驿费——它的信，本来就该从西关驿过。可中路驿同时也在替北岭驿发信：北岭驿的信路过中路驿，本该往北走，回北岭自己那片山里；中路驿却把那些信一并塞进了往西关驿的口袋。

牌上写的是"盐铁镇"，名字是真的，关防对得上，**准**。

关验吏放行了。他按规矩只验了最后一个名字，而这个规矩是他被教会的：一块牌要证明的，只是"这些信确实是盐铁镇发出的"。

它从来不证明**这些信是怎么走到这儿的**。

于是整整一个春天，北岭那片山谷的信，全都从西关驿绕了一个大圈。北路三家铺子一封也收不到，驿费白交；西关驿被硬塞了四倍的量，先是慢，后来是塌——雪化那半个月，西关驿门口积了九千封信，铺子当天关门，从悬河渡到盐铁镇，整条道的信停了四天。

这一种，比侯三厉害得多。侯三要伪造一个身份；中路驿什么都不用伪造，它只需要**把别人的信走一条不该走的路**。前者是撒谎，后者是占道。

而关防管不住占道，因为**关防管的是名字，不是路**。

**第三个办法，是给路上每一段也备案。**

总驿署最后补的那一册，叫**《上游名册》**。

每一家铺子除了立关防，还要再立一份：**我这一铺，只从哪几家上游铺子领信。** 别家的信我不接，我也不往别家转。这份名册同样火漆封存，同样进塘报。

关验吏从此要看两样东西，看牌上的整条路线：

- 牌上那个**最后的**名字，对得上关防吗？（信是谁发的）
- 牌上**每一段路的上下两家**，对得上名册吗？（路是谁准的）

第二条一加，中路驿的把戏当场露了：牌上"中路驿→西关驿"这一段，西关驿的名册里，中路驿不在其上游之列。**斥。**

可是到了这一步，官府也终于看清了一件事：**这套东西并不能拦住所有的坏牌，它只能拦住"写着别人名字的坏牌"。**

册子上查无此案的，依老规矩，放行。

全道四十七家铺子里，按时立了关防的只有三十一家。另外十六家嫌麻烦——立关防要跑一趟省城，要交火漆钱，要每年续。它们不立，它们的信照旧走，关验吏照旧放行，只是排在后面。

也就是说，最容易被伪造的那一片地方，恰恰是没人去备案的那一片。

而且塘报每半个时辰才补一次。侯三真正该挑的日子，不是秋天，是塘报刚送出、册子还没到的那半个时辰。

至于那块写着"盐铁镇第七坊"的木牌——它现在被钉在省城驿署的墙上，作为一条教训挂着。牌上那五个字是假的，可在那天之前，**没有任何一枚火漆证明过它是假的**，因为这条路从设计的第一天起，就只保证了"信能被送到"，没保证过"信是谁发的"。

到这儿，你大概已经认出来了：这就是 **BGP 路由起源验证**（*BGP route origin validation*，*RPKI* 加 *ROV*），以及它旁边那两件绕不开的事——**路由泄漏**（*route leak*）与 **ASPA**（*Autonomous System Provider Authorization*）。

### 这是什么

先说旧的世界。

互联网由几万个自治系统（*Autonomous System*，*AS*）拼成。每个 AS 拥有一段或几段 IP 地址（*prefix*，前缀），它向邻居宣告："这段地址在我这儿，发给我。" 邻居收下，改一改，再往外宣告。这就是 *BGP*。

BGP 的原初设计里**没有任何起源认证**。收到一条宣告，你能确认的只有一件事：它是从邻居那儿来的。至于"这段地址真的属于宣告它的那个 AS 吗"，协议一个字都没说。路由表因此是一张**默认信任**的表。

再加上一条转发规则，问题就被放大了：**最长前缀匹配**（*longest prefix match*）。一个 AS 合法地宣告了 `203.0.113.0/16`，攻击者宣告一条 `203.0.113.7/24`——更具体的那条永远赢。他不需要覆盖整个 /16，只需要挑一个 /24 写得更细。这就是侯三的五个字。

**RPKI**（*Resource Public Key Infrastructure*）的做法是给"牌子"配一本**备案册**。

它的信任链是自顶向下的：IANA 是根（*trust anchor*）→ 五大区域注册局（RIR）→ 国家级注册局 → 会员单位（ISP、云厂商、企业）→ 具体的前缀持有者。每一层用自己的私钥为下一层签一张证书，声明"我授权你持有这一段地址"。整棵树是一套 *X.509* 证书体系，公开可校验。

叶子节点上签发的，就是 **ROA**（*Route Origin Authorization*，RFC 6482）。一个 ROA 只有三个字段：

- `prefix` —— 这段地址（"盐铁镇"）
- `maxLength` —— 这份授权最细覆盖到多少位（"至镇为限"）
- `origin AS` —— 唯一被授权宣告它的 AS（"只有这一家可以代收"）

独立的 *RPKI validator* 把所有 ROA 汇总、逐条验签，产出一个集合，叫 **VRP**（*Validated ROA Payload*）。VRP 通过 **RTR 协议**（*RPKI-to-Router*，RFC 8210，增量推送）送到路由器上——这就是塘报。

路由器对每一条收到的 BGP 宣告做一次判定，只有三种结果，术语叫 **ROV**（*Route Origin Validation*，RFC 6811）：

- ***Valid*** —— 存在覆盖该前缀的 ROA，`maxLength` 也够，且 AS_PATH 末位的 origin 与 ROA 一致：接受，可以调高 *local-pref*。
- ***Invalid*** —— 存在覆盖该前缀的 ROA，但 origin 不符，或者前缀长度超过 `maxLength`：**丢弃 / 降级**。全部收益都在这一格里。
- ***NotFound*** —— 根本没有任何 ROA 覆盖它：**按传统规则接受**，只是不给优先级。

注意第三行。ROV 的默认动作是"没备案就放行"，不是"没备案就拒绝"。这不是疏忽，是部署现实：一步走到"白名单"会让半个互联网在一天内断掉。代价是明确的——**攻击者最理性的选择，永远是去打那片没有备案的地方。**

而 `maxLength` 是这套体系里最容易被写松的一格。写成 `/16-/24` 就等于说"这段地址下面任何一条 /24 都算我授权的"，于是别人宣告一条 /24，判定是 *Valid*，不是 *Invalid*。收得越松，越安全的是自己的可用性，越危险的是别人的。

还有一件关防永远做不到的事：**ROV 只看 AS_PATH 的最后一个 AS。**

它回答的是"这封信是谁发的"，不是"这封信是怎么走到这儿的"。于是有了另一类事故——**路由泄漏**：一个 AS 把从上游 A 学到的路由，转手告诉了上游 B。前缀是真的，origin 是真的，签也验得过，只是这条路本来不该存在。它的杀伤力和前缀劫持同级，甚至更大：2019 年那家把自家优化器配置推错、把全网的 Google 流量吸进来两小时的 ISP，用的就是这个机制，全程没有一条伪造的宣告。

**ASPA** 就是补这一格的：AS 到 RPKI 里再签一份"我授权的上游是谁"的清单，验证器沿 AS_PATH 逐跳检查上下家关系是否被授权。它检的是路，不是名字。

顺带把两个词分清楚，它们在新闻里常被混着写：

- **前缀劫持**（*hijack*）：宣告不属于自己的前缀，或用自己的 AS 冒充别人的 origin —— 侯三。
- **路由泄漏**（*leak*）：宣告一条自己有资格宣告、但没有资格传出去的路由 —— 中路驿。

### 为什么重要

原因只有一句：**整个互联网的转发，建立在一个没有认证的协议上，而认证是二十年后才补上去的一层外挂。**

三个直接的后果：

**一、更具体的宣告永远赢，这使"小规模"的攻击变得极其便宜。** 2008 年巴基斯坦电信为封锁国内 YouTube，宣告了一条 /24，结果这条更具体的路由顺着 BGP 扩散到全世界，把 YouTube 全球流量吸了过去、黑洞了两小时——不是攻击，是一次配置失误。2018 年，攻击者用同样的手法，通过劫持一家云厂商的 DNS 服务所依赖的路由，把 myetherwallet.com 的解析导向了自己的服务器。这两件事的机制，和侯三那五个字完全一样。

**二、ROV 的收益是真实的，但它的覆盖面才是它的上限。** *Invalid* 一旦被丢弃，绝大多数起源伪造立刻失效，这是这些年最大的一次互联网加固。但剩下的 *NotFound* 是一大片默认放行的空间，而部署速度受制于一件很朴素的事：**得有人愿意跑一趟省城，交那笔火漆钱。** 这也是为什么几乎所有 RIR 都在把 ROA 签发做成点几下鼠标的事，为什么大型云厂商在替客户代签。

**三、它把一个旧直觉推翻了：路由是有权威的。** 长期以来"路由"被当成一个纯技术事实——网络说这条路径通，它就通。RPKI 把它变成一个**授权事实**：这条路径通不通，取决于某个注册体系里有没有一枚签过字的记录。今天你在云上给一段弹性 IP 配 BGP 宣告，背后就挂着这条链：前缀在谁的证书下、origin 是谁、`maxLength` 写多宽。

这条链的每一环都是薄的，但缺任何一环，剩下的都不成立。这和 *mTLS* 的证书链、*DNSSEC* 的委派链、OIDC 的 *JWKS* 是同一个形状：**信任不是"我知道你是你"，信任是"有人为我签过字，而我能一路查到根"。**

_隐喻对应表_

- 每个驿铺立块木牌写明"可发往某地"，脚夫照牌走 → BGP 宣告：每个 AS 向邻居宣告自己持有并服务的前缀
- 岔口上没有任何人核验牌子的来处 → BGP 原初设计不含起源认证，路由表默认信任
- "牌子写得越细越算数" → 最长前缀匹配（longest prefix match）
- 侯三立一块"盐铁镇第七坊"，盖住别人的"盐铁镇" → 前缀劫持（hijack）：宣告更具体的 /24 吸走 /16 的流量
- 路人只看得见自己眼前那段路 → 逐跳信任：每个 AS 只信任邻居递过来的路由
- 信在四十七家铺子间转手六七道 → 出问题时定位极难，路径在途、随时变
- 田家的信经中路驿、西关驿到断崖口 → AS_PATH：一条路由经过的 AS 序列
- 拳师守五个岔口，砸一块立一块 → 用 ACL/前缀过滤逐点封堵，事实上不可维护
- 总驿署的关防：本铺代收哪一段、唯一代收者、最细到哪一层 → ROA 的三个字段：prefix、origin AS、maxLength
- 关防一年一签、火漆封存、存根留底 → X.509 证书与 ROA 的签名体系
- 关防从 IANA→RIR→国家注册局→会员→持有者逐层授权 → RPKI 的分层信任链（trust anchor 到 end entity）
- 关防册汇总全道所有关防，塘报发到每个岔口 → VRP 集合由 validator 汇总，经 RTR 协议增量推送到路由器
- 半个时辰补一次新条目 → RTR 增量刷新；册子到手之前是盲区
- 关验吏手里只验牌上最后一个名字 → ROV 只校验 AS_PATH 末位的 origin AS
- 三种结果：准 / 斥 / 依老规矩放行 → Valid / Invalid / NotFound
- "查无此案，依老规矩放行" → NotFound 默认接受，是全部残余风险的来源
- "至镇为限，坊巷不在其内" → maxLength 限制 ROA 覆盖的最大前缀长度
- maxLength 写成"凡其下属坊巷皆可" → maxLength 过宽，把本该 Invalid 的宣告变成 Valid
- 按时立关防的只有三十一家，其余十六家放行但排后 → ROA 覆盖率不足，未覆盖空间是攻击面的主体
- 立关防要跑一趟省城、交火漆钱 → RPKI 的部署成本与 ROA 签发的组织摩擦
- 侯三牌子被拆，但全镇只睡了一个冬天 → 单点修补解决了劫持，却不解决机制本身
- 中路驿把北岭驿的信塞进往西关驿的口袋 → 路由泄漏（route leak）：合法前缀、非法路径
- 牌上的名字是真的、路是错的 → ROV 不检路径，只检名单
- 西关驿一夜积九千封，全道停四天 → 泄漏造成的拥塞、丢包与大规模不可达
- 关防管的是名字，不是路 → 起源验证与路径验证的能力边界
- 《上游名册》：我这一铺只从哪几家领信 → ASPA：AS 签名的授权上游集合
- 逐段核对上下家是否对得上名册 → ASPA 沿 AS_PATH 逐跳校验上下家关系
- 关防被钉在省城墙上做教训 → 事故与 RFC 一起写进这套体系的记忆里
</section>
<section class="en" markdown="1">
West from Suspended River Ford, over three ridges, runs a post road a thousand years old.

Along it stand forty-seven relay posts. There is no hierarchy among them and no authority over them. Only one rule, grown up out of centuries by itself: **each post plants a wooden board at its door reading "this post carries mail toward such-and-such a place"; a porter reaching a fork reads the boards, finds the one whose place matches the address on the letter, and hands the letter over there.**

That rule works because of one patch-like detail: **the more specific the board, the more it counts.** A board reading "Salt-Iron Town" and a board reading "Salt-Iron Town, Seventh Ward" hanging at the same fork — the porter always takes the second. Whoever writes it finer, gets the mail.

In a thousand years, nobody ever asked one question: **who gave you permission to plant this board?**

Salt certificates from Salt-Iron Town, drafts from the Exchange House, government pouches sealed in red wax — all of it travelled this road. In high season, forty thousand letters a day.

The autumn it broke, a new post appeared at Broken Cliff Mouth.

Its keeper was named Hou the Third. He had kept books in Salt-Iron Town years before and knew its ways. He rented half a mud hut at Broken Cliff Mouth and planted a board at the door, inked in old soot, five characters:

**Salt-Iron Town, Seventh Ward.**

The board already at that fork read "Salt-Iron Town" — coarse. Hou's board was fine. So every letter bound for the Seventh Ward that day — salt certificates, drafts, pouches — went into Hou's mud hut, not one lost.

He robbed nothing and tore nothing. He opened each letter, copied it, sealed it back exactly, and sent it on after three days. Copying it put the amount on every draft, the batch number on every certificate, the names on every pouch, in his hands.

The magistrate came and found exactly one thing: **the board was not false.**

Because on this road there had never been such a thing as a false board. Anyone may plant one, and a planted board counts. Hou did not even need to lie. He needed only to pick a place nobody had bothered to describe that finely, and describe it one notch finer.

That autumn Salt-Iron Town lost drafts worth a hundred and ten thousand taels.

**The first remedy was to post guards at the forks.**

The town hired twenty boxers and set them at the five forks leading in, to smash any unfamiliar board. A month of smashing: eleven boards destroyed, and the letters kept vanishing.

Because the road is transitive. From Broken Cliff Mouth, Hou never had to deliver a letter to the Seventh Ward himself — he handed it to the next post, which handed it to the next. A boxer at a fork sees only his own stretch of road, and letters changed hands six or seven times among the forty-seven posts. The Tian family's letters came from the South Ridge, reached Middle Road Post, then West Gate Post, and finally entered town at Broken Cliff Mouth. The boxers smashed that board seven times. Hou planted it eight.

Smashing could not win. The road's problem was not at the forks. It was that **every post believed, by default, the board its neighbour handed it.**

**The second remedy was the Grand Relay Office's seal-registers.**

The Grand Relay Office sat in the provincial capital and held the road's whole roll. What it devised was not smashing but **filing**.

Every post came once a year and deposited a **sealed register entry**. It carried three things, and omitting any one voided it:

- first, **which stretch this post collects for**, described down to the town and no further (say, "the whole of Salt-Iron Town");
- second, **which single post may collect for that stretch** — one name, unchangeable;
- third, **the finest level permitted**: Salt-Iron Town's entry read "to the town as limit; wards and lanes not included."

The Office sealed each with red wax, kept one copy, and had the rest copied into a **register book**. Couriers carried the book to the **validating clerk** at every fork on the road, with new entries appended every half hour.

The clerk had only that book, and one instruction: **you verify the last name on the board, and nothing else.**

So the forks gained one procedure. A board goes up, the clerk opens the book, and there are three outcomes:

- **On record, and the name on the board matches** — allowed. The board hangs, and mail is dispatched down that road first.
- **On record, but the name does not match** — refused. The board comes down and the letters go back. This killed the great majority of forgeries outright.
- **No record at all** — by the old custom, passed through.

Hou's board came down that autumn. The clerk opened the book, and Salt-Iron Town's entry said plainly: "to the town as limit; wards and lanes not included." Hou had written "Seventh Ward," one notch past the finest level the entry permitted. **Refused.**

The board came down, the man was taken away, and Salt-Iron Town slept well that winter.

**But the next spring, letters began travelling roads they should never have travelled.**

This time there was no false board. This time every board was true.

It happened at Middle Road Post. Middle Road was West Gate's old customer, settling its relay fee quarterly — its mail was supposed to come through West Gate. But Middle Road was also carrying for North Ridge: North Ridge's letters passed through Middle Road and should have turned north, back into its own mountains; Middle Road instead stuffed them into the same sack headed for West Gate.

The board said "Salt-Iron Town." The name was real. The register matched. **Allowed.**

The clerk passed it. By the rule he had been taught, he had verified only the last name — and that rule rested on an assumption he never examined: that a board need only prove **these letters really were sent by Salt-Iron Town**.

It never proved **how these letters got here**.

So for a whole spring, the mail of an entire northern valley went the long way round through West Gate. Three posts on the north road collected nothing and paid their relay fees for air; West Gate took four times its capacity — slow at first, then collapsing. In the fortnight the snow melted, nine thousand letters piled at West Gate's door, the post shut that same day, and from Suspended River Ford to Salt-Iron Town the whole road stopped for four days.

This was far worse than Hou. Hou had to forge an identity. Middle Road forged nothing — it only **put other people's letters on a road they had no right to take.** Hou lied. Middle Road trespassed.

And no register could stop trespass, because **the register governed names, not roads.**

**The third remedy was to file a record for every stretch of the road as well.**

The last volume the Grand Relay Office added was called the **Upstream Roll**.

Beside its register entry, every post deposited a second document: **these are the only upstream posts I take mail from.** Another post's mail I do not accept; to another post I do not forward. Sealed in wax the same way, carried in the same courier dispatch.

The clerk now checked two things, against the whole route written on the board:

- does the **final** name on the board match a register entry? (who sent this)
- does **every up-and-down pair along the route** match the Upstream Rolls? (who permitted this road)

With the second check in place, Middle Road's trick fell apart at once: on the stretch "Middle Road → West Gate," Middle Road did not appear in West Gate's upstream list. **Refused.**

But by this stage the magistrate could finally see plainly: **this apparatus cannot stop all bad boards. It can only stop bad boards that name someone else.**

No record at all, and by the old custom it passes.

Of the forty-seven posts, only thirty-one filed their entries on time. The other sixteen found it a nuisance — filing meant a trip to the capital, a wax fee, a yearly renewal. They did not file. Their mail moved the same as ever, the clerk passed it the same as ever, only further down the queue.

Which is to say: **the ground easiest to forge was precisely the ground nobody had filed for.**

And the courier dispatch refreshed only every half hour. The day Hou should really have chosen was not in autumn. It was the half hour after the dispatch left and before the book arrived.

As for the board reading "Salt-Iron Town, Seventh Ward" — it now hangs on the wall of the provincial relay office as a lesson. The five characters on it were false. But until that day, **no drop of wax had ever attested that they were false**, because from its first day this road guaranteed only that letters could be delivered. It never guaranteed who sent them.

By now you have probably recognized it: this is **BGP route origin validation** — *RPKI* and *ROV* — together with the two things it cannot escape, **route leaks** and **ASPA** (*Autonomous System Provider Authorization*).

### What it is

Start with the old world.

The internet is assembled from tens of thousands of autonomous systems (*AS*). Each AS holds one or more blocks of IP addresses (*prefixes*) and announces to its neighbours: "this block is mine, send it to me." A neighbour accepts, edits, announces onward. That is *BGP*.

BGP's original design contains **no origin authentication whatsoever**. From an announcement, the only thing you can confirm is that it came from your neighbour. Whether the block truly belongs to the AS announcing it is a question the protocol never addresses. The routing table is therefore a table of **default trust**.

And one forwarding rule magnifies the problem: **longest prefix match**. An AS legitimately announces `203.0.113.0/16`; an attacker announces `203.0.113.7/24` — the more specific always wins. He does not need to take the whole /16. He needs one /24 described one notch finer. That is Hou's five characters.

**RPKI** (*Resource Public Key Infrastructure*) answers by giving boards a **register**.

The trust chain runs top-down: IANA is the root (*trust anchor*) → the five regional internet registries → national registries → member organizations (ISPs, cloud providers, enterprises) → the individual prefix holder. Each level signs a certificate for the level below with its own private key, declaring "I authorize you to hold this block." The whole tree is an *X.509* hierarchy, publicly verifiable.

What gets signed at the leaf is a **ROA** (*Route Origin Authorization*, RFC 6482). A ROA has exactly three fields:

- `prefix` — the block ("Salt-Iron Town")
- `maxLength` — the finest length this authorization covers ("to the town as limit")
- `origin AS` — the single AS authorized to announce it ("only this post may collect")

A standalone *RPKI validator* gathers every ROA, verifies each signature, and emits a set called the **VRP** (*Validated ROA Payload*) set. VRPs reach routers over the **RTR protocol** (*RPKI-to-Router*, RFC 8210, pushed incrementally) — that is the courier dispatch.

For each BGP announcement it receives, a router renders one of three verdicts, a procedure standardized as **ROV** (*Route Origin Validation*, RFC 6811):

- ***Valid*** — a covering ROA exists, `maxLength` permits the length, and the last AS in AS_PATH matches the ROA origin: accept, possibly raising *local-pref*.
- ***Invalid*** — a covering ROA exists, but the origin differs or the prefix is longer than `maxLength`: **drop / de-prefer**. All the benefit lives in this cell.
- ***NotFound*** — no ROA covers it at all: **accept by the old rules**, simply without preference.

Note the third row. ROV's default action is "no record, then pass," not "no record, then refuse." That is not an oversight; it is deployment reality. Going straight to a whitelist would take half the internet down in a day. The cost is explicit: **the attacker's most rational choice is always the ground with no record.**

And `maxLength` is the loosest cog in the machine. Writing `/16-/24` amounts to saying "any /24 inside this block counts as mine to announce," so somebody else announcing one is judged *Valid*, not *Invalid*. The looser you write it, the safer your own availability and the more dangerous everyone else's.

There is one more thing no register can ever do: **ROV inspects only the last AS of AS_PATH.**

It answers "who sent this," not "how did this get here." Which gives the other class of accident — a **route leak**: an AS takes routes learned from upstream A and hands them to upstream B. The prefix is real, the origin is real, the signature verifies; only the road should not exist. Its blast radius rivals prefix hijacking, and often exceeds it: the 2019 incident in which an ISP pushed a bad optimizer configuration and pulled the world's Google traffic into its network for two hours ran entirely on this mechanism, with not one forged announcement.

**ASPA** closes that gap: an AS signs, in RPKI, a list of **the upstreams it authorizes**, and validators check the up-and-down relationship hop by hop along AS_PATH. It inspects the road, not the names.

Two terms worth separating, because the press runs them together:

- **hijack** — announcing a prefix that is not yours, or impersonating another's origin AS: Hou.
- **leak** — announcing a route you are entitled to announce but not entitled to pass along: Middle Road Post.

### Why it matters

For one reason: **the entire internet's forwarding rests on a protocol with no authentication, and authentication arrived twenty years later as a bolt-on layer.**

Three consequences follow directly.

**One, more-specific always wins, which makes "small" attacks extremely cheap.** In 2008 Pakistan Telecom, trying to block YouTube domestically, announced a /24; that more-specific route spread through BGP worldwide, pulled YouTube's global traffic toward it, and blackholed it for two hours — not an attack, a misconfiguration. In 2018 an attacker used the same technique, hijacking routes belonging to a cloud provider's DNS service, to steer resolution of myetherwallet.com to his own servers. The mechanism in both is exactly Hou's five characters.

**Two, ROV's benefit is real, but its coverage is its ceiling.** Once *Invalid* routes are dropped, the great majority of origin forgeries stop working, and it is one of the largest hardenings the internet has had. But the remaining *NotFound* is a broad expanse that passes by default, and deployment speed is bound by something mundane: **someone has to be willing to make the trip to the capital and pay the wax fee.** Which is why essentially every RIR has driven ROA issuance down to a few clicks, and why large cloud providers sign on their customers' behalf.

**Three, it overturns an old intuition: a route has an authority.** For a long time a route was treated as a pure technical fact — the network says this path works, so it works. RPKI makes it an **authorization fact**: whether the path works depends on whether a signed record exists in some registry. When you configure a BGP announcement for an elastic IP on a cloud today, this chain hangs behind it: whose certificate the prefix sits under, which origin is named, how wide `maxLength` was written.

Every link in that chain is thin, and without any one of them the rest does not hold. It is the same shape as an *mTLS* certificate chain, a *DNSSEC* delegation chain, or OIDC's *JWKS*: **trust is not "I know you are you." Trust is "someone signed for you, and I can trace it to the root."**

_Metaphor mapping_

- Each post plants a board naming the places it carries toward, and porters follow the boards → BGP announcements: every AS advertises the prefixes it holds and serves
- Nobody at the forks verifies where a board came from → BGP's original design has no origin authentication; the routing table runs on default trust
- "The finer the board, the more it counts" → longest prefix match
- Hou plants "Salt-Iron Town, Seventh Ward" over someone's "Salt-Iron Town" → prefix hijack: a more-specific /24 swallowing a /16's traffic
- A passer-by sees only his own stretch of road → hop-by-hop trust: each AS trusts only what its neighbour hands it
- Letters changing hands six or seven times among forty-seven posts → hard attribution, paths in flight, constantly changing
- The Tian family's letters via Middle Road and West Gate to Broken Cliff Mouth → AS_PATH: the AS sequence a route traverses
- Boxers guarding five forks, one smashed and one replanted → point ACLs and prefix filters, unmaintainable in practice
- The Grand Relay Office's entry: which stretch, which single collector, finest level allowed → the ROA's three fields: prefix, origin AS, maxLength
- Signed yearly under red wax, copy retained → X.509 certificates and ROA signatures
- Authority flowing IANA → RIR → national registry → member → holder → RPKI's hierarchical trust chain, trust anchor to end entity
- The register book gathering every entry, carried to each fork → the VRP set produced by a validator and pushed to routers over RTR
- New entries appended every half hour → RTR incremental refresh; the window before the book arrives is blind
- The clerk verifies only the last name on the board → ROV validates only the origin AS at the end of AS_PATH
- Three outcomes: allowed / refused / passed by old custom → Valid / Invalid / NotFound
- "No record, then by the old custom it passes" → NotFound accepted by default; the source of all residual risk
- "To the town as limit; wards and lanes not included" → maxLength bounding the longest prefix a ROA covers
- Writing maxLength as "any ward beneath it counts too" → an over-wide maxLength turning what should be Invalid into Valid
- Only thirty-one posts filed; sixteen passed but queued last → incomplete ROA coverage; uncovered space is the bulk of the attack surface
- Filing costs a trip to the capital and a wax fee → RPKI's deployment cost and the organizational friction of issuing ROAs
- Hou's board comes down and the town sleeps one winter → fixing the hijack does not fix the mechanism
- Middle Road stuffs North Ridge's letters into West Gate's sack → route leak: legitimate prefix, illegitimate path
- The name on the board is true and the road is wrong → ROV inspects the name list, not the path
- Nine thousand letters at West Gate overnight, the whole road down four days → congestion, loss, and large-scale unreachability caused by a leak
- The register governs names, not roads → the capability boundary between origin validation and path validation
- The Upstream Roll: the only posts I take mail from → ASPA: an AS's signed set of authorized upstreams
- Checking each up-and-down pair against the Rolls → ASPA validating provider-customer relationships hop by hop along AS_PATH
- The board nailed to the capital's wall as a lesson → incidents and RFCs written into the system's memory
</section>
