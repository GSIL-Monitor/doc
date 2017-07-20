######从0到1认识ClassLoader
######前言
* 对于java程序员来说, classLoader可能是最熟悉的陌生类. 就像马爸爸一样, 我们大部分人都见过, 但没接触过. 本文从最基本的概念讲起, 最后结合实践, 试着把脑子里有关classLoader的零碎知识片段串成线.
######classLoader基础
#####classLoader是什么
* 见名知意, 名字中的class代表它的作用对象是类, loader代表它的功能是加载. 连起来就是把一个.class结尾的文件以JVM能识别的存储形式加载到内存中.
* 图一![](http://i.imgur.com/TSHmTR9.png)
* 一个类在被使用前, 会经历 class文件生成 -> 加载 -> 连接 -> 初始化等阶段, 这些阶段合起来组成了完整的 "类加载" 过程. 其中 "加载" 阶段(该阶段只是类加载过程中的一个环节)主要完成三件事 (下面内容引自 <<深入理解JVM>> ):

通过类的全限定名来获取定义此类的二进制字节流.
将该二进制字节流定义的静态数据结构转换成方法区的运行时数据结构.
在内存中生成一个代表该类的Class对象, 供外部通过该对象来获取该类的元数据信息.
上图中描述的类加载过程中的大部分阶段都是由jvm来亲自控制, 用户可控制范围不大. 但是jvm对于加载阶段需要完成的几件事没有做强制限制, 可以由用户去自定义实现. 比如从哪获取class文件, 如何获取class文件, 以及如何加载class文件都可以由用户自定义具体实现方案. 正是由于jvm给用户提供了这些钩子, 才衍生出了动态字节码生成, applet, OSGI, 类隔离等技术.

其中类隔离, 就是通过自定义多种classLoader, 并合理组织他们的协作方式来实现 (后面会举例pandora的类隔离机制, 来具体说明).

正常情况下, 一个classLoader需要两个必要属性:

parent: 用于指明当前classLoader的父类加载器.
url: 类命名空间, 用于指明当前classLoader从哪里加载class文件.
这也是自定义一个classLoader时, 需要传给构造函数的两个重要参数.
######classLoader分类
* jvm的类加载器一般分2大类:

系统自带的classLoader
用户自定义类加载classLoder
其中系统自带的类加载器又可以细分成3类:

BootstrapClassloader: 启动类加载器, 加载JAVA_HOME/lib/目录下的所有jar包, 而该目录下的主要放系统核心类库, 比如包含Object, String等类的rt.jar就是由该类加载器加载进内存的.
ExtClassloader: 扩展类加载器, 加载JAVA_HOME/jre/lib/ext/目录下的所有jar包或者由java.ext.dirs系统属性指定的jar包。放入这个目录下的jar包对所有AppClassloader都是可见的(后面讲到双亲委派模型, 就知道为什么).
AppClassloader | SystemClassLoader: 应用类加载器, 也叫系统类加载器. 是我们平时接触最密切的类加载器, 主要负责加载classpath下的所有类. 平时我们用maven构建项目时, 添加进pom文件中的依赖, 最后都是由idea帮我们直接把依赖关联的jar包放到classPath下, 并在运行时, 由AppClassloader帮我们把这些jar包中的class加载到内存中.
总体上可以看出, BootstrapClassloader和ExtClassloader主要负责加载JDK相关的系统类. 而AppClassloader来处理我们放到classpath下的所有类.

#####classLoader由谁加载
* 既然class使用前, 都要靠classLoader来加载进内存, 那classLoader本身又是由谁加载进来的? 看上去像是个鸡生蛋, 蛋生鸡的问题, 其实不然, BootstrapClassloader并不是用java语言写的, 而是用c++, 它属于jvm的一部分, 在jvm启动时, 就会被连带启动, 所以它不需要被某个classLoader加载. 其余的两个类加载器是定义在rt.jar中, 自然由BootstrapClassloader加载进来.
#####一个类什么时候会被加载
* 从最上面的图中可以看出, 类加载过程中有多个阶段, 对于第一个加载阶段, jvm没做强制约束, 但对于初始化阶段, jvm要求必须在规定的5种情况下, 进行类初始化. 而一旦初始化进行, 那么处于初始化阶段前面的几个阶段也必须先于初始化阶段进行. 这也就间接定义了加载阶段的执行条件.

关于规定的5种情况描述较多, 在 <<深入理解JVM>> 7.2小节做了详细说明, 这里不多说. 可以简单地理解成, 程序第一次使用某个类的时候, 就会去调用classLoader加载该类. 比如使用 new 创建一个类实例对象时, 或者使用反射class.forName(...)函数来获取Class对象时, 都会触发加载阶段.
#####双亲委派模型
* 上面提到classLoader有很多种, 我们现在只知道它们都有各自的加载路径, 而它们是什么关系, 如何协作, 这就产生了双亲委派模型.
* 图2 ![](http://i.imgur.com/ebeUKCs.png)
* 理解双亲委派模型, 首先要了解各种类加载器的层次关系. 如图所述, 从下往上看, 每个类加载器的parent是在它上方跟它相邻的类加载器, 比如用户自定义类加载器, 它的parent一般是appClassLoader, 依次类推, appClassLoader的parent是extClassLoader. 但要注意, 这种递推关系到最上方bootStrapClassLoader终止, 它是顶层类加载器, 他没有parent.

还有一点要注意, 这里的层次关系是用组合形式来实现, 而不是继承. 比如appClassLoader是customClassLoader的父类加载器, 但customClassLoader并不是appClassLoader的子类, 而是仅仅把自己的parent属性指向appClassLoader.

了解了类加载器层次关系后, 我们看它们之前如何协同工作. 总体原则是这样的: 当一个类加载器想要加载一个类时, 它会先把任务委托给它的父类加载器, 而不是自己先尝试加载, 以此类推, 父类加载器又会委托自己的父类加载器去执行加载任务, 直到最顶层bootStrapClassLoader为止, 如果bootStrapClassLoader在自己的类空间(可以理解成上面提到的url)找到了该类的class文件, 就会去加载该类到内存中. 如果找不到, 会把任务再向下传递回extClassLoader, 让它去尝试加载该类. 依次类推, 直到某个类加载器在自己的类命名空间里找到了该类的class文件, 就会把该类加载进内存.


可以看到, 从下往上传递任务时 (图中蓝色箭头), 秉着优先让上层去加载的原则, 而从上向下传递任务时 (图中红色箭头), 秉着优先自己去加载的原则.

这样做的好处是, 可以让每个类具备优先级层次, 越靠上方的类加载器 加载进来的类, 层级越高, 在程序中的可见范围也就越大. 比如Object类, 因为处于rt.jar中, 根据双亲委派模型的执行规律, 会被最上面的bootStrapClassLoader加载进来. 也就能保证该类在程序中只会存在一份, 所有程序使用的都是这一份, 不会出现自己定义一个Object类, 被自己定义的classLoader给加载进来的情况, 因为rt.jar中的Object会被最优先加载.

#####loadClass源码分析
* 上面说了那么多, 只说明了概念和原理, 现在可以直接 let code talk, 看一段类加载方法的源码 (java.lang.ClassLoader#loadClass(java.lang.String, boolean)):
* 图3 ![](http://i.imgur.com/1kt0hC3.png)
* 可以看到该函数一上来, 先调用findLoadedClass函数来检查该类是否已经加载过, 相当是一个缓存, 加载过的类, 不会重复加载. 如果是第一次加载, 这里分两种情况:

parent != null: 当前的classLoader有父类加载器, 那么直接调用父加载器的loadClass来加载该类.
parent == null: 当前类加载器是extClassLoader(或者自定义的一个没有parent的classLoader), 直接调用bootStrapClassLoader加载该类.
注意, 这里可能会有疑问, 不是只有bootStrapClassLoader才没有父类加载器么, 所以上面的第2个条件难道不是判断是否是bootStrapClassLoader么, 为什么"parent == null"这个条件居然指的是该加载器是extClassLoader? 那是因为bootStrapClassLoader是用c++写的, 在程序里是获取不到的, 所以extClassLoader.parent其实是null. findBootstrapClassOrNull函数就是调用bootStrapClassLoader来加载类的意思.

如果上层的类加载器都没加载成功 (该类不在上面任何类加载器的类命名空间下), 那么由最初接到加载任务的类加载器通过调用findClass函数, 来尝试加载类. 所以如果我们自定义一个classLoader, 可以直接覆盖该方法(findClass)来定制加载逻辑.

从源码中可以看出, 只要每次都优先调用parent来执行加载逻辑, 就实现了双亲委派模型. 同时也可以发现, 想破坏双亲委派模型也很容易, 覆盖loadClass方法, 里面不调用parent.loadClass方法即可.
#####实践案例 -- Pandora
* 有了上面介绍的classLoader相关知识后, 就可以开始研究如何合理利用自定义classLoader, 来实现一些定制功能. 而集团内部的实践案例有很多, 其中最常听见的就是pandora类隔离机制, 下面就简单说明实现原理.
#####pandora是什么
* 介绍pandora的文章很多很多, 这里不多说 好文推送. 总体来讲, 就是集团的中间件容器. 该容器把相互兼容的稳定版本的各种中间件放在其内部统一管理, 并通过提供类隔离机制解决中间件与中间件之间, 中间件与应用之间的冲突.
#####为什么会有类冲突
* 上面提到pandora通过类隔离机制可以解决类冲突, 那首先要明白为什么会有类冲突.

maven惹的祸

这里提前说明, 类冲突最根本的原因是同一个类命名空间下, 存在不兼容的同名class文件. 跟maven没有直接关系, 即使不使用maven, 手动管理各种依赖包, 应用程序也有可能出现类冲突.

我们构建java应用时, 最常用的构建工具就是maven. maven很重要的功能是依赖管理. 我们只需要在pom文件中指定依赖的GAV坐标 (groupId + artifactId + version) 就可以让maven帮我们去中心仓库把需要的jar包加入应用的classpath下. 但maven也有自己的原则, 当它根据pom文件作依赖分析, 发现通过直接依赖或者间接依赖, 有多个相同GA, 不同V的依赖时, 它会根据两点原则来筛选出唯一的一个依赖, 并最终把相应的jar包放到classpath下:

依赖路径长度: 比如应用的pom里直接依赖了A, 而A又依赖了B, 那么B对于应用来说, 就是间接依赖, 它的依赖路径长度就是2. 长度越短, 优先级越高. 当出现不同版本的依赖时, maven优先选择依赖路径短的依赖.
依赖声明顺序: 当依赖路径长度相同时, pom里谁的声明在上面, maven就选择谁.
这样就可能存在下面几种情况:

NoSuchMethodException
* 图4 ![](http://i.imgur.com/ocrUouQ.png)
* 应用程序里有个A类, 里面含有一个属性B, 这个B类来自b-1.0.jar包. A类有个func_a()方法, 里面会调用b类的func_b方法.

B类含有一个属性C, 这个C类来自c-1.0.jar. B类还提供一个方法func_b(), 里面调用C类的func_c()方法.

这时, 应用程序的主pom里直接显式依赖了c-1.1.jar包, 但是这个jar里的C类中已经把func_c()删除了. 这样由于B类使用的c-1.0.jar对于应用程序来说, 是间接依赖, 依赖路径长度是2 (A -> B -> C), 比应用程序主pom中直接依赖的c-1.1.jar路径长, 最后就会被maven排掉了 (也就是应用程序的classpath下, 最终会保留c-1.1.jar).

最后执行main函数时, 就会报 "NoSuchMethodError", 也就是找不到C类中func_c()方法.

NoClassDefFoundError
* 图5 ![](http://i.imgur.com/RCyF0FR.png)
* 应用程序里有个A类, 里面含有一个属性B, 这个B类来自b-1.0.jar包. A类有个func_a()方法, 里面会调用b类的func_b方法.

B类中提供func_b()方法, 里面依赖一个C类, 这个C类来自c-1.0.jar包.

这时, 应用程序的主pom里直接显式依赖了c-1.1.jar包, 但是这个jar里已经把C类删除了. 跟上面一样的道理, B类依赖的c-1.0.jar最后就会被maven排掉了 (也就是应用程序的classpath下, 最终会保留c-1.1.jar).

最最后执行main函数时, 就会报 "NoClassDefFoundError", 也就是找不到C类的定义.

这两种error, 我们经常碰到. 导致这两种error出现的情形有很多种, 上面提到的只是其中的两个比较简单的例子.
#####排包的几个问题
* 为什么应用的classpath下有时会出现两个同样名字不同版本 (甚至同样版本)的jar包?

maven在做依赖分析时, 不是可以根据自己的规则来筛选出唯一的依赖, 加进项目吗? 如果是, 那上面那个问题如何解释?

当项目中有多个groupId, artifactId相同, 但version不同的依赖项, maven会根据自己的原则(依赖路径长度, 依赖声明顺序), 只留下一个. 但如果两个依赖的groupId或者artifactId不完全一样, 那maven就认为是两个东西, 不去排除.

maven进行依赖分析时, 处理的是写在pom里的依赖, 它会对所有GA相同, 但V不同的依赖进行筛选, 最终只保留一个依赖, 然后根据这个依赖的pom坐标 (G + A + V) 拼接成一个路径, 并去中心仓库中把这个路径下存放的jar包加进应用的classpath下. 至于最后的jar包名字(一般是 artifactId + "-" + V)冲不冲突, maven管不着.

* 图6![](http://i.imgur.com/88MqZfY.png)
* maven只认识外面的封皮 (GAV), 在它做依赖分析时, 里面的jar包名字, 对它来说是透明的.

回到第一个问题, "为什么应用的classpath下有时会出现两个同样名字不同版本 (甚至同样版本)的jar包?"

出现这种情况, 一个很大的可能 (这里只是说很大可能, 还有其他可能, 这里不讨论) 就是这两个jar包的pom坐标中, GA不完全一致, 导致maven依赖分析时, 认为这是两个依赖, 而不是一个依赖的不同版本. 所以会把这两个依赖对应的jar都加到classpath下.

验证:

在一个应用的classpath下发现有两个zookeeper包.
* 图7 ![](http://i.imgur.com/kJzWZfS.png)
* 回到应用的pom文件中, 查看依赖关系, 发现3.3.0版本的zookeeper的pom坐标中的groupId是org.apache.hadoop.
* 图8 ![](http://i.imgur.com/bMBgufv.png)
* 而3.3.5版本的zookeeper的pom坐标中的groupId是org.apache.zookeeper.
* 图9 ![](http://i.imgur.com/hGHb1aj.png)
* 所以这两个依赖的G不同, maven自然认为是两个依赖, 不会排掉.

ps: 有时候想把某个版本的包排掉, 就直接在项目的pom里显式依赖了一个真正需要的版本的依赖. 但到机器上看, 发现那个老包还是没被排掉, 显示依赖的路径最短, 为啥不是只保留显示依赖进来的jar包. 跟上面的说一样, 这种情况, 最大的可能就是显示依赖的GV与想排掉的依赖的GV不一样, 所以maven认为是两个不同的依赖, 都会加载进来.
#####pandora类隔离实现原理

* 为什么要隔离

其实上面已经提到, 如果不隔离, 中间件和应用, 中间件和中间件之前可能会存在类冲突.

隔离作用范围

pandora的类隔离机制主要隔离两种类:

应用使用的类和中间件使用的类.
不同中间件各自使用的类.
隔离效果

应用和中间件可以分别使用自己依赖的类, 即使全类名一样, 也不会出现冲突.
不同中间件可以分别使用自己依赖的类, 即使全类名一样, 也不会出现冲突.
其实就是让大家可以分别使用相同GA (groupId + artifactId), 不同V (version) 的依赖.

举例来说, 假设应用依赖1.0.0版本的logback.jar, hsf依赖2.0.0版本的logback.jar. 程序运行时, 应用可以使用1.0.0版本的logback.jar中的相关类, hsf可以使用2.0.0版本的logback.jar中的相关类, 而互不干扰.

基本原理

classloader在加载jar包中的class时, jar包的版本信息对于classLoader来说是不可见的, 它只认识全类名. 而且jvm中定义两个类是否是同一个类, 是根据全类名 + 加载类的classLoader. 如果是同一个classLoader, 那么只要全类名相同, 就会认为该类已经加载过. 但是一般情况下, 一个包升级版本后, 很少会改动里面的全类名, 所以为了让jvm不认为两个全类名相等的类是同一个类, 那么就要改变classLoader, 想办法用另一个classLoader来加载类. pandora的类隔离就是利用这个基本原理.
* 图10 ![](http://i.imgur.com/SwgHnA4.png)
* 如上图所示, 蓝色的部分是部署在tomcat中的各种app, 包括webapp, pandora容器, 以及各种中间件. 红色部分是实现类隔离需要定制的模块.

首先来看pandora, 容器定制了一个classLoader -- pandoraClassLoader. 该类加载器继承自urlClassLoader, 有自己独立的类命名空间, 跟webapp的类加载器的类命名空间 (classpath) 不一样. 所以pandora容器自身需要的各种类都会放到pandoraClassLoader 的类命名空间下. 这样webapp和pandora之间就实现了隔离.

然后看pandora内部, 每个中间件再部署时, pandora都会为这个中间件单独创建一个moduleClassLoader, 该类加载器也会设置一个专属于该中间件的类命名空间, 所以该中间件相关的类都会放到该空间下. 这样每个中间件各自依赖的类都放在不同的目录下, 而他们各自的moduleClassLoader都拥有不同的类命名空间, 因为中间件跟中间件之前也实现了类隔离加载.

接下来就只剩下一个问题了, webapp需要使用中间件相关类时, 怎么才能拿到已经由moduleClassLoader加载好的相关类. 这里主要有两个定制点:

定制webapp自己的classLoader, 并覆盖loadClass方法, 不再遵循常规的双亲委派模型, 而是首先在一个叫MiddleWareMap (这里只是随便起的名字, 实际也并不是map) 的变量中查看是否已经加载了想要的中间件类, 如果有, 就直接返回该类, 如果没有, 会走普通的双亲委派模型机制, 请求parent去加载.
moduleClassLoader加载好的中间件相关类会导出到一个固定的地方, 供上面第一点提到的MiddleWareMap访问
