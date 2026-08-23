---
layout: fable
title: "云集镇的两位守门官 · The Two Gatekeepers of Cloud-Market Town"
title_zh: "云集镇的两位守门官"
title_en: "The Two Gatekeepers of Cloud-Market Town"
concept: "admission webhooks and failurePolicy"
tags: [kubernetes, security]
illustration: /assets/art/2026-07-05-admission-webhooks.jpg
---
<section class="zh" markdown="1">
云集镇是方圆百里最大的集市，镇子中央有一间_总账房_，镇上所有买卖成交与否，全靠那本_总账_说了算——只要写进总账，这笔交易就算数，谁也翻不了案。

可总账房不对外开放。所有进城的货车，想把货物记进总账，都得先过东门。东门口常年站着两位官爷。

第一位叫_批注官_。他不拦车，但会掰开每辆车的苫布，往货单上添几笔——货主没写产地的，他按惯例给你补一个；货主忘了报税额的，他照规矩添上默认数；要是这趟车按规矩该带一辆"随行护卫车"，他也会顺手给你挂上。等他忙完，这车货单已经跟进城前不太一样了。

第二位叫_验核官_。他排在批注官后面，只看不改。他拿着镇规逐条核对批注官改完之后的最终货单：斤两对不对、税额报没报、护卫车挂没挂齐。只要有一条不达标，车子原地打回，绝不放行——总账房那本账里，永远不会出现这笔不合规的记录。

镇上有个不成文的规矩：每一车货，不论大小，都要老老实实走完"先批注、后验核"这一整套流程才能进城——哪怕只是运一筐白菜。

问题是，这两位官爷也有打盹、请假、甚至被镇长临时调去别处忙别的事的时候。这时候东门该怎么办？镇子历史上试过两套规矩，刻在城门两侧的石碑上：

左边石碑写着"_宁缺毋滥_"——官爷不在场，城门先关上，一车都不许进，等官爷回来再说。集市因此可能大排长龙，货物堆在城外发愁，但总账房里绝不会混进一笔没人把关的交易。

右边石碑写着"_宁滥毋缺_"——官爷不在，城门照开，车子直接放行进城记账。集市不会堵车，但谁也不知道这段时间进城的货里，有没有缺斤短两、没上护卫的问题货。

——到这儿你大概已经认出来了：云集镇的这套东门规矩，其实讲的是 Kubernetes/OpenShift 里的 _admission webhook_（准入控制）机制，以及它的 _failurePolicy_：_Ignore_（fail open，宁滥毋缺）还是 _Fail_（fail closed，宁缺毋滥）。

_概念解释_
每当有人往 API server 提交一个对象（创建/更新 Pod、ConfigMap 等），这个对象在真正写入 _etcd_ 之前，要依次经过一串 _admission webhook_：先是 _mutating webhook_（可以修改对象本身，比如打默认值、注入 sidecar），再是 _validating webhook_（只读检查，不通过就直接拒绝这次请求）。这套机制是集群策略统一执行的关键入口——OPA/Gatekeeper 的策略拦截、Istio 的 sidecar 自动注入，靠的都是它。但它也是一个不容忽视的风险点：webhook 本身是一次网络调用，会给每个请求都加上延迟；一旦 webhook 所在的 Service/Pod 不可用或超时，_failurePolicy_ 就决定了集群的命运——设成 _Fail_，该类资源可能全部卡住无法创建；设成 _Ignore_，安全策略在那段时间形同虚设，却没人会注意到。

_隐喻对应表_

- 总账房 → _etcd_（持久化存储）
- 东门 → API server 的准入控制入口
- 批注官 → _mutating admission webhook_
- 验核官 → _validating admission webhook_
- 先批注、后验核的顺序 → webhook 执行链的固定顺序（mutating 先于 validating）
- 添默认值/挂护卫车 → webhook 对对象做的 _mutation_（打默认值、注入 sidecar 等）
- 逐条核对镇规 → validating webhook 的校验逻辑
- 官爷不在场时的两块石碑 → _failurePolicy: Ignore_（fail open）与 _Fail_（fail closed）
- 大排长龙 vs 混进不合规的货 → fail closed 的可用性代价 vs fail open 的安全代价
</section>
<section class="en" markdown="1">
Cloud-Market Town was the largest trading post for a hundred miles around. At its center stood the _Central Ledger House_, and whether a deal was real or not came down to one thing: whether it was written into the _Ledger_. Once it was written in, the deal was final — nobody could unwind it.

