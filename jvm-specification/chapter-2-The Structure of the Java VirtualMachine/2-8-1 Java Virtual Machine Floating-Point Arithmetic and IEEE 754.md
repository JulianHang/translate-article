### 翻译Java虚拟机规范第二章第八节-第一节(JSR-337/Java8)

主要内容：`介绍Java虚拟机浮点运算`

```java

    /**

    2.8.1 Java Virtual Machine Floating-Point Arithmetic

    Java虚拟机浮点运算

    The key differences between the floating-point arithmetic supported by the Java
    Virtual Machine and the IEEE 754 standard are:

    Java虚拟机和IEEE 754标准支持的浮点算法之间的主要区别是：

    • The floating-point operations of the Java Virtual Machine do not throw
    exceptions, trap, or otherwise signal the IEEE 754 exceptional conditions of
    invalid operation, division by zero, overflow, underflow, or inexact. The Java
    Virtual Machine has no signaling NaN value.

    Java虚拟机的浮点操作不会引发异常，陷阱或以其他方式发出信号，表明IEEE 754无效操作、被零除、上溢、下溢或不精确的特殊情况.

    • The Java Virtual Machine does not support IEEE 754 signaling floating-point
    comparisons.

    Java虚拟机不支持IEEE 754信令浮点比较.

    • The rounding operations of the Java Virtual Machine always use IEEE 754 round
    to nearest mode. Inexact results are rounded to the nearest representable value,
    with ties going to the value with a zero least-significant bit. This is the IEEE
    754 default mode. But Java Virtual Machine instructions that convert values
    of floating-point types to values of integral types round toward zero. The Java
    Virtual Machine does not give any means to change the floating-point rounding
    mode.

    Java虚拟机的舍入操作始终使用IEEE 754舍入到最近模式. 不精确的结果四舍五入到最接近的可表示值，并用最低有效位为零的值舍入. 这是IEEE 754的默认模式. 
    但是，将浮点类型的值转换为整数类型的值的Java虚拟机指令四舍五入为零. Java虚拟机不提供任何更改浮点舍入模式的方法.

    • The Java Virtual Machine does not support either the IEEE 754 single extended
    or double extended format, except insofar as the double and double-extendedexponent 
    value sets may be said to support the single extended format. The
    float-extended-exponent and double-extended-exponent value sets, which may
    optionally be supported, do not correspond to the values of the IEEE 754
    extended formats: the IEEE 754 extended formats require extended precision as
    well as extended exponent range.

    Java虚拟机不支持IEEE 754单一扩展格式或双重扩展格式，除非可以说double和double-extended指数值集支持单一扩展格式.
    可以可选地支持的float扩展指数和double扩展指数值集不对应于IEEE 754扩展格式的值：IEEE 754扩展格式需要扩展的精度以及扩展的指数范围.
     
    */



```