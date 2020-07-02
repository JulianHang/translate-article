### 翻译Java虚拟机规范第三章第五节(JSR-337/Java8)

主要内容：`通过示例来说明Java虚拟机处理while循环的指令`


```java

    /**

    3.5 More Control Examples

    更多控制示例

    Compilation of for statements was shown in an earlier section (§3.2). Most of the
    Java programming language's other control constructs (if-then-else, do, while,
    break, and continue) are also compiled in the obvious ways. The compilation of
    switch statements is handled in a separate section (§3.10), as are the compilation
    of exceptions (§3.12) and the compilation of finally clauses (§3.13).

    在较早的部分（第3.2节）中显示了for语句的编译. 大多数Java编程语言的其他控制结构（if-then-else，do，while，break和Continue）也以明显的方式进行编译.
    switch语句的编译在单独的部分（第3.10节）中进行处理，异常的编译（第3.12节）和finally子句的编译（第3.13节）也进行处理.

    As a further example, a while loop is compiled in an obvious way, although the
    specific control transfer instructions made available by the Java Virtual Machine
    vary by data type. As usual, there is more support for data of type int, for example:

    作为进一步的示例，尽管Java虚拟机提供的特定控制传输指令因数据类型而异，但显然以一种方式编译了while循环.
    像往常一样，对int类型的数据有更多的支持，例如：

        void whileInt() {
            int i = 0;
            while (i < 100) {
                i++;
            }
        }
    
    is compiled to:

        Method void whileInt()
        0 iconst_0
        1 istore_1
        2 goto 8
        5 iinc 1 1
        8 iload_1
        9 bipush 100
        11 if_icmplt 5
        14 return

    Note that the test of the while statement (implemented using the if_icmplt
    instruction) is at the bottom of the Java Virtual Machine code for the loop. (This
    was also the case in the spin examples earlier.) The test being at the bottom of the
    loop forces the use of a goto instruction to get to the test prior to the first iteration of
    the loop. If that test fails, and the loop body is never entered, this extra instruction
    is wasted. However, while loops are typically used when their body is expected
    to be run, often for many iterations. For subsequent iterations, putting the test at
    the bottom of the loop saves a Java Virtual Machine instruction each time around
    the loop: if the test were at the top of the loop, the loop body would need a trailing
    goto instruction to get back to the top.

    请注意，while语句（使用if_icmplt指令实现）的测试位于循环的Java虚拟机代码的底部. （前面的旋转示例中也是这种情况.）位于循环底部的测试强制使用goto指令在循环的第一次迭代之前到达测试.
    如果该测试失败，绝不会进入循环主体，则会浪费额外的指令. 然而，通常在希望运行它们的主体时使用while循环，通常用于多次迭代. 
    对于后续迭代，将测试置于循环的底部可在每次循环时保存一条Java虚拟机指令：如果测试位于循环的顶部，则循环主体将需要尾随goto指令才能返回到顶部.

    Control constructs involving other data types are compiled in similar ways, but
    must use the instructions available for those data types. This leads to somewhat
    less efficient code because more Java Virtual Machine instructions are needed, for
    example:

    涉及其他数据类型的控制结构以类似的方式进行编译，但必须使用可用于这些数据类型的指令. 由于需要更多的Java虚拟机指令，因此这会导致效率较低的代码，例如：

        void whileDouble() {
            double i = 0.0;
            while (i < 100.1) {
                i++;
            }
        }
    
    is compiled to:

        Method void whileDouble()
        0 dconst_0
        1 dstore_1
        2 goto 9
        5 dload_1
        6 dconst_1
        7 dadd
        8 dstore_1
        9 dload_1
        10 ldc2_w #4 // Push double constant 100.1
        13 dcmpg // To compare and branch we have to use...
        14 iflt 5 // ...two instructions
        17 return
    
    Each floating-point type has two comparison instructions: fcmpl and fcmpg for type
    float, and dcmpl and dcmpg for type double. The variants differ only in their
    treatment of NaN. NaN is unordered (§2.3.2), so all floating-point comparisons
    fail if either of their operands is NaN. The compiler chooses the variant of the
    comparison instruction for the appropriate type that produces the same result
    whether the comparison fails on non-NaN values or encounters a NaN. For
    instance:

    每个浮点类型都有两个比较指令：fcmpl和fcmpg用于float类型；dcmpl和dcmpg用于double类型. 这些变体仅在处理NaN有所不同.
    NaN是无序的（第2.3.2节），因此如果两个操作数之一是NaN，所有浮点比较将失败.

        int lessThan100(double d) {
            if (d < 100.0) {
                return 1;
            } else {
                return -1;
            }
        }

    compiles to:

    Method int lessThan100(double)
    0 dload_1
    1 ldc2_w #4 // Push double constant 100.0
    4 dcmpg // Push 1 if d is NaN or d > 100.0;
    // push 0 if d == 100.0
    5 ifge 10 // Branch on 0 or 1
    8 iconst_1
    9 ireturn
    10 iconst_m1
    11 ireturn

    If d is not NaN and is less than 100.0, the dcmpg instruction pushes an int -1 onto
    the operand stack, and the ifge instruction does not branch. Whether d is greater
    than 100.0 or is NaN, the dcmpg instruction pushes an int 1 onto the operand
    stack, and the ifge branches. If d is equal to 100.0, the dcmpg instruction pushes
    an int 0 onto the operand stack, and the ifge branches.

    如果d不是NaN并且小于100.0，则dcmpg指令将int -1压入操作数栈，并且ifge指令不会分支.
    无论d大于100.0还是NaN，dcmpg指令都会将int 1压入操作数栈，并且ifge分支. 
    如果d等于100.0，则dcmpg指令将int 0压入操作数堆栈，并且ifge分支.

    The dcmpl instruction achieves the same effect if the comparison is reversed:

        int greaterThan100(double d) {
            if (d > 100.0) {
                return 1;
            } else {
                return -1;
            }
        }
    
    becomes:

    Method int greaterThan100(double)
    0 dload_1
    1 ldc2_w #4 // Push double constant 100.0
    4 dcmpl // Push -1 if d is NaN or d < 100.0;
    // push 0 if d == 100.0
    5 ifle 10 // Branch on 0 or -1
    8 iconst_1
    9 ireturn
    10 iconst_m1
    11 ireturn

    Once again, whether the comparison fails on a non-NaN value or because it is
    passed a NaN, the dcmpl instruction pushes an int value onto the operand stack
    that causes the ifle to branch. If both of the dcmp instructions did not exist, one of
    the example methods would have had to do more work to detect NaN.

    再一次，无论是针对非NaN值的比较失败还是因为传递了NaN而失败，dcmpl指令都会将int值压入操作数堆栈，从而导致ifle分支.
    如果两个dcmp指令都不存在，则示例方法d的其中一个将不得不做更多的工作来检测NaN.


    */



```