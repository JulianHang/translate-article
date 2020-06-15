### 翻译Java虚拟机规范第二章第十一节-第四节(JSR-337/Java8)

主要内容：`阐述类型转换相关的指令，比如int到long这样子的一个类型转换，在转换过程中是否会发生精度丢失或溢出`

```java

    /**

    2.11.4 Type Conversion Instructions

    类型转换指令

    The type conversion instructions allow conversion between Java Virtual Machine
    numeric types. These may be used to implement explicit conversions in user code
    or to mitigate the lack of orthogonality in the instruction set of the Java Virtual
    Machine.

    类型转换指令允许在Java虚拟机数字类型之间进行转换. 这些可用于实现用户代码中的显式转换或减轻Java虚拟机的指令集中缺乏正交性的情况.

    The Java Virtual Machine directly supports the following widening numeric
    conversions:

    Java虚拟机直接支持以下扩展的数字转换：

    • int to long, float, or double

    int -> long , int -> float , int -> double

    • long to float or double

    long -> float , long -> double

    • float to double

    floaat -> double

    The widening numeric conversion instructions are i2l, i2f, i2d, l2f, l2d, and f2d. The
    mnemonics for these opcodes are straightforward given the naming conventions
    for typed instructions and the punning use of 2 to mean "to." For instance, the i2d
    instruction converts an int value to a double.

    扩展数字转换指令为i21，i2f，i2d，l2f，l2d和f2d. 考虑到类型化指令的命名约定以及将2表示为"to"的简洁用法，这些操作码的助记符非常简单. 例如，i2d指令将int值转换为double.

    Most widening numeric conversions do not lose information about the overall
    magnitude of a numeric value. Indeed, conversions widening from int to long and
    int to double do not lose any information at all; the numeric value is preserved
    exactly. Conversions widening from float to double that are FP-strict (§2.8.2)
    also preserve the numeric value exactly; only such conversions that are not FPstrict 
    may lose information about the overall magnitude of the converted value.

    大多数扩展的数值转换不会丢失有关数值总大小的信息. 实际上，从int到long的转换以及从int到double的转换根本不会丢失任何信息. 数值将精确保留.
    FP-strict（第2.8.2节）从float到double的转换也精确地保留了数值；只有非FP-strict的此类转换可能会丢失有关转换值的整体大小的信息.

    Conversions from int to float, or from long to float, or from long to double,
    may lose precision, that is, may lose some of the least significant bits of the value;
    the resulting floating-point value is a correctly rounded version of the integer value,
    using IEEE 754 round to nearest mode.

    从int到float或从long到float或从long到double的转换可能会失去精度，也就是说，可能会丢失该值的某些最低有效位；使用IEEE 754舍入到最接近的模式时，得到的浮点值是整数值的正确舍入版本.

    Despite the fact that loss of precision may occur, widening numeric conversions
    never cause the Java Virtual Machine to throw a run-time exception (not to be
    confused with an IEEE 754 floating-point exception).

    尽管可能会发生精度损失，但是扩大数字转换范围决不会导致Java虚拟机抛出运行时异常（不要与IEEE 754浮点异常混淆）.

    A widening numeric conversion of an int to a long simply sign-extends the two'scomplement 
    representation of the int value to fill the wider format. A widening
    numeric conversion of a char to an integral type zero-extends the representation
    of the char value to fill the wider format.

    将int扩展为long的简单数字符号扩展了int值的二进制补码表示形式，以填充更宽的格式. char的数字转换扩展为整数类型的零，char值的表示形式扩展为填充更宽的格式.

    Note that widening numeric conversions do not exist from integral types byte,
    char, and short to type int. As noted in §2.11.1, values of type byte, char, and
    short are internally widened to type int, making these conversions implicit.

    请注意，整数类型byte，char和short不存在扩展为int类型的数字转换. 如第2.11.1节所述，在内部将类型为byte，char和short的值扩展为int类型，使这些转换隐式进行.

    The Java Virtual Machine also directly supports the following narrowing numeric
    conversions:

    Java虚拟机还直接支持以下缩小的数字转换：

    • int to byte, short, or char

    int -> byte , int -> short , int -> char

    • long to int

    long -> int 

    • float to int or long

    float -> int , float -> long

    • double to int, long, or float

    double -> int , double -> long , double -> float

    The narrowing numeric conversion instructions are i2b, i2c, i2s, l2i, f2i, f2l, d2i,
    d2l, and d2f. A narrowing numeric conversion can result in a value of different
    sign, a different order of magnitude, or both; it may thereby lose precision.

    缩小数字转换指令为i2b，i2c，i2s，l2i，f2i，f21，d2i，d2l和d2f. 缩小的数字转换可能导致符号值不同，数量级不同或两者兼而有之； 因此可能会失去精度.

    A narrowing numeric conversion of an int or long to an integral type T simply
    discards all but the n lowest-order bits, where n is the number of bits used to
    represent type T. This may cause the resulting value not to have the same sign as
    the input value.

    从int或long到整数类型T的变窄数字转换会简单地丢弃除n个最低位之外的所有位，其中n是用于表示类型T的位数. 这可能导致结果值的符号与输入值不同.

    In a narrowing numeric conversion of a floating-point value to an integral type T,
    where T is either int or long, the floating-point value is converted as follows:

    在将浮点值缩小为整数类型T（其中T为int或long）的数字转换中，浮点值的转换如下：

    • If the floating-point value is NaN, the result of the conversion is an int or long 0.

    如果浮点值为NaN，则转换结果为int或long 0.

    • Otherwise, if the floating-point value is not an infinity, the floating-point value
    is rounded to an integer value V using IEEE 754 round towards zero mode. There
    are two cases:

    否则，如果浮点值不是无穷大，则使用IEEE 754向零模式舍入将浮点值舍入为整数值V. 有两种情况：

    – If T is long and this integer value can be represented as a long, then the result
    is the long value V.

    如果T为long，并且此整数值可以表示为long，则结果为long值V.

    – If T is of type int and this integer value can be represented as an int, then
    the result is the int value V.

    如果T为int类型，并且此整数值可以表示为int，则结果为int值V.

    Otherwise:

    否则：

    – Either the value must be too small (a negative value of large magnitude or
    negative infinity), and the result is the smallest representable value of type int
    or long.

    该值必须太小（负值较大或负无穷大），结果是int或long类型的最小可表示值.

    – Or the value must be too large (a positive value of large magnitude or positive
    infinity), and the result is the largest representable value of type int or long.

    或该值必须太大（正值较大或正无穷大），结果是int或long类型的最大可表示值.

    A narrowing numeric conversion from double to float behaves in accordance
    with IEEE 754. The result is correctly rounded using IEEE 754 round to nearest
    mode. A value too small to be represented as a float is converted to a positive
    or negative zero of type float; a value too large to be represented as a float is
    converted to a positive or negative infinity. A double NaN is always converted to
    a float NaN.

    从double到float的变窄数字转换的行为符合IEEE754. 使用IEEE 754四舍五入到最接近的模式可以正确舍入结果.
    太小而无法表示为float的值将转换为float类型的正零或负零；太大而无法表示为浮点数的值将转换为正或负无穷大.
    double NaN始终会转换为float NaN.
    
    Despite the fact that overflow, underflow, or loss of precision may occur, narrowing
    conversions among numeric types never cause the Java Virtual Machine to throw a
    run-time exception (not to be confused with an IEEE 754 floating-point exception).

    尽管可能会发生溢出，下溢或精度损失，但缩小数字类型之间的转换决不会导致Java虚拟机引发运行时异常（不要与IEEE 754浮点异常相混淆）.

    */


```