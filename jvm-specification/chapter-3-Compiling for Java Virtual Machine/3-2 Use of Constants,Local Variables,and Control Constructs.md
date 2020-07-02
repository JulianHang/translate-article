### 翻译Java虚拟机规范第三章第二节(JSR-337/Java8)

主要内容：`通过多个示例说明在操作数栈、局部变量的关系上如何更好地设计指令`

```java

    /**

    3.2 Use of Constants,Local Variables,and Control Constructs

    常量、局部变量、控制结构的使用

    Java Virtual Machine code exhibits a set of general characteristics imposed by the
    Java Virtual Machine's design and use of types. In the first example we encounter
    many of these, and we consider them in some detail.
    The spin method simply spins around an empty for loop 100 times:

    Java虚拟机代码表现出一组由Java虚拟机的设计和类型使用所强加的一般特性. 在第一个示例中，我们遇到了许多此类问题，并且我们将对其进行详细考虑. spin方法只是将一个空的for循环旋转100次：

    void spin() 
        int i;
        for (i = 0; i < 100; i++) {
            ; // Loop body is empty
        }
    }

    A compiler might compile spin to:

    编译器可能编译spin：

    0 iconst_0 // Push int constant 0
    1 istore_1 // Store into local variable 1 (i=0)
    2 goto 8 // First time through don't increment
    5 iinc 1 1 // Increment local variable 1 by 1 (i++)
    8 iload_1 // Push local variable 1 (i)
    9 bipush 100 // Push int constant 100
    11 if_icmplt 5 // Compare and loop if less than (i < 100)
    14 return // Return void when done

    The Java Virtual Machine is stack-oriented, with most operations taking one or
    more operands from the operand stack of the Java Virtual Machine's current frame
    or pushing results back onto the operand stack. A new frame is created each time
    a method is invoked, and with it is created a new operand stack and set of local
    variables for use by that method (§2.6). At any one point of the computation, there
    are thus likely to be many frames and equally many operand stacks per thread of
    control, corresponding to many nested method invocations. Only the operand stack
    in the current frame is active.

    Java虚拟机是面向栈的，大多数操作都从Java虚拟机当前帧的操作数栈中获取一个或多个操作数，或者将结果推回到操作数栈中.
    每次调用方法时都会创建一个新栈帧，并由此创建一个新的操作数栈和该方法使用的局部变量集（第2.6节）.
    因此，在计算的任何一点上，每个控制线程可能有许多栈帧和同样多的操作数栈，对应许多嵌套方法的调用.
    只有当前栈帧的操作数栈处于活动状态.

    The instruction set of the Java Virtual Machine distinguishes operand types by
    using distinct bytecodes for operations on its various data types. The method
    spin operates only on values of type int. The instructions in its compiled code
    chosen to operate on typed data (iconst_0, istore_1, iinc, iload_1, if_icmplt) are all
    specialized for type int.

    Java虚拟机的指令集通过使用不同的字节码对各种数据类型进行操作来区分操作数类型. 方法spin仅对int类型的值进行操作. 
    在已编译代码中选择对类型化数据（iconst_0，istore_1，iinc，iload_1，if_icmplt）进行操作的指令均专门用于int类型.

    The two constants in spin, 0 and 100, are pushed onto the operand stack using
    two different instructions. The 0 is pushed using an iconst_0 instruction, one of the
    family of iconst_<i> instructions. The 100 is pushed using a bipush instruction,
    which fetches the value it pushes as an immediate operand.

    使用两个不同的指令将spin中的两个常数0和100压入操作数栈. 使用iconst_0指令（iconst_ <i>指令系列之一）将0压入. 使用bipush指令压入100，该指令将其压入的值作为直接操作数取回.

    The Java Virtual Machine frequently takes advantage of the likelihood of certain
    operands (int constants -1, 0, 1, 2, 3, 4 and 5 in the case of the iconst_<i>
    instructions) by making those operands implicit in the opcode. Because the
    iconst_0 instruction knows it is going to push an int 0, iconst_0 does not need to
    store an operand to tell it what value to push, nor does it need to fetch or decode an
    operand. Compiling the push of 0 as bipush 0 would have been correct, but would
    have made the compiled code for spin one byte longer. A simple virtual machine
    would have also spent additional time fetching and decoding the explicit operand
    each time around the loop. Use of implicit operands makes compiled code more
    compact and efficient.

    Java虚拟机经常通过使某些操作数隐含在操作码中来利用某些操作数的可能性（在iconst_ <i>指令中为int常量-1、0、1、2、3、4和5）.
    因为iconst_0指令知道它将要推送一个int 0，所以iconst_0不需要存储操作数来告诉它要推送的值，也不需要获取或解码操作数.
    将推入0编译为bipush 0会是正确的，但会使spin的编译代码长一个字节. 
    一个简单的虚拟机每次在循环中都要花费额外的时间来获取和解码显式操作数. 隐式操作数的使用使编译后的代码更加紧凑和高效.

    The int i in spin is stored as Java Virtual Machine local variable 1. Because most
    Java Virtual Machine instructions operate on values popped from the operand stack
    rather than directly on local variables, instructions that transfer values between
    local variables and the operand stack are common in code compiled for the Java
    Virtual Machine. These operations also have special support in the instruction
    set. In spin, values are transferred to and from local variables using the istore_1
    and iload_1 instructions, each of which implicitly operates on local variable 1.
    The istore_1 instruction pops an int from the operand stack and stores it in local
    variable 1. The iload_1 instruction pushes the value in local variable 1 on to the
    operand stack.

    spin中的i存储为局部变量1. 因为大多数Java虚拟机指令都是对从操作数栈弹出的值进行操作，而不是直接对局部变量进行操作，所以在为Java虚拟机已编译的代码中，局部变量和操作数栈之间传输值的指令是很常见的.
    这些操作在指令集中也有特殊支持. 在spin中，使用istore_1和iload_1指令来回传递局部变量，每个指令对局部变量1隐式操作.
    istore_1指令从操作数栈中弹出一个int并将其存储在局部变量1中. iload_1指令将局部变量1中的值压入操作数堆栈.

    The use (and reuse) of local variables is the responsibility of the compiler writer.
    The specialized load and store instructions should encourage the compiler writer
    to reuse local variables as much as is feasible. The resulting code is faster, more
    compact, and uses less space in the frame.

    局部变量的使用（和重用）是编译器编写者的责任. 专门的加载和存储指令应鼓励编译器编写者尽可能重用局部变量. 生成的代码更快，更紧凑，并且在栈帧中使用的空间更少.

    Certain very frequent operations on local variables are catered to specially by
    the Java Virtual Machine. The iinc instruction increments the contents of a local
    variable by a one-byte signed value. The iinc instruction in spin increments the
    first local variable (its first operand) by 1 (its second operand). The iinc instruction
    is very handy when implementing looping constructs.

    Java虚拟机专门满足了对局部变量非常频繁的操作. iinc指令将局部变量的内容增加一个字节的有符号值. spin中的iinc指令将第一个局部变量（第一个操作数）加1（第二个操作数）.
    在实现循环结构时，iinc指令非常方便.

    The for loop of spin is accomplished mainly by these instructions:

    spin的for循环主要通过以下指令完成：

    5 iinc 1 1 // Increment local variable 1 by 1 (i++)
    8 iload_1 // Push local variable 1 (i)
    9 bipush 100 // Push int constant 100
    11 if_icmplt 5 // Compare and loop if less than (i < 100)

    The bipush instruction pushes the value 100 onto the operand stack as an int,
    then the if_icmplt instruction pops that value off the operand stack and compares
    it against i. If the comparison succeeds (the variable i is less than 100), control
    is transferred to index 5 and the next iteration of the for loop begins. Otherwise,
    control passes to the instruction following the if_icmplt.

    bipush指令将值100作为int值推送到操作数栈上，然后if_icmplt指令将该值弹出操作数栈，并将其与i进行比较.
    如果比较成功（变量i小于100），则控制权转移到索引5，并且for循环的下一次迭代开始. 
    否则，控制权传递给if_icmplt之后的指令.

    If the spin example had used a data type other than int for the loop counter,
    the compiled code would necessarily change to reflect the different data type. For
    instance, if instead of an int the spin example uses a double, as shown:

    如果spin示例为循环计数器使用了非int的数据类型，则编译后的代码将必须更改以反映不同的数据类型.

    void dspin() {
        double i;
        for (i = 0.0; i < 100.0; i++) {
            ; // Loop body is empty
        }
    }

    the compiled code is:

    Method void dspin()
    0 dconst_0 // Push double constant 0.0
    1 dstore_1 // Store into local variables 1 and 2
    2 goto 9 // First time through don't increment
    5 dload_1 // Push local variables 1 and 2
    6 dconst_1 // Push double constant 1.0
    7 dadd // Add; there is no dinc instruction
    8 dstore_1 // Store result in local variables 1 and 2
    9 dload_1 // Push local variables 1 and 2
    10 ldc2_w #4 // Push double constant 100.0
    13 dcmpg // There is no if_dcmplt instruction
    14 iflt 5 // Compare and loop if less than (i < 100.0)
    17 return // Return void when done

    The instructions that operate on typed data are now specialized for type double.
    (The ldc2_w instruction will be discussed later in this chapter.)
    Recall that double values occupy two local variables, although they are only
    accessed using the lesser index of the two local variables. This is also the case for
    values of type long. Again for example,

    现在，对类型化数据进行操作的指令专门用于类型double. （ldc2_w指令将在本章后面讨论）. 
    回想一下，尽管仅使用两个局部变量的较小索引来访问它们，但它们占用了两个局部变量. 对于long类型的值也是如此. 再例如，

    double doubleLocals(double d1, double d2) {
        return d1 + d2;
    }

    becomes

    Method double doubleLocals(double,double)
    0 dload_1 // First argument in local variables 1 and 2
    1 dload_3 // Second argument in local variables 3 and 4
    2 dadd
    3 dreturn

    Note that local variables of the local variable pairs used to store double values in
    doubleLocals must never be manipulated individually.

    请注意，在doubleLocal中永远不能单独操作局部变量组的局部变量来存储double值（double类型占用两个局部变量表的索引位置）.

    The Java Virtual Machine's opcode size of 1 byte results in its compiled code being
    very compact. However, 1-byte opcodes also mean that the Java Virtual Machine
    instruction set must stay small. As a compromise, the Java Virtual Machine does
    not provide equal support for all data types: it is not completely orthogonal
    (Table 2.11.1-A).

    Java虚拟机的操作码大小为1个字节，导致其编译后的代码非常紧凑.
    但是，1字节的操作码也意味着Java虚拟机指令集必须很小. 
    作为一种折衷，Java虚拟机不能为所有数据类型提供同等的支持：它不是完全正交的（表2.11.1-A）.

    For example, the comparison of values of type int in the for statement of example
    spin can be implemented using a single if_icmplt instruction; however, there is
    no single instruction in the Java Virtual Machine instruction set that performs a
    conditional branch on values of type double. Thus, dspin must implement its
    comparison of values of type double using a dcmpg instruction followed by an iflt
    instruction.

    例如，可以使用单个if_icmplt指令来实现示例spin的for语句中int类型的值的比较；但是，Java虚拟机指令集中没有单个指令对double类型的值执行条件分支.
    因此，dspin示例必须使用dcmpg指令和iflt指令来实现对double类型的值的比较.

    The Java Virtual Machine provides the most direct support for data of type int.
    This is partly in anticipation of efficient implementations of the Java Virtual
    Machine's operand stacks and local variable arrays. It is also motivated by the
    frequency of int data in typical programs. Other integral types have less direct
    support. There are no byte, char, or short versions of the store, load, or add
    instructions, for instance. Here is the spin example written using a short:

    Java虚拟机为int类型的数据提供了最直接的支持. 这部分是因为期望Java虚拟机的操作数栈和局部变量表的有效实现. 
    它也受典型程序中int数据的频率的影响. 例如，没有byte、char或short的存储、加载或添加指令的版本.
    这是使用short编写的spin示例：

    void sspin() {
        short i;
        for (i = 0; i < 100; i++) {
            ;   // Loop body is empty
        }
    }

    It must be compiled for the Java Virtual Machine, as follows, using instructions
    operating on another type, most likely int, converting between short and int
    values as necessary to ensure that the results of operations on short data stay within
    the appropriate range:

    Java虚拟机对其进行编译，如下所述，在其他类型上使用指令操作，最可能是int，必要时在short和int值之间进行转换，以确保对short数据的操作结果保持在适当的范围内：

    Method void sspin()
    0 iconst_0
    1 istore_1
    2 goto 10
    5 iload_1 // The short is treated as though an int
    6 iconst_1
    7 iadd
    8 i2s // Truncate int to short
    9 istore_1
    10 iload_1
    11 bipush 100
    13 if_icmplt 5
    16 return

    The lack of direct support for byte, char, and short types in the Java Virtual
    Machine is not particularly painful, because values of those types are internally
    promoted to int (byte and short are sign-extended to int, char is zero-extended).
    Operations on byte, char, and short data can thus be done using int instructions.
    The only additional cost is that of truncating the values of int operations to valid
    ranges.

    在Java虚拟机中缺少对byte、char和short类型的直接支持并不是特别痛苦，因为这些类型的值在内部被提升为int（byte和short被符号扩展为int，char被零扩展）.
    因此，可以使用int指令对byte、char和short数据进行操作.
    唯一的额外费用是将int运算的值截断为有效范围.

    The long and floating-point types have an intermediate level of support in the Java
    Virtual Machine, lacking only the full complement of conditional control transfer
    instructions.

    long和float类型在Java虚拟机中具有中等级别的支持，仅缺少条件控制转移指令的完整补充.

    */



```