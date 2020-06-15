### 翻译Java虚拟机规范第二章第六节-第五节(JSR-337/Java8)

主要内容：`阐述何为方法异常调用完成`

```java

    /**

    2..6.5 Abrupt Method Invocation Completion

    方法异常调用完成

    A method invocation completes abruptly if execution of a Java Virtual Machine
    instruction within the method causes the Java Virtual Machine to throw an
    exception (§2.10), and that exception is not handled within the method. Execution
    of an athrow instruction (§athrow) also causes an exception to be explicitly thrown
    and, if the exception is not caught by the current method, results in abrupt method
    invocation completion. A method invocation that completes abruptly never returns
    a value to its invoker.

    如果方法中执行Java虚拟机指令导致Java虚拟机抛出异常（第2.10节），并且该方法未处理异常，则方法调用将异常完成.
    执行athrow指令（§athrow）也会导致显式的抛出异常，如果当前方法未捕获该异常，则会导致异常的方法调用完成.
    异常完成的方法调用永远不会向其调用者返回值.

    */


```