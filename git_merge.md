######git Merge 原理算法
* 最近碰到一系列问题，正好求知所问深入学习了下git 内部原理，东西比较多，先从git merge 说起，因为merge是所有版本控制系统中最最核心之一，本文通过讨论是2个commit 之间的合并 类似git merge C1 C2 ，更多的 git merge C1 C2 C3 ..Cn-1,Cn 合并也是一样的，他们主要是先将Cn 和 Cn-1先合并然后从后往前在递归合并所有。
######merge 常见误区
* git merge 是用时间先后决定merge结果的，后面会覆盖前面的?

答 ：git 是分布式的文件版本控制系统，在分布式环境中时间是不可靠的，git是靠三路合并算法进行合并的。

git merge 只要两行不相同就一定会报冲突，叫人工解决?

答：git 尽管两行内容不一样，git 会进行取舍，当git无法进行取舍的时候才会进行人工解决冲突

#####merge 基础
* 我们知道git 合并文件是以行为单位进行一行一行进行合并的，但是有些时候并不是两行内容不一样git就会报冲突，因为smart git 会帮我们自动帮我们进行取舍，分析出那个结果才是我们所期望的，如果smart git 都无法进行取舍时候才会报冲突，这个时候才需要我们进行人工干预。那git 是如何帮我们进行Smart 操作的呢？
#####二路合并
* 二路合并算法就是讲两个文件进行逐行对别，如果行内容不同就报冲突。
* 图1 ![](http://i.imgur.com/OHwRNOl.png)
* Mine 代表你本地修改
Theirs 代表其他人修改
假设对于同一个文件，出现你和其他人一起修改，此时如果git来进行合并，git就懵逼了，因为Git既不敢得罪你(Mine)，也不能得罪他们(Theirs) 无理无据，git只能让你自己搞了，但是这种情况太多了而且也没有必要...

#####三路合并
* 三路合并就是先找出一个基准，然后以基准为Base 进行合并，如果2个文件相对基准(base)都发生了改变 那git 就报冲突，然后让你人工决断。否则，git将取相对于基准(base)变化的那个为最终结果。
* 图2 ![](http://i.imgur.com/yazR6Z9.png)
* Base 代表上一个版本，即公共祖先
Mine 代表你本地修改
Theirs 代表其他人修改
这样当git进行合并的时候，git就知道是其他人修改了，本地没有更改，git就会自动把最终结果变成如下，这个结构也是大多merge 工具的常见布局，比如IDEA
* 图三 ![](http://i.imgur.com/eEg3JnR.png)
* 如果换成下面的这样，就需要人工解决了
* 图4 ![](http://i.imgur.com/lwOvuQo.png)
* 上面就是git merge 最基本的原理 "三路合并"。

下面面的合并就是我们常见的分支graph,结合具体分析。
* 图5 ![](http://i.imgur.com/hKSTfdY.png)
* 上面①~⑨代表一个个修改集合(commit)每个commit都有一个唯一7位SHA-1唯一表示。
①，②，④，⑦修改集串联起来就是一个链，此时用master指向这个集合就代表master分支，分支本质是一个快照，其实类比C中指针
同样dev分支也是由一个个commit组成
现在在dev分支上由于各种原因要运行git merge master需要把master分支的更新合并到dev分支上，本质上就是合并修改集 ⑦(Mine) 和 ⑧(Theirs) ，此时我们要 利用DAG(有向无环图)相关算法找到我们公共的祖先 ②（Base）然后进行三方合并，最后合并生成 ⑨

git merge-base --all commit_id1(Yours/Theirs) commit_id2(Yours/Theirs) 就能找出公共祖先的commitId(Base)

现实总是残酷的，其中中分支的Graph并不像这样简单有序，看下面。
* 图6![](http://i.imgur.com/VMZMkJt.png)
* 图虽然复杂 但是核心原理是不变的，下面我们看 另外一个稍微高级一点的核心原理"递归三路合并" 也是我们很常见看到 git merge 输出的 recursive strategy
######递归三路合并
* 下图中我们如果要合并 ⑦(source) -> ⑥(destination):
* 图7 ![](http://i.imgur.com/JFjkKqC.png)
* 简短描述下 如何会出现上面的图：

在master分支上新建文件foo.c ,写入数据"A"到文件里面
新建分支task2 git checkout -b task2 0,0 代表commit Id
新建并提交commit ① 和 ③
切换分支到master，新建并提交commit ②
新建并修改foo.c文件中数据为"B",并提交commit ④
merge commit ③ git merge task2,生成commit ⑥
新建分支task1 git chekcout -b ④
在task1 merge ③ git merge task2 生成commit ⑤
新建commit ⑦，并修改foo.c文件内容为"C"
切换分支到master上，并准备merge task1 分支(merge ⑦-> ⑥)
从上面我们DAG图可以知道公共祖先有③和④，那到底选择哪个呢，我们分别来看：

如果选择③作为公共祖先 根据最基本的三路合并，可以看到最终结果⑧ 将需要手动解决冲突 /foo.c = BC???
* 图8 ![](http://i.imgur.com/1s0MkEc.png)
* 如果选择④作为公共祖先 根据最基本的三路合并，可以看到最终结果⑧ 将得到 /foo.c=C
*  图9 ![](http://i.imgur.com/wBCkSKc.png)
*  最终期待的结果是什么？

我们在Master上也是所有分支的起点定义了 /foo.c = A,在task2 分支上并没有进行任何修改。
最初修改 /foo.c = B 是在master 分支上，修改集④ 上修改为 /foo.c = B
第一次通过 ③，④ 合并生成 ⑥， 最终使得Master分支上 ⑥ /foo.c = B
第二次通过 ③，④ 又合并生成 ⑤， 最终使得task1分支上 ⑤ /foo.c = B
在task1分支上不希望 /foo.c = B ,所以在task1上新建一个⑦ /foo.c = C
我们知道 foo.c = B 是在 master分支上 ④ 进行修改的，其他的/foo.c = B 都是来自④这次修改。
我们能从图上可以知道 ⑦ 的修改一定是在 ④ 之后的，并不是因为⑦ > ④ 而是 ④ 是 ⑦ 的祖先节点，所以我们知道最终的修改合并之后就应该保留 /foo.c = C
所以 我们的最佳公共祖先应该是4，最终结果应该是 /foo.c = C
git 如何选择公共祖先呢？

你可能会说用 git merge-base ⑥ ⑦ 输出的是 ④ 但是git 就真的是用 ④ 做祖先吗 ？答案是No

When the history involves criss-cross merges, there can be more than one best common ancestor for two commits. For example, with this topology:

       ---1---o---A
           \ /
            X
           / \
       ---2---o---o---B
both 1 and 2 are merge-bases of A and B. Neither one is better than the other (both are best merge bases). When the --all option is not given, it is unspecified which best one is output.

从git的解释中，我们就知道 如果有2个都是最佳公共祖先时候，这个时候git 会随便输出一个不确定公共祖先。

git 是这样进行合并的：

* 图10 ![](http://i.imgur.com/ck6jPfr.png)
* git 既不是直接用③，也不是用④，而是将2个祖先进行合并成一个虚拟的 X /foo.c = B, 因为③ 和 ④ 公共祖先是 〇/foo.c = A
git 用 X 做为 base 合并 ⑥ 和 ⑦ 结果就是 /foo.c = C
那什么又叫递归(recursive)合并呢 ? 我们合并 ⑥ 和 ⑦ 的时候，我们将其 2 个公共祖先③ 和 ④ 进行 merge 为 X ,在合并 ③ 和 ④时候 我们又需要找到 他们的公共祖先，此时可能又有多个公共祖先，我们又需要将他们先进行合并，如此就是递归了 也就是 recursive merge，如下：

* 图11![](http://i.imgur.com/qKZ8Ocn.png)
######Fast-Forward
* Fast-Forward 翻译为快速前进，很多时候我们在找2个修改集合X,Y 公共祖先的时候，会发现公共祖先就是他们中的一个，此时我们进行merge 的时候，就是Fast-Forward即可，不会产生一个新的Commit 用于merge X和Y 。如下
* 图12![](http://i.imgur.com/HNmVqKv.png)
* 当merge ② 和 ⑥时候 由于②是公共祖先，所以进行Fast-Forward 合并，直接指向⑥ 不用生成一个新的⑧进行merge了。