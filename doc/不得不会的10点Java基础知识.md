﻿
### 1.实例变量和类变量

实例变量：指每个对象独立的，修改其中一个对象的实例变量，不会影响其他实例变量的值，变量值无 **static** 关键字修饰；

类变量：是指所有对象共享的，其中一个对象把该变量的值修改了，那么所有对象的该变量的值都修改了，使用 **static** 关键字修饰，一般使用 「类名.属性」 来访问；

### 2.静态方法和非静态方法

静态方法：是指使用 **static** 关键字修饰修饰的方法，一般使用 「类名.方法」 来访问，也可以通过 「对象.方法」 来访问，但是不建议这样写！静态方法中只能调用静态方法和使用静态成员，因为静态成员和静态方法会随着类的加载而加载，非静态成员或者方法的加载晚于静态的成员和方法，故静态方法不能调用非静态的方法和非静态的成员，即静态的对静态的；

非静态方法：是指没有使用 **static** 关键字修饰修饰的方法，一般使用 「对象.方法」 来访问，非静态方法中即可调用非静态方法和使用非静态成员也可以调用静态方法和使用静态成员；

### 3.Java 中包的作用

在 Java 中，包的声明一般写在文件的第一行，使用关键字 **package** + 包名，其作用有：①避免重名，例如我们有时候在调用某个类的某个方法时会发现会有相同类名的方法，这时候你就要导入你要调用的类所在的包或者使用全类名调用即可，这样避免了重名；②用于权限控制，在同一个包下的类是可以相互调用的，如果调用其他包下的类，就必须要导入调用类所在的包，这样可以避免外部的非法调用，另外还可以合理的使用访问控制权限的关键字 **public、protected、private** 来进行访问权限的设置（更多各关键字权限控制参考附表1）；③划分项目结构层次，使得文件管理更有条理，例如MVC、MVP模式分包；

