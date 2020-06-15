### 翻译Java虚拟机规范第二章第十一节-第一节(JSR-337/Java8)

主要内容：`阐述类型相关的指令，不是说每个类型都会对应到一种指令，比如说int类型对应到iload，而像byte类型实际上并没有对应的bload，在需要的情况下，Java虚拟机通过指令将这些值符号扩展成int类型的值`

```java

    /**

    2.11.1 Types and the Java Virtual Machine

    类型和Java虚拟机

    Most of the instructions in the Java Virtual Machine instruction set encode type
    information about the operations they perform. For instance, the iload instruction
    (§iload) loads the contents of a local variable, which must be an int, onto the
    operand stack. The fload instruction (§fload) does the same with a float value. The
    two instructions may have identical implementations, but have distinct opcodes.

    Java虚拟机指令集中的大多数指令对有关它们执行的操作的类型信息进行编码. 例如，iload指令（§iload）将局部变量的内容（必须为int）加载到操作数栈上. 
    fload指令（§fload）对浮点值执行相同的操作.
    两条指令可能具有相同的实现，但具有不同的操作码.

    For the majority of typed instructions, the instruction type is represented explicitly
    in the opcode mnemonic by a letter: i for an int operation, l for long, s for short,
    b for byte, c for char, f for float, d for double, and a for reference. Some
    instructions for which the type is unambiguous do not have a type letter in their
    mnemonic. For instance, arraylength always operates on an object that is an array.
    Some instructions, such as goto, an unconditional control transfer, do not operate
    on typed operands.

    对于大多数类型的指令，指令类型在操作码助记符中用字母明确表示：i表示int操作，l表示long，s表示short，b表示字节，c表示char，f表示float，d表示double，以供参考.
    类型明确的某些指令的助记符中没有类型字母. 类型明确的某些指令的助记符中没有类型字母。 例如，arraylength始终对作为数组的对象进行操作。 某些指令，例如goto，无条件控制转移，不会对类型化的操作数进行运算.

    Given the Java Virtual Machine's one-byte opcode size, encoding types into
    opcodes places pressure on the design of its instruction set. If each typed instruction
    supported all of the Java Virtual Machine's run-time data types, there would be
    more instructions than could be represented in a byte. Instead, the instruction set
    of the Java Virtual Machine provides a reduced level of type support for certain
    operations. In other words, the instruction set is intentionally not orthogonal.
    Separate instructions can be used to convert between unsupported and supported
    data types as necessary.

    指定Java虚拟机的一个字节操作码大小，将编码类型转换为操作码会给其指令集的设计带来压力. 
    如果每个键入的指令都支持Java虚拟机的所有运行时数据类型，那么指令的数量将超过字节中表示的数量.
    相反，Java虚拟机的指令集为某些操作提供了降低级别的类型支持。 换句话说，指令集有意不是正交的.
    必要时，可以使用单独的指令在不受支持和受支持的数据类型之间进行转换.

    Table 2.11.1-A summarizes the type support in the instruction set of the Java
    Virtual Machine. A specific instruction, with type information, is built by replacing
    the T in the instruction template in the opcode column by the letter in the type
    column. If the type column for some instruction template and type is blank, then
    no instruction exists supporting that type of operation. For instance, there is a load
    instruction for type int, iload, but there is no load instruction for type byte.

    表2.11.1-A总结了Java虚拟机指令集中的类型支持. 通过使用类型列中的字母替换操作码列中的指令模板中的T，可以构建具有类型信息的特定指令.
    如果某个指令模板的类型列和类型为空，则不存在支持该操作类型的指令. 例如，有一个用于int类型iload的加载指令，但是没有一个用于字节类型的加载指令.

    Note that most instructions in Table 2.11.1-A do not have forms for the integral
    types byte, char, and short. None have forms for the boolean type. A compiler
    encodes loads of literal values of types byte and short using Java Virtual Machine
    instructions that sign-extend those values to values of type int at compile-time
    or run-time. Loads of literal values of types boolean and char are encoded using
    instructions that zero-extend the literal to a value of type int at compile-time or
    run-time. Likewise, loads from arrays of values of type boolean, byte, short, and
    char are encoded using Java Virtual Machine instructions that sign-extend or zeroextend the 
    values to values of type int. Thus, most operations on values of actual
    types boolean, byte, char, and short are correctly performed by instructions
    operating on values of computational type int.

    请注意，表2.11.1-A中的大多数指令都没有整数类型byte，char和short的形式. 没有一个具有布尔类型的形式. 
    编译器使用Java虚拟机指令对字节和short类型的文字值的负载进行编码，这些指令在编译时或运行时将这些值符号扩展为int类型的值.
    使用布尔值和char类型的文字值的加载使用指令进行编码，这些指令在编译时或运行时将文字零扩展为int类型的值.
    同样，使用Java虚拟机指令对boolean，byte，short和char类型的值数组中的负载进行编码，这些指令将这些值进行符号扩展或零扩展为int类型的值.
    因此，对实际类型为boolean，byte，char和short的值的大多数操作均由对计算类型为int的值进行操作的指令正确执行.

    The mapping between Java Virtual Machine actual types and Java Virtual Machine
    computational types is summarized by Table 2.11.1-B.
    Certain Java Virtual Machine instructions such as pop and swap operate on the
    operand stack without regard to type; however, such instructions are constrained
    to use only on values of certain categories of computational types, also given in
    Table 2.11.1-B.

    表2.11.1-B总结了Java虚拟机实际类型和Java虚拟机计算类型之间的映射. 某些Java虚拟机指令（例如pop和swap）在操作数堆栈上操作而与类型无关；
    但是，此类指令只能在某些类型的计算类型的值上使用，如表2.11.1-B所示.


    */

```