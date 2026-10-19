---
layout: fable
title: "门口的账 · The Ledger at the Door"
title_zh: "门口的账"
title_en: "The Ledger at the Door"
concept: "Little's Law"
tags: [queueing-theory, performance, sre]
illustration: /assets/art/2026-10-19-littles-law.jpg
youtube_id: "0AqiLsZ6Fqo"
---
<section class="zh" markdown="1">
望江楼是渡口镇最大的一家茶楼。三层,临江,飞檐挑得像鹤翅。往来船家、盐商、镖师,下了船,头一件事就是进去讨一壶热茶,歇脚看江。

老掌柜在世时有个老毛病:想知道楼里坐了多少人,就派伙计从一楼数到三楼。伙计数完下楼,客人早换了两拨——数出来的数,出门就作废。每年腊月,防火的官差来问:"楼里若走了水,多少人要逃?"老掌柜只能支吾。

苏棠接手望江楼那年,头一件事不是修楼,也不是换茶,是把账桌搬到了大门口,自己坐在门边。

有人笑:"新掌柜怕见客?"

她不答话,只做三件事。

第一件,投豆。进门一位客人,她往青瓷碗里投一粒黄豆。日落收摊,数豆记墙:今日六个时辰,进来三百六十人——**每个时辰六十人**。

第二件,发筹。每位进门的客人领一枚竹筹,筹上烙着进门的时辰。客人起身,竹筹交回,她看一眼烙印:这位坐了一个时辰,那位坐了三个。月底一算:**客人平均坐两个时辰**。

第三件,是墙上的一道乘法:

**楼里的人数 = 每个时辰进门的人数 × 客人平均坐的时辰**

每时辰六十人进门,每人平均坐两个时辰——楼里便是一百二十人。她不必登楼,不必数人头,不必知道哪桌喧哗、哪桌冷清、哪位茶客续了几回水。**楼里的事她一概不问,只看门。**

伙计阿吉不服。某夜落闸,他提灯从一楼数到三楼:一百一十九人。苏棠头也不抬:"末了一位刚走,筹还没销。"

账一稳,用处就来了。

防火官差再来,不等开口,苏棠递上墙账:"楼中至多一百二十人,楼梯按一百五十人修。"官差回去,果然按一百五十人的份例给她家修了两道逃生梯。

到了八月大集,江上船挤成蚁,每时辰进门的人涨到一百。楼还是那个楼,苏棠心里雪亮:客人若还是一坐两个时辰,楼里就是两百人——要出事。她把竹筹收得只剩一百二十枚:筹尽,后来的客人请在门口长凳上坐,屋里走一位,放一位。伙计们手忙脚乱地加跑堂、催续水、把点心改成小份快上。到月底一算,进出的人比平常多了将近一倍,**楼里却始终没超过一百二十人**——进门的人多,楼里的人坐得就短。账,一点没乱。

有老茶客嘀咕:楼里拼桌的、占座的、雅间里谈生意的,毫无规矩,账能准吗?苏棠答:"楼里怎么乱,不碍门口的账。只要进出只有这一扇门,人不会凭空多,也不会凭空少。进多少人、坐多久,楼里就有多少人——**这是水一样平的道理**。"

只有一时不准:清晨头一个时辰,楼刚开门,门里门外都在涨潮,这账要偏小;散市的雨夜,账又要偏大。可把账拉长到一整天、十天半月,进来出去两相抵,这笔账就稳稳当当,分毫不欺。

后来她把同样的账算到了渡口:每时辰靠岸三条船,船家在镇上盘桓四个时辰,码头与江面上便常有三十六条船的人。楼也好,渡也好,凡有门、有进出、有逗留的地方,这道乘法都成立。

讲到这儿,你多半已经认出来了:这就是排队论里的 *Little's Law*——利特尔定律,L = λ × W。

### 这是什么

*Little's Law*(利特尔定律)是排队论中最著名的定理:任何一个系统,只要处在稳定状态,**系统内的平均人数 L,等于平均到达率 λ 乘以每人平均逗留时间 W**,即 L = λ × W。

它由 John D. C. Little 于 1961 年提出,后来经 Stidham 等人推广到几乎不设前提的一般形式。乍看只是乘法,威力在于它几乎不挑条件:

