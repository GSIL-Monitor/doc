#####Notify优化性能及Netty实践
#####Notify优化性能及Netty实践
* 每年双11,Notify承载了交易、库存、物流等核心链路消息。

Notify的性能至关重要，不仅直接影响发送方RT、消息投递是否延迟,还直接影响Notify的机器数量和运维成本

针对性能优化，我们做了很多工作且效果很明显。 

先看看今年和去年的数据对比

年份	峰值QPS	虚拟机数量
2013	490091	2350
2014	1742801	2993
从表中可以看出消息数量比去年多了3.5倍，但机器数量仅多了600.

下面就介绍下我们对Notify主要做了三个优化措施

网络层重构
订阅关系优化
Server稳定性

######网络层重构
* 性能及连接数问题
在高连接数下，压测试发现Notify有性能问题且无法导致更高的网络连接，

对内存dump分析，分析发现网络连接占用了很多内存，导致fgc频繁.

如果不优化，Notify无法支持今年异地双活项目所需要的连接数，更别说双11的高连接数和性能的需求。

问题分析
Notify网络架构原来是使用是自己开发的网络框架Gecko,Gecko默认为每个网络连接分配64KB的内存，支持1000个网络连接，就需要大概64MB的内存。
同时Gecko性能也还有一些问题,没有Netty性能好。
Gecko网络框架年久失修，维护成本高。虽然没有bug,但想提高性能也不是易事。
解决办法
我们决定使用Netty来重构Notify来重构网络层

Netty是什么

Netty is an asynchronous event-drivennetwork application framework for rapid development of maintainable high performance protocol servers & clients(摘自官网)
为什么选择Netty

Netty性能确实比较高
社区活跃
简单、易用
升级成本小，可维护性好
Netty线程模型

Netty4
在Netty4是采用 Reactor Pattern线程模型,
所谓Reactor Pattern模型是IO multiplexing event loop,Reactor负责处理所有IO事件，同时dispatching各IO事件的handler

在Netty中,有一个Boss线程池和Woker线程池,boss线程池主要处理socket accept事件，worker线程主要处理socket读写事件

Boss线程在处理完socket accept请求(实际也只有一个NioEventLoop在处理accept事件)后，向Worker线程池取出一个NioEventLoopIO线程(默认轮询策略)注册刚建立连接的Channel(socket连接),此Channel生命周期内的所有读写事件都由其注册的NioEventLoopIO线程负责。

关系如图所示
* 图1 ![](http://i.imgur.com/J36BwFY.png)
* 通过此模型，不仅可以使用事件驱动来提高性能，也减少了Channel的并发问题，因为都由一个NioEventLoop负责，不存在并发问题。

Netty3 
Netty3的线程模型和Netty4很像，不同之处在于downstream(outbound)IO事件处理是由调用的线程来处理的。

这样可能带来不必须要锁竞争和上下文切换。

仅从线程模型上看，Netty4比Netty3优化了不少。

Netty重要类型

Channel Netty自己对socket的统一封装
ChannelPipeline ChannelInboundHandler 、 ChannelOutboundHandler 关联
NioEventLoop 处理IO事件的线程
EventThreadGroup 一组NioEventLoop 即Boss、work线程池
* 图2 ![](http://i.imgur.com/yWX8qRl.png)


######Netty实践
* Notify使用的Netty配置
* 图 3 ![](http://i.imgur.com/5OT18B2.png)
######为什么使用堆外内存
* 图4 ![](http://i.imgur.com/nDwWEZO.png)
* 使用堆积外内存，可以减少堆内存开销
在Notify场景中，性能会提高15%左右
4.1及以后的版本，Netty已默认使用,说明已够的稳定
但在4月份我们决定用时，还是一定的稳定性风险,但带来的收益也很明显。

Channel几个重要方法Channel.write 、Channel.flush 、Channel.writeAndFlush
* 图5 ![](http://i.imgur.com/da5NR4Z.png)
* 如果可以尽量使用channel.voidPromise(),减少不必要的对象创建

write方法仅仅是写到Netty的缓存数组中，并不会写到相应的socket
flush 方法把在Netty缓存数组中的数据，flush到相应的socket
writeAndFlush是前面二个方法合并缩写，写到缓存中同时flush底层socket
如果可以应该多次write,最后一次flush ,这样会减少系统调用。
但如果写得太慢，对端处理太慢，有gc频繁或oom的风险，怎么解决？

Channel高低水位线

通过配置WRITE_BUFFER_HIGH_WATER_MARK、WRITE_BUFFER_LOW_WATER_MARK阀值, 当Channel缓存大小超过WRITE_BUFFER_HIGH_WATER_MARK时，Channel.isWritable返回false,写之前可通过此参数决定是否写入.
如果Chanel的水位比较高，表示对端处理很慢.

当Channel缓存内存低于WRITE_BUFFER_LOW_WATER_MARK时,相应的handlerrchannelWritabilityChanged方法 会被回调。

Notify的配置
* 图6 ![](http://i.imgur.com/FUoY5Vw.png)
* Notify在给订阅组投递消息时，先检查此订阅组的Channel是否超过最高位，如果是，则此次不投递，如果不是，继续投递。
* 图7 ![](http://i.imgur.com/y0FFyYe.png)
* 在双今年11中就发挥了作用,日志中发现有集群就遇到的类似的问题，但通过使用此配置，即使在订阅组有问题的情况，最大程度保持了Notify Server自身的稳定。

因为Notify的订阅组太多且订阅组稳定性没法保证，为了保护Notify Server自身的稳定性，必须配置此选项。 如果你的应用有类似的风险，强烈建议配置上此参数。
######Netty支持zero copy
* 图8 ![](http://i.imgur.com/Vkhs9UF.png)
* 比如metaq就充分利用此技术，来减少数据copy、上下文切换，提高性能

######Netty共享handler
* Notify中使用，通过@ChannelHandler.Sharable注解来配置
* 图9 ![](http://i.imgur.com/8NUtPjc.png)
* 如果你的hadnler是无状态，也就是线程安全的，完全可以使用此技术，来减少对象数量

