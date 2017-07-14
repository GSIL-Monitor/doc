######数据-缓存漫谈（本文不涉及系统的技术实现细节）
#####一、缓存特性以及应用场景
* 数据对互联网而言并不陌生，甚至是每天都需要打交道的部分，根据不同的行业不同的业务，都有不同的数据，而但凡规模增大到一定程度，即使分类后的数据也会庞大到一定的数量级。

* 几百-几万-几百万-亿级别的数据。
当数据量到达一个数量级之后，如何快速检索到用户需要的数据乃至经历过一层层数据逻辑筛选后最终吐露给用户所需的耗时也是开发者需要考虑的问题了。
与数据库访问优化漏斗法则相似，数据的访问优化也在大体上遵循类似的法则。
* ![](http://i.imgur.com/2lhuCDs.png)
* 减少数据访问
* 返回更少数据
* 减少与底层交互次数
* 减少CPU开销和利用更多的机器资源

当所有这些优化手段到达瓶颈，而仍然无法满足当前的业务需求量，我们引入了数据的缓存。
缓存的使用更加贴近于“减少数据访问”这一层。
JAVA开发缓存大致可以分为两种：本地缓存以及集群缓存。
本地缓存的优缺点：
优点：数据直接来源于JVM，访问速度极快，且使用门槛极低。最常见的MAP就可以拿来当本地缓存使用，市场上也有发展成熟的插件以供使用，例如ehcache、oscache、guava cache等(当然ehcache、oscache两者本身已经提供集群方法)。
缺点：服务器集群上会出现各台机器数据不一致的，差异性问题。当本地缓存过大时往往会引发服务器的GC问题。

集群缓存的优缺点：
优点：数据一致性得到保障，且数据扩容方便，对用户透明无感知。比较成熟的中间件有tair、redis、memcached等。
缺点：需要选择成熟的符合业务的集群缓存中间件以及集群框架，搭建并维护一套缓存集群服务器。使用门槛较高。存在缓存热点问题(例如tair热点问题)。

缓存的使用根据数据量的差异以及不同的场景都会有所不同。
这里推荐大家去看外网的一篇文章，讲双11大促的缓存使用：
基于“哨兵”的分布式缓存设计
原文讲的已经比较清楚、条理分明，这里就不再赘述。
简而言之就是三种不同的使用方式：
1、被动式缓存处理：命中则返回，未命中则查询底层，缓存结果集并返回
2、数据全量缓存预热或关键数据缓存预热
3、基于“哨兵”式缓存实现方式

每种不同的实现方式都有不同的代价，第一种最简单，适用于数据量较小的应用场景，实现也最为简便；第二种实现需要比较良好的设计，适用于数据量中等的热点访问应用场景；第三种方式各个层面都可以使用，但是全方位的使用比较考量技术。

######二、本地缓存的瓶颈
* 以上三种不同的缓存使用方式不拘于本地缓存或者集群缓存。无论本地缓存还是集群缓存从这几个方面延伸出去都有很多可深挖的内容，但是本文不会过多的讨论集群缓存的实现，感兴趣的同学可以自行去挖掘学习。
这里讲讲本地缓存的使用瓶颈，JVM的本地缓存，大部分情况下指的都是堆内存。

阿里系大部分常规应用的服务器单机虚拟机标配都是：4核CPU、8G内存。其中8G内存主要分配给两个：JVM以及服务器日常开销。划分给JVM的内存常规会按照以下的方式进行分配：2G年轻代、2G老年代、最大1G堆外内存、几百M的持久代PermSize。
而年轻代又会进一步以10:1:1的比例规划出Eden、so1、so2，即一个so所占大小约170M左右。

有没有人曾经考虑过这样划分的意义在哪里。
如果给年轻代分了3个G呢，会发生怎样的事情？
如果调整年轻代分布，不以10:1:1的方式划分呢，又会发生什么事？

总而言之有人可能会说，这些跟本地缓存有什么关系？
众所周知JAVA的开发不同于C的一个最大的特点在于JAVA的开发人员无需关注内存的申请以及释放，而由JVM的垃圾回收机制进行动态管理。而当JVM的堆内存占用达到一定比例的时候就会触发臭名昭著的GC，也就是STW（stop the world），当JVM发生GC的时候，对应用程序其实是无感知的，也就是说，假如用户来请求数据，缓存命中后正要返回的时候碰上JVM发生GC，整个应用程序会“僵住”，用户的请求也会hold在那里，等到GC结束后一切才会恢复如常，用户能够得到他想要的数据(如果GC没有超过请求的最大超时时间)，程序能够正常运转下去。

这是JVM堆对本地缓存使用影响的第一个点。有人会说，那可以增大JVM堆大小，这样GC发生的周期也就被延长了。

我们先来看另一个问题，JVM年轻代的比例设定对本地内存产生的影响。
目前淘系的服务器JVM垃圾收集采用的都是CMS回收机制。
对应的年轻代回收机制是：标记-复制。（图片来自网络）
* ![](http://i.imgur.com/Y2Mt13s.png)
* 对应的老年代回收机制是：标记-清除(或标记-整理)。（图片来自网络）
* ![](http://i.imgur.com/hqWFP1f.png)
* 至于为什么采用分代收集的方式以及年轻代采用“标记-复制”的方法，在于数据在不同阶段的特性不同。年轻代采用“标记-复制”算法的好处在于数据在年轻代的时候绝大多数的存活周期都不长，类似于方法体里的临时变量一样，使用结束之后都会直接废弃掉。而年轻代在进行YGC之后存活下来的对象寥寥无几，这时候转移这些存活的对象到另一个so里面的代价极低。因此“标记-复制”算法在此处得到了最大效果的使用。对用户产生的影响也微乎其微。
而且相比起YGC，开发人员更加关注FGC，因为FGC产生的停顿耗时一般来说远远超过YGC。如图
* ![](http://i.imgur.com/F1DXS8C.png)
* 这是一段GC耗时的对比，一般情况下YGC的耗时在10ms-200ms左右，而FGC的耗时可能在秒(s)级别。

但是请注意，上面说的情况是，“正常情况”下。你知道当年轻代分布按照10:1:1的比例时，一个so的最大大小也就是170M，经过回收后从so1拷贝到so2的剩余对象远远小于170M，可能就几M，如果调整Eden与so的比例，缩小Eden，增大so呢(例如调整比例到1:1:1)？
So的大小最大就会有682M，经过对象回收后可能存活的对象也会大大增多，从so1拷贝到so2的耗时也会增加，严重情况下会远超秒(s)级，甚至超过FGC带来的影响都有可能。
想象一下你用U盘拷贝电影的场景，几个G的电影从U盘拷贝到硬盘的耗时(当然内存拷贝速度远超硬盘拷贝，但是一个是分钟级别，一个再快，也会是秒级)。如图
* ![](http://i.imgur.com/vxVOZWf.png)
* 这是一个类似问题带来的YGC影响，耗时已经达到FGC的级别。
那你们可能会说，能否调整年轻代的分布比例，让so尽可能的小呢。这样做带来的影响就是，对象从so1拷贝到so2之后会发现不够放，过早的进入老年代，对象过早进入老年代就会过早的触发FGC的发生(大对象会导致老年代产生更多的内存碎片，这些碎片无法得到有效利用，致使提前产生FGC)。因此比例的大小设定是一个数据平衡的问题。10:1:1的比例设定，想必是阿里的前辈工程师经过无数次的实验验证的最优设置方案之一吧(在此致敬)。

年轻代的比例设定会影响本地缓存的存取，本地缓存的使用不当也会反过来影响JVM。也就是我们所说的“大对象”的存在。比如在一个类里申明一个List属性，存放百万级别的数据量，List占整体堆内存大小1G-2G，这种情况下年轻代已经完全不够吃。肯定会引发恶性GC。
这是比较极端的情况下，一般的情况可能这个对象占几十到几百M，这时候因为当做本地缓存使用，在年轻代的存活周期肯定比较长，而如果List大小又没超过so的大小，JVM不会提前把它丢到老年代，那么在年轻代回收的过程中就会重复十几次的so拷贝过程(默认值是15次)，想象一下U盘拷贝电影的场景。这十几次的拷贝就会带来十分恶劣的效果，而这种情况下无论缩小还是扩大Eden与so的比例，似乎都已经无效了。


总结一下以上的内容：
1、年轻代的比例设定不当会影响YGC，使YGC无法达到“短平快”的效果，“标记-复制”算法反而成为雷卓
2、当本地缓存使用过多，占用过大的堆内存时、或者当本地缓存使用不当，存在大对象时。同样会影响YGC的效果，“标记-复制”算法再次成为累赘。

当然老年代的设定不当同样会对本地缓存的使用产生影响（我知道，但是我不说）
至于调大年轻代会带来的影响（我也知道，但是我不说）

* 综上所述，很多开发人员写到最后，往往都会碰到JVM的垃圾回收瓶颈，甚至很多后期的开发都会变成与JVM垃圾回收的对抗甚至是妥协。写一个List的临时变量，for往List里面插入对象这种代码谁都会写。但是当for循环到几百万次，对象的量级到达几千万的时候。再这样写除了引发JVM问题外毫无意义，开发人员不得不进行大对象拆分处理，而于业务上不得不做更多的设计、写很多业务逻辑无关的代码。这就是JVM倒逼开发最典型应用案例。当我们碰到JVM之后，奋力一搏到最后可能无可奈何。是所有程序员都需要思考的一个问题。
Ps：补充一个番外知识，为什么多线程的使用不当会引发GC问题。
JAVA可以用Xmx、Xms设置堆内存大小，广义上的堆外内存是指虚拟机内存除去JAVA堆和永久代之外的部分。包括：
1、Direct Memory：可通过-XX:MaxDirectMemorySize设置
2、线程堆栈：可通过-Xss设置
3、Socket缓存区
4、JNI代码
5、虚拟机和GC：本身虚拟机和GC的代码执行也需要消耗一部分内存

      线程的分配默认是直接在堆外内存进行分配的，Xss也称之为ThreadStackSize(线程的栈空间)，jdk1.4的Xss默认大小是256k，jdk1.5+的Xss默认大小是1M.
      在虚拟机内存有限的情况下，堆内存设置的越大，留给堆外内存的空间就越小，线程的个数受到堆外内存大小的限制也有其上限。当线程数过多，超出堆外内存大小时，内部会调用System.gc()大喊一声通知JVM进行GC回收。如果JVM参数设置了-XX:DisableExplicitGC，该参数的作用相当于使“System.gc”无效化，这时候堆外内存就只能眼睁睁看着自己爆掉，然后抛出：StackOverFlowError或者OutOfMemoryError:unable to create new native thread.一般淘系的服务器会使用-XX:ExplicitGCInvokesConcurrent替代DisableExplicitGC，使原本的FGC变成CMS的并发GC。
      因此线程使用不当是有会导致应用程序频繁GC的。理论上可以通过设置比较小的Xss或者减小堆内存的大小来提升线程的个数，看具体情况而定。

Pss：可以指定-XX:+UseTLAB参数来设定线程直接在堆内存进行分配，TLAB(Thread Local Allocation Buffer)本地线程分配缓冲。该参数会直接在堆内存分配一块空间大小。

##### 三、堆外内存初探以及展望
* 本地缓存的使用要点：
1、避免大对象的产生，尽量拆分大的对象
2、避免过多的本地缓存
3、适量调整JVM配置以适应当前项目特性

以上3点，说起来简单，事实上实践起来却没那么容易。很多业务流程里拆分大对象本身就不是简单的事情，因为拆分就要考虑动态扩容的问题，而且需要为相同类型的数据做更多的设计，带来结构上的复杂性无可避免。鱼与熊掌不可兼得。
然而JVM又是开发无论如何也绕不开的一个问题。于是就有开发把目光着眼于JVM非堆内存上，因为非堆内存是没有自己的垃圾回收机制的，往往数据的回收都是在GC发生的时候一并回收。因此将数据存放在非堆内存似乎是一个完全可以绕开JVM-GC，同时又不用增加设计复杂性的一个好办法。
这里推荐一篇infoQ上的好文，撰写者深入探讨了本地缓存的进化历史以及将来。个人觉得值得一读：
新的实用的非堆技巧

事实上我后期研究了下市场上成熟的非堆内存使用，也发现了不少成熟的产品。然而无论哪种产品都没有代码的无缝接入方式，而需要长期的实验乃至多方配合才有可能达成一个比较好的结果。以下罗列出JAVA非堆内存的一些比较成熟的中间件。
1、MapDB：
优点：提供成熟的API，对数据的操作达到原子级别，提供类似sql的提交以及数据回滚的。
缺点：数据无法以普通MAP的形式常驻内存，每次从非堆中取都需要提前与非堆进行一个connect，取出之后必须commit才能生效，且读取效率对比普通的concurrentHashMap没有优势(除非堆的GC已经严重拖慢了HashMap的读取)。
2、Imcache
3、SharedHashMap
4、EHcache：只有企业版支持非堆内存
5、BigMemory：免费版有使用期限，企业版有成熟的应用。收费

所有以上的非堆内存都有类似的如下限制：
1、需要的非堆内存大小比较多，大部分在4G以上
2、数据存储在非堆内存，虽然可以完全避开GC带来的影响，但是开发人员需要关注非堆内存数据溢出的问题
3、数据存储在非堆内存，当客户端访问时，数据从非堆内存取出到堆内存，然后才能返回给客户端。因此取数据的过程仍然需要占用一部分堆内存大小。
4、大部分非堆内存都需要应用程序拥有某个服务器文件目录的读写权限，非堆内存数据其实是存储在这个文件里头的。

以上，我相信非堆内存是本地缓存系统的未来。而所有的本地缓存系统都需要经历大内存的堆实战以及大内存的非堆实战。4G内存实在太小，淘系的本地缓存系统最大的已经有到40G大小的项目。