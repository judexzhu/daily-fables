---
layout: fable
title: "竹筒里的铜钱 · Coins in the Bamboo Tube"
title_zh: "竹筒里的铜钱"
title_en: "Coins in the Bamboo Tube"
concept: "Token bucket rate limiting"
tags: [distributed-systems, networking, sre]
illustration: /assets/art/2026-08-30-token-bucket-rate-limiting.jpg
youtube_id: "T9UNZPc20Vk"
---
<section class="zh" markdown="1">
清河镇最热闹的渡口叫"三板桥"。每天清早，上游的货船、渔船、客船挤在河湾里等过闸。闸口窄，一次只能过一条船，要是不管不顾全放进来，船挤船、篙碰篙，轻则刮漆，重则翻船。

老闸丁姓陆，人称陆伯。他不识字，但管闸四十年从没出过事，靠的是一根竹筒和一把铜钱。

竹筒挂在闸门柱上，口朝天。陆伯每隔一炷香的工夫，往竹筒里丢一枚铜钱。不管河上有没有船等着，到时候就丢。竹筒最多装十枚——满了就不丢了，多的铜钱陆伯揣回兜里。

船要过闸，规矩只有一条：从竹筒里取一枚铜钱交给陆伯。筒里有钱，取了就过；筒里没钱，靠边等着，等陆伯丢下一枚再说。

平日里船不多，竹筒总是满的。一条船来，取一枚，过；后面第二条来，再取一枚，照过。连着来十条也能连着过——十枚铜钱刚好够。但第十一条就得等了，等陆伯到点丢进新的那一枚。

有一年发大水，上游三个码头的船全挤到三板桥。船老大们急得骂娘，催陆伯开闸。陆伯不慌："筒里有钱就过，没钱就等。水再大，闸口还是这么宽。"

有个年轻的船老大不服，问："凭什么你丢钱的速度说了算？"

陆伯指着闸口说："你看这闸门的石墩，两边各一个。船过去，水流从两边挤，石墩承的力有数。我一炷香丢一枚，是石墩能承受的速度。你嫌慢，我也嫌慢，但石墩不嫌——石墩塌了，谁也过不去。"

船老大又问："那清早没船的时候，你往筒里白丢钱，不浪费？"

陆伯说："不浪费。攒着。早上没船，筒里攒满十枚。中午突然来一拨船，头十条船一口气全过，不用等。这叫歇得起、忙得动。要是没有筒，每条船都得等一炷香，就算河面空着也快不了。"

镇上的账房先生听了这套规矩，摸着胡子说："妙。铜钱是令牌，竹筒是容量，一炷香是速率，十枚是上限。你这是……"

陆伯打断他："我这是不让船翻。"

——到这儿你大概已经认出来了：竹筒是 *token bucket*，铜钱是 *token*，一炷香丢一枚是 *refill rate*，十枚上限是 *bucket capacity*（即 *burst size*），没有铜钱就等是 *throttling*，石墩的承受力是下游服务的处理能力。这是 **token bucket rate limiting** 的故事。

### 这是什么

*Token bucket* 是最常用的流量控制算法之一。系统维护一个"桶"，桶里装着令牌（*token*）。每个请求必须消耗一个令牌才能通过；桶空了，请求要么排队、要么被拒绝（*HTTP 429 Too Many Requests*）。令牌以固定速率补充（*refill rate*），桶有容量上限（*burst size*）。

核心参数只有两个：

1. **速率**（*rate*）：每秒补充多少令牌，决定了稳态下的最大吞吐量。
2. **容量**（*burst*）：桶最多存多少令牌，决定了瞬时峰值能通过多少请求。

与漏桶（*leaky bucket*）的区别：漏桶以恒定速率放行请求，完全平滑流量；token bucket 允许瞬间放行桶中所有令牌对应的请求，天然支持突发。两者互补——API gateway 常用 token bucket 做粗粒度限流，后端队列用 leaky bucket 做精细整形。

在 Kubernetes 中，API server 对每个客户端施加 *priority and fairness*（APF），本质就是多级 token bucket。Envoy 的 *local rate limit filter* 和 *global rate limit service* 也是 token bucket 实现。Nginx 的 `limit_req` 则用 leaky bucket。

### 为什么重要

没有限流的系统就像没有闸门的渡口——晴天无事，暴雨天全完。一个失控的客户端可以发出海量请求，拖垮整个服务，殃及所有租户。这就是 *noisy neighbor problem*。

Token bucket 的精妙在于它用两个参数同时解决了两个矛盾：

- **稳态吞吐 vs. 突发容忍**：rate 控制平均速度，burst 允许短暂的峰值，比硬性"每秒 N 个"灵活得多。
- **保护后端 vs. 用户体验**：攒下的令牌让偶发的请求高峰瞬间通过，用户感觉不到延迟；只有真正的持续超载才会触发等待或拒绝。

在多租户云平台上，rate limiting 是公平性的第一道防线。AWS、GCP、Azure 的几乎每个 API 都有基于 token bucket 的限流，报错里写着 *ThrottlingException* 或 *429*——这就是陆伯的铜钱。

_隐喻对应表_

- 三板桥渡口 —— API gateway / 服务入口
- 竹筒 —— token bucket（令牌桶）
- 铜钱 —— token（令牌）
- 一炷香丢一枚 —— refill rate（令牌补充速率）
- 竹筒最多十枚 —— burst size / bucket capacity（桶容量）
- 取钱过闸 —— 请求消耗一个令牌后放行
- 筒空等着 —— throttling（限流 / 429 Too Many Requests）
- 清早攒满十枚，中午一拨全过 —— burst 能力：空闲时积攒令牌，突发时瞬间放行
- 石墩的承受力 —— 下游服务的实际处理能力
- 发大水，三个码头挤来 —— 突发流量 / noisy neighbor
- "铜钱是令牌，竹筒是容量" —— 账房先生的抽象 = 工程师建模
</section>
<section class="en" markdown="1">
The busiest river crossing in Qinghe town was called Three-Plank Bridge. Every morning, cargo boats, fishing skiffs, and passenger ferries jostled in the bend, waiting to pass through the lock. The lock was narrow — only one boat at a time. Let them all in at once and hulls would scrape, poles would tangle; at best scratched paint, at worst a capsized boat.

