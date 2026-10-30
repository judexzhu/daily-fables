---
layout: fable
title: "摆渡口的两块蜡板 · The Two Wax Tablets at the Ford"
title_zh: "摆渡口的两块蜡板"
title_en: "The Two Wax Tablets at the Ford"
concept: "HPACK header compression"
tags: [networking, performance]
illustration: /assets/art/2026-10-29-hpack-header-compression.jpg
youtube_id: "idI9Ht6gCt0"
---
<section class="zh" markdown="1">
上渡和下渡隔一条江。

上渡是盐埠，下渡是丝行。两岸各设一间关房，凡过江的货，都要在关房前写一张关单：货名、成色、税则、押运人的名号、查验的吏员、当日的干支，一行一行写全，写完才准上船。

起初这只是几笔字。

后来两岸的生意大了。一天两千条船，一条船三张单，一张单四十行。吏员写得手酸，船夫等得骂娘，江面上排着的船从渡口一直排到下游的沙洲。有关系的人开始插队，插队的人开始塞钱，塞钱的人开始多写两行。

那年秋天，关房来了个女书吏，叫阿箬。

她上任第一件事不是加快写字，是站在关房门口数单子。

数了七天，她发现一件荒唐事：两千条船的单子上，写的其实只有六十一种话。

"盐，上等，官引""丝，白，机户自织""查验：无""押运：本船船主""税则：常课"——这些话，每一张单子上都在重复。一天两千张单，四十行里有三十九行是同几句话，只有一行是新的：船号，或者斤两。

第八天，她在关房里钉了一块木牌。

木牌上预先刻好六十一条话，从上到下，编了号。全江上下所有的关房，刻的是同一块牌，同一套号，同一个顺序。

从此写单不用写字了。

吏员抬头看一眼木牌，喊一声"第八条"。对面提笔的人就在单子上写一个"八"。四十行的单子，三十九行是号码，一行是船号。一张单子从写一炷香变成写三口气。

船夫不骂娘了。

—

可是号码只管得住六十一种话。

新的货、新的税则、新的押运人名号，一天里冒出来的比木牌上刻的多得多。这些还得一字一字拼。而拼起来最费劲的，恰恰是最新的东西——木牌上没有，只能老老实实写全。

阿箬算过账：木牌挡住了七成，剩下三成里有一大半是最近才冒出来、这几天天天在冒的。它们今天是新的，明天就不再新了。

于是她钉了第二块板子，蜡做的。

规矩是这样的：

- 木牌上那六十一条是**死号**，谁都可以随时喊，永远不变。
- 蜡板上的话是**活号**。活号从六十二起往下排。**最新写上去的，号最小**；早写上去的，号慢慢变大。
- 每次在单子上用一句新话，同时要在蜡板上把它写一遍——**写在同一行的位置**。因为对面那块蜡板上也要写。
- 蜡板有分量。一句话占多少地方，不看字数，看"名字的长度 + 值的长度 + 三十二个刻痕"。那三十二个刻痕是固定的开销，句子再短也得占。
- 蜡板装满了，就从**最老的那一头**刮，刮到装得下为止。
- 要是一句话本身就比整块蜡板还重，那就把板子**整个刮干净**，而且不写上去——写上去也没用，下一句就会把它刮掉。

头一天，活号几乎没用上。第二天开始有了。第五天，两千张单子里有一千一百张的头几行全是活号。

第十天，阿箬让一个徒弟试着把一张单子全部用号码喊出来。

徒弟喊了六个号：八、六十二、六十三、六十五、六十六、七十一。

对面提笔的人一句一句写下来，写完抬头：这是一张完整的盐单，四十行，一个字没错。

六个字，换了四十行。

—

蜡板好用，但有个要命的脾气。

它**不在单子上**。

单子上只有号码。号码指的是什么，全靠两块蜡板——发货那头一块，收货那头一块。而这两块板子，从来没有人核对过，也**没办法**核对，因为板上写的东西根本不过江。过江的只有号码。

所以两块板子必须一模一样。

不是"大致一样"，是**逐行一样，连顺序都一样**。

阿箬立了三条规矩，刻在关房墙上：

