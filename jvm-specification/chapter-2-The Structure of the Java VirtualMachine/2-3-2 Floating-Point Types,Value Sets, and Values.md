### 翻译Java虚拟机规范第二章第三节-第二节-(JSR-337/Java8)

主要内容：`简单介绍浮点型的概念和范围`


```java

    /**

    2.3.2 Floating-Point Types, Value Sets, and Values

    浮点类型、值集和值

    The floating-point types are float and double, which are conceptually associated
    with the 32-bit single-precision and 64-bit double-precision format IEEE 754
    values and operations as specified in IEEE Standard for Binary Floating-Point
    Arithmetic (ANSI/IEEE Std. 754-1985, New York).

    浮点类型为float和double，它们在概念上与32位单精度和64位双精度格式IEEE 754值以及IEEE二进制浮点算术标准中指定的操作相关联.

    The IEEE 754 standard includes not only positive and negative sign-magnitude
    numbers, but also positive and negative zeros, positive and negative infinities, and
    a special Not-a-Number value (hereafter abbreviated as "NaN"). The NaN value
    is used to represent the result of certain invalid operations such as dividing zero
    by zero.

    IEEE 754标准不仅包括正负大小号，还包括正零号，负零号，正负无穷以及特殊的非数字值（以下简称为"NaN"）. NaN值用于表示某些无效运算的结果，例如零除零.

    Every implementation of the Java Virtual Machine is required to support two
    standard sets of floating-point values, called the float value set and the double value
    set. In addition, an implementation of the Java Virtual Machine may, at its option,
    support either or both of two extended-exponent floating-point value sets, called
    the float-extended-exponent value set and the double-extended-exponent value set.
    These extended-exponent value sets may, under certain circumstances, be used
    instead of the standard value sets to represent the values of type float or double.

    Java虚拟机的每个实现都需要支持两个标准的浮点值集，称为浮点值集和双精度值集. 另外，Java虚拟机的实现可以选择支持两个或其中一个扩展指数浮点值集，称为float-extended-exponent值集和double-extended-exponent值集.
    在某些情况下，可以使用这些扩展指数值集代替标准值集来表示float或double类型的值.

    The finite nonzero values of any floating-point value set can all be expressed in
    the form s ⋅ m ⋅ 2(e - N + 1), where s is +1 or -1, m is a positive integer less than
    2N, and e is an integer between Emin = -(2K-1-2) and Emax = 2K-1-1, inclusive,
    and where N and K are parameters that depend on the value set. Some values can
    be represented in this form in more than one way; for example, supposing that a
    value v in a value set might be represented in this form using certain values for
    s, m, and e, then if it happened that m were even and e were less than 2K-1, one
    could halve m and increase e by 1 to produce a second representation for the same
    value v. A representation in this form is called normalized if m ≥ 2N-1; otherwise
    the representation is said to be denormalized. If a value in a value set cannot be
    represented in such a way that m ≥ 2N-1, then the value is said to be a denormalized
    value, because it has no normalized representation.

    任何浮点值集的有限非零值都可以以s⋅m⋅2（e-N + 1）的形式表示，其中s为+1或-1，m为小于2N的正整数，并且e是Emin =-（2K-1-2）和Emax = 2K-1-1（含）之间的整数，其中N和K是取决于值集的参数. 某些值可以通过多种方式以这种形式表示.
    例如，假设某个值v可以使用s，m和e来表示，如果m为偶数且e小于2k-1，则可以将m减半并将e加1以产生相同值v的第二种表示形式. 如果m≥2N-1，这种形式的表示称为归一化. 否则，表示被归一化.
    如果不能以m≥2N-1的方式表示值，则将该值称为非规格化值，因为它没有规格化表示.

    The constraints on the parameters N and K (and on the derived parameters Emin
    and Emax) for the two required and two optional floating-point value sets are
    summarized in Table 2.3.2-A.

    表2.3.2-A中总结了两个必需的浮点值集和两个可选的浮点值集对参数N和K（以及导出的参数Emin和Emax）的约束.

    Where one or both extended-exponent value sets are supported by an
    implementation, then for each supported extended-exponent value set there is
    a specific implementation-dependent constant K, whose value is constrained by
    Table 2.3.2-A; this value K in turn dictates the values for Emin and Emax.

    如果一个实现支持一个或两个扩展指数值集，则对于每个受支持的扩展指数值集，都有一个特定的实现相关常数K，其值受表2.3.2-A约束. 该值K又决定了Emin和Emax的值.

    Each of the four value sets includes not only the finite nonzero values that are
    ascribed to it above, but also the five values positive zero, negative zero, positive
    infinity, negative infinity, and NaN.

    四个值集的每一个不仅包括有限非零值，而且还包括正零，负零，正无穷大，负无穷大和NaN的五个值.

    Note that the constraints in Table 2.3.2-A are designed so that every element of the
    float value set is necessarily also an element of the float-extended-exponent value
    set, the double value set, and the double-extended-exponent value set. Likewise,
    each element of the double value set is necessarily also an element of the doubleextended-exponent value set. Each extended-exponent value set has a larger range
    of exponent values than the corresponding standard value set, but does not have
    more precision.

    请注意，表2.3.2-A中的约束经过了设计，以使浮点值集的每个元素也必须是浮点扩展指数值集，双精度值集和双扩展指数值的元素. 
    同样，双精度值集的每个元素也必定也是双扩展指数值集的元素。 每个扩展指数值集的指数值范围比相应的标准值集大，但精度不高.

    The elements of the float value set are exactly the values that can be represented
    using the single floating-point format defined in the IEEE 754 standard, except
    that there is only one NaN value (IEEE 754 specifies 224-2 distinct NaN values).
    The elements of the double value set are exactly the values that can be represented
    using the double floating-point format defined in the IEEE 754 standard, except
    that there is only one NaN value (IEEE 754 specifies 253-2 distinct NaN values).
    Note, however, that the elements of the float-extended-exponent and doubleextended-exponent value sets defined here do not correspond to the values that
    can be represented using IEEE 754 single extended and double extended formats,
    respectively. This specification does not mandate a specific representation for the
    values of the floating-point value sets except where floating-point values must be
    represented in the class file format (§4.4.4, §4.4.5).

    浮点值集的元素恰好是可以使用IEEE 754标准中定义的单个浮点格式表示的值，除了只有一个NaN值（IEEE 754指定224-2个不同的NaN值）.
    double值集的元素正是可以使用IEEE 754标准中定义的double浮点格式表示的值，除了只有一个NaN值（IEEE 754指定253-2个不同的NaN值）.
    但是请注意，此处定义的float-extended-exponent和doubleextended-exponent值集的元素分别不对应于可以使用IEEE 754单扩展和双扩展格式表示的值.
    除了必须以类文件格式（第4.4.4节，第4.4.5节）表示浮点值的地方以外，本规范没有要求对浮点值集的值进行特定的表示.

    The float, float-extended-exponent, double, and double-extended-exponent value
    sets are not types. It is always correct for an implementation of the Java Virtual
    Machine to use an element of the float value set to represent a value of type float;
    however, it may be permissible in certain contexts for an implementation to use
    an element of the float-extended-exponent value set instead. Similarly, it is always
    correct for an implementation to use an element of the double value set to represent
    a value of type double; however, it may be permissible in certain contexts for
    an implementation to use an element of the double-extended-exponent value set
    instead.

    float、float扩展指数、double和double扩展指数值集不是类型. 对于Java虚拟机的实现，使用float值集的元素表示float类型的值始终是正确的.
    然而在某些情况下，对于一个实现来说，使用float扩展的元素来代替可能是允许的.
    同样，对于一个实现，使用double值集的元素表示double类型的值始终是正确的.
    然而在某些情况下，对于一个实现来说，使用double扩展的元素来代替可能是允许的.

    Except for NaNs, values of the floating-point value sets are ordered. When
    arranged from smallest to largest, they are negative infinity, negative finite values,
    positive and negative zero, positive finite values, and positive infinity.

    除NaN之外，浮点值集的值是有序的. 从最小到最大排列时，它们是负无穷大，负有限值，正负零，正有限值和正无穷大.

    Floating-point positive zero and floating-point negative zero compare as equal, but
    there are other operations that can distinguish them; for example, dividing 1.0 by
    0.0 produces positive infinity, but dividing 1.0 by -0.0 produces negative infinity.

    浮点正零和浮点负零的比较是相等的，但是还有其他操作可以区分它们. 例如，将1.0除以0.0将产生正无穷大，但将1.0除以-0.0将产生负无穷大.

    NaNs are unordered, so numerical comparisons and tests for numerical equality
    have the value false if either or both of their operands are NaN. In particular, a
    test for numerical equality of a value against itself has the value false if and only
    if the value is NaN. A test for numerical inequality has the value true if either
    operand is NaN.

    NaN是无序的，因此如果一个或两个操作数均为NaN，则数值比较和数值相等性检验的值为false. 
    特别是，当且仅当该值为NaN时，用于针对其自身的数值相等性的测试的值为false.
    如果任一操作数为NaN，则数值不等式的检验的值为true.

    */




```