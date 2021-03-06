#Java内存管理与垃圾回收

##一、内存简介

根据《Java虚拟机规范（第2版）》的规定，Java虚拟机所管理的内存将会包括以下几个运行时数据区域，如下图所示：

经常人把Java内存区分为堆内存（Heap）和栈内存（Stack），这种分法比较粗糙，Java内存区域的划分实际上远比这复杂。这种划分方式的流行是因为大多数程序员最关注的、与对象内存分配关系最密切的内存区域是这两块。
通常所说的堆（Heap），此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。这一点在Java虚拟机规范中的描述是：所有的对象实例以及数组都要在堆上分配。
通常所说的栈（Stack），是指虚拟机栈。虚拟机栈描述的是Java方法执行的内存模型：每个方法被执行的时候都会同时创建一个栈帧（Stack Frame）用于存储局部变量表、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。局部变量表存放了编译期可知长度的各种基本数据类型（boolean、byte、char、short、int、float、long、double）、对象引用（reference类型，它不等同于对象本身，是对象在堆内存的首地址）。因此局部变量表所需的内存空间在编译期间完成分配，当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。 方法执行完，自动释放。
方法区（Method Area）用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

##二、垃圾回收机制

垃圾回收（Garbage Collection，GC）：在程序的运行环境中，JVM（Java虚拟机）提供了一个系统级的垃圾回收器线程，它负责自动回收那些无用对象所占用的堆内存。这种内存回收的过程被称为垃圾回收。

Java语言中，内存回收任务由JVM来担当。
Java的垃圾回收机制是JVM提供的能力，由单独的系统级垃圾回收线程在空闲时间以不定时的方式动态回收无任何引用的对象占据的堆内存空间。

（1）程序只能决定一个对象不再被任何引用变量引用，使得它成为可以被回收的垃圾。
类比：决定什么样的东西扔掉

（2）不能决定垃圾回收的时间。垃圾回收器作为低优先级线程独立运行。在任何时候，程序都无法迫使垃圾回收器立即执行垃圾会后操作。 
类比：居民把无用物品放在指定的地方，清洁工人会把它收拾走。但垃圾被收走的时间，居民是不知道的，也无需了解。

（3）程序中可调用System.gc()或Runtime.gc()方法提示垃圾回收器尽快执行垃圾回收操作，但是不能保证调用后垃圾回收器会立即执行垃圾回收。
类比：小区垃圾成堆时，居民打电话给环保局，催促清洁工尽快来处理垃圾。但是清洁工不一定立即就来了，也有可能很长时间后再来。