**一、写新话，必须写在同一行。** 你说"这句进板子"，对面就得把同一句写在同一格。写晚一行，从此以后所有的号都错一位。

**二、刮板子，必须从最老的那头刮。** 不许因为哪句话看着没用就先刮它。板子的顺序不是"有用性"，是**年龄**。

**三、号码对不上，不许猜。** 谁喊了一个板上没有的号，或者喊的号跟对面记的对不上，**当场砍断渡绳，封江，重开**。不许"大概是这样"地补，不许跳过去接着喊。

第三条最不近人情。为了一个号码封江半天，商人们骂了她一整个冬天。

阿箬不解释。她只说过一句话：

**错一个号，后面全错。而错的东西，看起来跟对的一模一样。**

—

那年腊月，出了事。

江上起雾，一条船撞了渡桩，收货那头关房的半边屋梁塌了。人没事，蜡板碎在地上，捡起来拼，拼出六十一条和七十条之间的一个空洞——有一行蜡被踩没了。

关房的老吏员觉得这是小事。他把下面几行往上挪一格，把空填上，接着喊号。

那一整天，下渡的关单全是对的。

字迹清楚，格式合规，税也收了，货也放了。

只是有一批本该记成"官引盐，三百斤"的货，记成了"机户白丝，三十匹"。税按丝收，比盐贵一倍，商人当场就跟关房吵起来。而吵架的时候，另一批记成"盐"的丝，已经装上船，顺流下去了。

第三天对账，对不上的单子有两百七十张。

不是一句错了。是**从那一行开始，后面每一句都错**。而每一句单独拿出来看，都合法、都通顺、都像真的。

阿箬赶到下渡，看了一眼那块被"补"过的蜡板，什么也没说。

她走到渡口，抽出刀，砍断了渡绳。

封江七天。

七天里，两岸的关房把还在江上的单子全部作废，重新写了一遍——这次不用活号，一个字一个字写。

七天之后，阿箬在两岸各立了一块新规矩牌。上面只有两行：

**蜡板不许补。**
**对不上，就重来。**

—

后来阿箬又添了两条，都不太讲道理，但都有用。

第一条是给**密信**的。

有些货，押运人不肯让名号出现在任何一块板上——官盐的暗记、军器的引票、商号的私印。从前也有人图省事把这些写上蜡板，反正下次还能用。

阿箬全禁了。她说：**蜡板是给所有人看的。**

凡是不能留在板上的话，每次老老实实拼出来，而且要在单子上标一个记号：**此句不入板**。这个记号不但要让自己人看见，也要让对面知道"这一句你别记"。

她当时没讲理由。理由要过很多年才有人想明白：一句被写进板子的话，会和别人的话混在同一堆码里被反复压、反复刮；板子压得越狠，码越短；码越短，越能从码的长短反推出原来的字。**把一个秘密和你的敌人放进同一台秤里，秤会说话。**

第二条是给**坏人**的。

有商号发现，只要在单子开头塞几十句又长又没用的废话，有用的那几句就被挤出蜡板；等真正的单子来时，每一句都得重新拼。

阿箬的对策是：**板子的大小，由收的一方定，写的一方不许擅自改。** 你要刮成多大，先喊一声"我改成这么大"，而且这一句必须放在整张单子的最前面。

还有一条：一张单子能有多长，是有限度的。要是有人送来一张又短又轻、展开来却重得像座山的单子，那就不是单子，是来砸关房的。

—

阿箬在关房干了十一年。

她走那天，木牌和蜡板都还在。木牌上还是那六十一条。蜡板换过很多块，规矩一条没改。

接她班的年轻人问她：这办法好在哪？不就是省了几个字吗？

阿箬说：不是省字。

她说：**单子上的字少了，江上的船就能多。** 一条船等半天，是因为关房在写字；关房在写字，是因为每张单子都在把同样的话重新说一遍。你把同样的话头一遍写下来，第二遍只喊一个号——这不是省纸，这是把"重复"从这条江上拿走了。

年轻人又问：那要是有一天两块板子对不上呢？

阿箬看了一眼江面。

她说：**那就砍绳子。**

**不要试图修好它。**

