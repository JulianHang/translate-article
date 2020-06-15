### 翻译Java虚拟机规范第二章第十一节-第八节(JSR-337/Java8)

主要内容：`简单介绍方法调用和方法返回的相关指令`

```java

    /**

    2.11.8 Method Invocation and Return Instructions

    方法调用和返回指令

    The following five instructions invoke methods:

    以下五个指令调用方法：

    • invokevirtual invokes an instance method of an object, dispatching on the
    (virtual) type of the object. This is the normal method dispatch in the Java
    programming language.

    invokevirtual调用对象的实例方法，并根据对象的（虚拟）类型进行分派. 这是Java编程语言中的常规方法分派.

    • invokeinterface invokes an interface method, searching the methods
    implemented by the particular run-time object to find the appropriate method.

    invokeinterface调用接口方法，搜索由特定运行时对象实现的方法，以找到适当的方法.

    • invokespecial invokes an instance method requiring special handling, whether an
    instance initialization method (§2.9), a private method, or a superclass method.

    invokespecial调用需要特殊处理的实例方法，无论是实例初始化方法（第2.9节）、私有方法还是超类方法.

    • invokestatic invokes a class (static) method in a named class.

    invokestatic调用命名类中的类（静态）方法.

    • invokedynamic invokes the method which is the target of the call site object
    bound to the invokedynamic instruction. The call site object was bound to a
    specific lexical occurrence of the invokedynamic instruction by the Java Virtual
    Machine as a result of running a bootstrap method before the first execution of
    the instruction. Therefore, each occurrence of an invokedynamic instruction has
    a unique linkage state, unlike the other instructions which invoke methods.

    invokedynamic调用方法，该方法是绑定到invokedynamic指令的调用站点对象的目标.
    由于在第一次执行指令之前运行了引导程序方法，Java虚拟机将调用站点对象绑定到了invokedynamic指令的特定词法事件上.
    因此，与其他调用方法的指令不同，每次调用动态指令都具有唯一的链接状态.

    The method return instructions, which are distinguished by return type, are ireturn
    (used to return values of type boolean, byte, char, short, or int), lreturn, freturn,
    dreturn, and areturn. In addition, the return instruction is used to return from
    methods declared to be void, instance initialization methods, and class or interface
    initialization methods.

    方法返回指令按返回类型区分，它们是ireturn（用于返回布尔，字节，char，short或int类型的值）、lreturn、freturn、dreturn和areturn.
    另外，return指令用于从声明为void的方法，实例初始化方法以及类或接口初始化方法中返回.


    */



```