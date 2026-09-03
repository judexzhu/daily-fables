---
layout: fable
title: "城门之外的夜雨 · The Rain Beyond the City Gates"
title_zh: "城门之外的夜雨"
title_en: "The Rain Beyond the City Gates"
concept: "Kubernetes NetworkPolicy"
tags: [kubernetes, networking, security]
illustration: /assets/art/2026-09-03-kubernetes-network-policy.jpg
youtube_id: "VO2x8tMYH28"
---
<section class="zh" markdown="1">

临江城有三座大院：白鹭院住着账房先生，青瓦院住着送信人，南门院养着一群看夜路的犬。

城里原先没有什么规矩。院门彼此相通，谁从哪条巷子进来都没人过问。白鹭院的账房能随意跑到青瓦院借信鸽，南门院的犬也能钻进每间厨房找吃的。平日里倒也相安无事，直到一个雨夜。

那夜，一个外乡人带着泥水闯进了城。他先翻了白鹭院的账册，又顺着后巷摸进南门院，最后把青瓦院一整袋未寄出的家书都扔进了河里。天亮以后，城主才明白：**城门多，不等于城门有规矩；没有规矩的方便，只是还没遇上坏人。**

城主请来一位做过边关城防的老匠人。老匠人没有给每个居民发一把钥匙，也没有派一名守卫跟着每个人。他只在每座院门上挂了一块木牌。

白鹭院的木牌写着“账房”，青瓦院写着“信使”，南门院写着“守夜”。每个院子里的房间也各挂自己的小牌。老匠人说：“以后守门人不认脸，只认牌。牌上写的是什么，决定它能从哪里进、能往哪里去。”

第一道规矩是给白鹭院设的：只有青瓦院的“信使”可以进账房，南门院的“守夜”不行。第二道规矩是给青瓦院设的：只有白鹭院的“账房”能进来取信，其他人一律留在门外。两道规矩各管一扇门——一扇管进，一扇管出。

起初大家觉得这很简单。可第二天，白鹭院忽然收不到城外的药材清单。账房说：“我没有拦它！”老匠人查了半天，才发现白鹭院的后门已经不许任何东西出去；送信人连城门口负责报时的铜钟都问不到，连“今天有没有药车”也不知道。

于是老匠人在后门旁另立了一条窄规矩：账房可以去问城门的报时人，但只能问那一位；可以把信送到城外的药材站，但不能把账册交给陌生商队。**不是把门全打开，而是把必要的路单独留下。**

最麻烦的是青瓦院。老匠人本想让“信使”既是一个牌子，又是“青瓦院”里的一个房间牌子。有人把两块牌子分别写进两条规矩，结果守门人把它们当成了两个条件：既要是青瓦院的人，又要是那间房的人，才准进来。另一些人把两块牌子分开写，守门人则把它们当成两条任选其一的路。

老匠人把木牌重新排好：“同一张路条上的两块牌，是**同时满足**；同一扇门下的两张路条，是**任选其一**。规矩不是看起来像不像，而是要说清楚它们怎样组合。”

雨季最凶的那天，城主决定先把南门院封成一座真正的孤院。他只挂了一块空白木牌，没有写任何允许进出的路条。南门院里的人发现：外人进不来，他们也出不去。守夜的犬不能跑去厨房，厨房的饭也送不进来。

“这不是一座城门，”犬舍管事抱怨，“这是四面墙。”

“对，”老匠人说，“先把墙立起来，再一条一条开必要的门。若一开始人人都能走，最后没人知道哪一扇门本来就不该开。”

从此，城里的规矩不再写“谁是谁”，而写“哪一类院子能与哪一类院子通行”。新房间只要挂上正确的木牌，就自动继承对应的门规；旧房间换了名字，只要牌子没变，通路也不会悄悄改变。

不过，老匠人还留下最后一句警告：“守门规矩是合在一起算的，不是哪一张规矩把其他规矩盖住。你若想加一条路，就明确加上；你若以为写了一条禁止令就能抵消别人开的门，雨水会替你证明想错了。”

——到这儿你大概已经认出来了：这就是 *Kubernetes NetworkPolicy*。

### 这是什么

*Kubernetes NetworkPolicy* 是给 Pod 流量设置边界的声明式规则。它可以控制两个方向：*Ingress*（进入 Pod 的流量）和 *Egress*（Pod 发出的流量）。规则通过 `podSelector` 选择要保护的 Pod，再用 `namespaceSelector`、另一个 `podSelector` 或 `ipBlock` 描述允许的来源或目的地。