**有些东西，一旦开始不一致，就只能重来。**

— —

By now you've probably recognized it: this is *HPACK*.

### 这是什么

*HPACK* 是 HTTP/2 的**头部压缩**格式（RFC 7541）。HTTP/2 把请求拆成了 HEADERS 帧和 DATA 帧，但**头部依然每个请求都要发一遍**——`user-agent`、`cookie`、`authorization`、`:method`、`:path` 这些字段在一条连接上的重复率高得惊人。HPACK 就是把这份重复拿掉的办法。

它由三样东西组成。

**一、静态表**（*static table*）：61 条预先定义好的头部，索引 1–61，永久不变，两端内置。`:method: GET` 是 2，`:status: 200` 是 8，`accept-encoding: gzip, deflate` 是 16。一条常用头部可以压成 **1 个字节**——`0x82` 就是 `:method: GET`。

**二、动态表**（*dynamic table*）：一条连接一个方向一张，初始为空，索引从 62 开始。**最新加入的条目索引最小（62），最老的索引最大。** 每加入一条，两端**同时**在表头位置插入；表满了就从**最大索引那一头**逐条驱逐。条目大小是 `name长度 + value长度 + 32` 字节（32 是 RFC 规定的固定开销），表的上限由对端的 `SETTINGS_HEADER_TABLE_SIZE` 决定，**默认 4096 字节**。一个条目如果比整张表还大，就清空整张表且不插入。编码器可以用 `dynamic table size update`（`001` 前缀，5 位整数）改变表大小，而这个更新**必须出现在 header block 的最前面**。

**三、Huffman 编码**：不在表里的字符串字面量，用一张事先定好的静态 Huffman 码表逐字节编码。码表按 HTTP 头部的字符分布统计而来——常见字符短、罕见字符长，符号 0–255 之外还有一个 EOS（256），每段码长写在 5 位整数里。所以字面量通常还能再省 20%–30%。

一个 header block 里，每个字段用四种前缀之一表示：

- `1xxxxxxx`：**索引头部字段**，7 位索引，整个字段一个字节。
- `01xxxxxx`：**带增量索引的字面量**，6 位名称索引 + 值字面量。新条目进动态表主要走这条。
- `0000xxxx`：**不带索引的字面量**，只压 Huffman，不进表。
- `0001xxxx`：**永不索引的字面量**（*never indexed*），只压 Huffman，不进表，并且明确告诉链路上任何一环"别缓存它"。`authorization`、`cookie` 这类字段必须走这条。

索引 0 是保留值，意思是"名称也不在表里，名称同样用字面量发"。

### 为什么重要

- **头部是小请求的成本中心。** 一个典型的 HTTP/2 请求头部约 700–800 字节，而 body 可能只有几十字节。gRPC、移动端 API、服务网格里的 sidecar 调用全是这个形状：头部占整个请求八成以上。HPACK 让第二条及以后的同形请求头部降到几十字节，实测常见 80%–90% 的头部字节缩减。
- **它把连接变成了有状态的。** 单子上只有号码，号码的含义存在两端各自的表里，而那张表**不过网**。这是 HPACK 最重要的结构性事实：解码器必须按顺序处理每一个 header block，任何一方的表偏移一格，从那一点开始**所有**头部都是错的——而且错得合法、错得通顺。所以 RFC 规定解码失败是 `COMPRESSION_ERROR`，**连接级错误**，不是请求级错误。HTTP/2 因此要求同一条连接上的头部块严格有序。
- **它重新打开了压缩侧信道。** 把攻击者可控的输入和你不想泄露的秘密（cookie、token）放进同一个压缩上下文，码长的变化就会泄露内容——这就是 CRIME / BREACH 那一类攻击。HTTP/2 在 TLS 之上重新引入了有状态头部压缩，所以 `never indexed` 不是可选项，是安全边界。
- **它可以被武器化。** 解码端的内存和 CPU 消耗与"压缩比"脱钩：几十字节的 header block 能展开成巨大的内存占用（*HPACK bomb*）；而动态表的内存还要乘以并发连接数，所以 `SETTINGS_HEADER_TABLE_SIZE` 本身就是个 DoS 面。2019 年那批 HTTP/2 DoS 公告里，就有专门针对头部块处理的条目。
- **HTTP/3 为了它改了协议形状。** QUIC 的流可以乱序到达，而 HPACK 要求严格有序，所以 HTTP/3 不能直接用它——这就是 **QPACK** 存在的原因：动态表引用被拆到独立的 encoder / decoder 流上，接收端一旦引用了还没到的条目就必须**阻塞等待**，并用 `SETTINGS_QPACK_BLOCKED_STREAMS` 给这个等待设上限。一个压缩方案的约束，最后变成了下一个传输协议的架构决策。

