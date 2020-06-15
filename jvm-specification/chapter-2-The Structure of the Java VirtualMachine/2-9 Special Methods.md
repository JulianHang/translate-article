### 翻译Java虚拟机规范第二章第九节(JSR-337/Java8)

主要内容：`介绍了实例与类的初始化`

```java

    /**

    2.9 Special Methods

    特殊方法

    At the level of the Java Virtual Machine, every constructor written in the Java
    programming language (JLS §8.8) appears as an instance initialization method
    that has the special name <init>. This name is supplied by a compiler. Because
    the name <init> is not a valid identifier, it cannot be used directly in a program
    written in the Java programming language. Instance initialization methods may
    be invoked only within the Java Virtual Machine by the invokespecial instruction
    (§invokespecial), and they may be invoked only on uninitialized class instances.
    An instance initialization method takes on the access permissions (JLS §6.6) of the
    constructor from which it was derived.

    在Java虚拟机级别，用Java程序语言（JLS§8.8）编写的每个构造函数都将显示为实例初始化方法，其名称为<init>. 此名称由编译器提供.
    因为名称<init>不是有效的标识符，所以不能在Java程序语言编写的程序中直接使用它.
    实例初始化方法只能由invokespecial指令（§invokespecial）在Java虚拟机内调用，并且只能在未初始化的类实例上调用.
    在Java虚拟机内部中只能通过invokespecial指令（§invokespecial）调用实例初始化方法. 实例初始化方法具有从其获得构造函数的访问权限（JLS§6.6）.

    A class or interface has at most one class or interface initialization method and is
    initialized (§5.5) by invoking that method. The initialization method of a class or
    interface has the special name <clinit>, takes no arguments, and is void (§4.3.3).

    一个类或接口最多具有一个类或接口初始化方法，并通过调用该方法进行初始化（第5.5节）. 类或接口的初始化方法具有特殊名称<clinit>，不带参数，并且是void的（第4.3.3节）.

    ther methods named <clinit> in a class file are of no consequence. They are not class
    or interface initialization methods. They cannot be invoked by any Java Virtual Machine
    instruction and are never invoked by the Java Virtual Machine itself.

    在类文件中名为<clinit>的其他方法无关紧要. 它们不是类或接口的初始化方法. 它们不能被任何Java虚拟机指令调用，也不能被Java虚拟机本身调用.

    In a class file whose version number is 51.0 or above, the method must
    additionally have its ACC_STATIC flag (§4.6) set in order to be the class or interface
    initialization method.

    在版本号为51.0或更高版本的类文件中，该方法还必须设置其ACC_STATIC标志（第4.6节），以便用作类或接口初始化方法.

    This requirement was introduced in Java SE 7. In a class file whose version number is 50.0
    or below, a method named <clinit> that is void and takes no arguments is considered the
    class or interface initialization method regardless of the setting of its ACC_STATIC flag.

    Java SE 7中引入了此要求. 在版本号为50.0或更低的类文件中，无论其ACC_STATIC标志的设置如何，void且不带参数的名为<clinit>的方法被视为类或接口初始化方法.

    The name <clinit> is supplied by a compiler. Because the name <clinit> is
    not a valid identifier, it cannot be used directly in a program written in the Java
    programming language. Class and interface initialization methods are invoked
    implicitly by the Java Virtual Machine; they are never invoked directly from any
    Java Virtual Machine instruction, but are invoked only indirectly as part of the class
    initialization process.

    名称<clinit>由编译器提供. 因为名称<clinit>不是有效的标识符，所以不能直接在Java程序语言中编写的程序使用它. 类和接口的初始化方法由Java虚拟机隐式调用.
    绝对不能从任何Java虚拟机指令直接调用它们，而只能在类初始化过程中间接调用它们.

    A method is signature polymorphic if all of the following are true:

    如果满足以下所有条件，则方法是签名多态的：

    • It is declared in the java.lang.invoke.MethodHandle class.

    它在java.lang.invoke.MethodHandle类中声明.

    • It has a single formal parameter of type Object[].

    它具有类型为Object[]的单个形式参数.

    • It has a return type of Object.

    它具有对象的返回类型.

    • It has the ACC_VARARGS and ACC_NATIVE flags set.

    它设置了ACC_VARARGS和ACC_NATIVE标志.

    In Java SE 8, the only signature polymorphic methods are the invoke and invokeExact
    methods of the class java.lang.invoke.MethodHandle.

    在Java SE 8中，唯一的签名多态方法是类java.lang.invoke.MethodHandle的invoke和invokeExact方法.

    The Java Virtual Machine gives special treatment to signature polymorphic
    methods in the invokevirtual instruction (§invokevirtual), in order to effect
    invocation of a method handle. A method handle is a strongly typed, directly
    executable reference to an underlying method, constructor, field, or similar
    low-level operation (§5.4.3.5), with optional transformations of arguments or
    return values. These transformations are quite general, and include such patterns
    as conversion, insertion, deletion, and substitution. See the java.lang.invoke
    package in the Java SE platform API for more information.

    Java虚拟机对invokevirtual指令（§invokevirtual）中的签名多态方法进行了特殊处理，以实现方法句柄的调用. 
    方法句柄是强类型，直接可执行引用到基础方法、构造函数、字段或类似的低级操作（第5.4.3.5节），带有参数或返回值的可选转换.
    这些转换非常普遍，并且包括诸如转换、插入、删除和替换之类的模式. 有关更多信息，请参见Java SE平台API中的java.lang.invoke软件包.

    */
```