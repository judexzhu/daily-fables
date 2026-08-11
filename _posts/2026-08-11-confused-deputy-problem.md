---
layout: fable
title: "夜班门房的那串钥匙 · The Night Porter's Ring of Keys"
title_zh: "夜班门房的那串钥匙"
title_en: "The Night Porter's Ring of Keys"
concept: "The Confused Deputy Problem"
tags: [security, aws, kubernetes]
illustration: /assets/art/2026-08-11-confused-deputy-problem.jpg
---
<section class="zh" markdown="1">
长风客栈的夜班门房姓周，掌灯以后，整座楼就归他管。

他腰上挂一串铁钥匙，二十七间房，每一把都在上头。这不是他偷来的权力，是掌柜给的：夜里客人把自己锁在门外、炭盆倒了、水管冻裂，总得有个人能第一时间推门进去。老周勤快、老实，二十年没拿过客人一根线。

夜里的规矩也简单。客人下楼来，说一句"劳驾"，老周就跑一趟：取件披风、送壶热水、把落在房里的账本捎下来。他只核一件事——**你是不是本店的客人**。看一眼手里的房牌，是本店的，那就没错，跑腿去。

九号房那位穿灰袍的客人住了三天，每晚都托他跑腿，客气得很。第三夜，那人靠在柜台上说："周师傅，十二号房窗台上有只木匣子，替我取下来吧。"

老周应了一声，上楼，开门，取匣，落锁。

他没有被收买。他也没认错人——那位确实是九号房的客人，房牌真真切切。他甚至没偷懒：门是他亲手开的，锁是他亲手落的。每一步都合规矩。

可十二号房那扇门，从头到尾不知道是谁想进去。它只认钥匙，而那把钥匙是老周的。**门开的时候，用的是老周的权，办的是灰袍客人的事**——这中间的那道缝，一晚上没人低头看一眼。

第二天十二号房报了失窃。掌柜的把老周叫来问了半晌，问不出错处：他哪一步都照章办的。有洞的是那张章程本身。

后来客栈添了两条新规。

一条是木牌。客人若要托别人来取自己房里的东西，得先自己下楼到柜台，留一块亲手刻的木牌：**准取哪间房、准哪个人取**。刻什么字由房客自己定，外人猜不着，也仿不来。往后老周跑腿之前先翻木牌：没牌，不动；牌上的名字对不上眼前这位，也不动。他不再问"你是不是本店的客人"，改问一句——"**这趟差事，是谁的权？**"

另一条更干脆。掌柜的说：能不用那串万能钥匙，就别用。客人托你取他自己房里的东西，你就管他要他自己那把房门钥匙，拿着去开。开得开开不开，由他那把钥匙说了算，跟你腰上那串没关系。万能钥匙留给真正的急事——走水、爆管、人事不省。

老周后来想通了一件事：他腰上这串铁钥匙，比楼里任何一位客人手上的权都大。而**只要他肯替人跑腿，别人就等于借到了这串钥匙**——除非他每一趟都先弄清楚，这门到底是替谁开的。

——到这儿你大概已经认出来了：这就是 _confused deputy problem_（混淆代理人问题）。

## 这是什么

1988 年 Norm Hardy 给它起了这个名字。原始的例子是一个编译器服务：它有权写自己的计费文件，而用户可以指定输出文件名。某个用户把输出文件名填成计费文件的路径，编译器就用自己的权限把账本覆盖掉了。编译器没有被攻破，它只是分不清——**这一次写文件，用的是谁的授权**。

病根叫 _ambient authority_：权限跟着"我是谁"自动生效，而不是跟着"这次请求带了什么凭据"走。只要一个组件同时满足三条，洞就在那儿：它的权限大于任何一个调用方；它按调用方的指令使用自己的权限；指令里不携带授权。

云上它换过很多身衣服：

