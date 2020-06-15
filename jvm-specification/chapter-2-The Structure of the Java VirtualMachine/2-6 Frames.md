### 翻译Java虚拟机规范第二章第六节(JSR-337/Java8)

主要内容：`介绍了栈帧，每个方法对应一个栈帧，正在执行的方法称为当前栈帧，方法完成后栈帧被弹出（销毁）`

```java

    /**

    2.6 Frames
    
    栈帧

    A frame is used to store data and partial results, as well as to perform dynamic
    linking, return values for methods, and dispatch exceptions.

    栈帧用于存储数据和部分结果，以及执行动态链接，方法的返回值和调度异常.

    A new frame is created each time a method is invoked. A frame is destroyed when
    its method invocation completes, whether that completion is normal or abrupt (it
    throws an uncaught exception). Frames are allocated from the Java Virtual Machine
    stack (§2.5.2) of the thread creating the frame. Each frame has its own array of
    local variables (§2.6.1), its own operand stack (§2.6.2), and a reference to the 
    runtime constant pool (§2.5.5) of the class of the current method.

    每次调用方法时都会创建一个新栈帧. 当方法调用完成，无论是正常还是突然完成（抛出未捕获异常），栈帧都会被销毁.
    栈帧是从创建线程的Java虚拟机堆栈（第2.5.2节）中分配的. 每个栈帧都有自己的局部变量表、操作数栈、当前方法所属类的运行时常量池的引用第2.5.5节）.

    A frame may be extended with additional implementation-specific information, such as
    debugging information.

    实现可以额外指定信息来扩展栈帧，例如调试信息.

    The sizes of the local variable array and the operand stack are determined at
    compile-time and are supplied along with the code for the method associated with
    the frame (§4.7.3). Thus the size of the frame data structure depends only on the
    implementation of the Java Virtual Machine, and the memory for these structures
    can be allocated simultaneously on method invocation.

    局部变量表和操作数栈的大小在编译时确定，与栈帧关联的方法代码一起提供（第4.7.3节）.
    因此，栈帧的数据结构大小仅取决于Java虚拟机的实现，在方法调用时分配这些已经被计算过的结构的内存.

    Only one frame, the frame for the executing method, is active at any point in a given
    thread of control. This frame is referred to as the current frame, and its method is
    known as the current method. The class in which the current method is defined is
    the current class. Operations on local variables and the operand stack are typically
    with reference to the current frame.

    在给定的控制线程中的任何一点上，只有一个用于执行方法的栈帧处于活跃状态. 该栈帧称为当前栈帧，方法称为当前方法. 定义当前方法的类是当前类.
    局部变量和操作数栈上的操作通常参考当前帧.

    A frame ceases to be current if its method invokes another method or if its method
    completes. When a method is invoked, a new frame is created and becomes current
    when control transfers to the new method. On method return, the current frame
    passes back the result of its method invocation, if any, to the previous frame. The
    current frame is then discarded as the previous frame becomes the current one.

    如果栈帧的方法调用另一个方法或该栈帧的方法完成，则该栈帧将不再是当前栈帧. 调用方法时，将创建一个新的栈帧，并在控制权转移到新方法时成为当前栈帧.
    在方法返回时，当前栈帧将其方法调用的返回结果传回到上一个栈帧. 当上一个栈帧称为当前栈帧时，原来的当前栈帧将被丢弃.

    Note that a frame created by a thread is local to that thread and cannot be referenced
    by any other thread.

    请注意，线程创建的栈帧在该线程本地，并且任何其他线程都无法引用.

    */


```