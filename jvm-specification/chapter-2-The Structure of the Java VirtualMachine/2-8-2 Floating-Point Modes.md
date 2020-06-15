### 翻译Java虚拟机规范第二章第八节-第二节(JSR-337/Java8)

```java

    /**

    2.8.2 Floating-Point Modes

    浮点模式

    Every method has a floating-point mode, which is either FP-strict or not FPstrict. 
    The floating-point mode of a method is determined by the setting of the
    ACC_STRICT flag of the access_flags item of the method_info structure (§4.6)
    defining the method. A method for which this flag is set is FP-strict; otherwise, the
    method is not FP-strict.

    每种方法都有一个浮点模式，该模式是FP-strict或不是FPstrict. 方法的浮点模式由设置该方法的method_info结构（第4.6节）的access_flags项目的ACC_STRICT标志的设置确定.
    方法将此标志设置成FP-strict；否则，该方法不是FP-strict.

    Note that this mapping of the ACC_STRICT flag implies that methods in classes compiled
    by a compiler in JDK release 1.1 or earlier are effectively not FP-strict.

    请注意，此ACC_STRICT标志的映射意味着JDK 1.1版或更早版本中的编译器编译的类中的方法实际上不受FP-strict影响.

    We will refer to an operand stack as having a given floating-point mode when the
    method whose invocation created the frame containing the operand stack has that
    floating-point mode. Similarly, we will refer to a Java Virtual Machine instruction
    as having a given floating-point mode when the method containing that instruction
    has that floating-point mode.

    当调用创建包含操作数栈的栈帧的方法具有该浮点模式时，我们将操作数堆栈称为具有给定的浮点模式. 类似地，当包含该指令的方法具有该浮点模式时，我们会将Java虚拟机指令称为具有给定的浮点模式.

    If a float-extended-exponent value set is supported (§2.3.2), values of type float
    on an operand stack that is not FP-strict may range over that value set except
    where prohibited by value set conversion (§2.8.3). If a double-extended-exponent
    value set is supported (§2.3.2), values of type double on an operand stack that is
    not FP-strict may range over that value set except where prohibited by value set
    conversion.

    省略...

    In all other contexts, whether on the operand stack or elsewhere, and regardless
    of floating-point mode, floating-point values of type float and double may only
    range over the float value set and double value set, respectively. In particular, class
    and instance fields, array elements, local variables, and method parameters may
    only contain values drawn from the standard value sets.

    在所有其他上下文中，无论在操作数堆栈上还是在其他位置，无论浮点模式如何，float和double类型的浮点值都只能分别位于float值集和double值集的范围内.
    特别是，类和实例字段，数组元素，局部变量和方法参数只能包含从标准值集中得出的值.


    */



```