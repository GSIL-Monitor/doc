##  `Auther` ---mengke

 + [1.个人学习规划]($1)
 + [2.眼界（knowing）]($2)
 + [3.基础知识]($3)
 + [4.缓存（cache）]($4)
 + [5.WEB应用]($5)
 + [6.Linux--（服务器相关知识）]($6)
 + [7.数据库相关]($7)
 + [8.FrontEnd]($8) 
 + [9.解决问题能力提升]($9) 


 
###1.个人学习规划
 * 总结积累文章
 * 源码阅读
   * tomcat （服务器相关）
   * spring （通用框架相关）
   * redis  （缓存 KV-store相关）
   * hadoop （分布式计算系统相关）
   * galaxy （流式计算）
   * maven  （jar管理 pom -- bom）
   * nodeJS
     * 单进程--单线程--（瓶颈）
     * 基于事件驱动（callback回调事件机制）--异步非阻塞
     * 适合I/O密集型应用
     * 不适合cpu密集型应用
            * 单线程原因
       
##
###2.眼界（knowing）
 * 业界新技术
   * docker
   * mircoService(微服务)
   * SAAS  PAAS 
##

###3.基础知识
 * ####3.1 java
   * 反射 --JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。 
     * 在运行时判断任意一个对象所属的类；
     * 在运行时构造任意一个类的对象；
     * 在运行时判断任意一个类所具有的成员变量和方法；
     * 在运行时调用任意一个对象的方法；
     * 生成动态代理。
     * -------------------------------------------------  
     * 1.工厂模式：Factory类中用反射的话，添加了一个新的类之后，就不需要再修改工厂类Factory了
     * 2.数据库JDBC中通过Class.forName(Driver).来获得数据库连接驱动
     * 3.分析类文件：毕竟能得到类中的方法等等
     * 4.访问一些不能访问的变量或属性：破解别人代码
   * 代理
     * 动态代理
       * 优势：在不改变函数签名的前提，增加新扩展功能，确保原代码不变，正常可用。 
     * 静态代理
   * 序列化
     * 目的，把对象的生命周期延长超出JVM生命周期，延长对象生命，或者用来做对象持久化 
         * 其实是对对象属性进行序列化，（static变量除外--序列化是对对象的属性序列化，而静态变量是类的属性，需要区分）
         * 保存方式 --- 二进制文件
         * 方法 --- ObjectInputStream 和 ObjectOutputStream 进行对象的读写
         * 序列化ID，（客户端--服务端相同，JVM才能够允许分序列化对象） 
     * RMI（Remote Method Invocation---远程过程调用
     * RPC 
     * 对象持久化 ---（其实数据库一行就是一个DO对象持久化）
     * 序列化存储规则
       *  Java 序列化机制为了节省磁盘空间，具有特定的存储规则，当写入文件的为同一对象时，并不会再将对象的内容进行存储，而只是再次存储一份引用，上面增加的 5 字节的存储空间就是新增引用和一些控制信息的空间。反序列化时，恢复引用关系。并且两个对象相同。
     * java方案
         * ObjectOutputStream  
     * hessian方案（优）
         * HessianOutput   
   * java NIO
     * Channel
     * Buffer
     * Selector
   * netty
     * 定位： java通信框架（异步，NIO，Selector，epool，Event-Driven，Reactor） || NIO类库
     * Channel
     * ChannelPipeLIne
     * ChannelHandler  
   * java.net.socket--java.net.servicesocket
     * 无   
   * java--native （JNI--`Java Native Interface`）
     * native 是用做java 和其他语言（如c++）进行协作时使用的，也就是native 后的函数的实现不是用java写的
     * 既然都不是java，那就别管它的源代码了，我们只需要知道这个方法已经被实现即可。
     * native的意思就是通知操作系统， 这个函数你必须给我实现，因为我要使用。 所以native关键字的函数都是操作系统实现的， java只能调用。
     * java是跨平台的语言，既然是跨了平台，所付出的代价就是牺牲一些对底层的控制，而java要实现对底层的控制，就需要一些其他语言的帮助，这个就是native的作用了
   * Java多线程
     * 线程池
        * 
     * ThreadLocal  
        * what---ThreadLocal为每个使用该变量的线程提供独立的变量副本。 
        * why ---
        * how ---Map<线程变量副本，ThreadLocal变量值>
        * 解决问题：
          * 解决非线程安全对象的并发线程安全---私有-静态-ThreadLocal<>-变量
          * ![](http://i.imgur.com/XjoGR7x.jpg)  
   * 事务 （ACID）
     * 原子 ---事务执行原子性（要么执行成功， 要么全部撤销）
     * 一致 ---
     * 隔离 ---事务之间是隔离的。
     * 持久 ---  
   
  * JDK基础 
     * ConcurrentHashMap ---- （高并发，线程安全）
         * nulls aren't allowed in ConcurrentMaps key or value 
         * 基于java内存模型实现
         * 分离链接实现冲突检测  
     * Iterator
         * Iterator.remove(),使用迭代器遍历过程中，不能改变原结构，除非使用该方法取出该迭代器绑定  
   
  * java 文件锁 
    * FileLock类
  
  * CAS(Compare-And-Swap)  


 * #### 3.2 JVM知识
   * java平台
     * JDK--JRE--
   * JVM调优
     * 原因
        * 垃圾回收机制--算法
          * 寻找适合自己业务特性的算法
     * 方法
        *   
   * 内存管理
     * 内存监控--jstack--jmap--jstat   
 * ####3.3 日志框架学习
   * apache--common.logging----avalon(framework) 
 * ####3.4 定时功能
   * Spring + Quartz定时框架
     * cron 表达式 
   * JDk timer + timeTask  
 * ####3.5 系统监控管理|报警
 
 * ####3.6  Spring MVC 框架搭建---调优
   * 框架搭建工作流程
   * 系统瓶颈， 优化过程
 * ####3.7 Spring
   * Springbean--singleton 和java单例区别
     *  Spring  单例只是指spring只会给你提供一个单例bean对象。也就是通过context.getBean("")永远只能拿到相同的bean实例。
     *  java单例是JVM层面的概念，class单例，是指在jvm中永远只能有一个实例。
       * 另外，所有属性都是static的类，在意义上基本属于单例模式。 
   * IOC
     * bean factory生成，
   * AOP
     * 如何通过代理完成切面功能，
        * 
     * 通过代理实现，导致问题，类内部调用不能触发切面  
   * tineSpring

   * Spring Mvc 启动过程
     * 一切从servletContext开始， 先把ApplicationContext初始化好作为root扔进去 ，再用它作为父context创造各个DispatcherServlet， 并扔进servletContext，至此创造了整个世界。
      
     * 1 ，应用启动时通过contextloadListenser   初始化ApplicationContext（全应用都可见）
        * 通过web.xml 配置 context-param 完成context加载。
        * ApplicationContext接口是通过XmlApplicationContext 使用XmlBeanFactory完成Bean上下文加载
        * 加载处理ServletContext数据 
        * 通过web.xml 配置的servlet以及已经完成加载的 ApplicationContext（作为父上下文） 完成每一个ServletContext 的加载。 
        * 2，以Tomcat为例，想在Web容器中使用Spirng MVC，必须进行四项的配置：
        * 修改web.xml，添加servlet定义、编写servletname-servlet.xml（ servletname是在web.xm中配置DispactherServlet时使servlet-name的值） 、配置contextConfigLocation初始化参数、配置ContextLoaderListerner
        * DispatcherServlet：前端处理器，接受的HTTP请求和转发请求的类。
        * court-servlet.xml:定义WebAppliactionContext上下文中的bean。
        * contextConfigLocation：指定Spring IoC容器需要读取的定义了非web层的Bean（DAO/Service）的XML文件路径。
        * ContextLoaderListener：Spring MVC在Web容器中的启动类，负责Spring IoC容器在Web上下文中的初始化。
        * 3，Spring MVC启动过程大致分为两个过程：1、ContextLoaderListener初始化，实例化IoC容器，并将此容器实例注册到ServletContext中。2、DispatcherServlet初始化。

 * ####3.8fastJson
   * 序列化--反序列化
   * 速度优势 
##
###4.缓存（cache）
  * 本地缓存
    * 数据存储在应用代码所在内存空间.优点是可以提供快速的数据访问;缺点是数据无法分布式共享,无容错处理.典型的,如Cache4j，guavaCache  
    * 本地缓存数据过期-续期解决hotkey击穿到DB问题
      * 为了解决这样的击穿问题，这里加了一个过期时间续期的操作。当多个线程并发读取某个key1时，第一个竞争到锁的线程会判断key的过期时间，如果已经过期，则返回null，并对该key续期（即延长过期时间），由于返回的是null，该线程会击穿Localcache继续向server发起请求，并更新本地Localcache。其他线程访问key时，已经是续期之后，这些线程的访问该key时，就不会发现已经过期，读到的是老的value，等到第一个线程完成server端key-value到Localcache的更新结束后，所有线程都将读到新value。这样的好处是不会在某个key过期时间一到就击穿Localcache，并大量的访问server，对server造成压力。
      * 如图，只有thread-1会击穿Localcache，其他线程拿到的要么是老的value，要么是最新value。
      * ![](http://i.imgur.com/5pjpzsp.png) 
      
  * 分布式缓存-nosql
    * 数据在固定数目的集群节点间分布存储.优点是缓存容量可扩展(静态扩展);缺点是扩展过程中需要大量配置
    * tair
    * memberCache

  * 分布式缓存与NoSQL
    * 半结构化数据，  

##
###5.WebSocket 
  * 

##
###6.WEB应用
  * Tomcat （web-Service）
    * servlet 容器 
  * 线程
    * 线程池使用
    * 任务队列管理
    * 任务调度
    * 任务提交 
    * 任务状态监控 
  * 并发
  * http
    * 幂等 
    * RESTful
      * 忠实的http协议支持者，在应用层使用 
    * SOA
      * 仅作为传输层工具， 在上面包装自己的传输服务方式，工作在传输层
    * post请求---get请求 
      * 从幂等安全方面考虑他它们的区别 
  * 接口幂等性为应用转为分布式系统打下坚实基础
     * 实现方法--数据库（新增业务表）insert ignore   
  * cookie
     * 产生原因： http无状态请求
     * 在浏览器端实现状态链接 （存在size 和数量限制）
  * session 
     * 产生原因： http无状态请求
     * 在服务器端实现http状态链接
     * session 从服务端生成后，传递给客户端的方式
        * cookie
        * URL重写（浏览器cookie禁用后） 
     * 问题
       * 集群部署，session同步问题
       * 大流量应用，session占用内存问题
       * 解决方法： TaoBao 把session直接存储在Tire中（分布式缓存），为了容灾，对session加密存储在cookie中， 只有当分布式缓存故障时才会使用这部分session数据。   

###7.Linux--（服务器相关知识）
  * shell脚本
     * 熟悉， 可以自己写一些自动化工具 
     * grep 
     * ps -ef|grep java
     * pstree -p pid 
     * top -h
     * sar
     * echo
     * set（查询系统环境变量）
     * /proc/PID/status
       * proc 目录下， 
  * JVM
    * jstack -m 
    * jmap
    * jstat
    *     
        'ps -ef | grep java找到你的java程序的进程id, 定位 pid  top -Hp $pid ,shift+t 查看耗cpu时间最多的几个线程, 记录下线程的id, 把上诉线程ID转换成16进制小写 比如 : 0x12ef, kill -3 $pid 触发tomcat的thread dump,找到tomcat的catalin.out 日志, 把 上面几个线程对应的代码段拿出来.'
  * ulimit -a 命令 && 衍生
    * 查看机器单进程打开最多句柄数（open files），
    * web服务器 || cache服务器 系统为每个TCP连接都要创建一个socket句柄，每个socket句柄同时也是一个文件句柄。 进行高并发TCP连接处理时，最高的并发数量都要受到系统对用户单一进程同时可打开文件数量的限制。
    * 句柄存在的意义
       * linux 中一切都是文件，设备（dev）网络（socket）等都是文件，而要使用这个功能，就需要打开文件，打开文件的过程，就是建立进程和文件关系的过程, 而句柄就是唯一标识这种关系的元数据。（对文件的操作就是对句柄的操作） 

##
###8.数据库相关
  * 分布式数据存储
     * 关键点： 数据切分 && 数据冗余
     
  * $schema设计--支持分库分表
    * 物理主键ID
    * 业务自增主键ID 
  * 分页处理（精准的电梯分页）
    * limit方式&优化
       * 缺点当数据量增大时接口越来越慢,(会扫描所有300000+10 条数据，然后返回前10条)
       * 以下例子(50万数据)
       ``` 
       select * from wl_tagindex where byname=’f’ order by id limit 300000,10   执行时间是 3.21s
       ```

    * 改造后（子查询在索引中查询目标Id， 然后join到全表，把目标数据查询出来。扫描300000+10索引，速度超快）
       * 必要条件： id  byname 都需要添加索引
       ``` 
       select * from (
		select id from wl_tagindex
		where byname=’f’ order by id limit 300000,10
		) a
		left join wl_tagindex b on a.id=b.id	
		执行时间为 0.11s 速度明显提升 
       ```

    * 基于时间段的查找的分页（以上会出现问题）
       * ？？？待补充
      
  * query
    * 查询数据高效性
    * 查询数据完整性
  * SQL
    * 批量接口iterate
    *   
  * 事务
    * 切面级支持
    * Spring事务
  * 锁 （同时读和写，或者同时写才会产生冲突）
    * 数据库锁
      * 共享锁S
      * 排它锁X 
    * Mysql 锁粒度 
      * 表锁  页锁  行锁  
    * 悲观锁
      * 假设并发冲突概率较高，任何操作都会影响你的数据， 从开始准备操作数据加锁----- 最后数据提交释放锁
      * 加锁时间较长，
    * 乐观锁 
      * 假设并发冲突较少，最后提交数据更新时才会锁数据，不能解决数据脏读。
      * 加锁时间较短，

##
###9.FrontEnd
  * leilei_okk@163.com
  * 浏览器
     * 浏览器的线程模型： 浏览器的内核是多线程的，它们在内核制控下相互配合以保持同步，一个浏览器有5个常驻线程：
        * 1、javascript引擎线程
        * 2、GUI渲染线程
        * 3、浏览器事件触发线程
        * 4、浏览器定时器触发线程
        * 5、浏览器http异步请求线程 
  * JS
    * 约束规则较少， 玩法较多
    * JavaScript （事件响应流程介绍）
      * browers WebAPIs  
      * event queue
      * event loop
      * running time  call stack
      * [文章地址](https://danmartensen.svbtle.com/events-concurrency-and-javascript) 
    * 语法学习 
       * 数组操作（||对象数组）
         * CRUD
         * map-reduce-filter 
         * [文章地址](https://danmartensen.svbtle.com/javascripts-map-reduce-and-filter)
  
  * CSS
    * 需要很多代码经验 
  * angels JS
    * 前端框架， 双向绑定，页面数据模型抽象
  * 前端工程化
    * 脚手架工具
      * gulp
      * webpack  
 
##   
### 10.docker
 *  
##
###11.解决问题能力提升
 *

##
### 12.代码奇技淫巧
 * 方法入口处参数合法性检测（参考guava写法）
   *  heckArgument(hitCount >= 0);
   
   *  public static void checkArgument(boolean expression) {
     if (!expression) {
      throw new IllegalArgumentException();
    }
  }

  * 好处，不用点击进入到方法内部就可以直接看到不合法参数的界限。 

##
###MySql（关系型数据库）
  * [很不错的文章](http://www.itdadao.com/articles/c15a1145261p0.html)
  * Mysql 分库分表
  * Mysql 读写分离（主从复制技术）
  *
 
##
###NoSql（key-value）
  * redis
     * 需要学习 
  * MongDb
     * 需要学习 
  * Tair
     * 需要学习 


###12. 示例
  * 数据驱动产品的例子
    * linkin 分析大部分用户来自google搜索简历， -->提示用户完善简历，增加推广效应
    *  

###13. Project
  * Hecla 人员账号平台迁移 （技术PM 项目贡献度：90%  项目周期：2016-5-12/2016-08-30)
    * 项目简介： Hecla账号系统是阿里集团客户服务事业群通用账号组织结构，由于平台变更需要对原账号数据进行兼容性迁移，系统维护150万为阿里工作的小二账号（SP合作，云客服），组织结构关系，角色，权限等数据表。涉及总共2亿左右数据（12张表），数据量40G左右， 依赖应用29个，项目周期(2016-5-12/2016-08-30)
    * 主要解决问题
       * 1，依赖系统平滑迁移（兼容性）&& 迁移问题可控
         * 使用handler机制，包装老接口提供服务， 同时对入参-出参使用新服务进行逻辑调用，并与原接口对比，记录异常日志，设置报警，达到问题空
       * 2，服务优化 -- 支持15000 QPS （4台Linux虚拟机  4核8G配置）
         * 通过索引，sql优化确保数据库提供优质TPS，同时使用本地缓存（guava Cache）, 分布式缓存（Tair）在数据库前筑起高墙，确保系统提供高QPS服务同时系统水位在可接受范围内。 
       * 3，稳定性（增加服务client容灾能力）
         * 对client服务进行逻辑封装， 使用类全路径+函数签名作为key， 把请求结果存储在tire上（有效期2小时）。在服务异常（机器 || JVM 挂了）是使用tire数据，最大程度降低故障影响范围，实现服务本地容灾。
    * 使用主要技术： Spring  Mysql ibatias cache(guava-本地 Tire-分布式)  EventBus  
       

  * Xspace（SaaS化一站式新零售服务平台）
    * 简介： （集成在线， 热线，旺旺渠道， 工单...）
    * 热线（hotLineCS）
       * 
    * OMS-payment  后端贡献度（50%） 前端贡献度（80%）
       * 使用主要技术： SpringBoot Mysql Hbase（分部署存储）  React nodeJs
       * 

##
###需要研究技术
 * zookeeper mapreduce hadoop odps 
 * 分布式计算（jstrom， spark）
 * 
         