- **黑箱**:不必知道系统内部怎么运作——先来后到也好、抢占插队也好、乱成一团也好,一概无关;只看入口、出口与逗留时间。
- **守恒**:稳态下进出相抵,人数不会凭空增减,像水流找平。
- **知二得一**:三个量里知道任意两个,第三个便被死死钉住。
- **任意分段皆成立**:门口长凳是一段,楼里是一段,整个渡口又是一段,各守各的律。

### 为什么重要

1. **容量规划**:从吞吐量与延迟直接推算系统内的在途请求数(并发度),或反过来由并发上限与到达率推算延迟上限——望江楼的楼梯,就是这么修的。
2. **精益与看板的算术根基**:限制在制品(WIP)即固定 L;吞吐不变时,交付周期 W = L ÷ λ 被强制压低。"少囤积、快流动"的全部依据,就是这道乘法。
3. **延迟的翻译器**:队列深度(排队人数)除以到达率,就是等待时间——监控图上"队列暴涨"翻译成人话,就是"要等很久"。
4. **普适且免费**:不挑到达分布,不挑服务分布,不挑调度策略,几乎零假设,却给出硬约束。工程里这种守恒律级别的工具,凤毛麟角。

_隐喻对应表_

- 望江楼大门口 → 系统边界(*system boundary*)
- 每时辰进门人数(碗里的黄豆) → 平均到达率 λ(*arrival rate*)
- 客人平均逗留时长(竹筹烙印) → 平均逗留时间 W(*sojourn time*)
- 楼内客人数 → 系统内平均数 L(*number in system*)
- 只看门、不问楼里 → 黑箱性质(*black-box*)
- 拼桌占座再乱也不碍账 → 与内部调度策略无关(*policy independence*)
- 竹筹只剩一百二十枚、筹尽等位 → 在制品上限(*WIP limit*)
- 进门人近倍增、楼里人数不变 → 固定 L 时 W = L ÷ λ,吞吐升则逗留降
- 官差按一百五十人修梯 → 容量规划(*capacity planning*)
- 门口长凳 → 队列(*queue*),门前一段亦各守其律
- 清晨涨潮账偏、拉长即稳 → 稳态假设(*steady state*)
- 渡口船只的账 → 定律对任意"有进出、有逗留"的系统普适(*universality*)
- 老掌柜从一楼数到三楼 → 全量轮询,数完即过期(*stale census*)

</section>
<section class="en" markdown="1">
River-View Tower was the grandest teahouse in Ferry-Crossing Town: three stories, right on the water, eaves lifted like a heron's wings. Boatmen, salt merchants, and escort guards stepping off the boats all did the same first thing—go in, order a pot of hot tea, rest their legs, watch the river.

The old master had an old habit: to know how many guests sat inside, he sent a waiter counting from the first floor to the third. By the time the waiter climbed back down, the crowd had turned over twice—the number was stale before it left the building. Every twelfth month the fire warden came asking: "If this tower catches, how many must flee?" The old master could only mumble.

The year Su Tang took over River-View Tower, her first act was neither repair nor a new tea. She carried her account table to the great doorway and seated herself at the threshold.

Some laughed: "The new mistress is afraid of her own guests?"

She said nothing. She did three things.

First, the beans. For each guest who entered, she dropped one soybean into a celadon bowl. At dusk she counted the beans and chalked the wall: six watches today, three hundred sixty entered—**sixty per watch**.

Second, the tokens. Every entering guest received a bamboo tally branded with the hour of entry. When a guest rose to leave, the tally came back, and she read the brand: this one stayed one watch, that one three. At month's end: **guests stayed two watches on average**.

Third, one multiplication written on the wall:

**guests inside = guests entering per watch × average watches each stays**

Sixty per watch in, two watches each—so a hundred and twenty inside. No need to climb, no need to count heads, no need to know which table roared, which sat quiet, whose pot had been refilled how many times. **She asked nothing of the tower's affairs—only watched the door.**

The waiter A-Ji refused to believe it. One night at closing he climbed the tower, lamp in hand, floor by floor: one hundred nineteen. Su Tang didn't look up: "The last guest just left; his tally isn't redeemed yet."

Once the ledger steadied, uses followed.

