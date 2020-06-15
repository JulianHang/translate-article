### 翻译Java虚拟机规范第二章第十节(JSR-337/Java8)

主要内容：`介绍了异常与异常处理程序`

```java

    /**

    2.10 Exceptions

    异常

    An exception in the Java Virtual Machine is represented by an instance of the class
    Throwable or one of its subclasses. Throwing an exception results in an immediate
    nonlocal transfer of control from the point where the exception was thrown.

    Java虚拟机中的异常由Throwable类或其子类之一的实例表示. 引发异常会导致从引发异常的那一刻起立即进行非本地控制转移.

    Most exceptions occur synchronously as a result of an action by the thread in which
    they occur. An asynchronous exception, by contrast, can potentially occur at any
    point in the execution of a program. The Java Virtual Machine throws an exception
    for one of three reasons:

    大多数异常是由于发生它们的线程采取的措施而同步发生的. 相反，异步异常可能在程序执行的任何时候发生. Java虚拟机引发异常是由于以下三个原因之一：

    • An athrow instruction (§athrow) was executed.

    执行了一条athrow指令（§athrow）.

    • An abnormal execution condition was synchronously detected by the Java
    Virtual Machine. These exceptions are not thrown at an arbitrary point in the
    program, but only synchronously after execution of an instruction that either:

    Java虚拟机同步检测异常执行条件. 这些异常不会在程序中的任意点抛出，而只会在执行以下任一指令后才同步：

    – Specifies the exception as a possible result, such as:

    将异常指定为可能的结果，例如：

    › When the instruction embodies an operation that violates the semantics of
    the Java programming language, for example indexing outside the bounds
    of an array.

    当指令体现的操作违反Java程序语言的语义时，例如，在数组范围之外进行索引.

    › When an error occurs in loading or linking part of the program.

    在加载或链接程序的一部分时发生错误.

    – Causes some limit on a resource to be exceeded, for example when too much
    memory is used.

    导致超出资源的某些限制，例如，当使用太多内存时.

    • An asynchronous exception occurred because:

    发生异步异常是因为：

    – The stop method of class Thread or ThreadGroup was invoked, or

    调用了Thread或ThreadGroup类的stop方法，或者

    – An internal error occurred in the Java Virtual Machine implementation.

    Java虚拟机实现中发生内部错误.

    The stop methods may be invoked by one thread to affect another thread or all
    the threads in a specified thread group. They are asynchronous because they may
    occur at any point in the execution of the other thread or threads. An internal
    error is considered asynchronous (§6.3).

    一个线程可以调用stop方法以影响另一个线程或指定线程组中的所有线程. 它们是异步的，因为它们可能在其他一个或多个线程的执行中的任何时刻发生. 内部错误被认为是异步的（第6.3节）.

    A Java Virtual Machine may permit a small but bounded amount of execution to
    occur before an asynchronous exception is thrown. This delay is permitted to allow
    optimized code to detect and throw these exceptions at points where it is practical
    to handle them while obeying the semantics of the Java programming language.

    Java虚拟机允许在引发异步异常之前执行少量但有限的执行. 允许使用此延迟，以使优化的代码可以在遵循Java程序语言语义的情况下在实际可行的位置检测并抛出这些异常.

    A simple implementation might poll for asynchronous exceptions at the point of each
    control transfer instruction. Since a program has a finite size, this provides a bound
    on the total delay in detecting an asynchronous exception. Since no asynchronous
    exception will occur between control transfers, the code generator has some flexibility
    to reorder computation between control transfers for greater performance. The paper
    Polling Efficiently on Stock Hardware by Marc Feeley, Proc. 1993 Conference on
    Functional Programming and Computer Architecture, Copenhagen, Denmark, pp. 179–
    187, is recommended as further reading.

    一个简单的实现可以在每条控制转移指令的位置轮询异步异常. 因为程序的大小是有限的，所以这为检测异步异常提供了总延迟.
    由于在控制传输之间不会发生异步异常，因此代码生成器具有一些灵活性，可以在控制传输之间对计算进行重新排序，以提高性能.
    Marc Feeley，Proc 撰写的《在股票硬件上高效轮询》一文，建议将1993年函数编程和计算机体系结构会议（丹麦哥本哈根），第179-187页作为进一步阅读.

    Exceptions thrown by the Java Virtual Machine are precise: when the transfer of
    control takes place, all effects of the instructions executed before the point from
    which the exception is thrown must appear to have taken place. No instructions that
    occur after the point from which the exception is thrown may appear to have been
    evaluated. If optimized code has speculatively executed some of the instructions
    which follow the point at which the exception occurs, such code must be prepared
    to hide this speculative execution from the user-visible state of the program.

    Java虚拟机抛出的异常是精确的：当发生控制转移时，在抛出异常的那一点之前执行的指令的所有效果必须看起来已经发生. 引发异常之后没有发生任何指令似乎已被评估.
    如果优化的代码已经推测性地执行了发生异常之后的某些指令，则必须准备隐藏推测性执行对程序的用户可见状态.

    Each method in the Java Virtual Machine may be associated with zero or more
    exception handlers. An exception handler specifies the range of offsets into the Java
    Virtual Machine code implementing the method for which the exception handler
    is active, describes the type of exception that the exception handler is able to
    handle, and specifies the location of the code that is to handle that exception. An
    exception matches an exception handler if the offset of the instruction that caused
    the exception is in the range of offsets of the exception handler and the exception
    type is the same class as or a subclass of the class of exception that the exception
    handler handles. When an exception is thrown, the Java Virtual Machine searches
    for a matching exception handler in the current method. If a matching exception
    handler is found, the system branches to the exception handling code specified by
    the matched handler.

    Java虚拟机中的每个方法都可以与零个或多个异常处理程序关联. 异常处理程序指定实现该异常处理程序所针对的方法的Java虚拟机代码中的偏移量范围，描述异常处理程序能够处理的异常类型，并指定要处理的异常的代码位置.

    If no such exception handler is found in the current method, the current method
    invocation completes abruptly (§2.6.5). On abrupt completion, the operand stack
    and local variables of the current method invocation are discarded, and its frame
    is popped, reinstating the frame of the invoking method. The exception is then
    rethrown in the context of the invoker's frame and so on, continuing up the method
    invocation chain. If no suitable exception handler is found before the top of the
    method invocation chain is reached, the execution of the thread in which the
    exception was thrown is terminated.

    如果在当前方法中未找到此类异常处理程序，则当前方法调用将异常完成（第2.6.5节）. 突然完成时，当前方法调用的操作数堆栈和局部变量将被丢弃，并弹出其栈帧，以恢复调用方法的栈帧.
    然后，在调用者的栈帧的上下文中将异常抛出，依此类推，继续执行方法调用链. 如果在到达方法调用链的顶部之前未找到合适的异常处理程序，则终止引发异常的线程的执行.

    The order in which the exception handlers of a method are searched for a match is
    important. Within a class file, the exception handlers for each method are stored in
    a table (§4.7.3). At run time, when an exception is thrown, the Java Virtual Machine
    searches the exception handlers of the current method in the order that they appear
    in the corresponding exception handler table in the class file, starting from the
    beginning of that table.

    方法的异常处理程序查找匹配项的顺序很重要. 在类文件中，每种方法的异常处理程序都存储在表中（第4.7.3节）. 
    在运行时，引发异常时，Java虚拟机从当前方法的异常处理程序出现在类文件中相应的异常处理程序表中的顺序开始搜索，从该表的开头开始.

    Note that the Java Virtual Machine does not enforce nesting of or any ordering
    of the exception table entries of a method. The exception handling semantics of
    the Java programming language are implemented only through cooperation with
    the compiler (§3.12). When class files are generated by some other means, the
    defined search procedure ensures that all Java Virtual Machine implementations
    will behave consistently.

    请注意，Java虚拟机不强制方法的异常表条目的嵌套或任何顺序. Java程序语言的异常处理语义仅通过与编译器合作来实现（第3.12节）.
    当通过其他方式生成类文件时，定义的搜索过程确保所有Java虚拟机实现都将表现一致.


    */


```