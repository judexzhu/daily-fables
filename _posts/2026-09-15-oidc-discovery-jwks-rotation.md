---
layout: fable
title: "石经台的榜文与轮换印鉴 · The Stone Stele's Notice and the Rotating Seals"
title_zh: "石经台的榜文与轮换印鉴"
title_en: "The Stone Stele's Notice and the Rotating Seals"
concept: "OIDC Discovery & Dynamic JWKS Key Rotation"
tags: [security, identity, web]
illustration: /assets/art/2026-09-15-oidc-discovery-jwks-rotation.jpg
---
<section class="zh" markdown="1">
京师尚书省掌管天下公文，每日发往各州府、各大行会与关防榷场的文书数以千计。

早年间，各大商帮和各处关卡吃尽了苦头。
原来，每家行会账房都自己抄录了一本“尚书省指南”：哪座衙门管初审申饬、哪座库房管勘验发牒、后堂供奉的官印又是何种花纹。
可官署并非亘古不变：有时承办司署迁入东坊，有时交割粮饷改换西库；更要紧的是，为了防止内阁印信被宵小模拓盗用，尚书省每隔三五个月便要重铸一枚新铜印，旧印当场销毁。

每逢官署挪地或换印，京城便乱成一锅粥。
快马信使跑遍三十六行会传递更名告牒，可总有偏远的关卡或闭门盘账的商号未能及时收到消息。
茶商拿着加盖了新秋印的通关文书赶到南城门，守关军士翻出半年前抄录的老图谱比对，见花纹不符，怒斥文书伪造，当场将整队茶车扣押；
而更有行会掌柜固执己见，依旧信奉多年前私藏的废印拓片，反教投机奸商钻了空子。
尚书省老尚书为此大动肝火：“衙门动迁与印鉴防伪本是朝廷常事，怎能每换一次，就逼得天下所有账房跟着闭门停市？”

方尚书亲自定下一套“石经台章程”，立解此乱。

第一着，立**“石经台榜文”**。
尚书省在皇城正门最显要的白石经台上，立起一座永不移徙的照壁石碑。天下任何人、任何商贾，只要踏入京师，抬眼就能在此处读到一份官修告示：
“欲交验初审，请赴东华门文华阁；欲支领终结勘符，请赴景运门左银库；欲查验本朝在用官印拓本，请移步西阙验印阁木匣。”
从此各家行会账房再无须各凭道听途说死记地址；只要认得皇城脚下的石经台，抬眼一览，各项最新公署司职一目了然。

第二着，设**“轮换印匣与暗号”**。
在石经台告示指引的西阙验印阁内，悬挂着一只明晃晃的沉香木匣，专供天下人参阅官印拓本。
匣内从不单摆一枚死印，而是整齐陈列着两三张盖着红泥的蜡封拓样。每张拓样右下角，皆用极细的金线凿刻了清晰的印号——如“甲辰春印”、“甲辰夏印”。
尚书省每逢轮换新印，必提前半个月将新铸的“甲辰夏印”拓本放入验印匣中，与尚在使用的“春印”并列公示；
新签发的文书骑缝处，除了鲜红印章，亦明确标出“本件用甲辰夏印签发”。

守关军士与商行账房再见文书，规矩彻底变了：
军士扫一眼文书上的印号，若自己脑中只记得“春印”，便抬脚走到西阙验印匣前一瞧，果然新挂着一枚“夏印”拓片，字迹纹路分毫不差。当场验毕放行，并顺手在军营簿子上抄录下新拓本。
待再过数月，所有以“春印”签署的文书自然过期，尚书省才从匣中从容撤下老旧拓片。

自此，京师公署任凭升迁搬挪，尚书省官印任凭春秋轮换，天下公文流转从容不紊，再无一日滞碍。

— — —

### 这是什么

——到这儿你大概已经认出来了：这便是现代分布式系统与微服务架构中，实现身份认证高可用与安全治理的核心双璧：*OpenID Connect (OIDC) Discovery* 与 *JWKS 动态密钥轮换（Dynamic Key Rotation）*。

在分布式身份认证中，依赖方客户端（Relying Party / Client）与后端资源服务器（Resource Server）为了接入身份提供商（IdP，如 Auth0、Okta、Keycloak、Google），必须知道许多通信端点与加密参数：
- 登录认证端点（`authorization_endpoint`）
- 令牌换取端点（`token_endpoint`）
- 用户信息端点（`userinfo_endpoint`）
- 身份提供商的公钥集合地址（`jwks_uri`）
- 支持的签名算法列表（`id_token_signing_alg_values_supported`）

