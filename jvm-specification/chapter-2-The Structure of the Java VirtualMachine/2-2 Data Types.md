### 翻译Java虚拟机规范第二章第二节-(JSR-337/Java8)

主要内容：`Java虚拟机不负责标记值类型，而是由编译器完成，值类型分为原始类型与引用类型，引用类型的值被认为是指针；Java虚拟机的指令集使用具有特定类型的指令来操作不同类型的值，简单来说，比如iadd指令专门用来操作int类型的数据，而ladd专门用来操作long类型的数据`

```java

    /**

    2.2 Data Types

    数据类型

    Like the Java programming language, the Java Virtual Machine operates on two
    kinds of types: primitive types and reference types. There are, correspondingly, two
    kinds of values that can be stored in variables, passed as arguments, returned by
    methods, and operated upon: primitive values and reference values.

    像Java编程语言一样，Java虚拟机可对两种类型进行操作：原始类型和引用类型.
    相应地，可以将两种类型的值存储在变量中，作为参数传递，由方法返回和对其操作：原始值和引用值.

    The Java Virtual Machine expects that nearly all type checking is done prior
    to run time, typically by a compiler, and does not have to be done by the Java
    Virtual Machine itself. Values of primitive types need not be tagged or otherwise
    be inspectable to determine their types at run time, or to be distinguished from
    values of reference types. Instead, the instruction set of the Java Virtual Machine
    distinguishes its operand types using instructions intended to operate on values of
    specific types. For instance, iadd, ladd, fadd, and dadd are all Java Virtual Machine
    instructions that add two numeric values and produce numeric results, but each is
    specialized for its operand type: int, long, float, and double, respectively. For a
    summary of type support in the Java Virtual Machine instruction set, see §2.11.1.

    Java虚拟机希望几乎所有类型检查都在运行时之前完成，通常由编译器实现，不需要Java虚拟机本身实现. 基本类型的值不需要被标记或检查以在运行时确定其类型，也不必与引用类型的值区分开.
    相反，Java虚拟机的指令集使用对特定类型的值进行操作的指令来区分其操作数类型.  例如，iadd、ladd、fadd和dadd都是Java虚拟机指令，它们添加两个数值并产生数值结果，但是每个指令专门用于其操作数类型：int、long、float和double.
    有关Java虚拟机指令集中类型支持的摘要，请参见第2.11.1节.

    The Java Virtual Machine contains explicit support for objects. An object is
    either a dynamically allocated class instance or an array. A reference to an object
    is considered to have Java Virtual Machine type reference. Values of type
    reference can be thought of as pointers to objects. More than one reference to an
    object may exist. Objects are always operated on, passed, and tested via values of
    type reference.

    Java虚拟机包含明确的对象支持. 对象可以是动态分配的类实例，也可以是数组. 引用对象被认为具有Java虚拟机类型引用. 引用类型的值可以认为是指向对象的指针.
    可能存在对一个对象的多个引用. 对象始终通过类型引用的值进行操作，传递和测试.

     */

```