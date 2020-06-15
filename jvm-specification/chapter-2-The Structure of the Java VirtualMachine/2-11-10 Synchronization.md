### 翻译Java虚拟机规范第二章第十一节-第十节(JSR-337/Java8)

主要内容：`简单介绍Java虚拟机如何支持同步方法和同步代码块`

```java

    /**

    2.11.10 Synchronization

    同步
     
    The Java Virtual Machine supports synchronization of both methods and sequences of instructions within a method by a single synchronization construct: the monitor. 

    Java虚拟机通过监视器支持方法和代码块的同步. 

    Method-level synchronization is performed implicitly, as part of method invocation and return (§2.11.8). A synchronized method is distinguished in the run-time
    constant pool's method_info structure (§4.6) by the ACC_SYNCHRONIZED flag, which is checked by the method invocation instructions. When invoking a method
    for which ACC_SYNCHRONIZED is set, the executing thread enters a monitor, invokes the method itself, and exits the monitor whether the method invocation completes normally or abruptly. During the time the executing thread owns the monitor, no other thread may enter it. If an exception is thrown during invocation of the synchronized method and the synchronized method does not handle the exception, the monitor for the method is automatically exited before the exception is rethrown out of the synchronized method

    方法上的同步是隐式执行，作为方法调用和返回的一部分. 同步方法在运行时常量池的method_info结构中通过ACC_SYNCHRONIZED标志来区分，该标志由方法调用指令进行检查. 
    当调用设置了ACC_SYNCHRONIZED标志的方法时，执行线程将进入监视器，调用该方法本身，然后退出监视器，无论方法调用是正常完成还是突然完成. 
    在执行线程拥有监视器的时间内，没有其他线程可以进入它. 
    如果在调用同步方法期间抛出异常，并且同步方法无法处理该异常，则在异常从同步方法中抛出之前，该方法的监视器将自动退出.

    Synchronization of sequences of instructions is typically used to encode the synchronized block of the Java programming language. The Java Virtual Machine
    supplies the monitorenter and monitorexit instructions to support such language constructs. Proper implementation of synchronized blocks requires cooperation
    from a compiler targeting the Java Virtual Machine (§3.14).

    指令序列的同步通常用于对Java编程语言的同步块进行编码. Java虚拟机提供了monitorenter和monitorexit指令来支持这种语言构造. 
    正确实现同步块需要与面向Java虚拟机的编译器进行合作（第3章第14节）.

    Structured locking is the situation when, during a method invocation, every exit on a given monitor matches a preceding entry on that monitor. Since there is
    no assurance that all code submitted to the Java Virtual Machine will perform structured locking, implementations of the Java Virtual Machine are permitted but
    not required to enforce both of the following two rules guaranteeing structured locking. Let T be a thread and M be a monitor. Then:

    当方法调用期间，给定监视器上的每个出口与该监视器上的先前条目相匹配时，就是结构化锁定的情况. 
    由于不能保证提交给Java虚拟机的所有代码都将执行结构化锁定，因此允许Java虚拟机的实现，但并不要求执行以下两个保证结构化锁定的规则.
    设T为线程，M为监视器.

    1. The number of monitor entries performed by T on M during a method invocation must equal the number of monitor exits performed by T on M during
    the method invocation whether the method invocation completes normally or abruptly.

    无论方法调用是正常完成还是突然完成，在方法调用期间T对M执行的监视器进入的数量必须等于在方法调用期间T对M执行的监视器退出的数量.

    2. At no point during a method invocation may the number of monitor exits performed by T on M since the method invocation exceed the number of monitor entries performed by T on M since the method invocation.

    自方法调用以来，T对M执行的监视器退出的数量不得超过自方法调用以来T对M执行的监视器进入的数量.

    Note that the monitor entry and exit automatically performed by the Java Virtual Machine when invoking a synchronized method are considered to occur during
    the calling method's invocation.

    注意，在方法的调用期间，当调用同步方法时Java虚拟机被认为发生自动执行监视器的进入和退出.


    */

```
