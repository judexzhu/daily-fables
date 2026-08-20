---
layout: fable
title: "不必落地的麻袋 · The Sacks That Never Touched the Ground"
title_zh: "不必落地的麻袋"
title_en: "The Sacks That Never Touched the Ground"
concept: "Zero-copy I/O"
tags: [linux, networking, storage]
illustration: /assets/art/2026-08-20-zero-copy-io.jpg
---
<section class="zh" markdown="1">
清河码头的规矩，四十年没变过。

天刚亮，粮船靠岸。船工把麻袋从舱里扛下来，堆在石岸上；脚夫再一袋一袋扛进货栈内堂——那是账房先生的地界。先生要亲手解开每一只麻袋，过秤，重新扎口，记进册子。记完，脚夫又把麻袋从内堂扛出来，扛过整条石岸，装进等在下游的驳船。

一袋粮，落地三次，被人扛了四趟。

内堂门口还有一道更磨人的规矩：脚夫每进出一次，都要在门房停下，报名字、卸扁担、登记时辰。三十袋粮，光是在那道门口一站一站，就要耗掉半个上午。

今年新来的漕运使姓沈。头一天，他在岸边站了整整两个时辰，一句话没说。第二天清早，他叫来了账房先生。

"你解开麻袋，是为了什么？"

先生一愣："为了知道里头是几斗粮、什么成色。"

"船家封条上，写着几斗、什么成色么？"

"写着。"

"那你解开它，是为了**改动**里头的粮，还是为了**知道**数目？"

先生沉默了半晌："……为了知道数目。"

当天沈使就叫木匠打了一条长长的木槽，一头架在粮船舱口，一头伸进驳船的侧门，中间不落地。他又收走了先生的秤和账册，换给他一样小东西：一块木牌，上面只写三行——这一垛在船的哪个舱位、从第几袋起、一共几袋。

从此码头的早晨是这样的：船工推开舱门，麻袋顺着木槽自己滑进驳船；账房先生站在岸边，一块一块翻他的木牌，从头到尾没有碰过一粒粮；脚夫们坐在空箱子上晒太阳。

粮还是那些粮，数目还是那些数目，只是没有人再把它们扛来扛去了。

沈使留了两条例外。

一是：若这一船粮要验成色、要掺石灰防潮，那就老老实实进内堂——**要改动一样东西的人，必须亲手碰到它**。他后来在木槽边搭了一间小屋，让验货的师傅守在槽旁当场办，好歹省下往返内堂那一段路。

二是：木槽再快，也不能一袋一袋地滑。他让人把木牌按垛归并，一次报一垛，门房那一站一站的登记，就从三十次减到了三次。

——到这儿你大概已经认出来了：这不是码头的故事。麻袋是数据，账房内堂是 user space 缓冲区，脚夫的扁担是 CPU 的 `memcpy`，木槽是 DMA，木牌是描述符。这是 *zero-copy I/O* 的故事。

### 这是什么

把一个文件从磁盘发到网络上，最朴素的写法是 `read()` 再 `write()`。这一路数据被搬了四次：磁盘 DMA 进 page cache，CPU 把 page cache 拷进用户缓冲区，CPU 再把用户缓冲区拷进 socket buffer，网卡 DMA 把它取走。中间那两次是 CPU 亲手做的 memcpy——纯粹的搬运，一个字节都没有被改动。再加上两次 syscall 带来的四次 user/kernel 切换。

*Zero-copy* 的想法很简单：**如果应用根本不打算改动这份数据，它就不该经过应用的地址空间**。`sendfile()` 把"从这个文件的这个偏移，发这么多字节到这个 socket"整句话交给内核；配合网卡的 *scatter-gather DMA*，socket buffer 里放的只是描述符（地址 + 长度），网卡直接从 page cache 里把数据取走，CPU 一次都没有碰到数据本身。`splice()` 借一根 pipe 在两个 fd 之间挪的，同样只是页引用。

### 为什么重要

到 25/100 GbE 这个量级上，瓶颈早就不是网卡，而是**内存带宽和 CPU 周期**。省下的也不只是 memcpy 的那点时间，还有被这份根本不需要理解的数据冲刷掉的 L3 cache。Kafka 的高吞吐、nginx 的 `sendfile on`、HAProxy 用 `splice()` 转发 TCP、容器镜像层的分发，底下都是同一件事。

它的边界同样清楚：**只要应用需要改动数据，零拷贝就失效**。TLS 一度是最大的破坏者——要加密就得碰数据。解法不是放弃零拷贝，而是把加密搬到数据通路上去：kTLS 让内核在发送路径里完成加密，网卡的 crypto offload 更进一步。这就是沈使那间"槽边小屋"。

还有一句值得记住：零拷贝省的是**拷贝**，不是 **syscall**。一袋一袋地滑，门房那一关照样把你拖垮——所以 `io_uring` 的批量提交、registered buffer 和 zero-copy 是配套的两件事，不是一件。

_隐喻对应表_

- 麻袋里的粮 → 要传输的数据本身（文件内容 / 网络 payload）
- 粮船的货舱 → page cache：内核里已经存在的那一份数据
- 账房先生的内堂 → user space 缓冲区
- 脚夫扛麻袋的每一趟 → CPU 执行的 `memcpy`，烧的是 CPU 周期和内存带宽
- 门房的登记 → syscall 与 user/kernel context switch
- 那条木槽 → DMA / scatter-gather DMA，数据不经过应用就到达目的地
- 账房先生手里的木牌 → 描述符：地址 + 偏移 + 长度，`sendfile()`/`splice()` 传的就是它
- 一次报一垛，而不是一袋一袋 → 批量提交（`io_uring`、大块 sendfile），减少 syscall 次数
- 必须掺石灰的那一船 → 应用必须改动数据的场景：压缩、加密、解析——零拷贝在此失效
- 木槽边的小屋 → kTLS 与网卡 crypto offload：把改动搬到数据通路上，而不是把数据搬去改动
</section>
<section class="en" markdown="1">
The rules at Qinghe wharf had not changed in forty years.

