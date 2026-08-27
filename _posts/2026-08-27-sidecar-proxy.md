---
layout: fable
title: "每个商铺门口都站着的那个人 · The Person Standing at Every Shop Door"
title_zh: "每个商铺门口都站着的那个人"
title_en: "The Person Standing at Every Shop Door"
concept: "Sidecar proxy (service mesh data path)"
tags: [networking, kubernetes, microservices]
illustration: /assets/art/2026-08-27-sidecar-proxy.jpg
---
<section class="zh" markdown="1">
长安城西市有两百多家铺子，卖丝绸的、打铁的、磨镜的、卖茶的，各自做各自的生意。铺子和铺子之间经常要通货——绸缎铺需要铁匠的扣环，铁匠需要磨镜匠的砂，磨镜匠需要茶铺的水。以前都是掌柜亲自出门送货、收款、讲价。

后来出了三件事。

第一件：绸缎铺的伙计送了一批货到铁匠铺，铁匠说没收到。到底是伙计路上丢了还是铁匠赖账？没人说得清。

第二件：磨镜匠换了地方，从街东搬到街西。三家铺子的掌柜花了两天才弄清新地址，中间耽误了好几笔买卖。

第三件：有人冒充绸缎铺的伙计去铁匠铺提了一批货。铁匠没验过来人的腰牌，货就没了。

市令想了想，颁了一道令：从今天起，每家铺子门口站一个**市吏**。所有进出这家铺子的货，不管发的还是收的，都必须经过门口那个人的手。

掌柜们一开始很不高兴。"我自己送货送了十年了，凭什么多个人在门口碍事？"

但市吏做的事情很快让他们闭了嘴。

送货的伙计出门之前，门口的市吏先在单子上盖个戳，记下几时几分发了什么货、发给谁。到了铁匠铺，铁匠铺门口的市吏验腰牌、核单子，确认来人确实是绸缎铺派的，货也对得上。两边都有记录，再也没人说不清。

磨镜匠搬家了？掌柜不用操心。市吏自己会去市令衙门查新地址，下次送货自动往新地方送。掌柜甚至不知道磨镜匠搬过家。

有人冒充？市吏查腰牌。没有腰牌的人一律挡在门外，连铺子大门都进不去。

更妙的是：市令可以下一道令——"铁匠铺这个月出货太猛，每刻钟只放十件出门。"市吏照办，掌柜完全不需要改铺子里的任何规矩。要是铁匠铺突然着了火，市吏把门一关，来送货的伙计自动被挡在外面，不会跟着遭殃。

后来有个新掌柜搬进西市。他只管把铺子开起来，第二天早上醒来，门口已经站好了一个市吏，和所有老铺子门口的一模一样。他什么都没申请，什么表格都没填。

——到这儿你大概已经认出来了：这不是唐朝的故事。每家铺子就是一个 *Pod*，门口那个市吏是 *sidecar proxy*（通常是 *Envoy*），市令衙门是 *control plane*（*Istio* 的 istiod），"所有货物必须经过市吏"是 *iptables* 规则劫持了进出 Pod 的所有流量，新掌柜什么都没填门口就多了一个人是 *sidecar injection*——*mutating admission webhook* 自动给新 Pod 注入 Envoy 容器。这是 **service mesh** 数据平面的故事。

### 这是什么

*Service mesh* 的核心想法极简：在每个应用 Pod 旁边放一个透明代理（*sidecar*），所有进出应用的网络流量都先经过它。应用自己完全不知道代理的存在。

实现分两层：

**数据平面**（*data plane*）：每个 Pod 里的 Envoy sidecar。它做四件事：
1. **流量劫持**：Pod 一启动，init container 用 *iptables*（或 *eBPF*）把 Pod 内所有出站/入站 TCP 流量 redirect 到 Envoy 的监听端口。应用以为自己在直连对方，其实走了一道弯。
2. **服务发现**：Envoy 从 control plane 拿到所有服务的端点列表（*EDS*），自己做负载均衡。目标 Pod 搬了节点？Envoy 自动更新路由，应用无感知。
3. **mTLS**：sidecar 之间自动建立双向 TLS，不需要应用改一行代码。证书由 control plane 签发和轮转。
4. **可观测性**：每一跳的延迟、错误率、请求量全部自动上报——因为流量一定经过 Envoy，所以 100% 采样，不需要埋点。

