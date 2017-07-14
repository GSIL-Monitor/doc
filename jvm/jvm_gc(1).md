######JVM学习笔记-垃圾回收（一）
######前言
* 图一 ![](http://i.imgur.com/mGaExV8.png)
* 之前讲到过，JVM在运行时，将内存分为了以上几个部分。其中虚拟机栈、本地方法栈、程序计数器都是为线程而生的、为线程而灭。同时，栈中的栈帧都是随着方法的进出而有条不紊的执行出入栈操作，所以基本上每个栈需要分配多少内存基本上是在类结构确定（Class加载）时就知晓的。所以这些区域的内存分配和回收都是具有确定性的，当方法或线程结束后内存就自动回收，不用过多考虑内存空间回收问题。但是Java堆和方法区不同，因为由一个接口实现的多个类所需要的内存大小是不同的，一个方法中多个分支逻辑所需的内存也不尽相同，这些内存大小很多时候是在运行时才知晓的。所以一般意义上的垃圾收集是指堆和方法区。
######哪些对象会被收集，清除？
*  在程序运行时，如果堆内存不够，就需要young GC甚至Full GC。收集那些无用的对象，然后销毁，腾出多余的空间。但是哪些对象是可以被销毁的？
######引用计数算法
* 引用计数算法是最简单的方式，每个对象拥有一个引用计数器，当一个地方引用它时，计数器+1；引用失效时，引用器-1。很多语言使用了这种方式，比如耳熟能详的Python。但是Java并没有简单的使用这种方式。
###### 可达性分析算法 
* 主流语言（Java、C#、Lisp）使用的是可达性分析算法。如下图。
* 图2 ![](http://i.imgur.com/yPQfcU2.png)
* 这个算法的思路是通过一系列的称为“GC Roots”的对象作为起始点，从节点开始向下搜索，走过的路径称之为引用链。到一个对象与GC Roots没有引用链关联时，就认为不可达。将被垃圾收集器判定可回收。这样就能回避“引用计数法”的弊端，比如object6，object7虽然还有引用，但是它们的引用object5其实可以被回收了。或者几个类相互引用。这样将永远得不到回收。
###### 销毁前的finalize()
* 即使对象在可达性算法中不可达，它也不会立马判定为可回收。而且宣告对象的死亡，至少要经过2个标记过程。第一次标记就是可达性算法分析，第二次就是finalize是否被覆盖或者被JVM调用，如果是，这个对象就能被销毁。我们可以把finalize类比为C++的析构函数。在GC前，第一次被标记的对象执行finalize()。我们可以看以下一段代码

public class FinalizeEscapeGC {
    public String flag = null;
    public FinalizeEscapeGC(String flag){
        this.flag = flag;
    }
    public static  FinalizeEscapeGC SAVE_HOOK = null;
    public void isAlive() {
        System.out.println("yes, I am still alive");
    }
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("finalize method executed!");
        FinalizeEscapeGC.SAVE_HOOK = this;
        System.out.println("get reference" + this.flag);
    }
    public static void main(String[] args) throws Throwable {
        SAVE_HOOK = new FinalizeEscapeGC("A");
        //对象第一次拯救自己
        SAVE_HOOK = null;
        System.gc();
        Thread.sleep(500);
        if (SAVE_HOOK != null) {
            SAVE_HOOK.isAlive();
        } else {
            System.out.println("no, I am dead");
        }
        //第二次拯救自己
        SAVE_HOOK = null;
        System.gc();
        Thread.sleep(500);
        if (SAVE_HOOK != null) {
            SAVE_HOOK.isAlive();
        } else {
            System.out.println("no, I am dead");
        }
        
    }

}
 运行结果：

2017-03-14T22:24:00.015-0800: [GC [PSYoungGen: 2642K->416K(76800K)] 2642K->416K(251392K), 0.0014630 secs] [Times: user=0.01 sys=0.00, real=0.00 secs] 
2017-03-14T22:24:00.016-0800: [Full GC [PSYoungGen: 416K->0K(76800K)] [ParOldGen: 0K->230K(174592K)] 416K->230K(251392K) [PSPermGen: 2602K->2601K(21504K)], 0.0078140 secs] [Times: user=0.02 sys=0.00, real=0.01 secs] 
finalize method executed!
get referenceA
yes, I am still alive
2017-03-14T22:24:00.528-0800: [GC [PSYoungGen: 2641K->64K(76800K)] 2872K->294K(251392K), 0.0006850 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
2017-03-14T22:24:00.528-0800: [Full GC [PSYoungGen: 64K->0K(76800K)] [ParOldGen: 230K->230K(174592K)] 294K->230K(251392K) [PSPermGen: 2601K->2601K(21504K)], 0.0056560 secs] [Times: user=0.02 sys=0.01, real=0.01 secs] 
no, I am dead
 可以看到第一次GC的时候，JVM调用了finalize，并将flag为“A”的对象引用给了SAVE_HOOK，使得有其它地方又重新引用了该实例，所以第一次该实例GC时并没有被回收。然后二次GC时，因为finalize已经被调用，所以认为该对象可以被销毁。
######回收方法区
* 方法区在jdk8之前用永久代进行管理，jdk8使用元空间进行管理。一般存放常量和类等等。因为方法区的数据不像堆空间那样变动频繁，所以一般只有在FullGC时才对方法区进行垃圾回收。但是如何判定一个Class是无用的，可以被销毁的？一般要满足三个条件：
    1.该Class对应的所有实例都已销毁。
    2.该Class的ClassLoader是否被销毁。
    3.该Class对象没有被任何地方引用 