_隐喻对应表_

- 一天两千条船、一条船三张单、一张四十行 → 一条连接上的请求量与每请求的头部字段数
- 四十行里三十九行是同样的话 → HTTP/2 中头部字段的高重复率（`user-agent`、`cookie`、`:path`）
- 每张单都要把同样的话重写一遍 → 只用 HPACK 之前的明文头部，每个请求都要重发全部字段
- 刻着六十一条话的木牌 → 静态表（*static table*），索引 1–61，永久不变
- 全江上下刻同一块牌、同一套号、同一顺序 → 静态表写在规范里，两端内置，无需协商
- 抬头看牌、喊一声"第八条" → 索引头部字段，`1xxxxxxx` 前缀，7 位索引
- 一个"八"字写完一整行 → 一条常用头部压成 1 个字节（如 `0x82` = `:method: GET`）
- 蜡板上可以随时写新话 → 动态表（*dynamic table*），索引从 62 起
- 活号从六十二往下排，最新写的最小 → 动态表里最新条目索引 62，越老索引越大
- 一句话占"名字长度 + 值长度 + 三十二个刻痕" → 条目大小 `name + value + 32` 字节
- 板子有大小，由收的一方定 → `SETTINGS_HEADER_TABLE_SIZE`，默认 4096 字节
- 装满了从最老的那头刮 → 表满时驱逐最大索引（最老）的条目，FIFO
- 一句话比整块板子还重，就整块刮干净且不写 → 条目大于表容量时清空整张表并跳过插入
- 想改板子大小要先喊一声，且必须喊在最前面 → `dynamic table size update`，`001` 前缀，必须在 header block 最前
- 写新话时两端写在同一行 → 编码器与解码器逐条同步插入，位置必须一致
- 号码不过江，只有号码过江 → 动态表是隐式共享状态，表内容从不传输
- 错一个号，后面全错 → 一旦失步，从该点起所有头部解错
- 错的东西看起来跟对的一模一样 → 解出的头部语法合法、语义合理，因此错误极难察觉
- 补一行、把空填上 → 试图就地修正失步的表，而不是重置连接
- 砍断渡绳、封江、重开 → 报 `COMPRESSION_ERROR`，连接级错误，整条连接废弃
- 封江七天，所有在途单子作废重写 → 连接重置后所有请求重发，代价是整条连接的 RTT
- "蜡板不许补""对不上就重来" → 规范禁止增量修复压缩状态，只能重建
- 密信不许上板，还要标"此句不入板" → `never indexed` 字面量，`0001xxxx` 前缀
- 秘密和敌人放进同一台秤，秤会说话 → 有状态压缩的侧信道（CRIME / BREACH）
- 拿复用的号码替掉密信 → 把 `authorization` / `cookie` 索引进动态表，泄露风险
- 塞几十句废话把有用的挤出蜡板 → 用表驱逐（eviction）逼走热条目，抬高压缩成本
- 送来又短又轻、展开重如山的单子 → HPACK bomb：压缩比与解压资源脱钩
- 板子内存在每间关房各存一份 → 动态表内存 × 并发连接数，构成 DoS 面
- 乱序到达的船 vs 必须逐行对齐的板子 → QUIC 流可乱序，HPACK 要求有序，二者冲突
- QPACK：把"我写了什么"另开一条专线，必要时等它 → 独立 encoder / decoder 流 + `SETTINGS_QPACK_BLOCKED_STREAMS` 上限
- 不写字只喊号，船就能多 → 头部字节缩减直接换算成吞吐与延迟
- 木牌挡住七成，蜡板再挡住剩下的大半 → 静态表 + 动态表叠加命中，实测头部缩减 80%–90%
- 阿箬数了七天才钉牌 → 压缩表必须由真实流量分布驱动，不能凭直觉设计
</section>
<section class="en" markdown="1">
Upper Ford and Lower Ford face each other across a river.