![附表1](https://img-blog.csdn.net/20180622152533284?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 4.Java 中的静态导入

静态导入是 JDK1.5 引入的新特性，一般情况下，我们调用某个类中的静态成员或静态方法使用 「类名.属性」 来调用，而静态导入可以把某个类下的静态成员(方法和变量)使用类似导包的方式导入进来，即可以直接导入到类的成员级别(方法和变量)，这样可以使被导入类的静态变量和静态方法在当前类直接可见，使用这些静态成员无需使用 「类名.属性」 的方式来调用，直接调用方法名或者属性，像调用自己类中的方法和属性一样简单。

静态导入的语法是： 

 - import static 包名.类名.静态成员变量;   
 - import static 包名.类名.静态成员函数; 

注意导入的是静态成员变量和方法名，另外也可导入某个类下的所有静态成员：

 - import static 包名.类名.*;

*号代表通配所有。

一般情况下我们可以适度的使用静态导入，不建议大量滥用，因为我们知道 Java 是面向对象的语言，使用静态导入调用时，缺少了类名，即弱化了类的描述，有时候我们不知道调用的是哪个类的成员，可能还会思考一番，弱化了面向对象思想，导致类和类之间的调用关系不清晰！

### 5.Java 中的 this 关键字

在 Java 中，当创建一个对象后，Java 虚拟机就会为其分配一个指向对象本身的指针，这个指针就是 ** this** 。this 的使用场景：①当方法的布局变量和与实例变量重名时，需要使用 **this** 关键字来区分，如果不重名也可以使用，通常不重名的情况下不写 this；②使用 **this.方法名(参数);** 来调用类的成员方法，一般情况下不写 this，直接调用；③使用 **this(参数1...);** 来调用本类中的其他构造方法，例如在 Android 中自定义一个 View，一般都会写 3 个重载的构造方法，单参数的构造方法中使用 **this(参数1, 参数2);** 来调用双参数的构造方法，双参数的构造方法中调用三个参数的构造方法；④ this 作为参数传递，在 Android 的 Activity 中，经常会使用 this 来作为参数传递；⑤在内部类或者匿名内部类中调用外部类的方法，如果在内部类中使用 this，这里的 this 代表的是当前类本身，即内部类，要想调用外部类中的方法必须使用 **外部类名.this.方法名();** 来调用；

另外使用this关键字要注意一下几点：

 - 在构造方法中使用 this 调用另一构造，必须要将调用放在方法的第一句且只能使用一次；
 - 不能再实例方法中使用this 来调用构造，因为实例方法的调用必须要有对象，而对象的创建一定会调用构造方法，构造方法的调用应早于实例方法；
 - 在构造中不能使用 this 来相互调用构造，例如，如果在单参数的构造中调用双参数的构造，在双参数的构造中调用单参数的构造，我们使用单参数的构造来创建一个实例，调用过程为：单参数构造 -> 双参数构造 -> 单参数构造，这样会无穷无尽的调用下去，另外这违背了在一个方法的调用链中只能调用一次同一个参数的构造方法的规则；

### 6.Java 中的 super 关键字

 - **super();** 调用的父类是 Object的无参构造，默认自动调用，一般情况下都省略不写；
 - this 和 super 关键字都是为了解决重名问题的，如果没有重名问题可以省略不写；
 - this 和 super 都可以理解为父类的一个引用，即可以调用父类中的属性和方法；
 - **super.方法名();** 表示调用父类的方法；
 - **super.属性** 表示调用父类中的属性(用的不多)；
 - **super();**调用父类的构造方法；
 - this 和 super 都只能放在方法的第一行，所以不能一起出现；
 - super 只能出现在构造方法中；
 
在 Java 中，所有类的父类都是 Object，如果一个类没有使用 **extends** 关键字来继承父类，那么它默认继承的是 Object 类，子类不会继承父类的构造方法，但是一定会调用父类的构造方法，如果没有调用，必须要手动去调用，否则会编译报错；

### 7.Java 中的重载与重写

重载即 Overload，在同一个类中，方法名相同参数列表不同的几个方法称为重载方法；

重写即 Override，在子父类中，子类与父类方法名称相同，参数列表必须相同；对于返回值类型子类要么和父类的一致，要么是父类返回值类型的子类；对于权限修饰符，子类的权限修饰符要么和父类的相同，要么比父类的权限修饰符更宽泛；如果有异常抛出，子类的异常捕获范围要么和父类的相同要么比父类的异常范围更小，即子类的异常是父类异常的子类；

### 8.== 与 equals

关系操作符 “==” 生成的是一个boolean结果，它们计算的是操作数的值之间的关系，如果是基本数据类型，它比较的是他们值的本身，如果是引用数据类型，则比较的是对象在内存中的地址；

equals 方法是基类 Object 中的方法，所有类都直接或间接继承于 Object类，因此所有类中都有该方法，如果没有重写 **equals()** 方法，调用 **equals()** 方法和使用 **==** 的效果一样，也是比较的内存中的地址值；在 Java 提供的大多数类中，如 String，Double，Date，Integer等，都对equals 方法进行了重写，用来比较指向的对象所存储的内容是否相等，如果是我们自己的定义的类，如果没有重写 equals 方法，使用 equals 默认比较的是对象在内存中的地址，要比较内容，就必须要重写 equals 方法。

### 9.对类的理解

类是对一类事物的抽象描述，对象是类的实例，可以理解为类是汽车设计图，对象是实实在在的生产出来的汽车，一张设计图(类)可以生产出来多个汽车(对象)，类中包含属性和方法，属性描述了类所具有的特性，方法描述了类的具体功能，这样我们在设计类的时候就会更加注重某个类中的功能及特性，将他们抽象分装到类中，外部在调用的时候不需要关心类中的具体实现，这样就可以更多的考虑类与类之间的相互关系了，这样带来的好处就是可以提高对象的复用性，降低开发难度，多以所Java是面向对象的编程，即Everything is object. 对于面向对象的开发，一般来讲分为三个步骤：OOA（面向对象分析）、OOD（面向对象设计）、OOP（面向对象编程）。OOA 即根据具体的功能分析有哪些特性或者方法，OOD 即根据特性和方法抽象设计出具体的类；OOP 即建立类和类之间的相互调用的关联关系。

### 10.变量、内存和垃圾

变量是内存中的一块存储区域，它的值是变化的，不是固定的;

Java中的内存一般分为Java虚拟机栈和堆内存，栈中一般存放的是基本数据类型和引用类型的引用变量，堆内存中存放的是对象实例，Java堆是 Java 虚拟机所管理的内存中最大的一块，它是被所有线程共享的一块内存区域，在虚拟机启动时创建，可以不是物理上的连续区域，只要逻辑上连续即可；另外还有方法区、静态常量池、程序计数器、本地方法栈等内存分配区域的概念；

垃圾：指的是没有任何引用所指向的对象(一块堆内存空间)，这块空间就是垃圾，所有的垃圾都会等待GC（垃圾收集器）不定期的回收(释放内存空间)。

更多干货文章关注我的微信公众号：

![这里写图片描述](https://img-blog.csdn.net/20180622152651938?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

