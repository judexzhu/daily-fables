---
layout: fable
title: "不关门的图书馆 · The Library That Never Closed"
title_zh: "不关门的图书馆"
title_en: "The Library That Never Closed"
concept: "Expand–contract migration (parallel change)"
tags: [distributed-systems, operations]
illustration: /assets/art/2026-08-09-expand-contract-migration.jpg
---
<section class="zh" markdown="1">
村东头那座图书馆，一百年没关过门。

书是按颜色找的。红丝带是农事，蓝丝带是航海，褐丝带是药草，绿丝带是传说。老人们摸黑都能找到自己那一架——手往上一伸，指尖蹭到丝带的绒面，就是它了。

可是书越来越多。红丝带从一架变三架，三架变七架，找一本《北坡的霜期》要沿着红色摸过半个大厅。馆长阿梅决定改成编号：每本书的书脊上贴一枚小纸牌，写三段数字——楼层、区、位次。任何一本书，一个号码直达。

麻烦在于：图书馆不能关门。哪怕关半天，也算破了一百年的规矩。

阿梅没有在某个夜里把丝带一剪了之。

她做的第一件事，是让新进的每一本书**同时**挂上丝带和纸牌。丝带照老规矩系，纸牌按新规矩写。那几天来的读者什么都没察觉——他们还是摸丝带，丝带还在。

第二件事，她让学徒小满在闭馆后打着灯，一架一架给旧书补纸牌。补得很慢，一晚上两百本。补的时候不动丝带，一根也不剪。这活干了七个星期。

第三件事，小满走完最后一架，阿梅没有立刻宣布。她自己拿着册子，随机抽了三百本：丝带说它在哪，纸牌说它在哪，两个说法对不对得上。有十一本对不上——中间搬过架子，串位了。她把这十一本改好，又抽三百本，全对。

第四件事，前台改了口。有人来问"农事的书在哪"，馆员答"三·十四"。但丝带一根没剪。有个跑腿的孩子还是习惯直接钻进红色那一片自己翻——他照样找得到。头两个星期里，只要新法子出岔子，馆员随时可以退回去说一句"红丝带那一架"。

第五件事，是在两个月以后。阿梅在门口站着听了很多天，再没有一个人开口说过"红丝带"。她这才让小满把丝带一根一根解下来，收进箱子。

——到这儿你大概已经认出来了：这就是 _expand–contract_ 迁移，也叫 _parallel change_。

**它是什么**

在不停机的前提下改变数据结构或接口契约的标准做法。核心洞察很朴素：你没办法在同一瞬间既换掉存储、又换掉所有读它的人。rolling update 期间新旧两版代码必然并存，客户端更是各升各的。所以不要做"切换"，要做"重叠"——

1. **expand**：新旧两种表示同时存在、_dual write_ 同时写，旧路径完全不动；
2. **backfill**：后台分批把历史数据补齐到新表示；
3. **verify**：抽样或全量比对新旧两侧，确认一致再往下走；
4. **cutover**：把**读**切到新的一侧，旧的原样留着当 rollback 路径；
5. **contract**：等到确认再没有旧读者，才删掉旧字段、旧接口。

每一步单独回滚都是安全的，因为整个过程的任何一个瞬间，新读者和旧读者都能拿到正确答案。真正出事的迁移，几乎都是跳过了第 3 步，或者第 5 步做得太早——旧字段删掉的那天，才发现还有一个没人记得的 cronjob 在读它。

**隐喻对应表**

- 一百年没关过门 → 零停机（zero-downtime）约束
- 丝带 → 旧 schema / 旧 API 版本
- 纸牌编号 → 新 schema / 新版本
- 新书同时挂丝带和纸牌 → _expand_ 阶段的 _dual write_
- 小满夜里一架架补纸牌 → _backfill_，后台分批回填历史数据
- 全程一根丝带都不剪 → backward compatibility，旧路径始终可用
- 抽三百本两边对照 → 迁移后的一致性校验（reconciliation）
- 那十一本对不上的 → backfill 期间产生的 drift，必须先修再切
- 前台改口报编号 → read path _cutover_
- 还钻红色区自己翻的孩子 → 尚未升级的老客户端 / 老版本 Pod
- 随时能退回"红丝带那一架" → rollback path
- 阿梅在门口听了很多天 → 旧路径的使用度量（deprecation metrics）
- 两个月后才解丝带装箱 → _contract_ 阶段，确认无旧读者后才删除
</section>
<section class="en" markdown="1">
The library at the east end of the village had not closed its doors in a hundred years.