Upper Ford deals in salt; Lower Ford deals in silk. Each bank keeps a customs house, and every cargo crossing the water must be declared on a slip before it boards: goods, grade, duty class, the carrier's name, the inspecting clerk, the day's cyclical sign — line after line, all of it written out.

At first this was a few strokes of a brush.

Then business on both banks grew. Two thousand boats a day, three slips a boat, forty lines a slip. Clerks' wrists ached, boatmen cursed, and the queue of hulls ran from the landing all the way down to the sandbar. People with connections began cutting the line; people cutting the line began paying; people paying began adding two extra lines.

That autumn a woman clerk arrived at the customs house. Her name was A-Ruo.

Her first act was not to write faster. It was to stand at the door and count slips.

She counted for seven days, and found something absurd: across two thousand boats, the slips contained only sixty-one distinct phrases.

"Salt, upper grade, official permit." "Silk, white, woven by the household." "Inspection: none." "Carrier: the boat's own master." "Duty: standard." These phrases repeated on every slip. Of forty lines, thirty-nine were the same few sentences. Only one line was new: the hull number, or the weight.

On the eighth day she nailed a wooden board to the customs house wall.

Sixty-one phrases were carved into it in advance, top to bottom, each with a number. Every customs house on the river carved the same board, with the same numbers, in the same order.

From then on, nobody wrote the declaration. They called it.

A clerk glanced at the board and called "number eight." The scribe opposite wrote a single character: eight. A forty-line slip became thirty-nine numbers and one hull number. A slip that took an incense stick to write now took three breaths.

The boatmen stopped cursing.

—

But a number can only hold sixty-one phrases.

New goods, new duty classes, new carrier names — more of them appeared in a day than the board had room to carve. Those still had to be spelled out, letter by letter. And the most laborious things to spell were precisely the newest ones: not on the board, so you wrote them in full.

A-Ruo did the arithmetic. The board absorbed seventy percent. Of the remaining thirty, most were things that had appeared only recently and were appearing every day now. Today they were new. Tomorrow they wouldn't be.

So she nailed up a second board. This one was wax.

The rules went like this:

- The sixty-one on the wooden board were **dead numbers**. Anyone could call them at any time. They never changed.
- The phrases on the wax were **live numbers**. Live numbers started at sixty-two and ran downward. **The newest thing written had the smallest number.** The older a phrase, the larger its number grew.
- Whenever you used a new phrase on a slip, you also wrote it onto the wax — **in the same row**. Because the wax at the other end had to be written too.
- The wax had weight. A phrase's footprint was not measured in characters, but in "length of the name + length of the value + thirty-two notches." Those thirty-two notches were fixed overhead: no phrase was short enough to escape them.
- When the wax filled up, you scraped from **the oldest end** until the new phrase fit.
- If a single phrase was heavier than the whole tablet, you **scraped the tablet clean** and did not write it at all — writing it would be pointless, since the next phrase would scrape it off.

On the first day, live numbers barely appeared. On the second, they began. By the fifth, eleven hundred of the two thousand slips carried live numbers in their opening lines.

On the tenth day, A-Ruo had an apprentice try to call an entire slip in numbers.

The apprentice called six: eight, sixty-two, sixty-three, sixty-five, sixty-six, seventy-one.

The scribe opposite wrote them out one by one, then looked up: this was a complete salt declaration, forty lines, not one character wrong.

Six characters in exchange for forty lines.

—

The wax was useful, but it had a vicious temper.

It was **not on the slip**.

The slip carried only numbers. What a number pointed to lived entirely on the two tablets — one on the sending bank, one on the receiving bank. And those two tablets had never been compared, and **could not** be compared, because nothing written on them ever crossed the water. Only the numbers crossed.

So the two tablets had to be identical.