早期的系统将这些 URL 强行硬编码写在客户端的配置文件里。一旦 IdP 升级域名、变更路由或分拆服务，成百上千个微服务就必须停机修改配置并全量重发，极易引发严重故障。
*OIDC Discovery* 协议彻底终结了这一痛点：它定义了一个全球通用的“石经台”标准路径——`/.well-known/openid-configuration`。客户端只需要知道 IdP 的唯一根域名（Issuer URL），向该标准路径发起一次 `GET` 请求，即可动态获取包含全部端点元数据的标准 JSON 文档，实现零配置自适应发现。

而 *JWKS（JSON Web Key Set）* 则是保证密钥生命周期轮换（Key Rotation）平滑无感的核心规范：
IdP 使用非对称私钥（如 RSA256 或 ES256）为每个 *ID Token* 盖章签名，并将对应的公钥发布在 `jwks_uri`。
JWKS 是一个包含多个公钥对象的 JSON 数组，每个公钥都带有唯一的“印号”——`kid`（Key ID）。
当 IdP 需要轮换密钥时：
1. **多公钥并存**：IdP 生成新的密钥对，并将新公钥以全新的 `kid` 放入 JWKS 数组，与旧公钥同时暴露在 `jwks_uri`；
2. **新印发签**：IdP 开始使用新私钥签名新颁发的 JWT，在 Token 的 Header 中标明 `kid: "new-key-id"`；
3. **按需热刷新验签**：客户端解析 Token Header 中的 `kid`。若在本地内存缓存中找到对应的公钥则立即完成验签；若发现未知的 `kid`，则自动向 `jwks_uri` 重新拉取最新的 JWKS，缓存新公钥并成功验签，无需重启任何微服务；
4. **平滑退役**：直到所有旧 Token 彻底自然过期，IdP 才从 JWKS 中将旧公钥下线。

### 为什么重要

这套机制是现代互联网基础设施能够“永远在线、热更新不宕机”的关键设计：

1. **零停机动态密钥轮换（Zero-Downtime Rotation）**：
   安全合规（如 PCI-DSS、SOC 2）通常强制要求加密密钥每 90 天定期轮换，遭遇潜在泄露时更需要紧急熔断轮换。基于 `kid` 的动态匹配机制，让 IdP 的密钥更换对成千上万个下游客户端和微服务完全透明，彻底告别了“换一次公钥就要重启全世界”的噩梦。
2. **自愈与防雪崩缓存策略**：
   成熟的 OIDC 客户端（如 `go-oidc`、`node-jose`）对 JWKS 实行“常态内存缓存 + 未知 `kid` 异步/限流回源”的自愈策略。平时完全依靠本地内存极速验签（零网络开销），遇到新 `kid` 时平滑刷新，同时加入速率限制（Rate Limit）防止恶意伪造 `kid` 打垮 IdP 的 `jwks_uri`。
3. **环境迁移与配置解耦**：
   在容器化与 Kubernetes 部署中，应用只需注入单一环境变量（如 `OIDC_ISSUER_URL=https://auth.company.com`），无需配置十几个纷繁芜杂的内部端点。开发、测试与生产环境一键平移，极大降低了运维心智负担与配置漂移风险。

_隐喻对应表_

- 皇城正门前永不移徙的白石经台 → OIDC Discovery 标准发现路径（`/.well-known/openid-configuration`）
- 石经台镌刻的官修告示 → IdP 签发的 OpenID 元数据 JSON 文档（包含所有 endpoints）
- 礼部文华阁与景运门左银库 → 认证端点（`authorization_endpoint`）与令牌端点（`token_endpoint`）
- 西阙验印阁与沉香木匣 → JSON Web Key Set 公钥端点（`jwks_uri`）
- 盖满红泥的蜡封拓本公开展出 → 发布在 JWKS 中的公开非对称加密公钥（Public Keys）
- 拓片右下角凿刻的金线印号 → JWT Header 中的密钥标识符（`kid` - Key ID）
- 新旧两枚拓片在匣中并存公布 → 密钥平滑过渡期内 JWKS 数组同时保留新旧公钥
- 军士见新印号即去木匣取新样并记入营簿 → 客户端遭遇未知 `kid` 动态拉取并刷新本地 JWKS 缓存
- 掌柜凭老黄历抄本死记地址与印信 → 早年客户端硬编码端点与静态证书引发的停机雪崩
</section>

