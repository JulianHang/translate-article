### 翻译Java虚拟机规范第二章第十一节(JSR-337/Java8)

主要内容：`简单地概述指令`

```java

    /**

    2.11 Instruction Set Summary

    指令集概述

    A Java Virtual Machine instruction consists of a one-byte opcode specifying
    the operation to be performed, followed by zero or more operands supplying
    arguments or data that are used by the operation. Many instructions have no
    operands and consist only of an opcode.

    Java虚拟机指令由一个字节的操作码组成，该操作码指定要执行的操作，然后是零个或多个提供该操作使用的参数或数据的操作数. 许多指令没有操作数，仅由一个操作码组成.

    Ignoring exceptions, the inner loop of a Java Virtual Machine interpreter is
    effectively

    忽略异常，Java虚拟机解释器的内部循环是有效的.

    do {
        atomically calculate pc and fetch opcode at pc;
        if (operands) fetch operands;
        execute the action for the opcode;
    } while (there is more to do);

    The number and size of the operands are determined by the opcode. If an operand
    is more than one byte in size, then it is stored in big-endian order - high-order byte
    first. For example, an unsigned 16-bit index into the local variables is stored as two
    unsigned bytes, byte1 and byte2, such that its value is (byte1 << 8) | byte2.

    操作数的数量和大小由操作码确定. 如果操作数的大小超过一个字节，则将以大端顺序存储-首先是高位字节.
    例如，局部变量的无符号16位索引存储为两个无符号字节，字节1和字节2，因此其值是（byte1 << 8）| byte2

    The bytecode instruction stream is only single-byte aligned. The two exceptions
    are the lookupswitch and tableswitch instructions (§lookupswitch, §tableswitch),
    which are padded to force internal alignment of some of their operands on 4-byte
    boundaries.

    字节码指令流仅单字节对齐. 这两个例外是lookupswitch和tableswitch指令（§lookupswitch，§tableswitch），它们被填充以强制其某些操作数在4字节边界上进行内部对齐.

    The decision to limit the Java Virtual Machine opcode to a byte and to forgo data alignment
    within compiled code reflects a conscious bias in favor of compactness, possibly at the cost
    of some performance in naive implementations. A one-byte opcode also limits the size of
    the instruction set. Not assuming data alignment means that immediate data larger than a
    byte must be constructed from bytes at run time on many machines.

    将Java虚拟机操作码限制为一个字节并放弃已编译代码中的数据对齐的决定反映了倾向于紧凑性的有意识偏见，这可能以幼稚的实现为代价.
    一个字节的操作码也限制了指令集的大小. 不假设数据对齐意味着在许多机器上运行时必须从字节构造大于字节的直接数据.

    */


```