You found books by colour there. Red ribbon meant farming, blue meant seafaring, brown meant herbs, green meant old tales. The elders could find their shelf in the dark — reach up, feel the nap of the ribbon under a fingertip, and there it was.

But the books kept coming. Red went from one shelf to three, three to seven, and finding _Frost Dates on the North Slope_ meant groping along red halfway across the hall. So Mei, the head librarian, decided on numbers: a small paper tag glued to every spine, three figures on it — floor, section, position. Any book in the building, one number away.

The trouble was that the library could not close. Not even for half a day. A hundred years is a hundred years.

Mei did not cut the ribbons off one night and be done with it.

The first thing she did was give every newly arrived book **both** a ribbon and a tag. Ribbon tied the old way, tag written the new way. The readers who came in those days noticed nothing at all — they still reached for the ribbons, and the ribbons were still there.

The second thing: she sent her apprentice Man through the stacks after closing, lamp in hand, adding tags to the old books, shelf by shelf. Slow work — two hundred a night. He never touched a ribbon. Not one. It took seven weeks.

The third thing: when Man finished the last shelf, Mei announced nothing. She took a notebook and pulled three hundred books at random, and for each one asked whether the ribbon and the tag told the same story about where it lived. Eleven disagreed — shelves had been moved partway through and the positions had slipped. She fixed those eleven, pulled another three hundred, and found no disagreement.

The fourth thing: the front desk changed its answer. Someone asked where the farming books were and the librarian said "three, fourteen." But not a single ribbon was cut. One errand boy still went straight into the red section and dug around himself — and still found what he wanted. For the first two weeks, any time the new way stumbled, a librarian could fall back to "the shelf with the red ribbons" without anyone noticing.

The fifth thing came two months later. Mei stood by the door listening for many days, and not one person said the word "ribbon" any more. Only then did she have Man untie them, one by one, and pack them into a box.

— By now you have probably recognised it: this is _expand–contract_ migration, also called _parallel change_.

**What it is**

The standard way to change a data structure or an interface contract without downtime. The insight is humble: you cannot swap the storage and every reader of that storage in the same instant. During a rolling update both versions of the code are running at once, and clients upgrade on their own schedule entirely. So don't perform a switch — perform an overlap:

1. **expand** — both representations exist side by side, _dual write_ keeps them in step, and the old path is left completely untouched;
2. **backfill** — a background job brings historical rows up to the new representation, in batches;
3. **verify** — sample or compare both sides and confirm they agree before going further;
4. **cutover** — move **reads** to the new side, leaving the old one intact as a rollback path;
5. **contract** — only once you are sure no old readers remain, drop the old column or old API.

Every step is independently reversible, because at every instant of the whole process, both the new reader and the old reader get a correct answer. The migrations that go wrong almost always skipped step 3, or ran step 5 too early — and discovered on the day the old column was dropped that some forgotten cronjob had been reading it all along.

**Metaphor mapping**

- a hundred years without closing → the zero-downtime constraint
- the ribbons → the old schema / old API version
- the numbered paper tags → the new schema / new version
- new books getting both ribbon and tag → _dual write_ during the _expand_ phase
- Man tagging the stacks by lamplight → _backfill_ of historical data in batches
- never cutting a single ribbon → backward compatibility; the old path stays live throughout
- pulling three hundred books to compare → post-migration consistency verification (reconciliation)
- the eleven that disagreed → drift accumulated during backfill; fix before cutting over
- the front desk answering with numbers → read-path _cutover_
- the errand boy still digging through red → clients or pods that haven't upgraded yet
- being able to say "the red-ribbon shelf" at any moment → the rollback path
- Mei listening by the door for days → deprecation metrics on the old path
- untying the ribbons two months later → the _contract_ phase, only after no old readers remain
</section>