<section class="en" markdown="1">
In the imperial capital, the Grand Secretariat governed official decrees for the entire realm, dispatching thousands of wax-sealed dispatches each day to provincial magistracies, trade guilds, and garrison outposts.

In earlier years, merchant guilds and frontier sentries suffered ceaseless misery.
Every guild accounting house maintained its own hand-copied "Secretariat Directory": which eastern compound received initial petitions, which treasury vault dispensed ratified tallies, and what intricate cinnabar crest the imperial court currently stamped upon official seals.
Yet government offices were never cast in stone: petition desks relocated from the eastern avenue to the imperial palace gates; disbursement offices moved across the canal; and most critically, to prevent counterfeiters from copying worn stamps, the Secretariat recast its official bronze seal every few seasons, melting the obsolete cipher on the spot.

Whenever an office relocated or a seal was recast, the capital descended into utter disarray.
Couriers galloped across town bearing notices to thirty-six separate guild halls, but distant sentry towers and reclusive merchant bankers inevitably missed the heralds.
When a tea merchant arrived at the southern ramparts presenting a transit permit bearing the newly minted Autumn Seal, the garrison guard thumbed through his six-month-old illustrated parchment, found the floral whorls mismatched, decried the document as an audacious forgery, and seized the entire caravan on the spot.
Meanwhile, gullible clerks who stubbornly clung to obsolete seal impressions were routinely swindled by opportunistic frauds brandishing retired stamps.
The venerable Chancellor Fang grew furious: "Department reorganizations and cryptographic seal rotations are the necessary routines of statecraft! How can every renewal force the commerce of an entire empire to halt in confusion?"

Chancellor Fang personally instituted the **Stone Stele Protocol**, dissolving the chaos forever.

The first decree was the erection of the **Public Noticeboard on the Stone Stele**.
Outside the Meridian Gate of the Imperial Citadel, upon a permanent, unmoving monument of polished white granite, the Secretariat carved an eternal proclamation. Any traveler, merchant, or scholar entering the capital had only to glance at this single, universally known pillar to read the authoritative directory:
"To submit initial petitions for review → The Wenhua Pavilion at Donghua Gate;
To exchange verified tokens for currency → The Left Vault at Jingyun Gate;
To inspect the impressions of official seals currently in active use → The Cedar Cabinet at the Western Gatehouse."
No guild accountant ever needed to memorize shifting street addresses or trust street gossip again. So long as they knew the unchanging location of the Imperial Stele, all active departmental endpoints were revealed in a single glance.

The second decree was the creation of the **Rotating Seal Cabinet with Tagged Ciphers**.
Within the Western Gatehouse designated by the Stele's notice, the Secretariat suspended a polished cedar cabinet open to public view.
The cabinet never displayed a solitary seal impression. Instead, it neatly exhibited two or three distinct cinnabar wax seal impressions pressed into parchment. Beneath each impression, fine golden characters chiseled a distinct, permanent cipher tag — such as "Spring Seal, Year of Jiachen" or "Summer Seal, Year of Jiachen".
Whenever the Secretariat prepared to transition to a newly carved bronze stamp, it placed the new "Summer Seal" wax impression into the cabinet two full weeks in advance, displaying it alongside the active "Spring Seal".
Furthermore, every newly issued decree bore not only the vermillion stamp but an explicit notation in its margin: "Affixed under Summer Seal, Year of Jiachen."

For border guards and guild clerks, verification was instantly transformed:
When a sentry inspected a decree and noticed an unfamiliar seal tag, he did not reject the traveler. He walked ten paces to the Cedar Cabinet at the Western Gatehouse, matched the impression against the newly posted Summer Seal, verified every curve and stroke, and recorded the new impression into his field journal.
Months later, when all decrees bearing the old Spring Seal reached their natural expiration, the Secretariat quietly retired the old wax rubbing from the cabinet.

Ever after, though ministries relocated and imperial ciphers rotated with the seasons, the commerce and governance of the realm flowed smoothly, without a single hour of disruption.

— — —

### What it is

By now the architecture is unmistakable: this is the twin engine of high availability and robust security in modern distributed identity: *OpenID Connect (OIDC) Discovery* and *Dynamic JSON Web Key Set (JWKS) Key Rotation*.