**控制平面**（*control plane*）：Istio 的 istiod。它把 Kubernetes Service、VirtualService、DestinationRule 这些高层策略翻译成 Envoy 的 xDS 配置，推送给每个 sidecar。市令发号令，市吏执行。

注入方式最常见的是 *mutating admission webhook*：给 namespace 打一个标签 `istio-injection=enabled`，之后这个 namespace 里创建的每个 Pod 都会被 webhook 自动注入一个 Envoy sidecar 容器和一个 iptables init container。新掌柜什么表格都没填，门口就多了一个人。

### 为什么重要

Mesh 解决的是**跨服务关切的下沉**：重试、超时、限流、熔断、mTLS、灰度发布、流量镜像——这些逻辑如果写在每个服务的业务代码里，你就得在每种语言的每个框架里重复实现一遍，而且升级的时候要逐个服务重新部署。把它们下沉到基础设施层（sidecar），所有服务自动获得，升级也只需要滚动重启 sidecar。

代价不是零。每一跳多了两次用户态代理（源端 sidecar → 目标端 sidecar），延迟增加约 1-3ms。内存方面每个 Envoy 占 50-100MB。对于延迟敏感或 Pod 数量极多的场景，这笔帐要算清楚。

新的趋势是 *ambient mesh*（Istio ambient mode）：用节点级的 *ztunnel* 代替 per-Pod sidecar，mTLS 在节点层面完成，L7 策略按需走 *waypoint proxy*。思路不变——流量一定要过代理——但代理从"每个铺子门口一个人"变成了"每条街一个岗亭"。

### 隐喻对应表

- 西市的两百多家铺子 —— Kubernetes 集群里的 Pod
- 掌柜亲自送货 —— 服务间直连，无代理
- 门口的市吏 —— sidecar proxy（Envoy）
- "所有货物必须经过市吏" —— iptables/eBPF 流量劫持
- 盖戳、记录 —— 分布式追踪与可观测性
- 验腰牌 —— mTLS 双向认证
- 市令衙门 —— control plane（istiod）
- 市令下令限流 —— DestinationRule / 限流策略
- 查新地址 —— 服务发现（EDS endpoint 更新）
- 着火关门 —— 熔断（circuit breaking）
- 新掌柜门口自动站好市吏 —— sidecar injection（mutating admission webhook）
- 每条街改设岗亭 —— ambient mesh / ztunnel 节点级代理
</section>
<section class="en" markdown="1">
The West Market of Chang'an had more than two hundred shops — silk merchants, blacksmiths, mirror polishers, tea sellers — each running its own trade. Shops needed to exchange goods constantly: the silk shop needed the blacksmith's clasps, the blacksmith needed the polisher's sand, the polisher needed the tea shop's water. In the old days, each owner walked the delivery over personally, collected payment, and haggled face to face.

Then three things went wrong.

First: a silk shop clerk delivered a shipment to the blacksmith, who swore he never received it. Did the clerk lose it on the road, or was the blacksmith lying? Nobody could say.

Second: the mirror polisher moved from the east end of the street to the west. Three shop owners spent two days figuring out the new address, missing several deals in the meantime.

Third: someone impersonated a silk shop clerk and picked up a batch of goods from the blacksmith. The blacksmith had never checked the visitor's waist-token; the goods were gone.

The Market Prefect thought it over and issued an order: from today, a **market clerk** would stand at every shop's door. Every piece of goods entering or leaving the shop — whether sent or received — must pass through that person's hands.

The owners were unhappy at first. "I've been delivering my own goods for ten years — why do I need someone getting in my way at my own door?"

But the clerk's work silenced them quickly.

Before a delivery boy left, the clerk at the door stamped the manifest: what was sent, to whom, at what hour and minute. At the blacksmith's door, the receiving clerk checked the waist-token, matched the manifest, and confirmed the visitor really was sent by the silk shop and the goods matched. Both sides had records. No more disputes.

The polisher moved? The owner didn't need to worry. The clerk checked the new address at the Prefect's office and routed the next delivery there automatically. The owner didn't even know the polisher had moved.

An impostor? The clerk checked the token. Anyone without one was turned away — they couldn't even reach the shop door.

