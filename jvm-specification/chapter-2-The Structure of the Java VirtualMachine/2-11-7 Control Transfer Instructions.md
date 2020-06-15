### 翻译Java虚拟机规范第二章第十一节-第七节(JSR-337/Java8)

主要内容：`简单介绍控制转移的指令，个人理解就是if/else、switch等相关指令`

```java

    /**

    2.11.7 Control Transfer Instructions

    控制转移指令

    The control transfer instructions conditionally or unconditionally cause the Java
    Virtual Machine to continue execution with an instruction other than the one
    following the control transfer instruction. They are:

    控制转移指令有条件或无条件地使Java虚拟机继续执行除控制转移指令之后的另一条指令. 它们是：

    • Conditional branch: ifeq, ifne, iflt, ifle, ifgt, ifge, ifnull, ifnonnull, if_icmpeq,
    if_icmpne, if_icmplt, if_icmple, if_icmpgt if_icmpge, if_acmpeq, if_acmpne.

    条件分支：ifeq、ifne、iflt、ifle、ifgt、ifge、ifnull、ifnonnull、if_icmpeq、if_icmpne、if_icmplt、if_icmple、if_icmpgt、if_icmpge、if_acmpeq、if_acmpne.

    • Compound conditional branch: tableswitch, lookupswitch.

    复合条件分支：tableswitch，lookupswitch.

    • Unconditional branch: goto, goto_w, jsr, jsr_w, ret.

    无条件分支：goto、goto_w、jsr、jsr_w、ret.

    The Java Virtual Machine has distinct sets of instructions that conditionally
    branch on comparison with data of int and reference types. It also has distinct
    conditional branch instructions that test for the null reference and thus it is not
    required to specify a concrete value for null (§2.4).

    Java虚拟机具有不同的指令集，它们有条件地在与int和引用类型的数据进行比较时分支. 它还具有用于测试null引用的独特条件分支指令，因此不需要为null指定具体值（第2.4节）.

    Conditional branches on comparisons between data of types boolean, byte,
    char, and short are performed using int comparison instructions (§2.11.1). A
    conditional branch on a comparison between data of types long, float, or double
    is initiated using an instruction that compares the data and produces an int
    result of the comparison (§2.11.3). A subsequent int comparison instruction tests
    this result and effects the conditional branch. Because of its emphasis on int
    comparisons, the Java Virtual Machine provides a rich complement of conditional
    branch instructions for type int.

    使用int比较指令（第2.11.1节）在boolean，byte，char和short类型的数据之间进行比较的条件分支.
    使用对数据进行比较并产生比较的int结果的指令来初始化long，float或double类型数据之间的比较的条件分支.
    后续的int比较指令将测试此结果并影响条件分支.
    由于Java虚拟机强调int比较，因此为int类型提供了条件分支指令的丰富补充.

    All int conditional control transfer instructions perform signed comparisons.

    所有int条件控制转移指令均执行带符号的比较.

    */



```