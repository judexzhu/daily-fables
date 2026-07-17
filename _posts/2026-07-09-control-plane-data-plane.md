---
layout: fable
title: "云轨镇的调度塔 · The Dispatch Tower of Cloud-Rail Town"
title_zh: "云轨镇的调度塔"
title_en: "The Dispatch Tower of Cloud-Rail Town"
concept: "control plane vs data plane"
tags: [kubernetes, networking, distributed-systems]
---
<section class="zh" markdown="1">
云轨镇建在几座山谷之间，靠一张密密麻麻的铁轨网把各个车站连起来。镇子中央立着一座_调度塔_，塔里的规划员整天研究客流、货运需求，画出今天该开哪几条线、每趟车该怎么绕道。可他们从来不会亲自跑到铁轨上去推一节车厢——他们只把画好的方案，通过一条电报线，发给散布在每个岔口的小_道岔箱_。

每个道岔箱里都有一根扫道杆和一盏信号灯，杆子扫向哪边、灯亮什么颜色，就代表这个岔口眼下该往哪条线走。开过来的每一趟车，只看眼前这盏灯、这根杆子的位置就直接通过——它不会、也没时间在半路停下来问塔里"我该往哪儿拐"。道岔箱记着塔里最近一次发来的指令，翻来覆去、日夜不停地照着办，用不着每趟车都跟塔里通一次话。

某个暴雨夜，一道闪电勈断了塔与各个岔口之间唯一的那根电报线。乘客们起初什么都没察觉：所有已经在路上的火车照常运行，因为每个岔口的扫道杆早就是按上一道指令扫好的，它只管一趟接一趟地按老样子扫道，用不着靠电报线才能继续干活。

可麻烦在暗处攒着。北边有一趟运煤车，塔里那天下午就已经画好新方案——要把它绕开一座刚查出裂缝的老桥，可这道新指令还没来得及发下去，电报线就断了。岔口箱子联系不上塔，只能按老规矩继续把运煤车往裂桥那条线上扫，浑然不知那座桥已经不安全。塔里的规划员急得团团转，新指令攒在手里，却送不出去。

还有一条早就跑废了的支线，那天晚上刚好到了该封闭的日子，岔口本该被彻底锁死。可封闭的命令也死在了半路的电报线上——火车照样一趟趟往那条废线的死胡同里开，因为没人告诉当地的扫道杆"这条线已经不能走了"。

天快亮时，抢修队把电报线接了回去。塔里几个小时积压的指令一股脑儿全发了下去：运煤车改道的命令、废支线封闭的命令，几乎在同一时刻送到了各个岔口，全镇的道岔布局一下子又跟塔里最新的方案对齐了。

事后镇子里的人想明白了一件事：那一夜电报线断了，从没让一列车真正停下来过；它只是让"该怎么变"这件事停了下来。真正决定每趟车此刻往哪拐的东西，活在岔口那头——独立、飞快，就算和塔失联好几个小时也照样运转；而决定"接下来该怎么变"的东西，活在塔那头——它可以安安静静停摆一整夜，一列车都不会因此出轨，可代价是岔口执行的那套方案，会一小时一小时地变得越来越过时。

——到这儿你大概已经认出来了：那座调度塔和它的电报线，讲的其实是 _control plane_；而散布在各个岔口、装着扫道杆和信号灯的道岔箱，讲的是 _data plane_。

_概念解释_
在分布式系统和网络架构里，_control plane_（控制平面）负责"决定该怎么做"——计算路由、调度决策、期望状态，通常对一致性要求高、可以相对集中（比如靠 Raft/etcd 做共识），但不直接处理每一个具体请求。_data plane_（数据平面）负责"真正把活干了"——按已经拿到的规则，逐个转发、逐个处理每一次真实的流量或请求，必须够快，通常分散在大量节点上。

