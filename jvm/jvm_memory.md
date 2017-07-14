######JVM学习笔记-运行时数据区域
*   从一个Java coder成长一个engineer,着实有漫长的路需要走。今天我们就来学习一下java虚拟机-JVM运行时数据区域。

    JVM在执行Java程序的时候会把它所管理的内存划分为若干个不同的数据区域。现主流思想是将JVM内存分配为以下几个运行数据区域。
* 图一 ![](http://i.imgur.com/8lnBrWV.png)
* 按照线程隔离的特点，可以分为：

    a.线程隔离的数据区：程序计数器（Program Counter Register），虚拟机栈（VM Stack），本地方法栈（Native Method Stack）。

    b.所有线程共同的数据区：方法区（Method Area），堆（Heap）

###### 1.程序计数器
*  程序计数器（Program Counter Register）是一块比较小的内存空间，可以认为是当前线程所执行的字节码的行号指示器。字节码解释器的工作就是改变这个计数器的值来选择下一条需要执行的字节码指令。因为现在的Java虚拟机的多线程运行时通过线程轮流切换实现的。为了保证线程切换后能恢复到正确的执行位置，所以每条线程都需要一个独有的程序计数器。各个线程的计数器就互不影响了。此外，如果该Java执行的是native方法，则这个计数器的值为null。同时，此区域是唯一一个在JVM规范中没有规定任何OutOfMemoryError的地方。
######2，.Java虚拟机栈
*  与程序计数器一样，虚拟机栈（VM Stack）也是线程私有的，生命周期与线程相同。Stack描述的是Java方法执行的内存模型，每个方法在执行的时候会同时创建一个栈帧。用于储存局部变量表、操作数栈、动态链接、方法出口等信息。每个方法的执行及时一个栈帧入栈和出栈的过程。一般人喜欢把Java内存简单的分为堆内存（Heap）和栈内存（Stack），代指虚拟机栈中局部变量表部分，其中存放了编译期可知的基本数据类型和对象引用。
*  图二 ![](http://i.imgur.com/i8TqISf.png)
*  这一区域定义了两种异常状况：如果线程请求的栈深度大于虚拟机所允许深度，将抛出StackOverflowError异常。一般Stack可以动态扩展，如果扩展时无法申请到足够内存。就会抛出OutOfMemoryError异常。

    先模拟StackOverFlowError的场景，代码如下
 public class OutOfMemory {
    private int deep = 0;
    private void stackPrint(){
        deep++;
        stackPrint();
    }
    
    public static void main(String[] args) {
        StackOverflowError();
    }
    
    private static void StackOverflowError(){
        OutOfMemory oom = new OutOfMemory(); 
        try {
            oom.stackPrint();
        } catch (Throwable e) {
            System.out.println("stack feep:" + oom.deep);
            throw e;
        }
    }
    
}

输出结果
stack feep:489135 Exception in thread "main" java.lang.StackOverflowError 
at thread.OutOfMemory.stackPrint(OutOfMemory.java:7) 
at thread.OutOfMemory.stackPrint(OutOfMemory.java:7) 
at thread.OutOfMemory.stackPrint(OutOfMemory.java:7)

 
   但是OutOfMemoryError场景很难模拟，首先配置了VM参数 -Xmx10m -Xss256k 让每个栈线程分配的内存资源很少，然后创建多个线程，但是程序创建700+个线程后系统假死，有何高手可以指教一番

package thread;

public class OutOfMemory {
    
    private void stackLoop(){
        while(true){
            
        }
    }
    
    public static void main(String[] args) {
        OutOfMemoryError();
    }
    
    private static void OutOfMemoryError(){
        int count = 0;
        while(true) {
            count ++ ;
            System.out.println(count);
            new Thread(new Runnable(){
                @Override
                public void run() {
                    OutOfMemory oom = new OutOfMemory();
                    oom.stackLoop();
                }
            }).start();
        }
    }
}
结果系统假死

######3 本地方法栈
* 本地方法栈（Native Method Stack）作用与虚拟机栈（VM Stack相似）。区别是该栈的服务对象时是Native方法。但是虚拟机规范中对本地方法栈没有强制规定，所以可以自由实现。比如常见的HotSpot就是将本地方法栈和虚拟机栈合二为一。aliJDK应该用的也是这种机制。所以这块区域也定义了StackOverflowError和OutOfMemoryError异常。
######4 Java堆
对于大多数应用来说，Java堆（JavaHeap）是JVM所管理的内存中最大的一块。它是被所有线程共享的一块区域。在JVM启动时，它唯一的作用就是存放对象实例。几乎所有对象实例和数组都要在堆上分配。为什么用的是“几乎”，大家可以参考JIT编译器的发展、逃逸技术、栈上分配、标量替换。先说下逃逸，通俗一点讲，当一个对象的指针被多个方法或线程引用时，我们称这个指针发生了逃逸。

public class A{
    public static B b;  
    public void globalVariablePointerEscape(){//给全局变量赋值，发生逃逸
        b = new B();
    }
    public B methodPointerEscape(){//方法返回值，发生逃逸
        return new B();
    }
    public void instancePassPointerEscape(){
        methodPointerEscape().printClassName(this);//实例引用传递，发生逃逸
    }
}

class B{
    public void printClassName(A a){
        System.out.println(a.getClass().getName());
    }
}
    比如，一个方法内部的临时变量或对象，生命周期非常短暂，并且未发生逃逸。当该方法被执行时，则将这个变量或对象实例直接分配到栈上。待线程结束后立马回收，减少对堆内存的开销。

    Java堆是垃圾收集器管理的主要区域，因此很多时候也被称作“GC堆”（Garbage Collected Heap）现有收集器基本采用分代收集算法。一般JVM分为新生代和老年代。个别JVM还有永久代（关于HeapGC以后会做深入学习）。堆的大小可以是固定，也可以扩展实现（通过-Xmx和-Xms控制）。如果堆内存不足时，只定义了OutOfMemoryError:Java heap space异常。
######5.方法区
* 方法区（Method Area）与Java堆一样，是各个线程共享的内存区域。它用于储存已被虚拟机加载的类信息、常量、静态变量、JIT编译器编译后的代码等数据。有些虚拟机会把方法区描述为堆的一个逻辑部分。但是却有一个别名“Non-Heap”（非堆），目的将与Java堆区别开来。

    对于习惯在HotSpot上开发的人来说，习惯将将方法区称作“永久代”，因为HotShot的GC分代收集策略扩展到了方法区。用GC管理这部分内存。但是使用永久代来实现方法去并不是最优的方法，极个别方法如（String.intern()）在不同JVM中会有不同表现。现在HotShot已考虑使用NativeMemory实现方法区（这样减少内存溢出的可能性，毕竟NativeMemory内存大小取决于机器而不是JVM）。同时，从JDK7开始，将字符串常量池从永久代移到堆中。如果内存溢出，则定义了OutOfMemoryError:PermGen space异常。（到了JDK 1.8 持久代移到了matespace，使用native memory）