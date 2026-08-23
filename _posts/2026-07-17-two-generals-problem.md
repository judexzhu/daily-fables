---
layout: fable
title: "雾谷两端的火把 · The Torches Across the Fog Valley"
title_zh: "雾谷两端的火把"
title_en: "The Torches Across the Fog Valley"
concept: "Two Generals Problem"
tags: [distributed-systems, networking]
illustration: /assets/art/2026-07-17-two-generals-problem.jpg
---
<section class="zh" markdown="1">
雾谷两侧，东岭和西岭各有一支猎队，约好只有两边**同时**举火把，才能围住谷里的狼群——单独一边动手，不但围不住，还会打草惊蛇。可两岭之间隔着一条常年起雾的深谷，传话全靠跑腿的伙计，雾大路滑，十个伙计里保不齐哪个就迷了路。

东岭头领派人送信：“今夜子时动手。”但他不敢就这么定下来——万一信没送到，自己子时举火就是孤军。于是他要西岭回个“收到”。西岭头领接了信、派人回话，可回话的伙计同样可能迷在雾里——西岭也不敢确定东岭知道自己收到了，于是又要东岭再回一个“收到你的收到”。两位头领在各自的山头上算着算着，同时意识到一件可怕的事：这个“再确认一次”永远到不了头——无论多少个来回，**最后一个**送信的伙计都可能没走出雾谷，拍板时需要的那点“十拿九稳”永远差最后一口气。

后来他们认了命，换了个笨办法：不再追求“百分之百确定”。东岭连着派出好几个伙计，每人带的都是同一句话；西岭听到后，不管这句话重复听到了几遍——哪怕八遍——也只当一件事办，子时照常举火，连举三晚。狼群最终被围住了——不是因为他们消灭了雾，而是因为他们不再指望雾里能传出确定性。

——到这儿你大概已经认出来了：这就是分布式系统里著名的 _Two Generals Problem_。在不可靠信道上，两个节点**无法**通过任何有限轮次的消息交换，对“共同行动”达成有保证的一致——每一条 _ack_ 本身又需要被 ack，归纳法可证不存在这样的协议。它是 _exactly-once delivery_ 不可能的根源：网络层面只能做到 _at-least-once_（靠 retry）或 _at-most-once_，工程上的解法从来不是消灭不确定，而是 _retry + idempotency + 去重_：重发消息，重复收到时只生效一次。TCP 的三次握手也只是把成功概率做高，并没有打破这个定理——这也是为什么 Kubernetes 的 reconcile、消息队列的 consumer、云 API 的 mutation 都必须设计成 _idempotent_：你永远不知道对方是没收到，还是收到了但 ack 丢了。

_隐喻对应表_

- 必须同时举火的两支猎队 → 需要对“是否/何时执行”达成一致的两个节点
- 起雾的深谷与会迷路的伙计 → 不可靠信道（消息可能丢失）
- “收到”、“收到你的收到” → _ack_ 与对 ack 的 ack（无穷回归）
- 永远差最后一口气的确定 → 不存在能保证一致行动的有限协议（归纳证明）
- 连派几个伙计送同一句话 → _at-least-once delivery_（重试）
- 重复听八遍只当一件事办 → _idempotent_ 处理 / 消息去重（dedup）
- 没消灭雾，只是不再指望确定性 → 工程上绕开而非解决：概率足够高 + 幂等兜底
</section>
<section class="en" markdown="1">
On the two sides of a fog-bound valley, the East Ridge and West Ridge hunting parties had an agreement: only if both raised their torches **at the same moment** could they encircle the wolf pack in the valley below — one side acting alone would not only fail, it would scatter the wolves for good. But between the ridges lay a gorge where the fog never lifted, and every word passed between them had to travel by runner. The fog was thick, the paths were treacherous, and out of any ten runners, there was no telling which one might lose his way.

The East Ridge chief sent a runner: “We strike at midnight.” But he dared not commit on that alone — if the message never arrived, his torches at midnight would be a lone charge. So he demanded West Ridge send back a “received.” The West Ridge chief got the message and sent his reply — but the returning runner could just as easily vanish in the fog. Now West Ridge couldn't be sure East Ridge knew they'd received it, so they needed East Ridge to confirm the confirmation… Standing on their separate hilltops, both chiefs arrived at the same chilling realization: this “just one more confirmation” never ends. No matter how many round trips, the **last** runner can always be the one who never emerges from the fog — and the certainty needed to commit is always exactly one message short.

Eventually they gave up on certainty and settled for a humbler scheme. East Ridge sent several runners, each carrying the identical message; West Ridge, no matter how many times it heard the same sentence — even eight times over — treated it as a single instruction, raised torches at midnight, and kept doing so three nights running. The wolves were finally encircled — not because the fog was conquered, but because nobody was waiting for certainty to come out of it anymore.

By now you've probably recognized it — this is the famous _Two Generals Problem_. Over an unreliable channel, two nodes **cannot** reach guaranteed agreement on a coordinated action through any finite exchange of messages — every _ack_ itself needs acking, and a simple induction proves no such protocol exists. This is the root reason _exactly-once delivery_ is impossible: a network can give you _at-least-once_ (via retries) or _at-most-once_, never exactly-once. The engineering answer has never been to eliminate the uncertainty, but to route around it with _retry + idempotency + deduplication_: resend freely, and make receiving a message twice indistinguishable from receiving it once. TCP's three-way handshake merely drives the probability of agreement high — it does not break the theorem. This is why Kubernetes reconcile loops, message-queue consumers, and cloud API mutations must all be designed _idempotent_: you can never know whether the other side missed your message, or got it and lost the ack.

_Metaphor mapping_

- The two parties that must raise torches together → two nodes needing agreement on whether/when to act
- The fog-bound gorge and runners who lose their way → the unreliable channel (messages can be lost)
- “Received”, “received your received” → the _ack_, and the ack of the ack (infinite regress)
- Certainty always one message short → no finite protocol can guarantee coordinated action (the induction proof)
- Several runners carrying the identical message → _at-least-once delivery_ (retries)
- Hearing it eight times, acting on it once → _idempotent_ handling / message deduplication
- Not conquering the fog, just no longer expecting certainty from it → engineering around the theorem: high probability + idempotent safety nets
</section>
