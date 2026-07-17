---
layout: fable
title: "关外驿站的那道手谕 · The Hour-Long Chit at the Frontier Post"
title_zh: "关外驿站的那道手谕"
title_en: "The Hour-Long Chit at the Frontier Post"
concept: "IRSA / OIDC federation with STS"
tags: [aws, security, kubernetes]
---
<section class="zh" markdown="1">
在雁门关外，散布着十几处小驿站，各自看守一小段商道。驿站再往西，是朝廷设在漠北的"总粮仓"，专供紧急调粮。

老规矩是：每个驿站发一把粮仓的"铜钥匙"，钥匙一旦铸出，终身有效，谁拿着谁就能开仓。这办法用了很多年，直到有一次一名驿卒赶路时钥匙被贼人顺走——贼人不慌不忙，揣着钥匙走了三个月才用上，粮仓那头压根不知道换了主人，照样开仓不误。总仓管事这才后怕：钥匙这东西，一旦丢了，除非专程派人去漠北当面回收重铸，否则永远收不回来。

后来换了一套法子。漠北再不铸钥匙，而是由雁门关的关主（不是仓管事本人，是关主）按日给每个驿站派发一张_手谕_。手谕上盖着关主特有的花押，写清楚"此谕只发给第三驿站的领粮卒"，还标了个时辰——一个时辰之后，这张手谕自动作废，字迹会褪成白纸。

驿卒拿着手谕赶到总仓，仓管事从不用派人快马加鞭跑去雁门关核实"这花押是不是关主亲笔"——他早年就存了一张关主花押的拓片，挂在仓房墙上，来的手谕往拓片上一比对花纹，当场就能验真假，不必现问关主本人。

光花押对得上还不够。仓管事的账簿上还写着一条铁规矩：纵是关主亲笔盖的花押，若手谕上写的不是"第三驿站的领粮卒"这几个字，分毫不差，也照样不发粮——哪怕字迹一模一样，写的是"第五驿站"，一样打回去。

驿卒拿着验讦的手谕，仓管事这才写一张_临时领粮牌_给他，牌子只顶一个时辰用，过时自动作废，下次再来还得重新换谕、换牌。从此就算某张手谕、某块领粮牌不慎落入贼手，贼人拿着它，顶多也只能在那一个时辰里捣一次乱——用不了多久，牌子和谕都成了废纸，再没有一把"终身钥匙"可偷。

——到这儿你大概已经认出来了：这道手谕，其实是 _Kubernetes service account token_（一种 _OIDC_ 签发的 _JWT_）；挂在仓房墙上的花押拓片，是 AWS 那边预先注册好的 _OIDC identity provider_（连带它的 _JWKS_/证书指纹）；仓管事那条"字号必须分毫不差"的铁规矩，就是 IAM _trust policy_ 里对 `sub`、`aud` 的条件校验；而那张只顶一个时辰的临时领粮牌，正是调用 _sts:AssumeRoleWithWebIdentity_ 换来的_临时 STS 凭证_。

_概念解释_
这套机制在 AWS 上叫 _IRSA_（IAM Roles for Service Accounts），ROSA/OpenShift 用法类似，核心都是拿 _OIDC federation_ 替换掉长期存在 pod 里的静态 AWS access key。集群本身就是一个 _OIDC issuer_：每个 pod 的 service account 会被 kubelet 自动挂载一个短期、自动轮换的 _JWT_，里面带着 `iss`（签发者，即集群的 OIDC 地址）、`sub`（格式通常是 `system:serviceaccount:<ns>:<sa>`）、`aud`（受众，常见值是 `sts.amazonaws.com`）、`exp`（通常一小时左右过期）这几个关键 claim。AWS 侧预先把这个集群注册成一个 _IAM OIDC identity provider_，同时保存好它的证书指纹（thumbprint）——这样 STS 验证 JWT 签名时，不需要实时回调集群，只要拿本地存的公钥/指纹去验签就行，速度快、也不产生额外的信任链依赖。真正做访问控制的，是 IAM Role 的 _trust policy_ 里那条 `StringEquals` 条件，精确锁死只有哪个命名空间下、哪个 service account 签发的 token 才配换出这个角色的权限——光有效签名不够，claim 对不上照样拒绝。最后拿到的不是永久密钥，而是 `AssumeRoleWithWebIdentity` 换来的临时 _AccessKeyId/SecretAccessKey/SessionToken_ 三件套，通常一小时就过期，即便 token 或临时凭证泄露，窗口也被死死限死。这也是为什么调查 AccessDenied 时，十有八九是 `sub`/`aud` 条件写错、OIDC provider 没注册对，或者 token 已经过期。

_隐喻对应表_

- 漠北总粮仓 → AWS 账户里的目标资源/角色
- 雁门关的关主 → 集群自身的 _OIDC issuer_
- 每日一张的手谕 → pod 的 _service account token_（短期 _JWT_）
- 手谕上的花押 → JWT 的数字签名
- 挂在仓房墙上的花押拓片 → AWS 里注册的 _OIDC identity provider_ 及其证书指纹（_JWKS/thumbprint_）
- "字号必须分毫不差"的铁规矩 → IAM _trust policy_ 里对 `sub`/`aud` 的条件校验
- 一个时辰后自动作废的手谕 → token 的短 `exp`，kubelet 自动轮换
- 临时领粮牌 → `AssumeRoleWithWebIdentity` 换来的临时 STS 凭证
- "偷去也只能闹一个时辰" → 短期凭证泄露后风险窗口被大幅收窄
- 老办法里终身有效的铜钥匙 → 传统的静态 long-lived AWS access key，一旦泄露永久有效、极难收回
</section>
<section class="en" markdown="1">
Beyond Yanmen Pass, a string of small outposts kept watch over short stretches of trade road. Further west sat the imperial "Grand Granary," reserved for emergency grain requisitions.

