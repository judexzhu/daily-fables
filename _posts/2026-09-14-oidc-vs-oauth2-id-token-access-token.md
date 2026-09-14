---
layout: fable
title: "客栈铜匙与通关玉牒 · The Copper Room Key and the Jade Passport"
title_zh: "客栈铜匙与通关玉牒"
title_en: "The Copper Room Key and the Jade Passport"
concept: "OIDC vs. OAuth 2.0: ID Token vs. Access Token"
tags: [security, identity, web]
illustration: /assets/art/2026-09-14-oidc-vs-oauth2-id-token-access-token.jpg
---
<section class="zh" markdown="1">
临安城南的四海客栈，每日往来的行商足有数百人。

客栈里的老掌柜有一套用了几十年的规矩：每逢客商入住，柜台便发下一把沉甸甸的黄铜钥匙。这把铜钥匙能打开二楼天字二号房的雕花门栓，还能在清晨凭匙向后厨伙计支取一壶热水与一叠细软。

有一回，湖州来的丝商陆修揣着这把铜钥匙去城东的万盛茶庄谈一笔万两白银的茶丝大单。
茶庄的李掌柜验看生丝无误，正色问道：“陆客商，契书虽好，老朽却未曾与你谋面。今日落印前，你须得出示官府凭信，证明你确是湖州织造世家陆氏的大郎君，否则老朽怎敢将万两定金交予旁人？”

陆修在袖袋里摸索半晌，掏出了那把四海客栈的黄铜钥匙，推到案头：“掌柜请看，这是四海客栈上房的钥匙。客栈掌柜阅人无数，若非豪商正客，岂能住进天字二号房？”

李掌柜端详着那把钥匙，忽然摇头失笑：
“陆公子，你这铜匙，只能证明你拿得起二号房的锁簧，能开门取水，何曾写着你姓陆还是姓李？纵是昨夜客栈跑堂的小厮顺手偷了它，或是贼人在街角捡了去，拿到这间茶室，手里的钥匙也是一般模样。你拿一把只能开柜开门的‘开锁之物’，如何证明你是何方人士？”

更教李掌柜心惊的是：若自己当真收下这把铜钥匙打发伙计去四海客栈核对，伙计揣着钥匙，竟能顺理成章拧开天字二号房的门，把客房里的箱笼细软翻个底朝天——这把钥匙给出去是叫人“认人”，却在暗地里将“开箱之权”也一并拱手相让了。

次日，临安府户部掌籍署颁下新章程，专为此等乱象立规矩。从此客商出门，身上必佩两样信物：

第一件，唤作**“通关玉牒”**。
由户部官署亲手裁切温润白玉，以朱砂重印盖上官印。玉牒正面分毫不差刻明三件事：此牒由临安户部签发、持有者乃湖州陆修、此玉牒专呈给万盛茶庄李掌柜验看，底端更注有时辰与一枚防伪的暗纹刻痕。
万盛茶庄的李掌柜只需亲自接过玉牒，对着天光一照官印朱文，便立时断定眼前站的正是陆修本人。而这枚玉牒不能插进任何铜锁，纵被旁人瞧了去，也开不了半扇房门。

第二件，仍是那把**“客栈铜匙”**。
若陆修要差遣茶庄的粗使小厮替自己回客栈取样丝，他绝不给玉牒，只将铜匙交到小厮手里。客栈门房与后厨认匙不认人，小厮凭匙推门取物，片刻不得延误。门房绝不会盘问小厮祖籍何处，而小厮拿着铜匙，也休想跑去官府冒充陆家大郎。

自此，临安商贾皆知一句金科玉律：
“玉牒验人（Authentication），铜匙开门（Authorization）。
拿开门的钥匙去证明你是谁，是招贼入室；拿验人的玉牒去插锁孔，是缘木求鱼。”

— — —

### 这是什么

