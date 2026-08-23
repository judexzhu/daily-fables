---
layout: fable
title: "藏书阁的撤架清单 · The Archive's Withdrawal Checklist"
title_zh: "藏书阁的撤架清单"
title_en: "The Archive's Withdrawal Checklist"
concept: "finalizers and cascading deletion"
tags: [kubernetes]
illustration: /assets/art/2026-07-07-finalizers-cascading-deletion.jpg
---
<section class="zh" markdown="1">
临溪阁是城里最大的藏书楼，楼里最贵重的，是几套流传百年的《地方志》全集——每一套都是"母书"，母书之下又分装着几十册"分卷"，分卷各自单独上架，方便读者借阅。

藏书楼有一条铁律：任何一本书，想从"总目录"里彻底撤下，绝不能说撤就撤。哪怕馆长亲口下令"这套书不要了"，目录卡片也不会立刻消失——它先被盖上一枚红戳，写着"拟撤"，卡片照旧摆在抽屉里，任何人一查，都能看到"这本正在等着被撤"。

真正让它消失的，是卡片背面那张_待清单_。这张清单上列着几件事，每件事归不同部门管：账房要来核销这本书的采购成本；借阅处要来确认眼下没有读者正捧着它没还；装裱坊要来收回外借出去修复的封皮。每完成一件，对应的部门就在清单上划掉自己那一行。规矩很死：哪怕只剩最后一行没划掉，这张卡片也绝不会真的从抽屉里抽走——它就那样带着红戳，不尴不尬地留在那儿，馆长催也没用，馆长自己也划不了别的部门该划的那一行。

有一回账房迟迟没来核销——原来管账的师傅告假半年，没人替他划那一笔。那本书的卡片就这样"拟撤"了小半年，既撤不掉，也用不了，读者来借，馆长只能尴尬地说"它在撤架的路上，再等等"。

母书撤架时，还有个更棘手的问题：底下几十册分卷怎么办？临溪阁定了三条路子由馆长挑选。第一条，_连撤_——先把所有分卷也一并盖上拟撤戳，等分卷们的待清单一本本划完、真正从抽屉里抽走了，母书自己那张卡片才最后消失，一套书干干净净、里外一起没了影。第二条，_先撤母书，后台慢慢清分卷_——母书卡片立刻抽走，分卷们各自留着，由几个学徒在后面不紧不慢地把它们逐一按各自的待清单处理掉，读者暂时还看不出母书已经没了，但迟早会清完。第三条，_放它们自由_——母书一撤，分卷统统解除隶属，谁也不跟着走，依旧单独立在架上，成了没有母书的散册，自生自灭。

——到这儿你大概已经认出来了：那张写着"拟撤"的卡片和背面的待清单，讲的其实是 Kubernetes 里对象删除的机制——_deletionTimestamp_ 与 _finalizer_；母书和分卷之间的隶属关系，则是 _ownerReference_ 与三种级联删除策略：_Foreground_、_Background_、_Orphan_。

_概念解释_
在 Kubernetes 里，删除一个对象（比如一个 Namespace、一个自定义资源）从来不是"说删就删"。当你执行 `kubectl delete`，API server 并不会立刻把对象从 _etcd_ 里抹掉，而是先给它打上一个 _deletionTimestamp_，对象状态进入 _Terminating_。真正阻挡它彻底消失的，是它 metadata 里的 _finalizers_ 列表——每一个 finalizer 都代表某个 controller 承诺"删除前我要先做完清理工作"（比如释放云上的 LoadBalancer、卸载存储卷、核销外部计费记录）。只有当所有相关 controller 都完成自己的清理、并主动把自己的名字从 finalizers 列表里移除，API server 才会真正把对象从存储里删掉。这也是线上最常见的故障之一——_stuck Terminating_：某个该来移除 finalizer 的 controller 挂了、被卸载了，或者压根不存在了，finalizer 永远留在列表里，对象就永远卡在 Terminating，怎么删都删不掉，只能人工去 `kubectl edit` 把那一项 finalizer 硬摘掉。

至于母书和分卷，对应的是 _ownerReference_：子对象（比如 ReplicaSet 之于 Deployment，Pod 之于 ReplicaSet）在 metadata 里记着自己的 owner 是谁。删除 owner 时，_garbage collector_ 依据 `propagationPolicy` 决定子对象的命运：_Foreground_ 会先给所有子对象也打上 deletionTimestamp，等它们真正被删除完，owner 自己才最后消失（整棵树"看得见"地一起没）；_Background_（也是默认策略）则是 owner 立刻被删，子对象交给 GC 在后台异步清理，期间可能会有短暂的"母对象没了、子对象还在"的中间态；_Orphan_ 则是彻底解除关系，子对象保留下来，变成没有 owner 的独立对象。

_隐喻对应表_

- 《地方志》母书 → 拥有子对象的父资源（如 Deployment）
- 分装的分卷 → 子对象（如 ReplicaSet、Pod）
- 目录卡片盖上的"拟撤"红戳 → _deletionTimestamp_，对象进入 _Terminating_
- 卡片背面的待清单 → _finalizers_ 列表
- 账房核销、借阅处确认、装裱坊收回封皮 → 各个 controller 各自认领的 finalizer 清理逻辑
- 划掉清单上自己的一行 → controller 完成清理后，主动移除自己的 finalizer
- 账房师傅告假，那一行永远划不掉 → 对应 controller 挂了/缺失，finalizer 卡住，对象永久 _stuck Terminating_
- "连撤"：分卷先撤完，母书卡片才消失 → _Foreground_ 级联删除
- "先撤母书，后台慢慢清分卷" → _Background_ 级联删除（默认策略）
- "放它们自由，分卷独立留架" → _Orphan_ 策略，解除 ownerReference，子对象保留
</section>
<section class="en" markdown="1">
Linxi Pavilion is the grandest archive in the city. Its most prized holdings are several century-old "Gazetteer" collections — each collection a "parent volume," bound around dozens of individually shelved "sub-volumes," each of which patrons could borrow on its own.