Not roughly. **Row for row, in the same order.**

A-Ruo had three rules carved on the customs house wall:

**One. When you write a new phrase, write it in the same row.** If you say "this goes on the tablet," the other side writes the same phrase in the same cell. One row late, and every number after it is off by one.

**Two. When you scrape, scrape from the oldest end.** You may not scrape a phrase early because it looks useless. The order of the tablet is not usefulness. It is **age**.

**Three. If a number doesn't match, do not guess.** Whoever calls a number not on the tablet, or calls one the other side did not record, **cuts the ferry rope on the spot, closes the river, and starts over.** No "it's probably this." No skipping it and carrying on.

The third rule was the least reasonable. Closing the river for half a day over one number earned her a whole winter of merchants' curses.

A-Ruo did not explain. She said only this:

**One wrong number, and everything after it is wrong. And the wrong things look exactly like the right ones.**

—

That twelfth month, it happened.

Fog on the river. A boat struck a mooring post, and half the roof beam of the receiving customs house came down. Nobody was hurt. The wax tablet shattered on the floor, and when they pieced it together there was a hole between sixty-one and seventy — one row of wax had been trodden away.

The old clerk thought it a small matter. He slid the rows below up by one, filled the gap, and went on calling numbers.

All that day, every slip at Lower Ford was correct.

Clean brushwork. Proper form. Duty collected. Cargo released.

Except that a consignment which should have read "official-permit salt, three hundred catties" now read "household white silk, thirty bolts." Duty was charged at the silk rate, double the salt rate, and the merchant started shouting at the customs house on the spot. And while he was shouting, another consignment recorded as salt — silk, in fact — had already loaded and gone downstream.

By the third day, two hundred and seventy slips failed reconciliation.

Not one line was wrong. **From that row onward, every line was wrong.** And each line, taken alone, was legal, fluent, and entirely convincing.

A-Ruo reached Lower Ford, looked once at the patched tablet, and said nothing.

She walked to the landing, drew her blade, and cut the ferry rope.

The river closed for seven days.

In those seven days the customs houses on both banks voided every slip still on the water and rewrote them all — without live numbers this time, one character at a time.

When the seven days ended, A-Ruo raised a new rule board on each bank. It carried two lines:

**A wax tablet is never patched.**
**If it doesn't match, start over.**

—

Later she added two more rules. Neither was reasonable. Both were necessary.

The first was for **sealed letters**.

Some consignments could not have their carrier's name appear on any tablet — the secret marks on official salt, the permits for military goods, a trading house's private seal. People had been writing these onto the wax for convenience; after all, the phrase would be reusable next time.

A-Ruo banned all of it. She said: **the wax is for everyone to read.**

Anything that could not remain on the tablet had to be spelled out in full every time, and the slip had to carry a mark: **this phrase does not enter the tablet.** The mark was not only for your own side; it told the other side, explicitly, not to record it.

She gave no reason then. The reason took years for anyone to work out: a phrase written onto the tablet is mixed into the same pile of codes as everyone else's, pressed and scraped over and over; the harder the pressing, the shorter the codes; the shorter the codes, the more the length of a code tells you about the characters underneath. **Put a secret and your enemy on the same scale, and the scale will talk.**

The second rule was for **hostile parties**.

A trading house had discovered that by stuffing dozens of long, useless phrases at the head of a slip, the useful phrases got shoved off the wax; by the time the real declaration arrived, every line had to be spelled out afresh.

A-Ruo's answer: **the size of the tablet is set by the receiving side; the writing side may not change it on its own.** If you want to scrape it to a new size, you call out "I am resizing to this" first — and that call must come at the very front of the slip.

One more: a slip may only be so long. If someone hands you a slip that is short and light but unfolds into something as heavy as a mountain, that is not a declaration. That is a man come to demolish the customs house.

—

A-Ruo worked at the customs house for eleven years.

On the day she left, the wooden board and the wax were both still there. The board still carried its sixty-one. The wax had been replaced many times. Not one rule had changed.

The young man taking over asked her: what's so good about this method? Doesn't it just save a few characters?

A-Ruo said: it doesn't save characters.

