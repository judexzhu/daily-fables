---
layout: fable
title: "山间索道站的止运规矩 · The Mountain Cableway's Shutdown Rule"
title_zh: "山间索道站的止运规矩"
title_en: "The Mountain Cableway's Shutdown Rule"
concept: "circuit breaker"
tags: [distributed-systems, kubernetes]
illustration: /assets/art/2026-07-12-circuit-breaker.jpg
---
<section class="zh" markdown="1">
云岭索道有两站:上站建在半山腰的货场,下站在谷底的分拣场。每天清晨,上站的老站长把成箱的山货装上缆车,一趟趟送下去,下站的分拣工人当场卸货、过秤、装车转运。

这天赶上山货大丰收,货量比平时多了两倍。头几趟还好,下站的人手勉强跟得上。可到了第五趟,分拣场彻底堆满了——新到的箱子没地方放,只能眼睁睁看着缆车又运来一批,又一批。分拣工人手忙脚乱,干脆开始把箱子原样退回上站,连拆都没拆。

老站长看在眼里,定了条规矩:"如果连着三趟送下去的货都被原样退回,那就说明下站已经撑不住了——这时候再硬送,不但帮不上忙,连缆车本身的运力也会被堵死,上头堆积的货一箱都下不去。"于是当退货攒够三次,老站长立刻叫停:接下来一段时间,凡是要送下站的货,一律先在上站原地卸下,连缆车都不用发,直接跟货主说"下站现在收不了,先等等"。

但老站长没有干等。每隔一炷香功夫,他就挑一趟_空车_,只放一只不值钱的小样品箱,悄悄送下去探探风声。如果这只探路的箱子顺利被卸货、称重、放行,他就再壮着胆子多送一趟试试;要是连续两趟探路都顺利,他才敢重新打开闸门,恢复正常发货。可只要探路的箱子又被原样退回来,他立刻把闸门重新关上,继续按老规矩等下一炷香。

这样一来,上站从不会因为下站的忙乱而跟着瘫痪——它只是安静地等着,偶尔探一探路,而不是不管不顾地把货一趟趟往一个已经堆满的地方硬塞。

到这儿你大概已经认出来了:这条止运又探路的规矩,讲的其实是 _circuit breaker_(熔断器)模式——从平时放行的 _closed_(闭合)状态,到连续失败后一键切断的 _open_(断开)状态,再到派"探路车"试探的 _half-open_(半开)状态,三态循环切换。

_概念解释_
_Circuit breaker_ 是分布式系统里用来防止_级联失败_(cascading failure)的一种保护机制,常见于微服务之间的调用,比如 Istio/Envoy 的 _outlier detection_、resilience4j、Hystrix 之类的客户端库。它维护三种状态:_closed_ 时请求正常放行,同时暗中数错误率;一旦错误率(或连续失败次数)越过阈值,立刻跳到 _open_,在冷却时间内所有请求直接快速失败(_fail fast_),根本不再发往下游——这样做的关键,是保护本已吃紧的下游不被继续拖垮,也保护调用方自己的线程池/连接池不被一堆挂起的请求占满。冷却时间一到,进入 _half-open_,只放行少量试探请求;成功就回到 _closed_ 恢复正常流量,失败就立刻退回 _open_ 重新计时。这套机制在 Kubernetes/OpenShift 的 Service Mesh 场景(比如 Istio 的 `outlierDetection`、Envoy 的 circuit breaking 配置)里随处可见,是保证故障不会在服务依赖链上层层放大的关键防线。

_隐喻对应表_

- 老站长 → circuit breaker(熔断器本体/客户端库)
- 上站往下站送货 → 服务间的请求调用
- 下站分拣场堆满、原样退货 → 下游服务失败/超时
- 连续三次被退货 → 失败次数越过阈值(failure threshold)
- 全面停运、货物原地卸下 → 跳转到 open 状态,fail fast
- 每炷香一趟的空车探路 → half-open 状态下的试探请求
- 探路连续两次成功才恢复正常发货 → 成功次数达到阈值,回到 closed 状态
- 一炷香的等待 → 冷却时间(reset timeout)
</section>
<section class="en" markdown="1">
Cloud Ridge Cableway had two stations: an upper station perched at a mountainside loading yard, and a lower station down in the valley's sorting yard. Every morning, the old stationmaster at the top loaded crates of mountain produce onto the cable cars and sent them down, run after run, while the workers below unloaded, weighed, and forwarded them on.

One day the harvest came in twice as heavy as usual. The first few runs were fine — the sorting crew below kept pace, barely. But by the fifth run, the yard downstream was completely full. There was nowhere to put the new crates, and the workers could only watch helplessly as the cableway kept sending more, and more. Overwhelmed, they started sending crates straight back up, unopened.

The stationmaster took note and set a rule: "If three runs in a row come back rejected, that means the lower station can't keep up anymore. Forcing more crates down won't help — it'll only jam the cableway itself, and then nothing gets through at all, not even the crates already piled up here." So once rejections hit three in a row, he called an immediate halt: for a while, any crate bound for the lower station would simply be set down right where it was, without even dispatching a car. He'd tell the owners, "The lower station can't take deliveries right now — please wait."

But he didn't just wait blindly. Every so often, he'd send a single _empty car_ carrying only one cheap sample crate, just to test the waters. If that scout crate got unloaded, weighed, and cleared without trouble, he'd cautiously try one more run. If two scouting runs in a row went smoothly, only then would he reopen the gate and resume normal shipping. But the moment a scout crate came back rejected, he'd shut the gate again and wait out another full interval before trying again.

This way, the upper station never collapsed just because the lower one was overwhelmed — it simply waited, occasionally testing the waters, instead of blindly ramming crate after crate into a yard that was already full.

By now you've probably recognized it: this rule of halting shipments and sending scouts is really the _circuit breaker_ pattern — cycling between the normal _closed_ state, the tripped _open_ state after repeated failures, and the _half-open_ state where a handful of trial requests test whether it's safe to resume.

_Concept explanation_
A _circuit breaker_ is a protective mechanism in distributed systems used to prevent _cascading failure_, commonly seen between microservices — think Istio/Envoy's _outlier detection_, or client-side libraries like resilience4j and Hystrix. It maintains three states: in _closed_, requests flow normally while the breaker quietly tracks the error rate; once errors (or consecutive failures) cross a threshold, it flips to _open_, where for a cooldown period all requests fail immediately (_fail fast_) without ever reaching the downstream service — this protects an already-struggling downstream from being pushed further under, and also protects the caller's own thread pool or connection pool from filling up with stuck requests. When the cooldown ends, it enters _half-open_, letting through only a small number of trial requests: success sends it back to _closed_ and normal traffic resumes, failure sends it straight back to _open_ and the timer resets. This pattern shows up throughout Kubernetes/OpenShift service mesh setups — Istio's `outlierDetection`, Envoy's circuit breaking config — as a key line of defense against failures amplifying up a dependency chain.

_Metaphor mapping_

- Stationmaster → the circuit breaker itself (or the client-side library)
- Upper station shipping to lower station → a service-to-service request call
- Lower yard filling up, crates rejected → downstream failures/timeouts
- Three rejections in a row → the failure threshold being crossed
- Full halt, crates set down on the spot → tripping to the open state, failing fast
- The periodic single empty scout car → trial requests in the half-open state
- Two successful scouting runs before resuming → the success threshold that closes the breaker again
- The waiting interval between scouts → the reset/cooldown timeout
</section>