The old lock-keeper was surnamed Lu. People called him Uncle Lu. He could not read, but in forty years of tending the lock there had never been an accident. His secret was a bamboo tube and a handful of copper coins.

The tube hung on the lock-gate post, mouth facing up. Every incense-stick of time, Uncle Lu dropped one copper coin into the tube. It did not matter whether boats were waiting — when the time came, in went the coin. The tube held at most ten coins. Once full, he stopped; extra coins went back in his pocket.

To pass the lock, there was only one rule: take a coin from the tube and hand it to Uncle Lu. Coin in the tube? Take it and go. No coin? Pull aside and wait until Uncle Lu dropped the next one.

On ordinary days boats were few and the tube was always full. One boat arrived, took a coin, passed through. A second came, took another, passed too. Ten boats in a row could pass without stopping — ten coins, ten passages. But the eleventh had to wait for Uncle Lu's next scheduled drop.

One year the river flooded and boats from three upstream docks all crowded into Three-Plank Bridge. The boatmen cursed and shouted at Uncle Lu to open the lock. Uncle Lu did not flinch. "Coins in the tube, you pass. No coins, you wait. The river may be high, but the lock is still the same width."

A young boatman protested: "Why should your coin-dropping speed be the law?"

Uncle Lu pointed at the lock gate. "See those stone piers, one on each side? When a boat passes, the current squeezes through and pushes against them. Those piers can take only so much force. One coin per incense-stick is the speed the piers can bear. You think it's slow. I think it's slow. But the piers don't think it's slow — and if the piers collapse, nobody passes."

The boatman pressed: "When there are no boats at dawn, aren't you wasting coins dropping them into an empty tube?"

"Not wasting. Saving," said Uncle Lu. "No boats in the morning, the tube fills to ten. At noon a wave of boats arrives, the first ten pass in a burst, no waiting. That is what I call: rest when you can, move when you must. Without the tube, every boat would wait one incense-stick, even if the river were empty."

The town's bookkeeper overheard and stroked his beard. "Clever. The coins are tokens, the tube is capacity, one incense-stick is the rate, ten is the ceiling. What you have here is—"

Uncle Lu cut him off. "What I have here is no capsized boats."

— By now you have probably recognized it: the bamboo tube is a *token bucket*, the copper coins are *tokens*, one coin per incense-stick is the *refill rate*, the ten-coin limit is the *bucket capacity* (the *burst size*), waiting when the tube is empty is *throttling*, and the stone piers' structural limit is the downstream service's processing capacity. This is the story of **token bucket rate limiting**.

### What it is

The *token bucket* is one of the most widely used traffic-control algorithms. The system maintains a "bucket" holding tokens. Every request must consume one token to proceed; if the bucket is empty, the request is either queued or rejected (*HTTP 429 Too Many Requests*). Tokens are added at a fixed *refill rate*, and the bucket has a maximum *burst size*.

There are only two core parameters:

1. **Rate**: how many tokens are added per second — this sets the steady-state maximum throughput.
2. **Burst**: how many tokens the bucket can hold — this sets how many requests can pass in an instant.

The difference from the *leaky bucket*: a leaky bucket releases requests at a constant rate, perfectly smoothing traffic; a token bucket allows all accumulated tokens to be consumed at once, naturally supporting bursts. The two are complementary — an API gateway commonly uses a token bucket for coarse rate limiting while a backend queue uses a leaky bucket for fine traffic shaping.

In Kubernetes, the API server enforces *priority and fairness* (APF) on each client — essentially a multi-level token bucket. Envoy's *local rate limit filter* and *global rate limit service* are token-bucket implementations. Nginx's `limit_req` uses a leaky bucket.

### Why it matters

A system without rate limiting is like a river crossing without a lock gate — clear skies, no problem; flood day, total disaster. A single runaway client can fire an avalanche of requests, overwhelm the service, and drag down every other tenant. This is the *noisy neighbor problem*.

The elegance of the token bucket is that two parameters solve two tensions at once:

- **Steady throughput vs. burst tolerance**: the rate controls average speed while the burst allows short peaks — far more flexible than a hard "N per second" cap.
- **Backend protection vs. user experience**: accumulated tokens let occasional spikes pass instantly, invisible to the user; only sustained overload triggers waiting or rejection.

On multi-tenant cloud platforms, rate limiting is the first line of fairness. Nearly every API on AWS, GCP, and Azure has token-bucket-based throttling, and the error message reads *ThrottlingException* or *429* — that is Uncle Lu's copper coin.

### Metaphor mapping

- Three-Plank Bridge crossing — API gateway / service entry point
- Bamboo tube — token bucket
- Copper coins — tokens
- One coin per incense-stick — refill rate
- Ten-coin tube limit — burst size / bucket capacity
- Taking a coin to pass — request consuming a token before being admitted
- Empty tube, wait — throttling (429 Too Many Requests)
- Dawn coins accumulate, noon burst passes — burst capability: tokens accumulate during idle time, burst through during spikes
- Stone piers' structural limit — downstream service's actual processing capacity
- Flood bringing boats from three docks — traffic spike / noisy neighbor
- "Coins are tokens, tube is capacity" — the bookkeeper's abstraction = engineer's model
</section>