The archive had one iron rule: no book could simply be struck from the master catalog on command. Even if the head librarian personally declared "we're done with this set," the catalog card wouldn't vanish the instant he said so. It would first be stamped in red ink — "pending withdrawal" — and left right there in the drawer, plain for anyone to see: this one is waiting to be withdrawn.

What actually made it disappear was a _checklist_ pinned to the back of the card. The list named several tasks, each owned by a different department: the accounting office had to write off its acquisition cost; the circulation desk had to confirm no patron currently held it unreturned; the bindery had to reclaim any cover that had been sent out for repair. Each department, upon finishing its part, crossed off its own line. The rule was absolute: as long as even one line remained uncrossed, the card would never actually be pulled from the drawer — it would just sit there, red stamp and all, in limbo. The head librarian could complain all he liked; he couldn't cross off another department's line himself.

One time, accounting simply never showed up — the clerk in charge had taken half a year's leave, and no one covered his line. That book's card sat "pending withdrawal" for nearly six months: it couldn't be withdrawn, and it couldn't be lent either. When a patron came asking, the librarian could only say, awkwardly, "it's on its way out — please wait."

Withdrawing a parent volume raised a thornier question: what happens to its dozens of sub-volumes? Linxi Pavilion offered the librarian three paths. The first, _withdraw together_ — stamp every sub-volume "pending" too, and only once each sub-volume's own checklist is fully crossed off and each is truly pulled from its drawer does the parent's card finally vanish, the whole set disappearing cleanly, inside and out, together. The second, _withdraw the parent first, clean up the children in the background_ — the parent's card is pulled immediately, while the sub-volumes are left in place for a few apprentices to work through at their own pace, each according to its own checklist; patrons won't notice right away that the parent is gone, but eventually every sub-volume gets cleared too. The third, _set them free_ — the moment the parent is withdrawn, every sub-volume's affiliation is simply severed; none of them follow it out, they stay right where they are on the shelf, now parentless volumes left to fend for themselves.

By now you've probably recognized it: that card stamped "pending withdrawal," with its checklist on the back, is describing Kubernetes's object-deletion machinery — _deletionTimestamp_ and _finalizers_. And the relationship between parent volume and sub-volumes is _ownerReference_, along with the three cascading-deletion strategies: _Foreground_, _Background_, and _Orphan_.

_What this is_
In Kubernetes, deleting an object — a Namespace, a custom resource, whatever it is — is never as simple as "delete on command." When you run `kubectl delete`, the API server doesn't immediately erase the object from _etcd_. Instead it stamps the object with a _deletionTimestamp_, and the object enters the _Terminating_ state. What actually blocks it from disappearing for good is the _finalizers_ list in its metadata — each finalizer represents some controller's promise that "I need to finish my own cleanup before this object can really go" (releasing a cloud LoadBalancer, detaching a storage volume, writing off an external billing record, and so on). Only once every relevant controller has finished its cleanup and voluntarily removed its own name from the finalizers list will the API server actually delete the object from storage. This is also the source of one of the most common production headaches: _stuck Terminating_ — some controller that was supposed to remove its finalizer has crashed, been uninstalled, or simply no longer exists. Its finalizer just sits there forever, and the object is stuck in Terminating indefinitely, resisting every deletion attempt, until someone manually runs `kubectl edit` and forcibly strips the finalizer out.

As for the parent volume and its sub-volumes, that's _ownerReference_: a child object (a ReplicaSet under a Deployment, a Pod under a ReplicaSet) records who its owner is in its own metadata. When the owner is deleted, the _garbage collector_ decides the children's fate based on `propagationPolicy`: _Foreground_ first stamps every child with a deletionTimestamp too, and only once they've all actually been deleted does the owner itself finally vanish — the whole tree disappears together, visibly, in order. _Background_ (the default) deletes the owner immediately and hands the children off to the GC for asynchronous cleanup, which can leave a brief in-between state where the parent is gone but children are still around. _Orphan_ severs the relationship entirely — the children are left behind, becoming independent objects with no owner at all.

_Metaphor mapping_

- The "Gazetteer" parent volume → a parent resource that owns children (e.g. a Deployment)
- The bound sub-volumes → child objects (e.g. ReplicaSet, Pod)
- The red "pending withdrawal" stamp on the catalog card → _deletionTimestamp_, the object entering _Terminating_
- The checklist on the back of the card → the _finalizers_ list
- Accounting's write-off, circulation's confirmation, the bindery's cover reclaim → the cleanup logic each controller owns as its own finalizer
- Crossing off one's own line on the checklist → a controller finishing cleanup and voluntarily removing its own finalizer
- The accounting clerk on leave, that line never crossed off → the responsible controller crashed or is missing, the finalizer stuck, the object permanently _stuck Terminating_
- "Withdraw together": sub-volumes go first, then the parent's card disappears → _Foreground_ cascading deletion
- "Withdraw the parent first, clean up children in the background" → _Background_ cascading deletion (the default)
- "Set them free, sub-volumes stay on the shelf independently" → the _Orphan_ policy, severing ownerReference, children retained
</section>
