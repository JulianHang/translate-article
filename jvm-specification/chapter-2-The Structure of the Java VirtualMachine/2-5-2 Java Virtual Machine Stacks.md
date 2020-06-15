### 翻译Java虚拟机规范第二章第五节-第二节(JSR-337/Java8)

主要内容：`简单介绍Java虚拟机栈，其允许固定大小，也允许动态扩展，看具体的Java虚拟机实现，但固定大小在运行过程中不会OOM，只会发生StackOverflowError，相比动态扩展来说两种异常都有可能发生`

```java

    /**

    2.5.2 Java Virtual Machine Stacks

    Java虚拟机栈

    Each Java Virtual Machine thread has a private Java Virtual Machine stack, created
    at the same time as the thread. A Java Virtual Machine stack stores frames (§2.6).
    A Java Virtual Machine stack is analogous to the stack of a conventional language
    such as C: it holds local variables and partial results, and plays a part in method
    invocation and return. Because the Java Virtual Machine stack is never manipulated
    directly except to push and pop frames, frames may be heap allocated. The memory
    for a Java Virtual Machine stack does not need to be contiguous.

    每个Java虚拟机线程都有一个私有的Java虚拟机栈，与线程同时创建.  一个Java虚拟机栈的存储框架（第2.6节）.
    Java虚拟机栈类似于常规语言例如C的栈：它保存局部变量和部分结果，并在方法调用和返回中起作用.
    因为除了推送和弹出栈帧外，从不直接操纵Java虚拟机栈，所以栈帧可能是堆分配的.

    In the First Edition of The Java® Virtual Machine Specification, the Java Virtual Machine
    stack was known as the Java stack.

    在Java®虚拟机规范的第一版中，Java虚拟机栈称为Java栈.

    This specification permits Java Virtual Machine stacks either to be of a fixed size
    or to dynamically expand and contract as required by the computation. If the Java
    Virtual Machine stacks are of a fixed size, the size of each Java Virtual Machine
    stack may be chosen independently when that stack is created.

    该规范允许Java虚拟机栈具有固定大小，或根据计算要求动态扩展和收缩. 如果Java虚拟机栈的大小固定，则在创建每个Java虚拟机栈时可以独立选择其大小.

    A Java Virtual Machine implementation may provide the programmer or the user control
    over the initial size of Java Virtual Machine stacks, as well as, in the case of dynamically
    expanding or contracting Java Virtual Machine stacks, control over the maximum and
    minimum sizes.

    Java虚拟机实现为程序员或用户提供对Java虚拟机栈初始大小的控制，并且在动态扩展或收缩Java虚拟机栈的情况下，可以控制最大和最小值.

    The following exceptional conditions are associated with Java Virtual Machine stacks:

    以下异常条件与Java虚拟机栈相关：

    • If the computation in a thread requires a larger Java Virtual Machine stack than
    is permitted, the Java Virtual Machine throws a StackOverflowError.

    如果线程中的计算需要比允许的Java虚拟机更大的堆栈，则Java虚拟机将引发StackOverflowError.

    • If Java Virtual Machine stacks can be dynamically expanded, and expansion is
    attempted but insufficient memory can be made available to effect the expansion,
    or if insufficient memory can be made available to create the initial Java Virtual 
    Machine stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.

    如果可以动态扩展Java虚拟机栈，并且尝试进行扩展，但是没有足够的内存来实现扩展，或者如果没有足够的内存来为新线程创建初始Java虚拟机栈，则Java虚拟机抛出OutOfMemoryError.


    */

```