一个关键细节是：策略是**累加允许**，不是互相覆盖的拒绝列表。某个 Pod 一旦被 Ingress 策略选中，它能接收的流量就是所有匹配策略允许来源的并集；被 Egress 策略选中后，它能访问的目的地也是所有匹配策略允许目标的并集。Kubernetes 没有一个可以凌驾于所有允许规则之上的通用“显式拒绝”条目。

因此，选中一个 Pod 的空 Ingress 或 Egress 规则，通常会形成默认拒绝：先把方向隔离，再逐条添加必要通路。一次连接能否成功，还取决于两端的方向规则：目的 Pod 的 Ingress 要允许进入，来源 Pod 的 Egress（如果它被隔离）也要允许出去。

选择器的组合也容易出错。同一条规则里的 `namespaceSelector` 和 `podSelector` 表示“同时满足”：来自指定命名空间中、且带有指定标签的 Pod。分开的规则条目通常表示“满足其一”。如果把 DNS、监控、服务发现等必要依赖忘在默认拒绝之外，应用会看似健康却无法工作。

### 为什么重要

没有 NetworkPolicy 的集群，网络默认往往过于宽松：一个被攻破的 Pod 可能横向访问同一网络中的数据库、管理接口和其他服务。NetworkPolicy 把“网络可达”变成明确声明的边界，降低横向移动和误访问的风险。

但它不是防火墙的万能替代品。策略是否真正生效，取决于集群使用的网络插件是否支持 NetworkPolicy；策略只负责流量是否允许，不负责身份认证、加密或应用层授权。生产环境还要把策略和 DNS、指标、日志、服务网格以及外部出口的真实依赖一起验证。

最稳妥的落地方式是先盘点通信矩阵，再按命名空间、Pod 标签和方向逐步隔离：先观察，再建立默认拒绝，最后只放行必需路径。每加一道门，都要测试正常流量、故障流量和运维流量是否仍然符合预期。

_隐喻对应表_

- 临江城的三座大院 —— 集群中不同命名空间和工作负载
- 院子与房间上的木牌 —— Pod 和命名空间标签
- 只认牌、不认脸 —— 通过选择器匹配，而非依赖具体 Pod 名称
- 管进的门 —— Ingress 策略
- 管出的门 —— Egress 策略
- 青瓦院的“信使” —— 被允许访问的来源 Pod
- 空白路条封住南门院 —— 选择 Pod 后形成默认拒绝
- 同一张路条同时满足两块牌 —— 同一规则中的 AND 选择器
- 同一扇门的两张路条任选其一 —— 多条规则的 OR / 允许并集
- 城门报时人和药材站 —— DNS、服务发现等显式依赖
- 坏人横穿三座院子 —— 被攻破工作负载的横向移动

</section>
<section class="en" markdown="1">

At the city of Linjiang stood three large compounds: Egret House, where the accountants lived; Green-Tile House, where the couriers worked; and South Gate House, where the night-watch dogs were kept.

There had never been many rules. The courtyards were connected by open lanes, and no one cared which alley a visitor used. The accountants could wander into Green-Tile House to borrow carrier pigeons, and the dogs from South Gate House could nose into every kitchen looking for scraps. Most days it caused no trouble — until one rainy night.

That night, a stranger came through the gates trailing mud. He rifled through Egret House's ledgers, slipped down the back lane into South Gate House, and finally threw an entire sack of undelivered letters from Green-Tile House into the river. Only at dawn did the governor understand: **more gates do not mean better gates; convenience without rules is merely trouble that has not arrived yet.**

The governor summoned an old craftsman who had built defenses on the frontier. The craftsman did not give every resident a key, nor assign a guard to follow each person. He hung a wooden plaque on every compound.

Egret House's plaque said “accountant.” Green-Tile House said “courier.” South Gate House said “watch.” Each room inside the compounds received a smaller plaque of its own. “From now on,” said the craftsman, “the gatekeepers will not recognize faces. They will recognize plaques. What the plaque says decides where something may enter and where it may go.”

The first rule belonged to Egret House: only a “courier” from Green-Tile House could enter the accounting room; a “watch” dog from South Gate House could not. The second rule belonged to Green-Tile House: only an “accountant” from Egret House could come in to collect letters. The two rules governed two different gates — one controlled entry, the other departure.

At first everyone thought it straightforward. But the next day Egret House stopped receiving the city's medicine lists. “I did not block them,” said the accountant. The craftsman investigated and discovered that Egret House's back gate now allowed nothing to leave. The accountants could not even ask the timekeeper at the city gate whether a medicine cart was on its way.

