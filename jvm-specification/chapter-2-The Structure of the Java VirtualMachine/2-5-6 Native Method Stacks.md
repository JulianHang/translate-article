### 翻译Java虚拟机规范第二章第五节-第六节(JSR-337/Java8)

主要内容：`简单介绍本地方法栈，其大小可以是固定的，也可以是动态扩展的，有可能发生StackOverflowError和OOM`

```java

    /**

    2.5.6 Native Method Stacks

    本地方法栈

    An implementation of the Java Virtual Machine may use conventional stacks,
    colloquially called "C stacks," to support native methods (methods written in a
    language other than the Java programming language). Native method stacks may
    also be used by the implementation of an interpreter for the Java Virtual Machine's
    instruction set in a language such as C. Java Virtual Machine implementations
    that cannot load native methods and that do not themselves rely on conventional
    stacks need not supply native method stacks. If supplied, native method stacks are
    typically allocated per thread when each thread is created.

    Java虚拟机的实现可以使用传统的堆栈（俗称"C堆栈"）来支持本地方法（以不同于Java程序语言的语言编写的方法）.
    解释器的实现也可以使用诸如C之类的语言来解释Java虚拟机的指令集，从而使用本地方法栈. 
    无法加载本地方法并且自身不依赖于常规堆栈的Java虚拟机实现不需要提供本地方法栈.
    如果提供，通常在创建每个线程时为每个线程分配本地方法栈.

    This specification permits native method stacks either to be of a fixed size or to
    dynamically expand and contract as required by the computation. If the native
    method stacks are of a fixed size, the size of each native method stack may be
    chosen independently when that stack is created.

    该规范允许本地方法栈具有固定大小，或者根据计算需要动态扩展和收缩. 如果本地方法栈的大小固定，则在创建每个本地方法栈的大小时可以独立选择.

    A Java Virtual Machine implementation may provide the programmer or the user control
    over the initial size of the native method stacks, as well as, in the case of varying-size native
    method stacks, control over the maximum and minimum method stack sizes.

    Java虚拟机实现可以为程序员或用户提供对本地方法栈初始大小的控制，并且在本地方法栈大小变化的情况下，可以控制最大和最小方法栈大小.

    The following exceptional conditions are associated with native method stacks:

    以下异常条件与本地方法栈相关联：

    • If the computation in a thread requires a larger native method stack than is
    permitted, the Java Virtual Machine throws a StackOverflowError.

    如果线程中计算所需的本地方法栈超出允许的范围，则Java虚拟机将引发StackOverflowError.

    • If native method stacks can be dynamically expanded and native method stack
    expansion is attempted but insufficient memory can be made available, or if
    insufficient memory can be made available to create the initial native method
    stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.

    如果可以动态扩展本地方法栈，并且尝试进行扩展，但是没有足够的内存来实现扩展，或者如果没有足够的内存来为新线程创建初始本地方法栈，则Java虚拟机抛出OutOfMemoryError.

    */


```