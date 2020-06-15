### 翻译Java虚拟机规范第二章第六节-第二节(JSR-337/Java8)

主要内容：`介绍了操作数栈，存储结构为后进先出，操作局部变量或字段的值，虽然它可以存储任意类型的值，但使用指令进行操作时其类型必须符合要求，也就是说你推入两个float类型的值，却使用iadd指令将两个值相加`

```java

    /**

    2.6.2 Operand Stacks

    操作数栈

    Each frame (§2.6) contains a last-in-first-out (LIFO) stack known as its operand
    stack. The maximum depth of the operand stack of a frame is determined at
    compile-time and is supplied along with the code for the method associated with
    the frame (§4.7.3).

    每个栈帧（第2.6节）包含一个后进先出（LIFO）栈，称为其操作数栈. 栈帧的操作数栈的最大长度在编译时确定，以随着栈帧关联的方法代码（第4.7.3节）提供.

    Where it is clear by context, we will sometimes refer to the operand stack of the
    current frame as simply the operand stack.

    在上下文清楚的地方，我们有时将当前栈帧的操作数栈简称为操作数栈.

    The operand stack is empty when the frame that contains it is created. The
    Java Virtual Machine supplies instructions to load constants or values from local
    variables or fields onto the operand stack. Other Java Virtual Machine instructions
    take operands from the operand stack, operate on them, and push the result back
    onto the operand stack. The operand stack is also used to prepare parameters to be
    passed to methods and to receive method results.

    创建包含操作数栈的栈帧时，该操作数栈为空. Java虚拟机提供了将局部变量或字段中的常量或值加载到操作数栈上的指令.
    其他Java虚拟机指令从操作数栈中获取操作数，对其进行操作，然后将结果压回操作数栈.
    操作数栈还用于准备要传递给方法的参数并接收方法结果.

    For example, the iadd instruction (§iadd) adds two int values together. It requires
    that the int values to be added be the top two values of the operand stack, pushed
    there by previous instructions. Both of the int values are popped from the operand
    stack. They are added, and their sum is pushed back onto the operand stack.
    Subcomputations may be nested on the operand stack, resulting in values that can
    be used by the encompassing computation.

    例如，iadd指令（§iadd）将两个int值加在一起. 它要求将操作数栈的前两个int值相加，并由前面的指令压入该值.
    这两个int值都从操作数栈中弹出. 将它们相加，并将它们的总和推回操作数栈.
    子计算可嵌套在操作数栈上，从而产生可被包含计算使用的值.

    Each entry on the operand stack can hold a value of any Java Virtual Machine type,
    including a value of type long or type double.

    操作数栈上的每个条目都可以保存任何Java虚拟机类型的值，包括long类型或double类型的值.

    Values from the operand stack must be operated upon in ways appropriate to their
    types. It is not possible, for example, to push two int values and subsequently treat
    them as a long or to push two float values and subsequently add them with an
    iadd instruction. A small number of Java Virtual Machine instructions (the dup
    instructions (§dup) and swap (§swap)) operate on run-time data areas as raw values
    without regard to their specific types; these instructions are defined in such a way
    that they cannot be used to modify or break up individual values. These restrictions
    on operand stack manipulation are enforced through class file verification (§4.10).

    操作数堆栈中的值必须以适合其类型的方式进行操作. 例如，不可能推入两个int值，然后将它们视为long值，或推入两个float值，然后使用iadd指令将它们相加.
    少量的Java虚拟机指令（dup指令（§dup）和swap（§swap））在运行时数据区上作为原始值运行，而与它们的特定类型无关.
    这些指令的定义方式不能用于修改或破坏单个值. 通过类文件验证（第4.10节）强制执行对操作数栈操作的限制.

    At any point in time, an operand stack has an associated depth, where a value of
    type long or double contributes two units to the depth and a value of any other
    type contributes one unit.

    在任何时间点，操作数栈都具有关联的深度，其中long或double类型的值对该深度贡献两个单位，而任何其他类型的值则贡献一个单位深度.

    */

```