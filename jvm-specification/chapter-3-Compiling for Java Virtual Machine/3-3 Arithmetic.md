### 翻译Java虚拟机规范第三章第三节(JSR-337/Java8)

主要内容：`Java虚拟机在操作数栈上执行算术运算`

```java

    /**

    3.3 Arithmetic 

    算术

    The Java Virtual Machine generally does arithmetic on its operand stack. (The
    exception is the iinc instruction, which directly increments the value of a local
    variable.) For instance, the align2grain method aligns an int value to a given
    power of 2:

    Java虚拟机通常在其操作数栈上执行算术运算. （例外是iinc指令，它直接增加了局部变量的值.）例如，align2grain方法将int值对齐为给定的2次幂：

        int align2grain(int i, int grain) {
            return ((i + grain-1) & ~(grain-1));
        } 

    Operands for arithmetic operations are popped from the operand stack, and
    the results of operations are pushed back onto the operand stack. Results of
    arithmetic subcomputations can thus be made available as operands of their nesting
    computation. For instance, the calculation of ~(grain-1) is handled by these
    instructions:

    从操作数栈中弹出用于算术运算的操作数，然后将运算结果推回操作数栈.
    因此，算术子计算的结果可用作其嵌套计算的操作数. 例如，〜（grain-1）的计算由以下指令处理：

        5 iload_2 // Push grain
        6 iconst_1 // Push int constant 1
        7 isub // Subtract; push result
        8 iconst_m1 // Push int constant -1
        9 ixor // Do XOR; push result
    
    First grain-1 is calculated using the contents of local variable 2 and an immediate
    int value 1. These operands are popped from the operand stack and their difference
    pushed back onto the operand stack. The difference is thus immediately available
    for use as one operand of the ixor instruction. (Recall that ~x == -1^x.) Similarly,
    the result of the ixor instruction becomes an operand for the subsequent iand
    instruction.

    使用局部变量2的内容和直接数int值1计算第一个grain-1. 这些操作数从操作数栈中弹出，并将它们的差值推回到操作数栈.
    因此，该差立即可用作ixor指令的一个操作数.
    同样，ixor指令的结果成为后续iand指令的操作数.

    The code for the entire method follows:

        Method int align2grain(int,int)
        0 iload_1
        1 iload_2
        2 iadd
        3 iconst_1
        4 isub
        5 iload_2
        6 iconst_1
        7 isub
        8 iconst_m1
        9 ixor
        10 iand
        11 ireturn

    */


```