The fire warden came again; before he opened his mouth, Su Tang handed him the wall: "At most one hundred twenty inside. Build the stairs for one hundred fifty." The warden went back and, by the book, granted the tower two escape stairways rated for one hundred fifty.

Come the eighth-month market fair, boats packed the river like ants, and entries swelled to a hundred per watch. Same tower. Su Tang saw it plainly: if guests still lingered two watches each, that was two hundred inside—disaster. She drew down the bamboo tallies to one hundred twenty: when they ran out, newcomers waited on the benches by the door—one guest out, one guest in. The waiters scrambled—more runners, faster refills, pastries cut small and served quick. By month's end, nearly twice the guests had passed through, **and the tower never held more than one hundred twenty**—the faster they came in, the shorter they stayed. The ledger hadn't wavered a grain.

An old regular grumbled: sharing tables, hoarding seats, private rooms full of businessmen—no order at all. Can the count hold? Su Tang answered: "However the tower tangles itself, it cannot touch the door's account. So long as this is the only door, people appear from nowhere nor vanish into it. As many as enter, as long as they stay—that is how many are inside. **This holds as level as water.**"

Only one season it drifted: the first watch of morning, tower just opened, everything rising like a tide—the account read low; on rainy nights as the market scattered, it read high. But stretch the account across a whole day, ten days, half a month—entries and exits cancel, and the ledger stands, deceiving no one.

Later she ran the same arithmetic on the ferry crossing itself: three boats mooring per watch, boatmen lingering four watches in town—so thirty-six boats' worth of people around the landing, always. Tower or ferry—wherever there is a door, entries, and dwelling, the multiplication holds.

By now you've probably recognized it: this is *Little's Law* from queueing theory—L = λ × W.

### What it is

*Little's Law* is the most famous theorem of queueing theory: for any system in steady state, **the average number inside, L, equals the average arrival rate, λ, times the average time each stays, W**: L = λ × W.

Proposed by John D. C. Little in 1961 and later generalized by Stidham and others to forms with almost no assumptions, it looks like mere multiplication. Its power is in how little it requires:

- **Black box**: nothing about the interior matters—FIFO, preemption, priority, or sheer chaos; only the doorway, the exits, and the dwell time.
- **Conservation**: in steady state, entries and exits cancel; the count can neither grow from nowhere nor shrink into it, like water finding level.
- **Two known, one fixed**: know any two of the three quantities, and the third is nailed.
- **Any segment obeys**: the benches by the door, the hall upstairs, the whole ferry crossing—each holds its own law.

### Why it matters

1. **Capacity planning**: derive in-flight requests (concurrency) directly from throughput and latency—or reverse it, from concurrency ceilings and arrival rates to latency bounds. River-View's stairs were built exactly this way.
2. **The arithmetic beneath lean and Kanban**: capping work-in-progress fixes L; with throughput unchanged, lead time W = L ÷ λ is forced down. "Hold less, flow faster" is this one multiplication.
3. **A translator for latency**: queue depth divided by arrival rate is wait time—"queue exploding" on the monitoring dashboard means, in human speech, "you will wait a long time."
4. **Universal and free**: no assumption on arrival distributions, service distributions, or scheduling policy—almost no premises, yet a hard constraint. Tools of this conservation-law caliber are rare in engineering.

_Metaphor mapping_

- the great doorway of River-View Tower → the system boundary (*system boundary*)
- entries per watch (soybeans in the bowl) → average arrival rate λ (*arrival rate*)
- average dwell (the tally brands) → average sojourn time W (*sojourn time*)
- guests inside → average number in system L (*number in system*)
- watching only the door, never the hall → black-box property (*black-box*)
- however tables are shared or hoarded, the count holds → independence from internal scheduling policy (*policy independence*)
- one hundred twenty tallies, newcomers wait when they run out → work-in-progress limit (*WIP limit*)
- entries nearly double, inside-count unchanged → fixed L forces W = L ÷ λ; higher throughput, shorter stays
- the warden's stairs rated one hundred fifty → capacity planning (*capacity planning*)
- benches by the door → the queue (*queue*); that segment keeps its own law too
- morning tide drifts the ledger, length steadies it → the steady-state assumption (*steady state*)
- the ferry-boat accounting → the law's universality to any door-entry-dwell system (*universality*)
- the old master counting floor by floor → full-table polling, stale on arrival (*stale census*)
</section>
