### 翻译Java虚拟机规范第二章第十一节-第二节(JSR-337/Java8)

主要内容：`阐述加载与存储指令，及指令族，即iload_<n>，n表示非负整数的操作数，同样也有其他类型的表示`

```java

    /**

    2.11.2 Load and Store Instructions 

    加载与存储指令

    The load and store instructions transfer values between the local variables (§2.6.1)
    and the operand stack (§2.6.2) of a Java Virtual Machine frame (§2.6):

    加载和存储指令在Java虚拟机栈帧（第2.6节）的局部变量（第2.6.1节）和操作数栈（第2.6.2节）之间传递值：

    • Load a local variable onto the operand stack: iload, iload_<n>, lload,
    lload_<n>, fload, fload_<n>, dload, dload_<n>, aload, aload_<n>.

    将局部变量加载到操作数堆栈上：iload，iload_ <n>，lload，lload_ <n>，fload，fload_ <n>，dload，dload_ <n>，aload，aload_ <n>.

    • Store a value from the operand stack into a local variable: istore, istore_<n>,
    lstore, lstore_<n>, fstore, fstore_<n>, dstore, dstore_<n>, astore, astore_<n>.

    将操作数栈中的值存储到局部变量中：istore，istore_ <n>，lstore，lstore_ <n>，fstore，fstore_ <n>，dstore，dstore_ <n>，astore，astore_ <n>.

    • Load a constant on to the operand stack: bipush, sipush, ldc, ldc_w, ldc2_w,
    aconst_null, iconst_m1, iconst_<i>, lconst_<l>, fconst_<f>, dconst_<d>.

    将常量加载到操作数栈上：bipush，sipush，lcd，ldc_w，ldc2_w，aconst_null，iconst_m1，iconst_ <i>，lconst_ <l>，fconst_ <f>，dconst_ <d>.

    • Gain access to more local variables using a wider index, or to a larger immediate
    operand: wide.

    使用更大的索引访问更大的局部变量，或者访问更大的直接操作数：wide.

    Instructions that access fields of objects and elements of arrays (§2.11.5) also
    transfer data to and from the operand stack.

    访问对象和数组元素的字段（第2.11.5节）的指令与操作数栈传输数据.

    Instruction mnemonics shown above with trailing letters between angle brackets
    (for instance, iload_<n>) denote families of instructions (with members iload_0,
    iload_1, iload_2, and iload_3 in the case of iload_<n>). Such families of
    instructions are specializations of an additional generic instruction (iload) that takes
    one operand. For the specialized instructions, the operand is implicit and does not
    need to be stored or fetched. The semantics are otherwise the same (iload_0 means
    the same thing as iload with the operand 0). The letter between the angle brackets
    specifies the type of the implicit operand for that family of instructions: for <n>,
    a nonnegative integer; for <i>, an int; for <l>, a long; for <f>, a float; and for
    <d>, a double. Forms for type int are used in many cases to perform operations
    on values of type byte, char, and short (§2.11.1).

    上面显示的指令助记符在尖括号之间（例如，iload_ <n>）之间带有结尾字母，表示指令族（在iload_ <n>情况下，具有成员iload_0，iload_1，iload_2和iload_3）.
    这些指令族是采用一个操作数的附加通用指令（iload）的专业化. 对于专用指令，操作数是隐式的，不需要存储或获取.
    语义是相同的（iload_0的含义与操作数为0的iload相同）.  尖括号之间的字母指定该指令系列的隐式操作数的类型：对于<n>，非负整数；
    对于<i>，为int；对于<l>，很长；对于<f>，是浮点数；对于<d>，则为双精度. int类型的表单在许多情况下用于对byte，char和short类型的值（第2.11.1节）执行操作.

    This notation for instruction families is used throughout this specification.

    整个说明书中都使用了这种指令族的表示法.

    */



```
