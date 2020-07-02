### 翻译Java虚拟机规范第三章第一节(JSR-337/Java8)

主要内容：`简单说明编译后的字节码文件的格式，可通过javap命令反编译该文件来查看指令`


```java

    /**

    3.1 Format of Examples

    示例格式

    This chapter consists mainly of examples of source code together with annotated
    listings of the Java Virtual Machine code that the javac compiler in Oracle’s JDK
    release 1.0.2 generates for the examples. The Java Virtual Machine code is written
    in the informal “virtual machine assembly language” output by Oracle's javap
    utility, distributed with the JDK release. You can use javap to generate additional
    examples of compiled methods.

    本章主要由源代码示例以及Oracle JDK 1.0.2版中的javac编译器为示例生成的Java虚拟机代码的带注释的清单组成.
    Java虚拟机代码是用Oracle的javap实用程序输出的非正式"虚拟机汇编语言"编写的，并随JDK版本一起分发.
    您可以使用javap生成编译方法的其他示例.

    The format of the examples should be familiar to anyone who has read assembly
    code. Each instruction takes the form:

    阅读汇编代码的任何人都应该熟悉示例的格式. 每条指令采用以下形式：

    <index> <opcode> [ <operand1> [ <operand2>... ]] [<comment>]

    The <index> is the index of the opcode of the instruction in the array that contains
    the bytes of Java Virtual Machine code for this method. Alternatively, the <index>
    may be thought of as a byte offset from the beginning of the method. The <opcode>
    is the mnemonic for the instruction's opcode, and the zero or more <operandN>
    are the operands of the instruction. The optional <comment> is given in end-of-line
    comment syntax:

    <index>是数组中指令的操作码的索引，该数组包含此方法的Java虚拟机代码的字节. 或者，可以将<index>视为距方法开头的字节偏移量.
    <opcode>是指令的操作码的助记符，零个或多个<operandN>是指令的操作数.
    可选的<comment>在行尾注释语法中给出：

    8 bipush 100 // Push int constant 100

    Some of the material in the comments is emitted by javap; the rest is supplied by
    the authors. The <index> prefacing each instruction may be used as the target of
    a control transfer instruction. For instance, a goto 8 instruction transfers control
    to the instruction at index 8. Note that the actual operands of Java Virtual Machine
    control transfer instructions are offsets from the addresses of the opcodes of those
    instructions; these operands are displayed by javap (and are shown in this chapter)
    as more easily read offsets into their methods.

    注释中的某些内容是由javap发出的. 其余的由作者提供. 每个指令前面的<index>可以用作控制传递指令的目标.
    例如，goto 8指令将控制权转移到索引8处的指令. 请注意，Java虚拟机控件转移指令的实际操作数与这些指令的操作码地址之间存在偏移量.
    这些操作数由javap显示（并在本章中显示），因为更容易将偏移量读取到它们的方法中.

    We preface an operand representing a run-time constant pool index with a hash
    sign and follow the instruction by a comment identifying the run-time constant pool
    item referenced, as in:

    我们在操作数前添加一个带有哈希符号的运行时常量池索引，并在指令后加上注释，以标识所引用的运行时常量池项，如下所示：

    10 ldc #1 // Push float constant 100.0

    or:

    9 invokevirtual #4 // Method Example.addTwo(II)I

    For the purposes of this chapter, we do not worry about specifying details such as
    operand sizes.

    就本章而言，我们不必担心指定诸如操作数大小之类的细节.

    */



```