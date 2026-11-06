---
layout: fable
title: "一托盘九百袋 · Nine Hundred Sacks to a Tray"
title_zh: "一托盘九百袋"
title_en: "Nine Hundred Sacks to a Tray"
concept: "Vectorized Query Execution (Columnar Batches and SIMD)"
tags: [databases, performance]
illustration: /assets/art/2026-11-06-vectorized-execution-columnar-batches.jpg
youtube_id: "id--R3jOQYM"
---
<section class="zh" markdown="1">
落雁关的盐关，只管一件事：过关的每一车盐，都要按斤两和成色课税。

顾先生在那里做了三十一年。

他的做法是从他师傅那里学来的，一个字没改：**一车一车地过**。

一辆车推到桌前，他起身，走到院里的官秤下，把盐袋挂上钩，等秤星停稳，记下斤两；走回案子，翻《成色册》，按封条上的印记查这一车归哪一等；再走到院子另一头取税则；最后回到案子前，在流水簿上写一行。

一车，四趟路。一趟路，换一次手里的家伙：秤钩、册子、税则、笔。

三十一年，他的手比嘴快，可这四趟路一趟也省不掉。

落雁关平常日子过八百辆盐车。秋后人称"盐车过江"的旺季，一天一万两千辆。四趟路，四万八千趟。顾先生走到六十岁那年，膝盖先不干了。

接他班的是沈砚，二十四岁，念过新学。他来关上的第三天，说了一句话，把满屋老吏都说愣了：

"先生，不是您老了。是您每一车，都在换家伙。"

第二天，沈砚没动那台官秤，也没动税则一个字。他只做了一件事：**把车拆成托盘。**

盐车推上关，不入院，先在场院东头的台子上倒货。倒货的活儿不归他管，他只立了三条规矩。

第一，一车货按**名目**分倒在不同的托盘里：斤两在斤两盘，封条印记在印记盘，成色在成色盘。一盘只装一种名目的东西。

第二，每只托盘有九百个格子，格子从左到右编号。**第一车的第一格，在所有托盘里都是第一格。** 一辆车在斤两盘里排第一百三十七格，它在印记盘里也一定排第一百三十七格。

第三，一只托盘，正好摊满一张案子。他伸开胳膊，从头够到尾。

第一趟，他走到斤两盘前，不下手。他叫人抬来一根铜尺——尺身和托盘一样长，尺上九百个刻度，一刻对一格。他把铜尺往托盘上一压，看一眼，九百袋的斤两全在眼前。

这一压，顶他从前九百趟路。从前他是"一袋一看秤星"，秤星是活的，他得等它停；现在是"一盘一压尺"，尺是死的，九百个数一起进他的眼睛。

第二趟，问题来了：九百袋里，超重的也许只有十一袋。从前他每袋都要问一句"超了没有"，问九百次，八百八十九次的答案是"没有"，可这八百八十九次问话，一样费他的神。

沈砚的办法是一张**穿孔的铜片**。铜片和托盘一样宽，压在斤两盘上，超重的格子上方的铜片就被顶起一个小包，顶起的地方底下是空的。他拿一张薄纸覆上去，用手指一抹——纸上只留下十一个小洞。

然后他只处理这十一个小洞。九百袋只碰十一袋，剩下八百八十九袋，一个指头都没动。

第三趟，顾先生发觉自己一整天几乎没走过路。

因为托盘就摆在他手边一条窄廊里，一排排立着，像书架。他的手往前一探，摸到的就是下一格，不用看，不用找。从前盐车散在院子里，他每取一辆都得先找它在哪儿；现在一伸手，货自己就来了。

秋天旺季到了。八个人，一万两千辆。

沈砚没按车主分活。他把场院划成窄条，一条窄条正好九百格；谁手上空下来，就去拿下一张空条。不问这条是谁的，不问谁拉的车多。从前按车主分，一个盐商带来四千车，领了活的那个人趴在那堆里出不来，另外七个人靠着墙抽烟。

腊月里，顾先生把沈砚叫到案子前，问了三个问题。

**第一个问题，是关于托盘大小的。**

沈砚一开始做的托盘，能装三万袋。三万袋摊不满一张案子，得铺到地上。货一铺到地上，他弯腰的趟数比从前还多，一天下来腰直不起来。

他改小的第一版是五十袋一盘。盘子小得他的胳膊用不上，一整天都在换盘，换盘的功夫比看货的功夫长。

