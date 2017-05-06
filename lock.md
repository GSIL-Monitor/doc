### JAVA资源竞争问题
####Java锁
 * 锁产生的原因： 多线程需要共享访问资源（实例资源， Class资源， 其他共享资源）
 * JUC--java.util.concurrent 包
   * 基于CAS 模式进行封装 unsafe（this,offset, expect, update） nataive方法 

##### synchronized（对象锁，类锁）

* Java的所有对象都含有1个互斥锁，这个锁由JVM自动获取和释放。线程进入synchronized方法的时候获取该对象的锁，当然如果已经有线程获取了这个对象的锁，那么当前线程会等待；synchronized方法正常返回或者抛异常而终止，JVM会自动释放对象锁。

* 对象锁
  * synchronized（this）{}
  * synchronized 修饰类非静态方法

* 类锁
  * synchronized(Class.calss){}
  * synchronized 修饰静态变量 || 静态方法

* 区别： 
  * 类锁和对象锁不是同1个东西，一个是类的Class对象的锁，1个是类的实例的锁。也就是说：1个线程访问静态synchronized的时候，允许另一个线程访问对象的实例synchronized方法。反过来也是成立的，因为他们需要的锁是不同的。 

* 优势    
  * 1.6 之后被优化， 性能大大提升（使用不用考虑性能问题）
  * 用synchronized来加锁的1个好处，方法抛异常的时候，锁仍然可以由JVM来自动释放。

#####可重入锁
* 

####JAVA信号量
* 

####不得不说的问题 AQS（AbstractQueuedSynchronizer）
* [参考文章链接地址](http://blog.csdn.net/pfnie/article/details/53191892)  