Better still: the Prefect could issue a decree — "The blacksmith's output is too high this month; only ten items may leave per quarter-hour." The clerk enforced it; the blacksmith changed nothing about how he ran his shop. And when the blacksmith's forge caught fire, the clerk shut the door, and delivery boys heading that way were turned back automatically, spared from the blaze.

Later, a new owner moved into the West Market. He simply opened his shop. The next morning, a clerk was already standing at his door — identical to the ones at every other shop. He had filed no application, filled out no forms.

— By now you have probably recognized it: this is not a story about Tang Dynasty commerce. Each shop is a *Pod*, the clerk at the door is a *sidecar proxy* (usually *Envoy*), the Prefect's office is the *control plane* (*Istio*'s istiod), "all goods must pass through the clerk" is the *iptables* rules hijacking all traffic in and out of the Pod, and the new owner who filed nothing yet found a clerk at his door is *sidecar injection* — a *mutating admission webhook* that automatically injects an Envoy container into every new Pod. This is the story of the **service mesh** data path.

### What it is

The core idea of a *service mesh* is minimal: place a transparent proxy (*sidecar*) next to every application Pod, and route all network traffic in and out of the application through it. The application is completely unaware the proxy exists.

The implementation has two layers:

**Data plane**: the Envoy sidecar in every Pod. It does four things:
1. **Traffic interception**: when the Pod starts, an init container uses *iptables* (or *eBPF*) to redirect all outbound and inbound TCP traffic to Envoy's listener port. The application thinks it's connecting directly; it isn't.
2. **Service discovery**: Envoy gets the endpoint list for every service from the control plane (*EDS*) and does its own load balancing. If the target Pod moves to another node, Envoy updates the route transparently.
3. **mTLS**: sidecars automatically establish mutual TLS between each other — no code changes in the application. Certificates are issued and rotated by the control plane.
4. **Observability**: latency, error rate, and request volume for every hop are reported automatically — because traffic always passes through Envoy, you get 100% sampling without instrumenting a single line.

**Control plane**: Istio's istiod. It translates high-level policies — Kubernetes Services, VirtualServices, DestinationRules — into Envoy's xDS configuration and pushes it to every sidecar. The Prefect issues decrees; the clerks enforce them.

The most common injection method is a *mutating admission webhook*: label a namespace with `istio-injection=enabled`, and every Pod created in that namespace is automatically injected with an Envoy sidecar container and an iptables init container. The new owner filed nothing; a clerk appeared at his door.

### Why it matters

The mesh solves **cross-cutting concern offloading**: retries, timeouts, rate limiting, circuit breaking, mTLS, canary releases, traffic mirroring — if you implement these in every service's business code, you must reimplement them in every language and every framework, and upgrading means redeploying every service. Push them down to the infrastructure layer (the sidecar) and every service gets them for free; upgrading means rolling restart of sidecars only.

The cost is not zero. Every hop adds two userspace proxy traversals (source sidecar → destination sidecar), adding roughly 1–3ms of latency. Each Envoy instance consumes 50–100MB of memory. For latency-sensitive workloads or clusters with very many Pods, the arithmetic matters.

The emerging trend is *ambient mesh* (Istio ambient mode): a per-node *ztunnel* replaces the per-Pod sidecar, handling mTLS at the node level, while L7 policies are applied on demand through a *waypoint proxy*. The principle is unchanged — traffic must still pass through a proxy — but the proxy moves from "one person at every shop door" to "one checkpoint on every street."

### Metaphor mapping

- Two hundred shops in the West Market — Pods in a Kubernetes cluster
- Owners delivering goods personally — direct service-to-service calls, no proxy
- The clerk at the door — sidecar proxy (Envoy)
- "All goods must pass through the clerk" — iptables/eBPF traffic interception
- Stamping manifests and keeping records — distributed tracing and observability
- Checking the waist-token — mTLS mutual authentication
- The Prefect's office — control plane (istiod)
- The Prefect ordering rate limits — DestinationRule / rate-limiting policy
- Looking up the new address — service discovery (EDS endpoint update)
- Shutting the door when the forge catches fire — circuit breaking
- New owner finds a clerk already at his door — sidecar injection (mutating admission webhook)
- Replacing per-shop clerks with street checkpoints — ambient mesh / ztunnel node-level proxy
</section>