——到这儿你大概已经认出来了：这便是分布式身份架构中最经典、也最容易被混淆的孪生体系：*OAuth 2.0* 与 *OpenID Connect (OIDC)*。

*OAuth 2.0* 诞生之初，是一个纯粹的**受限资源授权框架（Authorization Framework）**。它解决的核心问题是“委托授权”（Delegation）：用户通过授权服务器给第三方客户端发放一枚 *Access Token*（访问令牌，即故事里的客栈铜匙），客户端凭它去调用资源服务器（Resource Server / API）获取数据。
Access Token 是一张典型的“不记名凭证”（Bearer Token）——资源服务器只校验这枚令牌是否具备访问权限（Scope），完全不在乎持有它的人到底是谁。早期互联网应用普遍误将 Access Token 当作“登录凭证”，由客户端直接读取它来判定用户已登录，导致了极易遭受令牌置换与混淆代理攻击（Confused Deputy / Token Substitution Attack）的巨大安全漏洞。

为了彻底解决“认人”的问题，互联网工程界在 OAuth 2.0 之上构建了专门的**身份认证层（Identity Layer）**，这便是 *OpenID Connect (OIDC)*。
OIDC 引入了一枚全新的凭证——*ID Token*（身份令牌，即故事里的通关玉牒）。ID Token 是一个符合 *JWT*（JSON Web Token）规范的紧凑型结构化数据，由身份提供商（IdP）用私钥进行数字签名。它包含标准化的身份声明（Claims）：
- `iss`（Issuer）：标识签发玉牒的身份认证中心；
- `sub`（Subject）：被认证用户的全局唯一标识符；
- `aud`（Audience）：该身份令牌**专为哪个客户端**签发（必须匹配客户端自身的 `client_id`）；
- `iat` 与 `exp`：签发时间与失效过期时间；
- `nonce`：防止令牌被截获重放的密码学校验字符串。

### 为什么重要

厘清两者的分工，是构建一切零信任架构与现代 Web/移动应用的安全底线：

1. **凭据目标受众与消费者的严格隔离**：
   *ID Token* 的唯一消费者是**前端或第三方客户端应用（Client）**，客户端必须就地校验其签名、`aud` 和 `nonce`，从中提取用户的身份画像（Profile），绝不能拿 ID Token 去请求后端业务 API；
   相反，*Access Token* 的消费者是**资源服务器（API）**，客户端只负责传递它，不应解析其内部细节。
2. **根除令牌置换攻击（Token Substitution）**：
   在没有 OIDC 的时代，恶意黑客可以将在恶意网站 App A 授权获得的 Access Token 拦截并发送给目标网站 App B；如果 App B 单纯以“能换到数据”来认证用户，黑客就能非法登录受害者的账户。OIDC 的 ID Token 中包含强制性的 `aud: app-b-id`，App B 发现受众不符会当场拒绝，直接阻断跨站点身份冒用。
3. **无状态高性能验签**：
   客户端只需在启动时拉取 IdP 的公钥集合（JWKS），后续便可对每一个 ID Token 在本地进行微秒级的非对称验签，彻底告别了每一次用户交互都要向 IdP 发起阻塞式远程调用（如 `/verify_token`）的性能瓶颈。

_隐喻对应表_

- 客栈黄铜钥匙 → OAuth 2.0 访问令牌（Access Token，代表调用 API 的能力与授权）
- 通关白玉牒 → OIDC 身份令牌（ID Token，基于 JWT 签发的用户身份证明）
- 临安府户部掌籍署 → 身份提供商（Identity Provider / IdP，如 Okta / Auth0 / Keycloak）
- 万盛茶庄李掌柜 → 依赖方客户端（Relying Party / Client App）
- 客栈天字二号房与箱笼 → 资源服务器（Resource Server / Protected API）
- 玉牒刻下“专呈万盛李掌柜” → ID Token 中的受众声明（aud = client_id）
- 玉牒朱砂官印与暗纹 → JWT 数字签名（Signature）与防重放随机数（nonce）
- 粗使小厮持钥匙开门取丝 → 客户端持 Access Token 向 API 读取受保护资源
- 错拿开门钥匙去证名分 → 滥用 OAuth 2.0 Access Token 做伪认证引发的身份混淆漏洞
</section>