最后他定在九百。九百袋摊开，正好在他两条胳膊之间；一盘的账，正好写满一页纸。**大到值得压一次尺，小到还没离开他的手。**

**第二个问题，是关于混装的。**

有一回，一个急着过关的车主把一斤茶叶混进了盐盘；又有一回，印记盘里混进了三张盐引。铜尺一压，刻度全乱了：茶叶是圆的，压不住；盐引是软的，尺一过就卷起来。

更要命的是——出来的数，一个也不像错的。每个格子都有读数，每个读数都写进了簿子。

顾先生问他怎么办。沈砚说："一盘只能有一种东西，因为尺只认一种东西。"

所以货物进场时，要先在**号房**里过一遍：散货不写名字，写号。盐写一号，茶叶写二号，盐引写三号；盘子上只记号，不记名。一名目一托盘，尺就有东西可认。

至于那些实在摊不平的货——景德镇的瓷，形状各异的漆器——他不硬摊。案子旁边另设一张小桌，那种货还是一车一车地过，慢，但错不了。

**第三个问题，是关于"都对，但都错"。**

这是顾先生自己发现的。有一年腊月，他把一季的账翻出来复核，发现某一日的税银，每一笔单独看都对，加起来却比实收少了三成。

查到最后，是一张托盘装错了。装盘那天下雪，伙计把印记盘的第七百格和第七百零一格装反了。于是那一整盘里，第七百格以后的每一辆车，印记都配到了别人的斤两上。

铜尺量得准，秤星读得准，税则查得准，流水簿写得准。四样都准，账是错的。

"每一个数都对，不等于这一行是对的。"顾先生说，"这一行对不对，靠的是那九百个格子在所有盘子里，是不是同一辆车的九百个格子。这件事，你量不出来，你只能保证。"

他把这句话写在了关门的木牌上。

到这儿，你大概已经认出来了：这就是 *向量化执行*（*vectorized execution*），配上 *列式存储*（*columnar storage*）。

### 这是什么

先看旧引擎怎么干活。

数据库执行查询，传统上是 *Volcano* 模型，也叫 *迭代器模型*（*iterator model*）：每个算子对外只有一个 `next()`，每次调用**返回一个元组**。一条 `SELECT ... WHERE ... GROUP BY` 展开成一棵树，根节点向上吐行，中间每个节点向子节点要一行，拿到、算一格、再往上传。

```c
// 逐行：每返回一个 tuple，都要穿过一次虚函数调用
while ((tuple = child->next()) != NULL) {
    if (eval(pred, tuple)) emit(project(tuple));   // 每个 tuple 再调一次
}
```

问题不在算法，在**每次调用之间做了什么**。一行 16 个字段、5 层算子，意味着每前进一行要穿过 5 次虚调用、5 次指针跳转、5 次分支预测。CPU 在这段时间里几乎不碰数据——它在忙着搞清楚"下一步该跑哪段代码"。顾先生三十一年的四趟路，走的正是这个开销：**每一车都在换家伙，而换家伙的时间比干活长。**

*向量化执行* 把 `next()` 的粒度从"一个元组"改成"**一批**"：通常 1024 行、一列。

```c
// 向量化：一批 1024 个值，一个无分支的紧循环
for (size_t i = 0; i < n; i++)
    sum += col[i];              // n = 1024；编译器可自动展开成 AVX-512（一次 8 个 int64）
```

换成这一批之后，三件事同时变了。

**一、控制流的开销被摊薄了。** 一次虚调用不再是"每行一次"，而是"每一千行一次"。循环体里没有虚函数、没有分支——只有一个连续的数组和一条确定的路。编译器可以把它编成 SIMD 指令（AVX2 一次 4 个 int64，AVX-512 一次 8 个），一条指令算八袋，这就是那根铜尺。

**二、分支被换成了掩码。** 逐行引擎里，`WHERE weight > 30` 是一个**每个元组都要跳一次**的分支，而分支预测器在随机数据上几乎必错（一次误预测十几个周期）。向量化引擎把比较变成一张**位图**（*bitmask*）或一个**选择向量**（*selection vector*，记录幸存行的下标），下游算子只遍历下标。九百次"超了没有"的提问，变成一次掩码运算——这就是那张穿孔铜片。

**三、数据布局变了。** 这就是 *列式存储*：一列的值连续存放，而不是一行的字段挤在一起。这会带出三件事：

