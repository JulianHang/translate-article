### 翻译Java虚拟机规范第二章第五节-第三节(JSR-337/Java8)

主要内容：`简单介绍堆，其大小可以是固定的或动态扩展，有可能发生OOM`

```java

    /**

    2.5.3 Heap

    堆

    The Java Virtual Machine has a heap that is shared among all Java Virtual Machine
    threads. The heap is the run-time data area from which memory for all class
    instances and arrays is allocated.

    Java虚拟机具有一个在所有Java虚拟机线程之间共享的堆. 堆是运行时数据区，分配所有类实例和数组的内存.

    The heap is created on virtual machine start-up. Heap storage for objects is
    reclaimed by an automatic storage management system (known as a garbage
    collector); objects are never explicitly deallocated. The Java Virtual Machine
    assumes no particular type of automatic storage management system, and the
    storage management technique may be chosen according to the implementor's
    system requirements. The heap may be of a fixed size or may be expanded as
    required by the computation and may be contracted if a larger heap becomes
    unnecessary. The memory for the heap does not need to be contiguous.

    堆是在虚拟机启动时创建的. 自动存储管理系统（称为垃圾收集器）可以回收对象的堆存储. 对象永远不会显式释放. Java虚拟机不假设自动存储管理系统的特定类型，实现者可以按照系统要求选择存储管理技术.
    堆的大小可以是固定的，也可以根据计算要求进行扩展，如果不需要更大的堆，则可以将其收缩. 堆的内存不必是连续的.

    A Java Virtual Machine implementation may provide the programmer or the user control
    over the initial size of the heap, as well as, if the heap can be dynamically expanded or
    contracted, control over the maximum and minimum heap size.

    Java虚拟机实现可以为程序员或用户提供对堆初始大小的控制，并且，如果堆可以动态扩展或收缩，则可以控制最大和最小堆.

    The following exceptional condition is associated with the heap:

    以下异常情况与堆相关联：

    • If a computation requires more heap than can be made available by the
    automatic storage management system, the Java Virtual Machine throws an
    OutOfMemoryError.

    如果计算需要的堆多于自动存储管理系统所能提供的堆，则Java虚拟机将引发OutOfMemoryError.

    */

```