But the Ledger House wasn't open to the public. Every cart that wanted its goods entered into the Ledger first had to pass through the East Gate. Two officials stood there, day in and day out.

The first was the _Annotator_. He never turned a cart away, but he'd lift the tarp on every single one and add a few notes to the manifest — if the merchant forgot to list an origin, he'd fill in the customary default; if the tax amount was missing, he'd pencil in the standard rate; if town rule required this kind of cart to travel with an "escort wagon," he'd hitch one on right there. By the time he was done, the manifest looked a bit different from when the cart arrived.

The second was the _Inspector_. He stood just past the Annotator, and he never touched anything — only checked. He went through the town's rules line by line against the _final_ manifest, the one the Annotator had already amended: was the weight right, was the tax filled in, was the escort wagon attached. If even one line failed, the cart was turned back on the spot, no exceptions — and that entry would never appear in the Ledger House's book at all.

There was an unspoken rule in town: every cart, no matter how small, had to go through the full sequence — Annotator first, Inspector second — before it could enter. Even a single basket of cabbage.

The trouble was, these two officials also napped, took leave, or got pulled away by the mayor for other errands. So what was the East Gate supposed to do when that happened? Over the years, the town had tried two different policies, each carved into a stone tablet on either side of the gate.

The left tablet read: _"Better empty than wrong"_ — if an official wasn't at his post, the gate simply closed. Not a single cart got through until he came back. Lines could stretch for miles and goods piled up outside the walls, but the Ledger House would never end up with an entry nobody had actually checked.

The right tablet read: _"Better wrong than empty"_ — if an official wasn't there, the gate stayed open and carts rolled straight through into the Ledger. The market never stalled, but nobody could say for sure whether some of what got waved through during that window was underweight, untaxed, or missing its escort.

—by now you've probably recognized it: this East Gate arrangement is really Kubernetes/OpenShift's _admission webhook_ mechanism, and its _failurePolicy_ setting: _Ignore_ (fail open, "better wrong than empty") versus _Fail_ (fail closed, "better empty than wrong").

_What it is, and why it matters_
Whenever someone submits an object to the API server — creating or updating a Pod, a ConfigMap, whatever — that object passes through a chain of _admission webhooks_ before it's actually written to _etcd_. First come the _mutating webhooks_, which can modify the object itself (filling in defaults, injecting a sidecar container). Then come the _validating webhooks_, which only inspect the final result and can reject the request outright if it fails a check. This chain is the chokepoint where cluster-wide policy gets enforced — OPA/Gatekeeper policy checks and Istio's automatic sidecar injection both run through it. But it's also a real risk: each webhook call is a network round-trip that adds latency to every single request, and if the Service or Pod backing a webhook becomes unavailable or times out, _failurePolicy_ decides the cluster's fate. Set it to _Fail_, and that entire class of resource may become impossible to create cluster-wide. Set it to _Ignore_, and your policy silently stops being enforced during the outage — with nobody the wiser.

_Metaphor mapping_

- Central Ledger House → _etcd_ (the persistent store)
- East Gate → the API server's admission control entry point
- The Annotator → _mutating admission webhook_
- The Inspector → _validating admission webhook_
- Annotator-then-Inspector order → the fixed order of the admission chain (mutating runs before validating)
- Filling defaults / hitching an escort wagon → the _mutation_ a webhook applies to the object (defaults, sidecar injection, etc.)
- Checking the manifest line by line → the validating webhook's check logic
- The two stone tablets for when officials are absent → _failurePolicy: Ignore_ (fail open) vs _Fail_ (fail closed)
- Long lines outside the gate vs. bad goods slipping into the Ledger → the availability cost of fail-closed vs. the safety cost of fail-open
</section>
