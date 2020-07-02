### 翻译Java虚拟机规范第三章第十四节(JSR-337/Java8)

主要内容：`简单说明Java虚拟机对同步的处理`

```java

    /**

    3.14 Synchronization

    同步

    Synchronization in the Java Virtual Machine is implemented by monitor entry and
    exit, either explicitly (by use of the monitorenter and monitorexit instructions) or
    implicitly (by the method invocation and return instructions).

    Java虚拟机中的同步是通过监视器的进入和退出来实现的，可以显式地（通过使用monitorenter和monitorexit指令）或隐式地（通过方法调用和返回指令）来实现.

    For code written in the Java programming language, perhaps the most common
    form of synchronization is the synchronized method. A synchronized method is
    not normally implemented using monitorenter and monitorexit. Rather, it is simply
    distinguished in the run-time constant pool by the ACC_SYNCHRONIZED flag, which
    is checked by the method invocation instructions (§2.11.10).

    对于用Java编程语言编写的代码，也许最常见的同步形式是synchronized方法. 通常不使用monitorenter和monitorexit实现同步方法.
    相反，它在运行时常量池中仅通过ACC_SYNCHRONIZED标志加以区分，该标志由方法调用指令（第2.11.10节）检查.

    The monitorenter and monitorexit instructions enable the compilation of
    synchronized statements. For example:

    monitorenter和monitorexit指令可启用同步语句的编译. 例如：

        void onlyMe(Foo f) {
            synchronized(f) {
                doSomething();
            }
        }

    is compiled to:

        Method void onlyMe(Foo)
        0 aload_1 // Push f
        1 dup // Duplicate it on the stack
        2 astore_2 // Store duplicate in local variable 2
        3 monitorenter // Enter the monitor associated with f
        4 aload_0 // Holding the monitor, pass this and...
        5 invokevirtual #5 // ...call Example.doSomething()V
        8 aload_2 // Push local variable 2 (f)
        9 monitorexit // Exit the monitor associated with f
        10 goto 18 // Complete the method normally
        13 astore_3 // In case of any throw, end up here
        14 aload_2 // Push local variable 2 (f)
        15 monitorexit // Be sure to exit the monitor!
        16 aload_3 // Push thrown value...
        17 athrow // ...and rethrow value to the invoker
        18 return // Return in the normal case
        Exception table:
        From To Target Type
        4 10 13 any
        13 16 13 any

    The compiler ensures that at any method invocation completion, a monitorexit
    instruction will have been executed for each monitorenter instruction executed
    since the method invocation. This is the case whether the method invocation
    completes normally (§2.6.4) or abruptly (§2.6.5). To enforce proper pairing
    of monitorenter and monitorexit instructions on abrupt method invocation
    completion, the compiler generates exception handlers (§2.10) that will match
    any exception and whose associated code executes the necessary monitorexit
    instructions.

    编译器确保在任何方法调用完成时，将为自方法调用以来执行的每个monitorenter指令执行一条monitorexit指令.
    方法调用是正常完成（第2.6.4节）还是突然完成（第2.6.5节），都是这种情况.
    为了在突然的方法调用完成时强制正确配对Monitorenter和MonitorExit指令，编译器会生成异常处理程序（第2.10节），该异常处理程序将匹配任何异常，并且其关联代码执行必要的MonitorExit指令.


    */



```