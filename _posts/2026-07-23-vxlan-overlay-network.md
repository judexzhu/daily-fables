---
layout: fable
title: "套了两层信封的信 · The Letter in Two Envelopes"
title_zh: "套了两层信封的信"
title_en: "The Letter in Two Envelopes"
concept: "VXLAN overlay network encapsulation"
tags: [networking, kubernetes]
illustration: /assets/art/2026-07-23-vxlan-overlay-network.jpg
---
<section class="zh" markdown="1">
清晨的邮路上，有十几个小村子，靠几条公家修的土路连在一起。土路是全国通用的，谁都能走，但它只认门牌到门牌的大地址——「三号村北口」到「七号村南口」，别的一概不管。

麻烦在于：每个村子内部，各家各户都按自己村的老规矩起了小名。三号村有户人家叫「井边第二家」，七号村里也有一户叫「井边第二家」。村里人天天用这些小名，从没想过要改。可一旦把写着「井边第二家」的信直接丢上公路，路根本不知道该往哪个村送——小名在村外不作数。

于是每个村口都坐着一位系围裙的**门房**。村里人要往外村寄信，就把写着小名的**里层信**交给他。门房从不拆信、也不改一个字，他做三件事：先拿一个**大信封**把里层信整个套进去；在大信封外面写上公路认得的**村到村大地址**；最后压一坨**彩色的火漆**——红漆是米行会的信，蓝漆是布行会的信，绿漆是渔行会的信。

公路上的脚夫只看大信封上的村名，闷头把一摞摞火漆封好的大信封挑到目的村。路自始至终没见过里层的小名，也不在乎——它只在村与村之间搬东西。

到了目的村口，那边的门房接过大信封，撕掉外层，先看火漆的颜色：红漆的送米行会的分拣间，蓝漆的送布行会的。这样一来，两个村里都有的「井边第二家」永远不会串门——红漆的「井边第二家」和蓝漆的「井边第二家」，走的是同一条公路，却像隔着一堵墙。门房核对颜色对上了，才把里层信原封不动递给本村那户人家。

——到这儿你大概已经认出来了：这位系围裙、只往信外头再套一层的门房，就是 overlay 网络里的 *VXLAN* 隧道端点（*VTEP*）。

### 这是什么

*VXLAN*（Virtual eXtensible LAN）是一种 overlay（覆盖）网络封装技术。物理网络（*underlay*，那条公路）只负责把数据包在宿主机之间搬运，它只认宿主机的 IP。而 Pod、虚机这些「住户」用的是各自的虚拟地址，这些地址在物理网络上不可路由、还可能互相重复。

*VTEP*（VXLAN Tunnel Endpoint，那位门房）坐在每台宿主机上，把原始的二层帧整个塞进一个新的 UDP 包里（MAC-in-UDP），外层写上宿主机到宿主机的地址，再打上一个 24 位的 *VNI*（VXLAN Network Identifier，那坨火漆颜色）。VNI 有一千六百万种取值，让成千上万个互相隔离的虚拟网络能共用同一张物理网络，谁也看不见谁。收端 VTEP 拆掉外层、按 VNI 投递到正确的虚拟网段。

为什么重要：这正是 OpenShift/Kubernetes 里 Pod 能「拉平」地跨节点互通的底层魔法。underlay 完全不用知道 Pod 网段长什么样，跨节点、跨机架、甚至跨可用区都照常通信；多租户之间靠 VNI 天然隔离。代价是每个包多背一层封装头（约 50 字节开销），所以你得留意 MTU——外层塞不下时会分片或丢包，这是 overlay 网络最常见的坑。OpenShift 默认的 OVN-Kubernetes 用的是近亲 *Geneve*，思路一模一样，只是外层头更灵活。

### 隐喻对应表

- 公家土路（只认村到村大地址）→ *underlay* 物理网络，只路由宿主机 IP
- 各村内部重名的小名「井边第二家」→ Pod 的虚拟 IP，underlay 上不可路由、可重复
- 系围裙的门房 → *VTEP*（VXLAN 隧道端点，跑在每台宿主机上）
- 里层信（写着小名，门房从不拆改）→ 原始的二层帧 / Pod 发出的包
- 套在外面的大信封（写村到村地址）→ VXLAN 外层封装（MAC-in-UDP，宿主机到宿主机）
- 彩色火漆（红/蓝/绿分行会）→ 24 位 *VNI*，标记并隔离虚拟网段
- 脚夫只看大信封挑担 → 物理网络按外层地址转发，看不见里层
- 目的村门房拆外层、按火漆颜色分拣 → 收端 VTEP 解封装，按 VNI 投递
- 大信封本身占地方 → 封装开销 / MTU 问题
</section>
<section class="en" markdown="1">
On the morning mail route sat a dozen little villages, strung together by a few dirt roads the county had built. Those roads were public—anyone could use them—but they understood only gate-to-gate addresses: "North gate, Village Three" to "South gate, Village Seven," and nothing finer than that.

