#####文件系统之读写高级篇 

#####文件系统读写效率
* 文件系统IO函数， wrtie read都会是系统调用，有系统调用，就会有上下切换，在一般系统里，上下文切换不会有什么影响，但在要求高性能的场景下，上下文切换是一笔不少的开销。

对文件系统来说，提高性能的一种办法就是可以减少系统调用 ，进而减少了上下文切换，提高性能。

文件系统IO系统调用，除了有上下文切换外，还有额度的数据复制开销，如从用户态的数据复制到内核态，或反之，从内核态数据复制到用户态。

这篇文章主要从讲如何优化文件系统IO。


######文件系统Page Cache
* 当文件的数据，被读入到内存里，会有一个Page Cache与之对应，Page Cache就是文件的数据在内存中的表示。当然，这些Page Cache 本身也会一定的开销。

如果系统调用IO需要对这个文件就行read或write调用，只需要直接对对应的 Page Cache进行相应的操作就好了。只有logic I/O,没有Physical I/O,也就没有磁盘的机械操作，性能自然就好了。

如果是第一次读取，或Page Cache被替换掉时，内存中不存在对应的Page Cache，还是会有Physical I/O，但第二次再使用，可以使用已经在内存中的Page Cache。

Linux系统还有一个特性，会把读到的数据都会缓存起来，直到内存不够，才会进行Page Cache替换。

所以使用free命令查看内存使用情况，会发现内存基本被使用了，基本没内存可用，可实际不是这样的，因为有需要，系统会自动进行内存回收，重新供程序使用。

######文件系统Buffer Cache
* 磁盘块中的数据是缓存在一个叫Buffer Cache里，是磁盘块的数据在内存中的映射。和Page Cache类似，都是为了提高IO性能。

######Page Cache与Buffer Cache关系
######先说下历史
* 其实操作系统最早只有Buffer Cache，没有Page Cache。80年代操作系统的书，只有介绍Buffer Cache，没有介绍Page Cache的。

后来有人认为，把缓存往高一层次抽象会更好，在文件系统之上，这样与操作系统另一组合内存管理结合更紧密。

所以Buffer Cache与 Page Cache ，是人们在不同时期，对实现、性能追求不同的产物。
###### 为什么还保留Buffer Cache
* 其实磁盘上数据，大多数情况下，存储的也是文件数据。所以文件数据，在操作系统会缓存在两这个地方，不仅带了内存的开销的，也带了两份缓存数据保持一致性的开销。

既然如此，为什么还要两个都要。因为磁盘上的数据，不全是文件数据。有可能是Raw IO产生的数据，所谓的 Raw IO就是不需要经过文件系统，而是直接操作磁盘。
######优化
* 前面解释了为什么会有Buffer Cache和Page Cache,既然都必须存在，但一份数据会缓存在两个地方，带来了额度的空间开销和维护数据一致性的开销。应该怎么优化呢？

人们想到了Buffer Cache和Page Cache可以共用一份缓存，也就是说Buffer Cache缓存是通过Page Cache来实现。这两个东西共用一份Page Cache，这样就解决了额外空间开销和维护数据一致性的开销。
在内核版本2.4以后，就使用了此优化。

