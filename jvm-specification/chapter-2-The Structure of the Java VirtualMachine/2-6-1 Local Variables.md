### 翻译Java虚拟机规范第二章第六节-第一节(JSR-337/Java8)

主要内容：`介绍了局部变量，每个栈帧都会有一个局部变量表，表中通过索引来标识一个局部变量，对于实例方法来说，其索引从1开始计算局部变量，索引0表示调用方法的实例对象，而对于静态方法来说，其索引从0开始计算局部变量`


```java

    /**

    2.6.1 Local Variables
    
    局部变量

    Each frame (§2.6) contains an array of variables known as its local variables. The
    length of the local variable array of a frame is determined at compile-time and
    supplied in the binary representation of a class or interface along with the code for
    the method associated with the frame (§4.7.3).

    每个栈帧（第2.6节）都包含一个称为其局部变量的变量数组. 栈帧的局部变量数组的长度在编译时确定，以随着栈帧关联的方法代码的类或接口的二进制表示形式（第4.7.3节）提供.

    A single local variable can hold a value of type boolean, byte, char, short, int,
    float, reference, or returnAddress. A pair of local variables can hold a value
    of type long or double.

    单个局部变量可以保存以下类型的值：布尔值、字节、字符、短型、整数、浮点型、引用或returnAddress. 一对局部变量可以容纳long或double类型的值.

    Local variables are addressed by indexing. The index of the first local variable is
    zero. An integer is considered to be an index into the local variable array if and only
    if that integer is between zero and one less than the size of the local variable array.

    局部变量通过索引解决. 第一个局部变量的索引为零. 当且仅当该整数比局部变量数组的大小小零至一之间时，该整数才被视为局部变量数组的索引.

    A value of type long or type double occupies two consecutive local variables.
    Such a value may only be addressed using the lesser index. For example, a value of
    type double stored in the local variable array at index n actually occupies the local
    variables with indices n and n+1; however, the local variable at index n+1 cannot
    be loaded from. It can be stored into. However, doing so invalidates the contents
    of local variable n.

    long类型或double类型的值占用两个连续的局部变量. 只能使用较小的索引来解决该值. 例如，存储在索引为n的局部变量数组中的double类型的值实际上占据了索引为n和n + 1的局部变量.
    但是，不能从索引n + 1上加载局部变量. 可以存储到索引n + 1（覆盖）. 但是，这样做会使局部变量n的内容无效.

    The Java Virtual Machine does not require n to be even. In intuitive terms, values
    of types long and double need not be 64-bit aligned in the local variables array.
    Implementors are free to decide the appropriate way to represent such values using
    the two local variables reserved for the value.

    Java虚拟机不要求n为偶数. 用直观的术语来说，long和double类型的值不需要在局部变量数组中进行64位对齐. 实现者可以使用保留的两个局部变量来自由决定表示此类值的适当方式.

    The Java Virtual Machine uses local variables to pass parameters on method
    invocation. On class method invocation, any parameters are passed in consecutive
    local variables starting from local variable 0. On instance method invocation,
    local variable 0 is always used to pass a reference to the object on which the
    instance method is being invoked (this in the Java programming language). Any
    parameters are subsequently passed in consecutive local variables starting from
    local variable 1.

    在方法调用时Java虚拟机使用局部变量传递参数. 在类方法调用中，在连续的局部变量中从局部变量0开始传递所有参数. 在调用实例方法中，局部变量0始终用于传递一个指向实例方法被调用的对象引用.
    在连续的局部变量中从局部变量1开始随后传递所有参数.


    */


```