At first light the grain boat ties up. Boatmen haul the sacks out of the hold and stack them on the stone quay. Porters carry them, one sack at a time, into the inner hall of the warehouse — the clerk's domain. There the clerk unties every sack, weighs it, ties it up again, and enters it in the ledger. When he is finished the porters carry the sacks back out of the inner hall, the whole length of the quay, and load them onto the barge waiting downstream.

One sack of grain. Set down three times, carried four.

There is a worse rule at the door of the inner hall: every time a porter passes through it he must stop at the gatehouse, give his name, set down his pole, and have the hour written into a book. For thirty sacks, the stopping and starting at that one door eats half the morning.

The new harbor commissioner that year was named Shen. On his first day he stood on the quay for four hours and said nothing. The next morning he sent for the clerk.

"When you untie a sack, what is it for?"

"To know how many pecks are inside, and of what grade."

"Does the boatman's seal say how many pecks, and of what grade?"

"It does."

"Then do you untie it to **change** the grain, or to **know** the number?"

The clerk was quiet for a long moment. "...To know the number."

That same day Shen had the carpenters build a long wooden chute, one end resting at the boat's hatch, the other reaching into the barge's side door, nothing in between touching the ground. And he took away the clerk's scales and ledger and gave him something small instead: a wooden tally board with only three lines on it — which hold the pile is in, which sack it starts at, how many sacks it holds.

After that, mornings at the wharf went like this. The boatmen swing the hatch open and the sacks slide down the chute into the barge by themselves. The clerk stands to one side turning his tally boards over, checking them off, and from beginning to end he never touches a single grain. The porters sit on empty crates in the sun.

Same grain. Same numbers. Only nobody carries it back and forth any more.

Shen kept two exceptions.

First: if a boat's grain must be graded by hand, or dusted with lime against the damp, then that boat goes into the inner hall the old way — because **whoever changes a thing must put his hands on it**. Later he built a small shed right beside the chute so the inspector could do the work there, saving at least the walk to the inner hall and back.

Second: fast as the chute is, it must not run one sack at a time. He had the tally boards grouped by pile, one report per pile, and the stop-and-start at the gatehouse fell from thirty times to three.

— By now you have probably recognized it: this is not a story about a wharf. The sacks are the data, the clerk's inner hall is the user-space buffer, the porter's shoulder pole is a CPU `memcpy`, the chute is DMA, and the tally board is a descriptor. This is a story about *zero-copy I/O*.

### What this is

The naive way to send a file from disk to the network is `read()` then `write()`. The data moves four times: DMA from disk into the page cache, a CPU copy from page cache into the user buffer, a CPU copy from the user buffer into the socket buffer, then DMA out to the NIC. Two of those are memcpys performed by the CPU itself — pure hauling, with not one byte altered — plus four user/kernel transitions for the two syscalls.

*Zero-copy* rests on one observation: **if the application never intends to change the data, the data should never pass through the application's address space**. `sendfile()` hands the kernel the whole sentence — "from this offset in this file, send this many bytes to this socket". With *scatter-gather DMA* on the NIC, what lands in the socket buffer is only a descriptor (address + length); the NIC pulls the bytes straight out of the page cache and the CPU never touches the payload at all. `splice()` does the same between two file descriptors through a pipe, moving nothing but page references.

### Why it matters

At 25/100 GbE the bottleneck stopped being the NIC long ago; it is **memory bandwidth and CPU cycles**. What you save is not only the time spent in memcpy but the L3 cache that gets flushed by data the CPU had no reason to look at. Kafka's throughput, nginx's `sendfile on`, HAProxy splicing TCP, container image layer distribution — all the same trick underneath.

Its limit is just as clear: **the moment the application must change the data, zero-copy is off**. TLS was long the great spoiler — to encrypt, you must touch. The answer was not to give up zero-copy but to move the encryption onto the data path: kTLS encrypts inside the kernel's send path, and NIC crypto offload goes further still. That is Shen's shed beside the chute.

One more thing worth keeping: zero-copy saves **copies**, not **syscalls**. Slide the sacks down one at a time and the gatehouse will still ruin your morning — which is why `io_uring`'s batched submission and registered buffers are a companion to zero-copy, not the same thing.

_Metaphor mapping_

- grain in the sacks → the payload itself (file contents / network bytes)
- the boat's hold → the page cache: the copy the kernel already has
- the clerk's inner hall → the user-space buffer
- each trip a porter makes → a CPU `memcpy`, paid for in cycles and memory bandwidth
- signing in at the gatehouse → syscalls and user/kernel context switches
- the wooden chute → DMA / scatter-gather DMA: data reaching the destination without passing through the app
- the clerk's tally board → the descriptor: address + offset + length, which is all `sendfile()`/`splice()` moves
- reporting one pile instead of one sack → batched submission (`io_uring`, large sendfile chunks) to cut syscall count
- the boat that must be dusted with lime → cases where the app must transform data — compression, encryption, parsing — and zero-copy no longer applies
- the shed beside the chute → kTLS and NIC crypto offload: move the transformation onto the data path instead of moving the data to the transformation
</section>