<section class="en" markdown="1">
At the Four Seas Inn south of the imperial capital of Lin'an, hundreds of merchant travelers passed through the timber gates each day.

For decades, the venerable innkeeper adhered to an unbroken convention: upon check-in, the front desk bestowed a heavy brass key upon the guest. This key turned the engraved mortise lock of Room Two on the upper floor, and in the crisp dawn, presenting it to the kitchen apprentices entitled the bearer to a kettle of piping hot water and fresh linen.

One spring afternoon, Lu Xiu, a young silk merchant from Huzhou, carried this brass key across town to the Wansheng Tea House, seeking to seal a contract for ten thousand taels of silver.
Master Li, the seasoned proprietor of the tea house, inspected the raw silk samples with satisfaction. But before wetting his brush with ink, he leveled a piercing gaze at the young traveler: "Young Master Lu, your merchandise is flawless, yet we have never broken bread before. Before I stamp this agreement, I require an official seal of identity proving that you are indeed the eldest heir of the venerable Lu clan. How else could I entrust ten thousand taels of silver to an unfamiliar hand?"

Lu Xiu rummaged through his embroidered sleeves and produced the heavy brass key of the Four Seas Inn, sliding it across the cypress table: "Master Li, pray inspect this. This is the key to the finest guest room at the Four Seas Inn. The innkeeper has seen emperors and ministers; would he entrust Room Two to any common drifter?"

Master Li scrutinized the piece of forged metal and broke into a quiet chuckle:
"Young Master, this key only proves you can turn the brass tumblers of a lock on the second floor of the Four Seas Inn. Does it bear your surname? Does it record your ancestral home? If an errand boy swiped it from your washbasin an hour ago, or if a highwayman found it in the gutter, it would turn that same lock just as easily. How can an instrument meant solely for turning locks testify to the living soul standing before me?"

Master Li leaned forward, his voice dropping: "What is worse: were I to accept this key and send my apprentice to verify it at the inn, he could effortlessly slip into your bedchamber and plunder your silk chests bare. In using a key to prove your identity, you have unwittingly surrendered the power to strip your rooms!"

The very next day, the Imperial Ministry of Revenue promulgated an imperial decree to rectify such perilous confusions across the capital. From that hour forward, all traveling merchants were required to carry two distinct credentials:

The first was the **Carved Jade Passport** (*The ID Token*).
Quarried from white mutton-fat jade, it bore the official vermillion seal of the Ministry. Upon its polished face, three truths were carved in deep relief: that it was issued by the Imperial Ministry, that the subject was Lu Xiu of Huzhou, and that it was crafted solely for the inspection of Master Li at Wansheng Tea House. Beneath these ran a precise timestamp and an intricate counter-fraud cipher.
Master Li had only to inspect the jade plaque against the morning sun, verify the cinnabar seal with his own eyes, and instantly establish the identity of the young merchant. The jade plaque possessed no notches, fitted no lock, and even if held by a thief, could not budge a single wooden latch.

The second was the familiar **Brass Key** (*The Access Token*).
When Lu Xiu required a hired porter to retrieve silk spools from the inn, he never surrendered his jade plaque. He handed the brass key to the porter. The innkeeper and kitchen staff recognized the key without demanding genealogies; the porter turned the lock, gathered the requested bolts, and departed. The innkeeper never questioned the porter's bloodline, and the porter, key in hand, could never march into the Magistrate's hall pretending to be the master of the Lu estate.

