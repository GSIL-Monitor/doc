######JVM学习笔记-对象的创建
######前言
*  原来打算紧接下一章节写GC的，但是现在时过214，身为面向对象的coder，没有就自己new一个呗。而且想要了解对象实例如何在GC之后被终结，总归先得了解对象实例是如何被创建的吧。
######对象创建的基本过程
* 图一 ![](http://i.imgur.com/HsVwSnW.png)
######类加载机制
* 虚拟机遇到一条new关键字时，首先去常量池里面获得该类的符号引用。比如“java.lang.String”，然后检查对应的Class类是否已被加载。如果没有被加载，则尝试加载。我们看一下一段源码：

protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
    {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    long t1 = System.nanoTime();
                    c = findClass(name);

                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }
      显而易见，先判断String对应的Class类是否被加载，如果不是，则依次向上通过父类找到顶级的Bootstrp loader加载，如果加载不到，最后再尝试自己的加载器。但为什么JVM会通过这种加载方式呢？首先有一点，同一类被两个类加载器后，被JVM认为是不同的2个类，为了保证类的唯一性，最好是通过同一个loader加载。所以保险起见，选择顶级的Bootstrp loader加载。保证加载后的类的唯一性。
######类加载过程
* 类的加载，说到底就是通过符号引用，找到类对应的.class字节码文件，通过类加载器，载入到JVM中。大致经过一下步骤：
      1.装载：查到并导入class文件
      2.链接：将class文件的字节码数据转化为二进制数据并入JRE中
        （1）校验：检查Class文件正确性
        （2）准备：为静态变量分配内存空间（这类信息一般应该在方法区）
        （3）解析：将符号引用变成直接引用（给出Class加载在内存的地址）
      3.初始化：对类的静态变量，静态代码块执行初始化操作
######分配内存
* 在类加载过后，JVM将为新生对象分配内存。对象所需的内存大小在Class加载完毕后就可以计算出来。同时，JVM要将这块大小确定的内存从JVM的堆内存中划分出来。一般有2种分配策略，如果内存的分配在物理空间上是连续的，就可以将一个指针作为分界指示器。分配空间时，就将指针往空闲区移动与对象大小相等的距离。这种方式为“指针碰撞”。但是当内存空间并不是物理连续时，就不能简单地使用这种方式了。JVM就会维护一张列表来记录哪些内存是可用的。通过这张列表，从堆内存划分一块合适的区域给新生的对象。一般，GC中新生代的Serial，ParNew等收集器使用的是“指针碰撞”，老年代的CMS使用的是“空闲列表”

      但是JVM是支持多线程的，可能会遇到多个线程向一块内存请求空间，这样就线程不安全了。所以JVM一般使用失败重试保证操作原子性，或者为每个线程预先分配本地线程分配缓冲区（Thread Local Allocation Buffer）TLAB，每个线程只在自己的空间分配内存。

      分配内存后，将空间初始化为零值。

######设置实例信息
* 在JVM中，对象实例在内存布局可以划分为3块区域：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。实例数据的来源从Class文件或者构造器入参。
* 图2 ![](http://i.imgur.com/ogfzvlE.png)
* 在填充实例的过程中，我们可以看以下代码

public class TestInstanceInit {
    // 静态变量  
    public static String staticField = "静态变量";
    // 变量  
    public String        field       = "变量";

    // 初始化块  
    {
        System.out.println(field + "0");
        System.out.println("初始化代码块0\n");
    }

    // 静态初始化块  
    static {
        System.out.println(staticField + "0");
        System.out.println("初始化静态代码块0\n");
    }

    // 初始化块  
    {
        System.out.println(field + "1");
        System.out.println("初始化代码块1\n");
    }

    // 静态初始化块  
    static {
        System.out.println(staticField + "1");
        System.out.println("初始化静态代码块1\n");
    }

    // 构造器  
    public TestInstanceInit() {
        System.out.println("构造器\n");
    }

    public static void main(String[] args) {
        new TestInstanceInit();
    }
}

输出结果

静态变量0
初始化静态代码块0

静态变量1
初始化静态代码块1

变量0
初始化代码块0

变量1
初始化代码块1

构造器
从中我们可以看出，这些数据的填充顺序是（静态变量，静态代码块）->（变量，代码块）->构造器
这样，一个对象实例的创建就基本完成了。

######对象的访问定位
*  当对象建立完后，使用方经常希望获得这个对象的引用，Java程序需要通过栈上的reference数据来操作堆上的具体对象。主流的访问方式是使用句柄和指针两种。
      使用句柄的话，如下图，栈存放的是句柄地址，引向句柄池内的句柄，句柄再引向实例数据。

* 图3 ![](http://i.imgur.com/i9gd83W.png)
* 或者使用直接指针访问，栈中存放的是对象地址，如下图
* 图4 ![](http://i.imgur.com/lo52Ix7.png)
* 对比以上2者，各有优点：
    1.通过句柄访问时，句柄比较稳定。当堆GC时，实例数据在堆内存中可能会被移动。这时，栈上的地址不需要改变，只要改变句柄池内的指针就可以了。
    2.通过指针直接访问最大好处就是速度更快，节省二次指针定位的开销，由于Java程序中对对象访问频繁，所以一般使用第二种进行访问对象。