The old rule was simple: each outpost was issued a bronze key to the granary. Once cast, a key worked forever — whoever held it could open the gate. This worked fine for years, until one day a courier's key was lifted by a thief mid-journey. The thief was in no hurry; he sat on the key for three months before using it. The granary had no idea the key had changed hands — it opened the gate just the same. Only then did the granary keeper realize the danger: once a key like that goes missing, there's no way to reclaim it short of sending someone all the way to the frontier to physically recast it.

So they devised a new system. The frontier no longer cast keys. Instead, the Pass Commander — not the granary keeper, the commander at the pass itself — issued each outpost a fresh _chit_ every day. The chit bore the commander's personal seal, stated plainly "issued only to the grain-courier of the Third Outpost," and carried a time limit: one hour after issuance, the ink would fade and the chit would go blank.

When a courier arrived at the granary with a chit, the keeper never sent a rider back to the pass to ask "is this really the commander's seal?" Years earlier, he'd hung a rubbing of the commander's seal pattern on the storehouse wall. He simply compared the chit's seal against that rubbing on the spot — no need to consult the commander in person.

A matching seal wasn't enough on its own, either. The keeper's ledger carried an ironclad rule: even a chit bearing the commander's genuine seal would be refused unless it read, word for word, "grain-courier of the Third Outpost." A chit for the Fifth Outpost, seal and all, got turned away just the same.

Only once a chit passed both checks did the keeper hand over a _temporary grain token_ — good for exactly one hour, after which it too went blank, forcing a fresh chit and a fresh token on the next visit. From then on, even if some chit or token fell into a thief's hands, the most damage he could do was cause trouble for that single hour. Soon enough the chit and token turned to waste paper — no more lifetime key to steal.

——By now you've probably recognized it: that chit is a _Kubernetes service account token_ — an _OIDC_-issued _JWT_. The seal rubbing hanging on the storehouse wall is the _OIDC identity provider_ that AWS registered ahead of time (along with its _JWKS_ / certificate thumbprint). The keeper's ironclad rule that the wording must match exactly is the IAM _trust policy_'s condition check on `sub` and `aud`. And that one-hour grain token is the _temporary STS credential_ obtained by calling _sts:AssumeRoleWithWebIdentity_.

_The concept, and why it matters_
On AWS this mechanism is called _IRSA_ (IAM Roles for Service Accounts); ROSA/OpenShift use an equivalent pattern. The core idea is replacing long-lived static AWS access keys sitting in a pod with _OIDC federation_. The cluster itself acts as an _OIDC issuer_: kubelet automatically mounts each pod's service account with a short-lived, auto-rotated _JWT_ carrying key claims — `iss` (the issuer, i.e. the cluster's OIDC endpoint), `sub` (typically `system:serviceaccount:<namespace>:<service-account>`), `aud` (audience, commonly `sts.amazonaws.com`), and `exp` (usually about an hour). On the AWS side, the cluster is pre-registered as an _IAM OIDC identity provider_, along with its certificate thumbprint — so when STS verifies the JWT's signature, it doesn't need to call back to the cluster in real time; it just checks against the locally stored public key/thumbprint, which is fast and avoids extra trust-chain dependencies. The actual access control happens in the IAM role's _trust policy_, via a `StringEquals` condition that pins down exactly which namespace and service account's token may assume that role — a valid signature alone isn't enough; a claim mismatch still gets rejected. What comes back isn't a permanent key but a temporary _AccessKeyId/SecretAccessKey/SessionToken_ triple from `AssumeRoleWithWebIdentity`, typically expiring within an hour — so even if the token or the temporary credential leaks, the exposure window is tightly bounded. This is also why, nine times out of ten, an `AccessDenied` investigation traces back to a mismatched `sub`/`aud` condition, a misregistered OIDC provider, or a simply expired token.

_Metaphor mapping_

- The Grand Granary out west → the target AWS resource/role
- The Pass Commander → the cluster's own _OIDC issuer_
- The daily chit → the pod's _service account token_ (a short-lived _JWT_)
- The seal on the chit → the JWT's digital signature
- The seal rubbing on the storehouse wall → the _OIDC identity provider_ registered in AWS, with its _JWKS_/thumbprint
- "The wording must match exactly" → the IAM _trust policy_'s condition check on `sub`/`aud`
- The chit that fades after an hour → the token's short `exp`, auto-rotated by kubelet
- The temporary grain token → the temporary STS credential from `AssumeRoleWithWebIdentity`
- "Stolen, it can only cause trouble for an hour" → the shrunken blast radius of a leaked short-lived credential
- The old lifetime bronze key → a traditional static, long-lived AWS access key — permanently valid once leaked, nearly impossible to revoke
</section>