这个划分最关键的性质，就是这则寓言想讲的：_control plane 出问题，不等于 data plane 立刻瘫痪_。Kubernetes 里，kube-apiserver、etcd、各种 controller 是 control plane；已经在跑的 Pod、kube-proxy 早就写好的 iptables/ipvs 规则、CNI 装好的路由，是 data plane。哪怕 API server 一时联系不上，已经跑起来的 Pod 照样对外提供服务——但你没法调度新 Pod、没法改 Service、没法让集群"学会"任何新东西。Service mesh 里也是同样的结构：istiod 是 control plane，Envoy sidecar 是 data plane，Envoy 会带着它手头最后一份配置继续转发流量，哪怕 istiod 刚好在重启。BGP 更是经典例子：控制平面在交换路由信息、计算路径，数据平面（FIB/转发表）拿着已经算好的路径继续转发数据包，哪怕 BGP 会话正在重建。

也正因为如此，这个划分是排障时最该先问的一个问题：现在到底是"正在跑的东西跑不动了"（data plane 出事），还是"没法让它变成新样子"（control plane 出事）？两者的紧急程度、影响面完全不同。

_隐喻对应表_

- 调度塔 → control plane
- 塔里的规划员画路线、定方案 → control plane 里的调度器/控制器/reconcile 逻辑
- 电报线 → control plane 与 data plane 之间的控制通道（如 API server 的 watch 连接、配置下发通道）
- 道岔箱、扫道杆、信号灯 → data plane（kube-proxy 的 iptables/ipvs 规则、Envoy 已加载的配置、路由器的转发表）
- 火车照常通行 → 已建立的流量继续被正确转发/处理，即使暂时联系不上 control plane
- 闪电勈断电报线 → control plane 与 data plane 之间失联/网络分区
- 运煤车没能改道、冲向裂桥 → data plane 只能照旧规则执行，收不到紧急更新，存在"用旧规则处理新情况"的风险
- 该封闭的废支线没能封 → 该被清理的旧路由/失效端点没能及时下线（比如已经下线的 Pod IP，还留在 iptables 规则里直到 controller 追上进度）
- 电报线修复后指令瞬间全部发下 → control plane 恢复后重新 reconcile，把 data plane 追平到最新的期望状态
- 一夜之间火车没停，只是没法用新方式动 → control plane 与 data plane 解耦的核心价值：局部故障不等于全局瘫痪，但也意味着"看起来正常"未必等于"状态是最新的"
</section>
<section class="en" markdown="1">
Cloud-Rail Town was built across a cluster of mountain valleys, its stations stitched together by a dense web of rail lines. At the town's center stood a _dispatch tower_, staffed by planners who spent their days studying passenger and freight demand, drawing up which lines should run each day and how each train should be routed. But the planners never once walked out onto the tracks to push a railcar themselves — they only sent their finished plans, over a single telegraph wire, down to the small _junction boxes_ scattered at every switch point.

Each junction box held a switch lever and a signal lamp: which way the lever was thrown and which color the lamp showed decided, in that instant, which line a train at that junction should take. Every train that rolled up simply read the lamp and lever in front of it and passed straight through — it neither could nor needed to stop halfway and ask the tower "which way do I go?" The junction box carried the tower's most recent instruction in its own lever position, and executed it train after train, day and night, with no need to phone the tower to keep working.

One stormy night, a bolt of lightning severed the single telegraph wire connecting the tower to every junction. At first, passengers noticed nothing at all: every train already en route kept running exactly as before, because every junction's lever was already set from the last instruction it had received, and it simply kept switching each arriving train the same way — no telegraph wire required to keep doing its job.

But trouble was quietly building. A coal train from the north was supposed to be rerouted that afternoon — the tower had already drawn up a new plan to send it around an old bridge whose crack had just been discovered — but the new order hadn't gone out yet when the wire died. Cut off from the tower, the junction box could only keep doing what it had always done, switching the coal train onto the cracked bridge's line exactly as before, with no idea the bridge was no longer safe. Up in the tower, the planners paced anxiously, the new order in hand with nowhere to send it.