She said: **fewer characters on the slips means more boats on the river.** A boat waits half a day because the customs house is writing. The customs house is writing because every slip is saying the same things over again. Write the same thing down the first time, and the second time you only call a number — that isn't saving paper. That is taking *repetition* off the river.

The young man asked: and if one day the two tablets don't match?

A-Ruo looked at the water.

She said: **then cut the rope.**

**Don't try to repair it.**

**Some things, once they start to disagree, can only be started over.**

— —

By now you've probably recognized it: this is *HPACK*.

### What it is

*HPACK* is HTTP/2's **header compression** format (RFC 7541). HTTP/2 split requests into HEADERS and DATA frames, but **headers still travel on every single request** — and fields like `user-agent`, `cookie`, `authorization`, `:method`, and `:path` repeat at astonishing rates on one connection. HPACK is how that repetition gets removed.

It is built from three pieces.

**One, the static table:** 61 predefined header fields, indices 1–61, permanent, built into both endpoints. `:method: GET` is 2, `:status: 200` is 8, `accept-encoding: gzip, deflate` is 16. A common header compresses to **a single byte** — `0x82` *is* `:method: GET`.

**Two, the dynamic table:** one per connection per direction, initially empty, indices starting at 62. **The newest entry has the smallest index (62); the oldest has the largest.** Every insertion happens **simultaneously** at the front on both ends; when the table is full, eviction proceeds from **the largest index** — the oldest — one entry at a time. An entry's size is `name length + value length + 32` bytes (32 is the RFC's fixed overhead), and the table's ceiling comes from the peer's `SETTINGS_HEADER_TABLE_SIZE`, **4096 bytes by default**. An entry larger than the entire table clears the table and is not inserted. The encoder may resize with a `dynamic table size update` (`001` prefix, 5-bit integer), and that update **must appear at the very start of the header block**.

**Three, Huffman coding:** string literals that aren't in a table are encoded byte by byte with a predefined static Huffman code, derived from the character distribution of real HTTP headers — frequent characters short, rare ones long, with an EOS symbol (256) beyond 0–255, and each code length written as a 5-bit integer. Literals typically save another 20–30%.

Inside a header block, each field uses one of four prefixes:

- `1xxxxxxx`: **indexed header field**, 7-bit index, one byte for the whole field.
- `01xxxxxx`: **literal with incremental indexing**, 6-bit name index plus a value literal. This is how new entries get into the dynamic table.
- `0000xxxx`: **literal without indexing** — Huffman only, never stored.
- `0001xxxx`: **literal never indexed** — Huffman only, never stored, and an explicit instruction to every hop: do not cache this. `authorization` and `cookie` must use it.

Index 0 is reserved: it means the name is not in a table either, so the name travels as a literal too.

### Why it matters

- **Header compression is the cost center of small requests.** A typical HTTP/2 request carries 700–800 bytes of headers against a body of perhaps a few dozen bytes. gRPC, mobile APIs, and service-mesh sidecar calls all have this shape: headers are over 80% of the request. HPACK drives the headers of the second and subsequent similar requests down to tens of bytes, with 80–90% header-byte reduction commonly measured.
- **It makes the connection stateful.** The slip carries only numbers; what a number means lives in a table at each end, and that table **never crosses the wire**. This is HPACK's most important structural fact: the decoder must process header blocks in order, and if either table shifts by one slot, **every** header from that point on is wrong — wrong in a way that is legal and fluent. That is why the RFC makes a decoding failure `COMPRESSION_ERROR`, a **connection-level** error rather than a request-level one. It is also why HTTP/2 requires header blocks on a connection to be processed strictly in order.
- **It reopens the compression side channel.** Put attacker-controlled input and a secret you would rather not leak (a cookie, a token) into the same compression context, and variations in code length leak the content — the CRIME / BREACH family. HTTP/2 reintroduced stateful header compression on top of TLS, so `never indexed` is not an option. It is a security boundary.
- **It can be weaponized.** Decoder memory and CPU are decoupled from the compression ratio: a few dozen bytes of header block can expand into an enormous allocation (an *HPACK bomb*). And dynamic-table memory multiplies by concurrent connections, which makes `SETTINGS_HEADER_TABLE_SIZE` itself a DoS surface. The 2019 wave of HTTP/2 DoS disclosures included entries aimed squarely at header-block handling.
- **HTTP/3 changed the protocol's shape for it.** QUIC streams arrive out of order, while HPACK demands order, so HTTP/3 cannot use it directly — which is why **QPACK** exists: dynamic-table references move onto separate encoder and decoder streams, a receiver that references an entry it hasn't received must **block**, and `SETTINGS_QPACK_BLOCKED_STREAMS` caps how much blocking is allowed. One compression scheme's constraint became the next transport protocol's architectural decision.