- **只读你要的。** 40 列的表扫 3 列，行存要读 40 列的字节再扔掉 37 列；列存只读 3/40 的字节。现代分析查询的瓶颈是**内存带宽**，不是指令数，省下的带宽直接变成速度。
- **缓存和预取都成立。** 顺序访问连续数组，硬件预取器能提前把下一页抓进缓存——手往前一探，货自己就来了。而 1024 行 × 8 字节 × 若干列只有几十 KB，正好装进 L1/L2。托盘必须是"恰好摊满一张案子"的尺寸：太小，每次都在换盘；太大，摊到地上，捡起来比看还贵。
- **同类型的值挤在一起就能压缩。** 五十个连续的相同印记只需要一行文字，字典编码把字符串换成整数号，位打包把整数压进更少的位。而且很多算子可以**直接在压缩数据上运算**，不必先解压——这就是号房和捆扎。

另外两件工程上的事，在这套结构里是自然长出来的：

**晚物化（late materialization）。** 先用选择向量在几列上把行筛掉，只为幸存的行去取那些又宽又贵的列（大字符串、嵌套结构）。九百袋先筛成十一袋，再去翻十一袋的账页。

**Morsel 驱动的并行。** 把表切成几十万行的小块（*morsel*），哪个线程空下来就去领下一块。按分区静态分活，一个热点分区能把一个线程压死、其余线程空转；按小块动态领活，谁也不闲着——窄条，不问是谁的车。

代价与边界同样真实：

- **类型必须单一。** SIMD 内核是单态的——8 个 int64 一起加，或者 8 个 int32 一起加，不能一半整数一半浮点。变长字符串先过字典编码换成 ID（号房），实在不规则的表达式（正则、复杂 UDF、JSON 路径）就退回逐行路径（案子旁边那张小桌）。**引擎里最容易出 bug 的地方，就是这两条路径的交界处：它们在语义上必须完全等价，实现上却几乎不共享代码。**
- **位置对齐是全部正确性的地基。** 所有列数组必须等长、且下标一一对应同一行。一旦某列被过滤、被重排、或者某个算子在传递时忘了带上选择向量，结果就**静默地错**——每个读到的值都是真的，组成的行是假的。顾先生那句"每一个数都对，不等于这一行是对的"，是列式引擎最著名的故障类型。
- **点更新很糟。** 改一行的 40 个字段，在列存里是往 40 个分离的数组里各写一处，随机写、缓存全失效。这就是为什么 OLTP 至今仍是行存 + 逐行 + 索引查找，而 OLAP 走向列存 + 向量化：同一句 SQL，物理设计选错，代价是几十到上百倍。

这条路的实证起点是 2005 年的 MonetDB/X100 论文（Boncz、Zukowski、Kersten）：不改算法、不改复杂度，只改数据布局与循环粒度，TPC-H 上比逐行引擎快一到两个数量级。今天它已经是分析引擎的默认形态——DuckDB、ClickHouse、Snowflake、BigQuery、Databricks Photon、Velox，以及把"托盘规格"直接标准化成内存格式的 Apache Arrow（列式缓冲 + 有效性位图）。

### 为什么重要

因为这是过去二十年里，**算法一点没变、性能却变了一个数量级**的最干净的案例。

同一个查询，同样的复杂度，同样的结果。变的只有两件事：数据在内存里怎么排，以及循环里一次处理几个值。前面的二十年硬件没有变快多少——主频停在 3-5 GHz 十几年了；变宽的是 SIMD（128 位到 512 位）、变深的是存储层次（L1 到 DRAM 差两个数量级）、变多的是核。逐行执行把这三样全部浪费掉了：它让 CPU 忙着做控制流，而不是做数据。

由此产生的那条分界线，比任何单一技术都重要：

- **OLTP**：行存、逐行、索引查找、点更新——它优化的是"找到一行并改掉它"。
- **OLAP**：列存、向量化、顺序扫描、压缩——它优化的是"扫过几十亿个值，只输出一小撮"。

这两套设计不是风格偏好，是物理约束的必然。HTAP 之所以难，不是因为厂商懒，是因为"一次改一行"和"一次扫一列"对内存布局的要求是直接冲突的。

对写代码的人来说，这套东西有三条可以直接搬走的经验，而且不限于数据库：