优化之前的关系图
* 图1 ![](http://i.imgur.com/5DDkpaG.png)
* 优化之后的关系图
* 图2![](http://i.imgur.com/lMD63ma.png)
#######文件系统预读
* 如果是顺序读以文件，应用程序调用 read可能只读4KB数据，但文件系统会会从磁盘读取更多的数据，也就是会从磁盘读取到内核的数据是大于4KB。这就是预读。当下次再读，就可以直接返回了。
当然这个预读针对顺序读优化，如果是随机读，可能还会有额外的开销，因为多读取了无用的数据。

看下预读的图
* 图3 ![](http://i.imgur.com/kPGcHtG.png)
#######文件系统之高级读写
* map
* 函数参数

* 图4![](http://i.imgur.com/R1GP3N9.png)
* 详细用法参见 man 2 mmap。
#######原理
* 通过mmap系统调用，把一个文件映射到进程虚拟址址空间上。也就是说磁盘上的文件，现在在在系统看来是一个内存数组了，这样应用程序访问文件就不需要系统IO调用，而是直接读取内存。

调用mmap时，文件数据并不会马上读取内存，还是在实际需要时，才会按需调度进内核。

调用mmap
* 图5![](http://i.imgur.com/HG1QEoz.png)
* 使用mmap还可以实现多外进程间共享数据，多个进程读取时，是无害的，但有一个进程要写时，通过Copy On Write(COW)来实现。

COW图
* 图6 ![](http://i.imgur.com/Bc6taip.png)
* Java如何使用mmap技术

 randomAccessFile = new RandomAccessFile(file, "rw");
 fileChannel = randomAccessFile.getChannel();
 mappedByteBuffer = fileChannel.map(MapMode.READ_WRITE, 0, file.length());
#######缺点
* 前面说了优点，有没有缺点的，答案是肯定的。

内存映射始终是Page Size大小的整数倍，对于小文件，会造成内存浪费
有额外的管理开销
所以一般情况下，针对大文件，mmap性能效果更好
#######Zero Copy
* 可能大家都听过Zero Copy,Zero Copy是什么，为什么都说性能更好呢？

函数签名

sendfile(socket, file, len);
在Linux内核2.4以后，提供更高效的sendfile系统调用，来实现Zero Copy。

普通的调用过程
* 图7 ![](http://i.imgur.com/NDwLB6Z.png)
* 从图中可以看出

read调用会产一次从磁盘上文件通过DMA Copy技术把数据读到内核中，如图中标1所示。把数据从内核通过CPU Copy复制到用户态，如图标2所示。

write调用会产生一个从用户态通过CPU Copy复制到内核态，如图中标3所示。内核在适应的时候，会把数据通过DMA Copy复制到磁盘上，如图中标4所示。

图的红线，每进出一次红线 ，都会产生一次上下文切换。

所以普通的read， write会产生4次上下文切换，2次CPU Copy，2次DMA Copy。

因为CPU是计算机的宝贵资源，所以CPU Copy CPU时间开销是很大的，而DMA Copy是异步的，事件通知机制，所以开销很小。
######mmap调用过程
* 图8 ![](http://i.imgur.com/EwQXFqc.png)
* map调用会产一次从磁盘上文件通过DMA Copy技术把数据读到内核中，如图中标1所示。

然后数据就可以和其它进程共享了。

刷新数据调用会产生一个从用户态通过CPU Copy复制到内核态，如图中标2所示。

图的红线，每进出一次红线 ，都会产生一次上下文切换。

所以普通的mmap会产生2次上下文切换，1次CPU Copy，2次DMA Copy。

相对普通的read 、write而言，mmap减少上下文切换和CPU Copy,所以性能比普通的IO要好。

######sendfile调用过程
* 图9 ![](http://i.imgur.com/Gmk7QU6.png)
* sendfile会产生2次DMA Copy，2次上下切换，没有CPU Copy。

前面也介绍了，CPU Copy是比较浪费CPU时间的，而sendfile 没有CPU Copy。

站在内核的角度，没有CPU Copy，所以也就称为Zero Copy。
######Netty 的Zero Copy
* Netty是Java 世界一款高性能的网络框架，基本已经统一了Java开源高能的网络框架。

更多详细的介绍请看我2014年为双11优化Notify性能的文章，Notify优化性能及Netty实践。

我们看看Netty是怎么给用户提供高性能的Zero Copy技术的。

核心代码如下，为了简洁，去掉了异常处理代码。

 @Override
    public long transferTo(WritableByteChannel target, long position) throws IOException {
        long count = this.count - position;
        long written = file.transferTo(this.position + position, count, target);
        return written;
    }
######总结
* 正因为Zero Copy可以提高性能，所以很多产品都很了此技术。如集团的消息中间件MetaQ就大量此文章介绍的技术。
了解完原理，可以看看消息中间件MetaQ高性能原因分析一些实践。


