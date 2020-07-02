### 翻译Java虚拟机规范第三章第四节(JSR-337/Java8)

主要内容：`介绍访问运行时常量池的指令`

```java

    /**

    3.4 Accessing the Run-Time Constants Pool

    访问运行时常量池

    Many numeric constants, as well as objects, fields, and methods, are accessed
    via the run-time constant pool of the current class. Object access is considered
    later (§3.8). Data of types int, long, float, and double, as well as references
    to instances of class String, are managed using the ldc, ldc_w, and ldc2_w
    instructions.

    可通过当前类的运行时常量池访问许多数字常量以及对象，字段和方法. 稍后考虑对象访问（第3.8节）.
    使用ldc，ldc_w和ldc2_w指令管理int、long、float和double类型的数据以及对String类实例的引用.

    The ldc and ldc_w instructions are used to access values in the run-time constant
    pool (including instances of class String) of types other than double and long.
    The ldc_w instruction is used in place of ldc only when there is a large number of
    run-time constant pool items and a larger index is needed to access an item. The
    ldc2_w instruction is used to access all values of types double and long; there is
    no non-wide variant.

    ldc和ldc_w指令用于访问类型不是double和long的运行时常量池（包括String类的实例）中的值.
    仅当存在大量运行时常量池项目并且需要更大的索引来访问项目时，才使用ldc_w指令代替ldc.
    ldc2_w指令用于访问double和long类型的所有值；没有非广泛的变体.

    Integral constants of types byte, char, or short, as well as small int values,
    may be compiled using the bipush, sipush, or iconst_<i> instructions (§3.2).
    Certain small floating-point constants may be compiled using the fconst_<f> and
    dconst_<d> instructions.

    可以使用bipush，sipush或iconst_ <i>指令（第3.2节）来编译类型为byte、char或short的整数常量以及较小的int值.
    某些小的浮点常量可以使用fconst_ <f>和dconst_ <d>指令进行编译.

    In all of these cases, compilation is straightforward. For instance, the constants for:

    在所有这些情况下，编译都是很简单的. 例如，以下常量：

        void useManyNumeric() {
            int i = 100;
            int j = 1000000;
            long l1 = 1;
            long l2 = 0xffffffff;
            double d = 2.2;
            ...do some calculations...
        }

    are set up as follows:

    设置如下：

        Method void useManyNumeric()
        0 bipush 100 // Push small int constant with bipush
        2 istore_1
        3 ldc #1 // Push large int constant (1000000) with ldc
        5 istore_2
        6 lconst_1 // A tiny long value uses small fast lconst_1
        7 lstore_3
        8 ldc2_w #6 // Push long 0xffffffff (that is, an int -1)


    */




```