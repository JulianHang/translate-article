### 翻译Java虚拟机规范第二章第五节-第四节(JSR-337/Java8)

主要内容：`简单介绍方法区，其位置并未做要求，大小可以是固定的，也可以是动态扩展，会发生OOM异常`

```java

    /**

    2.5.4 Method Area

    方法区

    The Java Virtual Machine has a method area that is shared among all Java Virtual
    Machine threads. The method area is analogous to the storage area for compiled
    code of a conventional language or analogous to the "text" segment in an operating
    system process. It stores per-class structures such as the run-time constant pool,
    field and method data, and the code for methods and constructors, including
    the special methods (§2.9) used in class and instance initialization and interface
    initialization.

    Java虚拟机具有一个在所有Java虚拟机线程之间共享的方法区. 方法区类似常规语言的编译代码的存储区域，或者类似于操作系统过程中的"文本"段.
    它存储每个类的结构，例如运行时常量池，字段和方法数据，以及方法和构造函数的代码，包括类、实例、接口初始化的特殊方法（§2.9）.

    The method area is created on virtual machine start-up. Although the method area
    is logically part of the heap, simple implementations may choose not to either
    garbage collect or compact it. This specification does not mandate the location of
    the method area or the policies used to manage compiled code. The method area
    may be of a fixed size or may be expanded as required by the computation and may
    be contracted if a larger method area becomes unnecessary. The memory for the
    method area does not need to be contiguous.

    方法区域是在虚拟机启动时创建的. 尽管方法区域在逻辑上是堆的一部分，但是简单的实现可以选择不进行垃圾回收或压缩. 
    没有规定方法区的位置或用于管理已编译代码的策略.
    方法区可以是固定大小的，或者可以根据计算的需要进行扩展，如果不需要更大的方法区可以缩小. 
    方法区的内存不必是连续的.

    A Java Virtual Machine implementation may provide the programmer or the user control
    over the initial size of the method area, as well as, in the case of a varying-size method area,
    control over the maximum and minimum method area size.

    Java虚拟机实现可以为程序员或用户提供对方法区初始大小的控制，以及在方法区大小可变的情况下，可以控制最大和最小方法区.

    The following exceptional condition is associated with the method area:

    以下异常条件与方法区域相关联：

    • If memory in the method area cannot be made available to satisfy an allocation
    request, the Java Virtual Machine throws an OutOfMemoryError.

    如果使方法区中的内存无法满足分配要求，则Java虚拟机将引发OutOfMemoryError.

    */


```