1. **环顾你手上的记录处理循环。** 如果你在写"对每条记录调用一个函数"，试试改成"把一批记录的同一字段装进连续数组，然后一个紧循环"。不换语言、不换算法，十倍是常见的。
2. **批次大小要按缓存定，不是按感觉定。** 小到值得摊开一次，大到还没离开缓存。几千行通常是对的；几百万行通常是错的——那不是批处理，那是又一次逐行，只是把逐行挪到了数组上。
3. **位置对齐要当成不变量来守。** 一旦你把一条记录拆成多个平行数组，任何一次过滤、排序、合并都必须同时作用于所有数组。这是唯一会静默出错的地方——性能问题你会看见，对齐问题你只会看见错误的报表。

而顾先生那句话，值得写在每个列式引擎的设计文档首页：**每一个数都对，不等于这一行是对的。**

_隐喻对应表_

- 顾先生一车一车地过 → *Volcano* / 迭代器模型，`next()` 每次返回一个元组
- 一车四趟路：秤、册子、税则、笔 → 每个元组要穿过一整套算子，每层一次虚函数调用
- 每趟路换一次手里的家伙 → 逐行执行的控制流开销：指针跳转、分支预测、指令缓存抖动
- 换家伙的时间比干活长 → 解释开销（interpretation overhead）压过了真正的数据处理
- "不是您老了，是您每一车都在换家伙" → 瓶颈不在硬件主频，在执行模型的每行开销
- 把车拆成托盘 → 把元组流换成按列的批次（vector / batch）
- 一车货按名目分倒在不同的盘 → 列式存储：同一属性的值连续存放
- 九百格一盘 → 批次大小，约 1024 行
- 托盘正好摊满一张案子，伸开胳膊够得到 → 批次必须装进 L1/L2 缓存
- 三万袋一盘，摊到地上，弯腰比从前还多 → 批次过大导致缓存溢出，回落到内存甚至磁盘访问
- 五十袋一盘，整天在换盘 → 批次过小，每次批处理自身的固定开销重新占主导
- 九百格，账正好写满一页 → 批次大小的甜点：摊销开销，同时留在缓存里
- 所有托盘里第一车都在第一格 → 位对齐不变量：各列数组下标一一对应同一行
- 铜尺压一次读九百格 → SIMD：一条指令处理 4 个（AVX2）或 8 个（AVX-512）int64
- 秤星是活的，要等它停；尺是死的 → 分支与数据依赖 vs 无分支的紧循环
- 九百次"超了没有"的提问 → 逐行谓词求值，每次一个必然的分支
- 八百八十九次的答案是"没有"，却一样费神 → 分支预测在随机数据上高失效率
- 穿孔铜片、覆纸、抹出十一个小洞 → 谓词求值产出位图 / 选择向量
- 只处理十一个小洞，其余不再过手 → 晚物化：只有幸存行才去取昂贵的宽列
- 托盘立在手边窄廊，一探手就到 → 顺序访问 + 硬件预取，数据布局与缓存层次匹配
- 盐车散在院子里，每取一辆先找它在哪 → 行存扫描的随机访问与无效带宽
- 只读你要的那几列（省下的字节） → 列存投影裁剪：40 列读 3 列，省 37/40 的内存带宽
- 号房：散货不写名字只写号（盐一号、茶二号） → 字典编码，把变长字符串换成整数 ID
- 一名目一托盘，因为尺只认一种东西 → SIMD 内核是单态的，批次内类型必须统一
- 捆扎：五十个相同印记只记一行再乘出来 → RLE / 位打包 / 字典编码，且在压缩态上直接运算
- 案子旁的小桌，瓷器漆器仍一车一车过 → 复杂表达式与不规则类型的逐行回退路径
- 两条路径交界处最容易出 bug → 向量化与回退路径必须语义等价，实现却几乎不共享代码
- 场院划成窄条，谁闲着领下一条，不问是谁的车 → morsel 驱动并行，动态派发消除长尾与倾斜
- 按车主分活，四千车那位趴着出不来，七个人抽烟 → 静态分区在数据倾斜下的负载不均
- 雪天装错两格，印记配到了别人的斤两上 → 选择向量丢失 / 列错位，静默产生错误结果
- 四样都准，账是错的 → 每个读到的值都正确，组成的行是假的；这类 bug 不报错、只出错数
- "每一个数都对，不等于这一行是对的" → 列式引擎的正确性依赖位置对齐，而非单值正确性
- 改一行的四十个字段要写四十个地方 → 列存上的点更新代价高，这是 OLTP 仍用行存的原因
- 盐关分 OLTP / OLAP 两套做法 → 行存逐行与列存向量化是物理约束的必然分岔，不是风格选择
- 顾先生六十岁膝盖不干 → 逐行模型在数据量增长下先撞上的是内存带宽和分支预测，不是算力
</section>
<section class="en" markdown="1">
The salt customs at Luoyan Pass had exactly one job: every cart of salt crossing the pass was taxed by weight and by grade.