There was also a worn-out branch line that happened to reach its scheduled retirement that very night — its junction was supposed to be sealed for good. But the sealing order died on the wire too, and trains kept rolling toward that dead end all night long, because no one had told the local lever "this line is closed now."

Near dawn, a repair crew finally restrung the telegraph wire. Hours of backlogged orders rushed down all at once: the coal train's reroute, the branch line's sealing order, arriving at every junction almost simultaneously — and within moments, the whole town's switch layout matched the tower's latest plan again.

Looking back, the town realized something important: that night, the broken wire never once stopped a single train from running. It only stopped things from _changing_. What decides, moment to moment, which way a train turns lives down at the junction — independent, fast, perfectly capable of running for hours with no word from the tower at all. What decides what _should_ change next lives up in the tower — it can sit silent for an entire night without derailing a single train, but the cost is that the plan being executed down at the junctions grows staler with every passing hour.

——By now you've probably recognized it: that dispatch tower and its telegraph wire are describing the _control plane_, and the junction boxes scattered across every switch point, with their levers and signal lamps, are the _data plane_.

_The concept, and why it matters_
In distributed systems and networking, the _control plane_ is responsible for deciding what should happen — computing routes, scheduling decisions, desired state. It typically demands strong consistency and can be relatively centralized (often backed by consensus mechanisms like Raft/etcd), but it never touches individual requests directly. The _data plane_ is responsible for actually getting the work done — forwarding and processing every real request or packet, one at a time, against rules it has already received. It has to be fast, and it's usually spread across a large number of nodes.

The single most important property of this split — and the point of this fable — is that _a control plane outage does not mean the data plane instantly stops working_. In Kubernetes, kube-apiserver, etcd, and the various controllers are the control plane; already-running Pods, the iptables/ipvs rules kube-proxy already wrote, and the routes the CNI already installed are the data plane. Even if the API server becomes unreachable, Pods that are already running keep serving traffic just fine — but you can't schedule new Pods, can't update a Service, can't teach the cluster anything new. Service mesh has the same shape: istiod is the control plane, Envoy sidecars are the data plane, and Envoy keeps forwarding traffic using whatever config it last received, even while istiod is restarting. BGP is a classic example too: the control plane exchanges routing information and computes paths, while the data plane (the FIB / forwarding table) keeps forwarding packets along already-computed paths even while a BGP session is being re-established.

This is exactly why the split matters most during an incident: is what's _already running_ failing (a data plane problem), or is the system simply _unable to change_ (a control plane problem)? The urgency and blast radius of those two failures are completely different.

_Metaphor mapping_

- The dispatch tower → the control plane
- Planners drawing routes and plans → the control plane's schedulers, controllers, reconcile logic
- The telegraph wire → the control channel between control plane and data plane (e.g. the API server's watch connections, a config push channel)
- The junction box, switch lever, and signal lamp → the data plane (kube-proxy's iptables/ipvs rules, Envoy's already-loaded config, a router's forwarding table)
- Trains running normally → already-established traffic continuing to be correctly forwarded/served, even while the control plane is unreachable
- Lightning severing the telegraph wire → a network partition / connectivity loss between control plane and data plane
- The coal train never getting rerouted, heading for the cracked bridge → the data plane executing stale rules with no way to receive urgent updates — the risk of "handling a new situation with an old rulebook"
- The worn-out branch line never getting sealed → stale routes/dead endpoints that should have been cleaned up but weren't (e.g. a decommissioned Pod's IP still sitting in iptables rules until the controller catches up)
- The backlog of orders rushing down the moment the wire is restored → the control plane resuming reconciliation, catching the data plane back up to the latest desired state
- Trains never stopped that night, they just couldn't move differently → the core value of decoupling control plane from data plane: a partial failure isn't a total outage, but it also means "looks fine" doesn't guarantee "state is current"
</section>