_Metaphor mapping_

- Two thousand boats a day, three slips a boat, forty lines a slip → request volume and header fields per request on one connection
- Thirty-nine of forty lines are the same phrases → the high repetition rate of HTTP/2 header fields (`user-agent`, `cookie`, `:path`)
- Rewriting the same phrases on every slip → plaintext headers before HPACK, resent in full per request
- The carved board of sixty-one phrases → the static table, indices 1–61, permanent
- Every customs house carves the same board, same numbers, same order → the static table is specified, built into both ends, never negotiated
- Glancing at the board and calling "number eight" → the indexed header field, `1xxxxxxx` prefix, 7-bit index
- One character "eight" replacing a whole line → one common header compressed to one byte (`0x82` = `:method: GET`)
- Wax that accepts new phrases at any time → the dynamic table, indices from 62
- Live numbers run down from 62; the newest is smallest → newest entry is index 62; older entries have larger indices
- A phrase occupies "name length + value length + thirty-two notches" → entry size `name + value + 32` bytes
- Tablet size set by the receiving side → `SETTINGS_HEADER_TABLE_SIZE`, 4096 bytes by default
- Scraping from the oldest end when full → evicting the largest-index (oldest) entry, FIFO
- A phrase heavier than the whole tablet clears it and is not written → entries larger than the table clear it and are skipped
- Announcing a resize, which must come first → `dynamic table size update`, `001` prefix, first in the header block
- Both sides writing new phrases in the same row → encoder and decoder inserting in lockstep at the same position
- Only numbers cross the water → the dynamic table is implicit shared state; its contents are never transmitted
- One wrong number and everything after is wrong → a single desync corrupts every subsequent header
- The wrong thing looks exactly like the right thing → decoded fields remain syntactically valid and semantically plausible, so errors hide
- Patching the gap by shifting rows → attempting to repair a desynced table in place instead of resetting the connection
- Cutting the ferry rope and closing the river → `COMPRESSION_ERROR`, a connection-level failure; the whole connection dies
- Seven days of voided slips rewritten from scratch → all in-flight requests reissued after a connection reset, at full RTT cost
- "A wax tablet is never patched"; "if it doesn't match, start over" → the spec forbids incremental repair of compression state
- Sealed letters barred from the tablet, marked "this phrase does not enter" → the `never indexed` literal, `0001xxxx` prefix
- A secret and your enemy on the same scale, and the scale talks → side channels in stateful compression (CRIME / BREACH)
- Replacing the sealed phrase with a reused number → indexing `authorization` / `cookie` into the dynamic table, leaking them
- Stuffing useless phrases to shove hot ones off the wax → using eviction to force out hot entries and inflate compression cost
- A light slip that unfolds into a mountain → HPACK bomb: expansion decoupled from decode resources
- Every customs house holds its own copy of the tablet → dynamic-table memory times concurrent connections, a DoS surface
- Boats arriving out of order versus a tablet that must align row for row → QUIC's unordered streams against HPACK's ordering requirement
- QPACK: a dedicated line for "what I wrote," plus a limit on waiting → separate encoder/decoder streams and `SETTINGS_QPACK_BLOCKED_STREAMS`
- Calling numbers instead of writing, so more boats cross → header-byte reduction turns directly into throughput and latency
- The board absorbs seventy percent; the wax absorbs most of the rest → static plus dynamic table hits, producing 80–90% measured header reduction
- A-Ruo counts slips for seven days before carving the board → compression tables must be driven by real traffic distributions, not intuition
</section>