Mr. Gu had done that job for thirty-one years.

His method came from his master, unchanged by a single character: **one cart at a time.**

A cart rolled up to the desk. He rose, walked to the official scale in the yard, hung the sacks on the hook, waited for the beam to settle, wrote down the weight. Walked back to the desk, opened the Register of Grades, looked up which class this cart belonged to by the seal on its sacks. Walked to the far end of the yard for the tariff book. Walked back to the desk and wrote one line in the day ledger.

One cart, four trips. One trip, one change of tool: scale hook, register, tariff, brush.

For thirty-one years his hands were faster than his mouth, and he never saved a single one of those trips.

On an ordinary day eight hundred salt carts crossed Luoyan Pass. After the autumn harvest came what people called "the salt carts fording the river," and on those days twelve thousand crossed. Four trips each: forty-eight thousand trips. The year Mr. Gu turned sixty, his knees quit first.

His replacement was Shen Yan, twenty-four, educated in the new schools. On his third day at the pass he said something that left the whole room of clerks speechless:

"Sir, it isn't that you've gotten old. It's that you change tools on every single cart."

The next day Shen Yan did not touch the scale, and did not change a character of the tariff. He did one thing: **he took the carts apart into trays.**

Carts coming up the pass no longer entered the yard. They were unloaded first, at a platform at the east end. The unloading was not his business. He set only three rules.

First: a cart's goods were poured out onto different trays **by kind**. Weights onto the weight tray, seal marks onto the seal tray, grades onto the grade tray. One tray held one kind of thing.

Second: every tray had nine hundred slots, numbered left to right. **The first slot of the first cart was slot one on every tray.** If a cart sat in slot one hundred thirty-seven of the weight tray, it sat in slot one hundred thirty-seven of the seal tray too.

Third: one tray lay flat across exactly one desk. He could reach it end to end with his arms out.

On his first pass he walked to the weight tray and did not reach for anything. He had them bring a copper rod — as long as the tray, with nine hundred graduations cut into it, one per slot. He laid the rod down on the tray, pressed, and glanced once. Nine hundred weights were in front of him.

That one press replaced nine hundred trips. Before, he was "one sack, one look at the beam," and the beam was alive — he had to wait for it to settle. Now it was "one tray, one press of the rod," and the rod was dead: nine hundred numbers arrived together.

On his second pass a problem appeared. Of nine hundred sacks, perhaps eleven were overweight. Before, he had to ask "over or not?" nine hundred times. Eight hundred eighty-nine times the answer was no — and those eight hundred eighty-nine questions cost him just as much attention.

Shen Yan's answer was a **pierced copper sheet**. As wide as the tray, laid over the weight tray: where a slot was overweight, the sheet bulged up a little, and beneath the bulge it was hollow. He laid thin paper over it and wiped with one finger. The paper came away with eleven small holes in it.

Then he handled only those eleven holes. Nine hundred sacks, eleven touched. The other eight hundred eighty-nine were never lifted.

On his third pass, Mr. Gu noticed he had hardly walked all day.

Because the trays stood in a narrow aisle right beside him, upright in rows like a bookcase. His hand reached forward and found the next slot without looking, without searching. Before, the salt carts were scattered across the yard and every single one had to be located first. Now his hand went out and the goods came to him.

Then the autumn rush arrived. Eight men, twelve thousand carts.

Shen Yan did not divide the work by cart owner. He cut the yard into narrow strips, each strip exactly nine hundred slots, and whoever's hands came free took the next empty strip. Nobody asked whose strip it was, or whose merchant had brought more carts. Under the old way, one salt merchant arrived with four thousand carts, the man who drew that lot disappeared into the pile, and the other seven leaned against the wall and smoked.

In the twelfth month Mr. Gu called Shen Yan to the desk and asked three questions.

**The first was about the size of the trays.**

Shen Yan's first trays held thirty thousand sacks. Thirty thousand did not fit on a desk; they had to be spread on the ground. Once goods went on the ground, he bent over more times than ever before, and by the end of the day he could not straighten his back.