In modern distributed identity systems, Relying Party clients (web frontends, mobile apps) and Resource Servers (backend APIs) need to coordinate with an Identity Provider (IdP, such as Okta, Auth0, Keycloak, or Google) across numerous endpoints and cryptographic parameters:
- The user login endpoint (`authorization_endpoint`)
- The token exchange endpoint (`token_endpoint`)
- The user profile endpoint (`userinfo_endpoint`)
- The public key discovery endpoint (`jwks_uri`)
- The supported cryptographic signing algorithms (`id_token_signing_alg_values_supported`)

In the fragile days of early microservices, developers hardcoded these URLs into individual configuration files. Whenever an IdP migrated domains, updated routing, or isolated endpoints, hundreds of applications had to be manually reconfigured and redeployed, triggering catastrophic outages.
*OIDC Discovery* resolved this by standardizing an immutable "Stone Stele" path: `/.well-known/openid-configuration`. Any client needs only the IdP's root Issuer URL. Performing a single HTTP `GET` against this standardized path dynamically returns a structured JSON document containing every authoritative endpoint and capability, enabling self-configuring, zero-maintenance integration.

Complementing this is the *JSON Web Key Set (JWKS)* specification, which governs smooth cryptographic key lifecycle management:
The IdP signs each *ID Token* with an asymmetric private key (e.g., RSA256 or ECDSA) and publishes the corresponding public verification keys at `jwks_uri`.
The JWKS payload is a JSON array of public keys, where each key object is stamped with a unique Key ID (`kid`).
When the IdP needs to rotate its signing keys:
1. **Multi-Key Coexistence**: The IdP generates a new cryptographic key pair and publishes the new public key alongside the active one in the `jwks_uri` array;
2. **Tagging New Signatures**: The IdP begins signing newly minted tokens with the new private key, tagging the JWT header with `kid: "new-key-id"`;
3. **On-Demand Cache Refresh**: When a client receives an ID Token, it inspects the header's `kid`. If the key is already in memory, it verifies the signature in microseconds. If the `kid` is unfamiliar, the client automatically re-fetches `jwks_uri`, updates its local cache with the new public key, and completes verification without dropping a single user session;
4. **Graceful Deprecation**: Only after all tokens signed with the old key have naturally expired does the IdP prune the legacy public key from the JWKS array.

### Why it matters

This architectural pairing is why enterprise identity infrastructures can remain continuously operational during security maintenance and credential lifecycle rotations:

1. **Zero-Downtime Cryptographic Key Rotation**:
   Regulatory compliance frameworks (SOC 2, ISO 27001, PCI-DSS) mandate that signing keys rotate on regular schedules (e.g., every 90 days), and security incidents require instantaneous key revocation. Tagging signatures with a `kid` that points to a dynamic JWKS array makes private key rollover entirely transparent to downstream clients. The nightmare of coordinated global microservice restarts is abolished.
2. **Self-Healing, Low-Latency Cache Architecture**:
   Production-grade OIDC client libraries (e.g., `go-oidc`, `jose`) maintain an in-memory key cache for microsecond local verifications, triggering an asynchronous, rate-limited refresh against `jwks_uri` only when encountering an unseen `kid`. This guarantees blazing-fast local validation while remaining fully resilient to key rotations.
3. **Environment Agility and Zero Configuration Drift**:
   In containerized and Kubernetes environments, an application requires only a single environment variable: `OIDC_ISSUER_URL=https://auth.example.com`. Development, staging, and production clusters discover all routing and cryptographic parameters dynamically, completely decoupling application deployments from identity provider topology.

_Metaphor mapping_

- The permanent white granite Stone Stele outside the citadel → The OIDC Discovery well-known path (`/.well-known/openid-configuration`)
- The carved imperial notice chiseled on the stele → The OpenID Configuration JSON document detailing all IdP endpoints
- The Wenhua Pavilion and Jingyun Left Vault → The authorization endpoint (`authorization_endpoint`) and token endpoint (`token_endpoint`)
- The Cedar Cabinet at the Western Gatehouse → The JSON Web Key Set discovery URI (`jwks_uri`)
- The public cinnabar wax seal impressions displayed in the cabinet → The public cryptographic verification keys in the JWKS array
- The chiseled golden cipher tag beneath each impression → The Key ID (`kid`) in the JWT header
- Displaying Spring and Summer seal impressions side by side in the cabinet → The transitional period where JWKS serves multiple active public keys
- Sentry encountering a new tag, checking the cedar cabinet, and recording it → Client encountering an unknown `kid` and refreshing its local JWKS cache
- Guild clerks hardcoding office locations and using outdated seal rubbings → Brittle hardcoded endpoints and static certificates causing systemwide outages
</section>