Here was the snag: inside each village, every household went by an old local nickname of its own making. Village Three had a house called "Second-by-the-Well." So did Village Seven. The locals used these nicknames every day and never thought to change them. But drop a letter addressed to "Second-by-the-Well" straight onto the public road, and the road hadn't the faintest idea which village to carry it to—nicknames mean nothing outside the village walls.

So at each village gate sat a **gatekeeper** in an apron. When a villager wanted to write to another village, they handed over the **inner letter**, nickname and all. The gatekeeper never opened it, never changed a word. He did three things: he slipped the whole inner letter into a big **outer envelope**; on that envelope he wrote the **village-to-village address** the road understood; and finally he pressed a blob of **colored wax**—red wax for the rice guild's mail, blue for the cloth guild's, green for the fishers' guild.

The couriers on the road read only the village name on the outer envelope, and hauled their bundles of wax-sealed envelopes to the destination village. The road never once saw the inner nickname, and didn't care—its only job was carrying things between villages.

At the far gate, that village's gatekeeper took the outer envelope, tore off the outer layer, and first checked the color of the wax: red went to the rice guild's sorting room, blue to the cloth guild's. This way the "Second-by-the-Well" that existed in *both* villages never got confused—the red-wax "Second-by-the-Well" and the blue-wax one traveled the very same road, yet stood as though a wall ran between them. Only once the color checked out did the gatekeeper hand the inner letter, untouched, to the right household in his own village.

—By now you've probably recognized him: this aproned gatekeeper, who only ever wraps one more layer around a letter, is the *VXLAN* tunnel endpoint (*VTEP*) of an overlay network.

### What it is

*VXLAN* (Virtual eXtensible LAN) is an overlay network encapsulation technique. The physical network (the *underlay*, that dirt road) only moves packets between hosts, and it only knows host IPs. Meanwhile the "residents"—Pods, VMs—use their own virtual addresses, which aren't routable on the physical network and may even collide with each other.

A *VTEP* (VXLAN Tunnel Endpoint, the gatekeeper) sits on every host. It stuffs the original layer-2 frame whole into a new UDP packet (MAC-in-UDP), writes host-to-host addresses on the outside, and stamps it with a 24-bit *VNI* (VXLAN Network Identifier, the wax color). With sixteen million possible VNI values, tens of thousands of isolated virtual networks can share one physical fabric without ever seeing one another. The receiving VTEP strips the outer layer and delivers by VNI to the correct virtual segment.

Why it matters: this is the underlying magic that lets Pods in OpenShift/Kubernetes talk to each other on one flat network across nodes. The underlay never needs to know what the Pod subnets look like; communication crosses nodes, racks, even availability zones just fine, and tenants are isolated by VNI for free. The cost is that every packet carries an extra encapsulation header (~50 bytes of overhead), so you must mind your MTU—when the outer packet won't fit, you get fragmentation or drops, the single most common trap in overlay networking. OpenShift's default OVN-Kubernetes uses the close cousin *Geneve*, which works the same way with a more flexible outer header.

### The metaphor, mapped

- The public dirt road (knows only village-to-village addresses) → the *underlay* physical network, routing only host IPs
- The colliding nickname "Second-by-the-Well" in each village → a Pod's virtual IP: not routable on the underlay, possibly duplicated
- The aproned gatekeeper → the *VTEP* (VXLAN tunnel endpoint, running on every host)
- The inner letter (nickname on it, never opened) → the original layer-2 frame / packet the Pod sent
- The big outer envelope (village-to-village address) → the VXLAN outer encapsulation (MAC-in-UDP, host-to-host)
- The colored wax (red/blue/green per guild) → the 24-bit *VNI*, tagging and isolating virtual segments
- Couriers reading only the outer envelope → the physical network forwarding by outer address, blind to the inner frame
- The far gatekeeper tearing the outer layer and sorting by wax color → the receiving VTEP decapsulating and delivering by VNI
- The bulk of the outer envelope itself → encapsulation overhead / the MTU problem
</section>