Ever after, the merchants of Lin'an lived by an inviolable maxim:
"The Jade Passport verifies who you are (Authentication); the Brass Key unlocks what you may touch (Authorization).
To present a door-key as proof of pedigree is to invite thieves into your home; to thrust a jade genealogy into a lock-tumbler is to shatter your own name."

— — —

### What it is

By now the architecture is unmistakable: this is the classic, vital distinction at the foundation of modern distributed identity: *OAuth 2.0* versus *OpenID Connect (OIDC)*.

*OAuth 2.0* was conceived purely as an **authorization framework for delegated access**. It was engineered to solve one problem: allowing a resource owner to delegate restricted access to an application via an *Access Token* (the brass room key). The client presents this token to a Resource Server (API) to retrieve data.
The Access Token is a bearer credential: the API verifies whether the token has the requisite permissions (*scopes*), entirely indifferent to who originally presented it. In the early days of the web, developers dangerously abused Access Tokens as pseudo-identity passes—inferring that if a client could fetch an email address with a token, the user was authenticated. This misconception fostered devastating security vulnerabilities, including Confused Deputy and Token Substitution attacks.

To resolve identity once and for all, the security community built a dedicated **identity authentication layer** atop OAuth 2.0: *OpenID Connect (OIDC)*.
OIDC introduced an entirely separate artifact: the *ID Token* (the carved jade passport). The ID Token is a cryptographically signed *JSON Web Token (JWT)* issued by an Identity Provider (IdP) for direct consumption by the client application. It contains standardized identity assertions (*claims*):
- `iss` (Issuer): The trusted identity authority that minted the token;
- `sub` (Subject): The globally unique, immutable identifier for the authenticated user;
- `aud` (Audience): The specific client application for which this token was minted (matching its registered `client_id`);
- `iat` & `exp`: Precise issued-at and expiration timestamps;
- `nonce`: A cryptographic string binding the request to prevent replay attacks.

### Why it matters

Maintaining a rigorous separation between ID Tokens and Access Tokens is an absolute prerequisite of Zero Trust architecture:

1. **Audience and Consumer Segregation**:
   The sole intended consumer of an *ID Token* is the **client application** itself. The client validates its signature, audience, and nonce locally to establish a user session. The client must never send the ID Token to a downstream resource API. Conversely, the consumer of an *Access Token* is the **resource server (API)**. The client treats it as an opaque credential, merely relaying it in the `Authorization: Bearer` header.
2. **Elimination of Token Substitution Attacks**:
   Before OIDC, an attacker could authorize with a malicious app, intercept their own Access Token, and send it to a victim application. If the victim app trusted any valid token to establish identity, the attacker could hijack user accounts. With OIDC, every ID Token carries an explicit `aud` claim pointing strictly to the client that requested it. A token minted for App A will be rejected on sight by App B.
3. **Stateless, Low-Latency Local Verification**:
   Clients download the IdP's public keys (*JWKS*) once and verify ID Token signatures locally in microseconds. This eliminates synchronous round-trips to remote `/verify_token` endpoints on every user action, removing a notorious architectural bottleneck while fortifying security.

_Metaphor mapping_

- Brass room key → OAuth 2.0 Access Token (Delegated capability to invoke protected APIs)
- Carved white jade passport → OIDC ID Token (Signed JWT proving the verified identity of the user)
- Ministry of Revenue Registrar → Identity Provider (IdP, e.g., Okta, Auth0, Keycloak)
- Master Li of Wansheng Tea House → Relying Party / Client Application
- Inn room lock and silk chests → Resource Server / Protected API
- Jade carving specifying "For Master Li" → The audience claim (`aud = client_id`) in the ID Token
- Cinnabar imperial seal and cipher → Cryptographic JWT signature and anti-replay `nonce`
- Hired porter using the key to fetch silk → Client presenting Access Token to access resource endpoints
- Presenting a room key to prove ancestry → Pseudo-authentication vulnerability using Access Tokens
</section>