- **AWS 跨账号角色**。第三方 SaaS 拿自己账号的角色去 assume 你账号里的角色。如果 trust policy 只写"允许 SaaS 那个账号"，那么 SaaS 的**任何一位客户**都能哄着 SaaS 去动你的资源。解法就是那块木牌：`sts:ExternalId` —— 由你（资源所有者）指定一个不可猜测的值，SaaS 每次 `AssumeRole` 必须带上它。AWS 文档里对这个参数的解释，标题就叫 The confused deputy problem。
- **SSRF 打 IMDS**。应用被诱导去访问 `169.254.169.254`，用的是节点自己的凭据。IMDSv2 的 session token 加 hop limit 就是在补这条缝。
- **CSRF**。浏览器是那个门房，cookie 是那串钥匙。
- **Kubernetes / OpenShift**。一个握着 cluster-admin 的 controller，照着 CR 里的一个字段去读任意 namespace 的 Secret；或者一个 operator 替用户创建它自己本来无权创建的东西。正解是 _SubjectAccessReview_ 或 user impersonation：按**请求者**的权限判定，而不是拿自己的 ServiceAccount 一路横推。

为什么值得记住：这类漏洞不是"权限配错了"。逐行读代码，每一步都对。它是权限模型的形状问题，而形状问题只有两种修法——**designation**（把授权和指令绑在一起：capability、ExternalId、签名过的 token），或者 **impersonation**（干脆用调用方的身份去做事）。

## 故事里的谁是谁

- 夜班门房老周 → _the deputy_：权限高于所有调用方的那个组件（SaaS 的角色、operator 的 ServiceAccount、setuid 程序、你的浏览器）
- 腰上那串万能钥匙 → _ambient authority_：随身份自动生效、不随请求传递的权限
- 客人下楼说"劳驾" → 来自低权限调用方的请求：只带指令，不带授权
- 老周核房牌 → _authentication_：确认"你是谁"、"你是不是本店的"
- 全程没人问"十二号房凭什么归你" → 缺失的 _authorization_：没有确认请求者对**目标资源**的权利
- 灰袍客人 → 攻击者或恶意租户：自己没权限，但能借到 deputy 的权限
- 十二号房那扇门 → 目标资源：只认钥匙，看不见请求是谁发起的
- 房客亲手刻的木牌 → `sts:ExternalId` / capability token：由资源所有者指定、不可猜测、把"谁可以代办"写进授权本身
- 老周改口问"这趟是谁的权" → 授权决策从 deputy 的身份挪到调用方的身份（_SubjectAccessReview_）
- 改用客人自己的钥匙开门 → _impersonation_ / 降权凭据：以调用方身份执行，权限自然收敛
- 万能钥匙只留给走水爆管 → break-glass：高权限只走真正需要它的那条路径
</section>
<section class="en" markdown="1">
At the Changfeng Inn, the night porter was a man named Zhou. Once the lamps were lit, the whole building was his.

On his belt hung an iron ring of keys — twenty-seven rooms, every one of them. It was not power he had stolen; the innkeeper had given it to him. Guests lock themselves out, braziers tip over, pipes freeze and burst, and somebody has to be able to open a door at once. Zhou was diligent and honest, and in twenty years had never taken so much as a thread from a guest.

The night rules were simple. A guest came down, said "if you'd be so kind," and Zhou made the trip: fetch a cloak, carry up hot water, bring down a ledger left on the table. He checked exactly one thing — **are you a guest of this inn**. One look at the room tally in your hand: ours, all right, off he went.

The traveller in the grey robe in room nine had been there three days, and every evening sent him on some errand, always politely. On the third night, the man leaned on the counter and said: "Master Zhou, there's a small wooden box on the windowsill in room twelve. Fetch it down for me, would you."

Zhou said yes, climbed the stairs, opened the door, took the box, locked up behind him.

He had not been bribed. He had not mistaken the man for someone else — that really was the guest from room nine, and the tally was real. He hadn't even cut a corner: he opened the door himself and locked it himself. Every step was by the book.

But the door of room twelve never knew who wanted to come in. It knew only the key, and the key was Zhou's. **The door opened on Zhou's authority, in the grey-robed man's service** — and all night, nobody looked down at the gap between those two things.

The next morning room twelve reported a theft. The innkeeper questioned Zhou for a long while and could not find the mistake: he had followed the rules at every step. The hole was in the rules.

So the inn added two new ones.