His first correction was fifty sacks to a tray. The tray was so small his arms were useless, and he spent the whole day swapping trays — the swapping took longer than the inspecting.

He settled on nine hundred. Nine hundred sacks spread out exactly between his two arms; the account for one tray filled exactly one page. **Big enough to be worth pressing the rod, small enough to still be in his hands.**

**The second was about mixing.**

Once, a merchant in a hurry let a catty of tea into the salt tray. Another time three salt permits got mixed into the seal tray. One press of the copper rod and the graduations went to nonsense: tea is round and will not lie flat, permits are soft and curl as the rod passes over.

Worse — not one number looked wrong. Every slot had a reading, and every reading went into the ledger.

Mr. Gu asked what he intended to do about it. Shen Yan said: "A tray can hold only one kind of thing, because the rod knows only one kind of thing."

So goods passed through a **number room** on the way in: loose goods were not written by name, but by number. Salt was one, tea was two, permits were three; the trays carried only numbers, never names. One kind, one tray, and the rod had something it could read.

As for the goods that would not lie flat at all — Jingdezhen porcelain, lacquerware in every shape — he did not force them. A small side table stood beside the desk, and those still went through one cart at a time. Slow, but never wrong.

**The third was about "all correct, all wrong."**

Mr. Gu found this one himself. One twelfth month he went back over a season of accounts and found a day whose tax silver was correct line by line, and yet came out thirty percent short of what had actually been collected.

The cause, traced to the end, was one tray loaded wrong. It had snowed that day, and a hand had swapped slots seven hundred and seven hundred-one on the seal tray. So for every cart after slot seven hundred on that tray, the seal was paired with somebody else's weight.

The rod measured truly, the beam read truly, the tariff was looked up truly, the ledger was written truly. All four true, and the account was false.

"Every number being right is not the same as the row being right," Mr. Gu said. "Whether the row is right depends on whether those nine hundred slots, across every tray, are the nine hundred slots of the same carts. That is not something you can measure. It is only something you can guarantee."

He had that sentence carved on the board above the gate.

By now you have probably recognized it: this is *vectorized execution*, together with *columnar storage*.

### What it is

Start with how the old engine works.

A database traditionally executes queries with the *Volcano* model, also called the *iterator model*: every operator exposes a single `next()`, and each call **returns one tuple**. A `SELECT ... WHERE ... GROUP BY` unfolds into a tree; the root pulls rows upward, each node asks its children for a row, computes one cell, and passes it up.

```c
// Tuple at a time: every returned tuple crosses a virtual call
while ((tuple = child->next()) != NULL) {
    if (eval(pred, tuple)) emit(project(tuple));   // and another call per tuple
}
```

The problem is not the algorithm. It is what happens **between** the calls. A row of 16 fields through 5 operators means, per row, 5 virtual dispatches, 5 pointer chases, 5 branch predictions. The CPU barely touches data during all of it — it is busy working out which code to run next. Mr. Gu's four trips per cart are exactly that overhead: **every cart forces a tool change, and the tool change costs more than the work.**

*Vectorized execution* changes the granularity of `next()` from "one tuple" to "**one batch**" — typically 1024 rows of a single column.

```c
// Vectorized: a batch of 1024 values, one branch-free tight loop
for (size_t i = 0; i < n; i++)
    sum += col[i];              // n = 1024; the compiler can widen this to AVX-512 (8 int64 per instruction)
```

Three things change at once.

**One: control-flow overhead is amortized.** A virtual call is no longer once per row but once per thousand rows. The loop body has no virtual functions and no branches — just a contiguous array and one determined path. The compiler can emit SIMD (4 int64 per instruction with AVX2, 8 with AVX-512): one instruction, eight sacks. That is the copper rod.

**Two: branches become masks.** In a tuple-at-a-time engine, `WHERE weight > 30` is a branch **taken once per tuple**, and on random data the predictor is wrong nearly every time (a dozen-plus cycles per misprediction). A vectorized engine turns the comparison into a **bitmask** or a **selection vector** (the indices of survivors), and downstream operators iterate only those indices. Nine hundred questions of "over or not?" become one mask operation — that is the pierced copper sheet.

**Three: the data layout changes.** This is *columnar storage*: the values of one column stored contiguously, instead of one row's fields packed together. Three consequences follow:

- **Read only what you need.** Scanning 3 columns of a 40-column table, a row store reads all 40 columns' bytes and throws 37 away; a column store reads 3/40 of the bytes. The bottleneck in modern analytical queries is **memory bandwidth**, not instruction count, so saved bytes become speed directly.
- **Caching and prefetching start working.** Sequential access to a contiguous array lets the hardware prefetcher pull the next page in before you ask — the hand reaches out and the goods come to it. And 1024 rows × 8 bytes × a few columns is tens of KB, which fits in L1/L2. The tray must be "exactly one desk wide": too small and you spend the day swapping trays; too large and it spills to the floor, where picking it up costs more than inspecting it.
- **Values of the same type, packed together, compress.** Fifty identical seal marks in a row take one line of text, dictionary encoding turns strings into integer IDs, bit-packing squeezes integers into fewer bits. And many kernels can **operate directly on compressed data** without decompressing first — that is the number room and the bundling.

Two further engineering consequences grow naturally out of this structure:

**Late materialization.** Use the selection vector to filter on a few cheap columns first, and fetch the wide, expensive columns (large strings, nested structures) only for the survivors. Nine hundred sacks narrowed to eleven, and only then are eleven ledgers opened.

**Morsel-driven parallelism.** Cut the table into chunks of a few hundred thousand rows (*morsels*), and let whichever thread comes free take the next chunk. Static partitioning by key lets one hot partition pin one thread while the rest idle; dynamic morsel dispatch keeps everyone busy — narrow strips, and nobody asks whose carts they are.

The costs and boundaries are just as real:

- **Types must be uniform.** SIMD kernels are monomorphic — eight int64s added at once, or eight int32s, never half integers and half floats. Variable-length strings go through dictionary encoding to IDs first (the number room), and genuinely irregular expressions (regexes, complex UDFs, JSON paths) fall back to a row-at-a-time path (the small table beside the desk). **The boundary between those two paths is where engines break:** they must be semantically identical, and they share almost no code.
- **Positional alignment is the foundation of all correctness.** Every column array must be the same length and index-for-index the same rows. If a column is filtered, reordered, or an operator forgets to carry the selection vector along, the result is **silently wrong** — every value read is genuine, and the row assembled from them is a fiction. "Every number right is not the same as the row being right" is the signature failure mode of columnar engines.
- **Point updates are terrible.** Changing 40 fields of one row means one write into each of 40 separate arrays: random writes, cache wiped. That is why OLTP still runs on row stores with tuple-at-a-time execution and index lookups, while OLAP moved to column stores and vectorization. Same SQL, wrong physical design: the cost is tens to hundreds of times.

The empirical starting point was the 2005 MonetDB/X100 paper (Boncz, Zukowski, Kersten): no algorithmic change, no complexity change, only data layout and loop granularity — one to two orders of magnitude over tuple-at-a-time engines on TPC-H. Today it is the default shape of analytical engines: DuckDB, ClickHouse, Snowflake, BigQuery, Databricks Photon, Velox, and Apache Arrow, which standardizes the tray specification itself into an in-memory format (columnar buffers plus validity bitmaps).

### Why it matters

Because it is the cleanest case in the last twenty years of **an algorithm that did not change at all while performance changed by an order of magnitude.**

Same query, same complexity, same answer. Only two things changed: how the data is arranged in memory, and how many values the loop touches per iteration. The preceding twenty years of hardware did not get much faster — clock speeds have sat at 3–5 GHz for a decade and a half. What got wider was SIMD (128 bits to 512), what got deeper was the memory hierarchy (L1 to DRAM is two orders of magnitude), what got more numerous were cores. Tuple-at-a-time execution wastes all three: it keeps the CPU busy on control flow instead of on data.

The dividing line that follows matters more than any single technique:

- **OLTP**: row store, tuple-at-a-time, index lookups, point updates — optimized for "find one row and change it."
- **OLAP**: column store, vectorized, sequential scans, compression — optimized for "sweep billions of values and emit a handful."

These are not stylistic preferences; they are forced by physics. HTAP is hard not because vendors are lazy but because "change one row at a time" and "sweep one column at a time" place directly conflicting demands on memory layout.

For anyone writing code, three lessons transfer well beyond databases:

1. **Look at your record-processing loops.** If you are writing "call a function for each record," try "pack the same field of a batch of records into a contiguous array, then one tight loop." No new language, no new algorithm; ten times is common.
2. **Size batches by the cache, not by feel.** Small enough to be worth spreading out once, large enough to still be in cache. A few thousand rows is usually right; a few million is usually wrong — that is not batching, that is tuple-at-a-time again, moved onto an array.
3. **Treat positional alignment as an invariant.** The moment you split a record across parallel arrays, every filter, sort, and merge must act on all the arrays together. It is the one thing that fails silently — a performance problem you will see, an alignment problem you will only see in a wrong report.

And Mr. Gu's sentence belongs on the first page of every columnar engine's design doc: **every number right is not the same as the row being right.**

_Metaphor mapping_

- Mr. Gu going one cart at a time → the *Volcano* / iterator model, where `next()` returns one tuple
- Four trips per cart: scale, register, tariff, brush → each tuple crossing a whole stack of operators, one virtual call per layer
- Changing tools on every trip → per-row control-flow overhead: pointer chasing, branch prediction, instruction-cache churn
- Tool-changing taking longer than the work → interpretation overhead dominating actual data processing
- "It isn't that you've gotten old, it's that you change tools on every cart" → the bottleneck is the execution model's per-row cost, not clock speed
- Taking the carts apart into trays → replacing the tuple stream with per-column batches (vectors)
- Pouring a cart's goods onto trays by kind → columnar storage: values of one attribute stored contiguously
- Nine hundred slots to a tray → batch size, roughly 1024 rows
- A tray lying flat across one desk, reachable end to end → the batch must fit in L1/L2
- A thirty-thousand-sack tray spread on the ground, more bending than before → batches too large, spilling the cache to memory or disk
- A fifty-sack tray, the whole day spent swapping trays → batches too small, per-batch fixed cost dominating again
- Nine hundred slots, the account filling exactly one page → the batch-size sweet spot: amortize overhead while staying in cache
- Every tray's first cart in slot one → the positional-alignment invariant: all column arrays correspond index-for-index to the same rows
- One press of the copper rod reads nine hundred slots → SIMD: 4 int64 per instruction (AVX2) or 8 (AVX-512)
- A live beam you must wait on, versus a dead rod → data-dependent branches versus a branch-free tight loop
- Nine hundred questions of "over or not?" → per-row predicate evaluation, one unavoidable branch each
- Eight hundred eighty-nine "no" answers costing just as much attention → branch misprediction rates on random data
- Pierced copper sheet, paper, one wipe, eleven holes → predicate evaluation producing a bitmask / selection vector
- Handling only the eleven holes and never touching the rest → late materialization: fetch expensive wide columns only for survivors
- Trays upright in a narrow aisle, found by feel → sequential access plus hardware prefetch, data layout matched to the cache hierarchy
- Carts scattered in the yard, each needing to be located first → row-store scans: random access and wasted bandwidth
- Reading only the few columns you need → projection pushdown in a column store: 3 of 40 columns, 37/40 of the bandwidth saved
- The number room writing numbers instead of names (salt one, tea two) → dictionary encoding, variable-length strings replaced by integer IDs
- One kind per tray, because the rod knows only one kind → SIMD kernels are monomorphic; a batch must be type-uniform
- Bundling fifty identical seal marks into one line and multiplying → RLE / bit-packing / dictionary encoding, and operating directly on compressed data
- The small side table where porcelain and lacquerware go one cart at a time → the row-at-a-time fallback path for complex expressions and irregular types
- The junction of the two paths being where bugs live → vectorized and fallback paths must be semantically equal yet share almost no code
- Cutting the yard into narrow strips, whoever is free takes the next → morsel-driven parallelism, dynamic dispatch eliminating tails and skew
- Dividing by cart owner: the four-thousand-cart man vanishes into the pile while seven men smoke → static partitioning collapsing under data skew
- A snowy day swapping two slots, seals paired with others' weights → a lost selection vector / misaligned column producing silently wrong results
- All four steps true, the account false → every value read is correct and the assembled row is a fiction; no error, only wrong numbers
- "Every number being right is not the same as the row being right" → columnar correctness rests on positional alignment, not on individual values
- Changing one row's forty fields requiring forty separate writes → point updates are expensive on column stores, which is why OLTP stays row-oriented
- The pass keeping two entirely different working methods → row-store/tuple-at-a-time and column-store/vectorized are a forced fork in physical design, not a style choice
- Mr. Gu's knees quitting at sixty → as data grows, the tuple model hits memory bandwidth and branch prediction before it hits compute
</section>
