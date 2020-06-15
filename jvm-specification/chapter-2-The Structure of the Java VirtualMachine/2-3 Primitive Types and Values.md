### 翻译Java虚拟机规范第二章第三节-(JSR-337/Java8)

主要内容：`简单介绍Java虚拟机支持的基本数据类型、布尔类型、returnAddress类型，特别说明一下returnAddress是指向操作码的指针，至于什么是操作码可以搜索一下`

```java


    /**

    Primitive Types and Values

    基本类型和值

    The primitive data types supported by the Java Virtual Machine are the numeric
    types, the boolean type (§2.3.4), and the returnAddress type (§2.3.3).

    Java虚拟机支持的基本数据类型是数字类型、布尔类型（第2.3.4节）和returnAddress类型（第2.3.3节）.

    The numeric types consist of the integral types (§2.3.1) and the floating-point types
    (§2.3.2).

    数值类型由整数类型（第2.3.1节）和浮点类型（第2.3.2节）组成.

    The integral types are:

    整数类型：

    • byte, whose values are 8-bit signed two's-complement integers, and whose
    default value is zero

    字节，其值为8位有符号的二进制补码整数，并且其默认值为零.

    • short, whose values are 16-bit signed two's-complement integers, and whose
    default value is zero

    short，其值为16位有符号的二进制补码整数，其默认值为零.

    • int, whose values are 32-bit signed two's-complement integers, and whose
    default value is zero

    int，其值为32位有符号的二进制补码整数，并且其默认值为零.

    • long, whose values are 64-bit signed two's-complement integers, and whose
    default value is zero

    long，其值为64位有符号的二进制补码整数，并且其默认值为零.

    • char, whose values are 16-bit unsigned integers representing Unicode code
    points in the Basic Multilingual Plane, encoded with UTF-16, and whose default
    value is the null code point ('\u0000')

    char，其值是16位无符号整数，表示基本多语言平面中的Unicode代码点，并以UTF-16编码，其默认值为空代码点（'\ u0000'）.

    The floating-point types are:

    浮点类型：

    • float, whose values are elements of the float value set or, where supported, the
    float-extended-exponent value set, and whose default value is positive zero

    float，其值是float值集的元素，或者在受支持的情况下是float-extended-exponent值集的元素，其默认值为正零.

    • double, whose values are elements of the double value set or, where supported,
    the double-extended-exponent value set, and whose default value is positive zero

    double，其值是double值集的元素，或者在支持的情况下是double-expended-exponent值集的元素，其默认值为正零.

    The values of the boolean type encode the truth values true and false, and the
    default value is false.

    布尔类型的值对真值true和false进行编码，默认值为false.

    The First Edition of The Java® Virtual Machine Specification did not consider boolean
    to be a Java Virtual Machine type. However, boolean values do have limited support in
    the Java Virtual Machine. The Second Edition of The Java® Virtual Machine Specification
    clarified the issue by treating boolean as a type.

    Java虚拟机规范的第一版未将布尔值视为Java虚拟机类型. 但是，布尔值在Java虚拟机中的支持确实有限. 《Java虚拟机规范》第二版通过将布尔值视为类型来澄清问题.

    The values of the returnAddress type are pointers to the opcodes of Java Virtual
    Machine instructions. Of the primitive types, only the returnAddress type is not
    directly associated with a Java programming language type.

    returnAddress类型的值是指向Java虚拟机指令的操作码的指针. 在原始类型中，只有returnAddress类型没有与Java编程语言类型直接关联.

    */

```