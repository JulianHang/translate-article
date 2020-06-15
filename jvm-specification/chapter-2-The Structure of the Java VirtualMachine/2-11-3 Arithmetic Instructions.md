### 翻译Java虚拟机规范第二章第十一节-第三节(JSR-337/Java8)

主要内容：`阐述算数指令`

```java

    /**

    2.11.3 Arithmetic Instructions

    算术指令

    The arithmetic instructions compute a result that is typically a function of two
    values on the operand stack, pushing the result back on the operand stack. There
    are two main kinds of arithmetic instructions: those operating on integer values and
    those operating on floating-point values. Within each of these kinds, the arithmetic
    instructions are specialized to Java Virtual Machine numeric types. There is no
    direct support for integer arithmetic on values of the byte, short, and char types
    (§2.11.1), or for values of the boolean type; those operations are handled by
    instructions operating on type int. Integer and floating-point instructions also
    differ in their behavior on overflow and divide-by-zero. The arithmetic instructions
    are as follows:

    算术指令计算的结果通常是操作数栈上两个值的函数，将结果推回到操作数栈上. 算术指令主要有两种：对整数值进行运算的指令和对浮点值进行运算的指令.
    在每种类型中，算术指令专用于Java虚拟机数字类型. 不直接支持对byte，short和char类型的值（第2.11.1节）的整数算术，或者对布尔类型的值的支持.
    这些操作由对int类型进行操作的指令处理. 整数指令和浮点指令在溢出和被零除时的行为也有所不同. 算术指令如下：

    The semantics of the Java programming language operators on integer and floatingpoint 
    values (JLS §4.2.2, JLS §4.2.4) are directly supported by the semantics of
    the Java Virtual Machine instruction set.

    Java虚拟机指令集的语义直接支持Java程序语言运算符对整数和浮点值（JLS§4.2.2，JLS§4.2.4）的语义.

    The Java Virtual Machine does not indicate overflow during operations on integer
    data types. The only integer operations that can throw an exception are the integer
    divide instructions (idiv and ldiv) and the integer remainder instructions (irem and
    lrem), which throw an ArithmeticException if the divisor is zero.

    在对整数数据类型进行操作期间，Java虚拟机不指示溢出.
    唯一可以引发异常的整数运算是整数除法指令（idiv和ldiv）和整数余数指令（irem和lrem），如果除数为零，则它们将引发ArithmeticException.

    Java Virtual Machine operations on floating-point numbers behave as specified in
    IEEE 754. In particular, the Java Virtual Machine requires full support of IEEE
    754 denormalized floating-point numbers and gradual underflow, which make it
    easier to prove desirable properties of particular numerical algorithms.

    浮点数上的Java虚拟机操作的行为符合IEEE 754中的规定.
    特别是，Java虚拟机需要完全支持IEEE 754非规范化浮点数和渐进下溢，这使得更容易证明特定数值算法的理想特性.

    The Java Virtual Machine requires that floating-point arithmetic behave as if every
    floating-point operator rounded its floating-point result to the result precision.
    Inexact results must be rounded to the representable value nearest to the infinitely
    precise result; if the two nearest representable values are equally near, the one
    having a least significant bit of zero is chosen. This is the IEEE 754 standard's
    default rounding mode, known as round to nearest mode.

    Java虚拟机要求浮点算术的行为就像每个浮点运算符将其浮点结果四舍五入到结果精度一样.
    不精确的结果必须四舍五入到最接近无限精确结果的可表示值；如果两个最接近的可表示值都相等，则选择最低有效位为零的那个.
    这是IEEE 754标准的默认舍入模式，称为"舍入到最近模式".

    The Java Virtual Machine uses the IEEE 754 round towards zero mode when
    converting a floating-point value to an integer. This results in the number being
    truncated; any bits of the significand that represent the fractional part of the operand
    value are discarded. Round towards zero mode chooses as its result the type's value
    closest to, but no greater in magnitude than, the infinitely precise result.
    The Java Virtual Machine's floating-point operators do not throw run-time
    exceptions (not to be confused with IEEE 754 floating-point exceptions). An
    operation that overflows produces a signed infinity, an operation that underflows
    produces a denormalized value or a signed zero, and an operation that has no
    mathematically definite result produces NaN. All numeric operations with NaN as
    an operand produce NaN as a result.

    当将浮点值转换为整数时，Java虚拟机使用IEEE 754向零模式舍入. 这导致数字被截断；表示操作数值的小数部分的有效位的任何位都将被丢弃.
    向零取整模式选择的结果是类型值最接近但幅度不超过无限精确的结果。Java虚拟机的浮点运算符不会抛出运行时异常（不要与IEEE 754浮点异常混淆）.
    溢出的操作将生成有符号无穷大，下溢的操作将生成非规范化的值或有符号的零，而在数学上没有确定结果的操作将生成NaN.
    以NaN作为操作数的所有数值运算都会产生NaN.

    Comparisons on values of type long (lcmp) perform a signed comparison.
    Comparisons on values of floating-point types (dcmpg, dcmpl, fcmpg, fcmpl) are
    performed using IEEE 754 nonsignaling comparisons.

    对long类型（lcmp）类型的值进行比较会执行带符号的比较. 使用IEEE 754非信令对浮点类型（dcmpg，dcmpl，fcmpg，fcmpl）的值进行比较.

    */



```