The first was the tally. If a guest wanted someone else to fetch something from his room, he had to come down to the counter himself and leave a wooden tally, carved in his own hand: **which room may be opened, and which person may ask**. What was carved on it was the guest's own choice — no outsider could guess it or forge it. From then on Zhou checked the tally before any errand: no tally, no trip; a name on the tally that didn't match the man in front of him, no trip. He stopped asking "are you a guest here," and started asking — "**whose authority is this errand running on?**"

The second rule was blunter. Where you don't need the master ring, said the innkeeper, don't use it. If a guest wants something from his own room, ask him for his own room key and go up with that. Whether the door opens is then his key's business, not the business of the ring on your belt. Keep the master ring for what it was for — fire, burst pipes, a man collapsed behind a locked door.

Zhou worked out one thing afterwards. His ring held more power than any single guest in the building. And **as long as he was willing to run errands, anyone could borrow that ring** — unless, every single time, he first made sure whose door he was really opening.

— By now you've probably recognised it: this is the _confused deputy problem_.

## What this is

Norm Hardy gave it the name in 1988. The original case was a compiler service: it had permission to write its own billing file, and users could choose the output filename. A user passed the billing file's path as the output name, and the compiler cheerfully overwrote the ledger with its own privileges. The compiler was never compromised. It simply could not tell **whose authority this particular write was running on**.

The root cause is _ambient authority_: permission that attaches to "who I am" rather than travelling with "what this request carried." Wherever a component satisfies three conditions, the hole is already there — it holds more authority than any of its callers; it uses that authority on its callers' instructions; and those instructions carry no authorisation of their own.

In the cloud it wears many costumes:

- **AWS cross-account roles.** A third-party SaaS assumes a role in your account using its own. If your trust policy says only "allow the SaaS account," then **any customer of that SaaS** can talk it into touching your resources. The fix is the wooden tally: `sts:ExternalId` — an unguessable value chosen by you, the resource owner, that the SaaS must present on every `AssumeRole`. The AWS documentation for that parameter is literally titled "The confused deputy problem."
- **SSRF against IMDS.** An application is tricked into fetching `169.254.169.254` and does so with the node's own credentials. IMDSv2's session token and hop limit exist to close that gap.
- **CSRF.** The browser is the porter; your cookie is the ring of keys.
- **Kubernetes / OpenShift.** A controller holding cluster-admin reads a Secret from an arbitrary namespace because a field in a CR told it to; or an operator creates, on a user's behalf, something that user could never have created. The correct shape is a _SubjectAccessReview_ or user impersonation: decide against the **requester's** permissions, instead of bulldozing through with your own ServiceAccount.

Why it's worth remembering: this class of bug is not "permissions were misconfigured." Read the code line by line and every step is correct. It's a defect in the shape of the authority model, and shapes admit only two repairs — **designation** (bind the authorisation to the instruction: capabilities, ExternalId, signed tokens), or **impersonation** (act as the caller and inherit their limits).

## Who's who in the story

- Zhou the night porter → _the deputy_: a component with more authority than any of its callers (a SaaS role, an operator's ServiceAccount, a setuid binary, your browser)
- The master ring on his belt → _ambient authority_: permission that applies automatically by identity and never travels with the request
- A guest saying "if you'd be so kind" → a request from a lower-privileged caller: instruction without authorisation
- Zhou checking the room tally → _authentication_: confirming who you are, that you belong here
- Nobody asking why room twelve is yours → the missing _authorization_: never confirming the requester's claim on the **target resource**
- The traveller in grey → the attacker or hostile tenant: no authority of his own, but able to borrow the deputy's
- The door of room twelve → the target resource: it knows the key, and cannot see who asked
- The tally carved by the room's own occupant → `sts:ExternalId` / a capability token: chosen by the resource owner, unguessable, encoding _who may act on my behalf_ into the authorisation itself
- Zhou's new question, "whose authority is this?" → moving the decision from the deputy's identity to the caller's (_SubjectAccessReview_)
- Going up with the guest's own key → _impersonation_ / scoped-down credentials: run as the caller and the permissions shrink to fit
- The master ring reserved for fire and burst pipes → break-glass: high privilege only on the path that genuinely needs it
</section>