So the craftsman added a narrow rule beside the back gate: the accountants could ask the timekeeper, but only that person; they could send a letter to the medicine depot beyond the wall, but could not hand their ledgers to an unknown caravan. **Do not open every gate. Leave only the necessary paths open.**

Green-Tile House caused an even subtler problem. The craftsman wanted “courier” to be both the name of a compound and the plaque on one particular room. Someone wrote both plaques into one rule, and the gatekeeper treated them as two conditions: a visitor had to be from Green-Tile House **and** belong to that room. Someone else wrote the plaques as separate rules, and the gatekeeper treated them as two alternative routes.

The craftsman rearranged the plaques. “Two plaques on the same route must both match — **AND**. Two routes at the same gate mean either route may be used — **OR**. A rule is not defined by how it looks, but by how its pieces combine.”

On the fiercest day of the rainy season, the governor decided to make South Gate House a truly isolated compound. He hung a blank plaque and wrote no allowed paths beneath it. Outsiders could no longer enter, but the people inside could not leave either. The watch dogs could not run to the kitchen, and the kitchen could not send them food.

“This is not a gate,” complained the kennel keeper. “It is four walls.”

“Yes,” said the craftsman. “Build the walls first, then open the necessary gates one by one. If everyone can walk everywhere at the beginning, no one will know which gate should never have been open.”

From then on, the city's rules no longer described exactly who someone was. They described which kinds of compounds could communicate with which other kinds. A new room inherited the right gate rules simply by hanging the right plaque; an old room could change its name without silently changing its paths, as long as its plaques stayed the same.

But the craftsman left one final warning: “The gate rules are combined. One rule does not erase another. If you want to add a path, add it clearly. If you think a written prohibition will cancel a path someone else allowed, the rain will prove you wrong.”

— By now you have probably recognized it: this is *Kubernetes NetworkPolicy*.

### What it is

*Kubernetes NetworkPolicy* is a declarative set of boundaries for Pod traffic. It controls two directions: *Ingress* (traffic entering a Pod) and *Egress* (traffic leaving a Pod). A policy uses a `podSelector` to choose the Pods it protects, then uses a `namespaceSelector`, another `podSelector`, or an `ipBlock` to describe allowed sources or destinations.

A crucial detail is that policies are **additive allow rules**, not mutually overriding deny lists. Once a Pod is selected by an Ingress policy, the sources it may receive from are the union of the sources allowed by every matching policy. Once selected by an Egress policy, its allowed destinations are the union of every matching policy's destinations. Kubernetes has no universal “explicit deny” entry that overrides all allowed rules.

Therefore, selecting a Pod with an empty Ingress or Egress rule commonly creates default denial for that direction: isolate first, then add the paths that are actually needed. A connection can succeed only when both directions permit it: the destination Pod's Ingress must allow the traffic, and the source Pod's Egress must also allow it if the source is egress-isolated.

Selector composition is another common trap. A `namespaceSelector` and `podSelector` in the same rule mean “both must match”: a Pod with the specified labels from the specified namespace. Separate rule entries generally mean “either one may match.” If DNS, monitoring, service discovery, or another required dependency is forgotten behind default denial, an application can look healthy while being unable to function.

### Why it matters

Without NetworkPolicy, a cluster's network is often too permissive: a compromised Pod may move laterally toward databases, management interfaces, and unrelated services on the same network. NetworkPolicy turns “network reachable” into an explicit declaration, reducing lateral movement and accidental access.

It is not a universal firewall replacement. Whether a policy is enforced depends on the cluster's network plugin supporting NetworkPolicy; policies govern whether traffic is allowed, not identity authentication, encryption, or application-level authorization. Production rollout must validate policies against real dependencies such as DNS, metrics, logs, service meshes, and external egress.

The safest rollout is to map the communication matrix first, then isolate by namespace, Pod labels, and direction: observe, establish default denial, and finally allow only required paths. Every new gate should be tested against normal traffic, failure traffic, and operational traffic.

_Metaphor mapping_

- Linjiang's three compounds — different namespaces and workloads in a cluster
- Wooden plaques on compounds and rooms — Pod and namespace labels
- Recognizing plaques, not faces — selector matching rather than concrete Pod names
- The gate controlling entry — an Ingress policy
- The gate controlling departure — an Egress policy
- Green-Tile House's “courier” — an allowed source Pod
- A blank route sheet closing South Gate House — default denial after selecting a Pod
- Two plaques on one route — AND selectors within one rule
- Two routes at one gate — OR / additive allow behavior across policies
- The timekeeper and medicine depot — explicit DNS and service-discovery dependencies
- The stranger crossing all three compounds — lateral movement by a compromised workload

</section>