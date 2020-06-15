### 翻译Java虚拟机规范第二章第十一节-第五节(JSR-337/Java8)

主要内容：`简单介绍创建对象和数组的相关指令`

```java

    /**

    2.11.5 Object Creation and Manipulation

    对象创建和操作

    Although both class instances and arrays are objects, the Java Virtual Machine
    creates and manipulates class instances and arrays using distinct sets of
    instructions:

    尽管类实例和数组都是对象，但是Java虚拟机使用不同的指令集创建和操作类实例和数组:

    • Create a new class instance: new.

    创建一个新的类实例：new.

    • Create a new array: newarray, anewarray, multianewarray.

    创建一个新数组：newarray，anewarray，multianewarray.

    • Access fields of classes (static fields, known as class variables) and fields
    of class instances (non-static fields, known as instance variables): getstatic,
    putstatic, getfield, putfield.

    类的访问字段（静态字段，称为类变量）和类实例的字段（非静态字段，称为实例变量）：getstatic、putstatic、getfield、putfield.

    • Load an array component onto the operand stack: baload, caload, saload, iaload,
    laload, faload, daload, aaload.

    将数组组件加载到操作数堆栈上：baload、caload、saload、iaload、laload、faload、daload、aaload.

    • Store a value from the operand stack as an array component: bastore, castore,
    sastore, iastore, lastore, fastore, dastore, aastore.

    将操作数堆栈中的值存储为数组组件：bastore，castore，sastore，iastore，lastore，fastore，dastore，aastore.

    • Get the length of array: arraylength.

    获取数组长度：数组长度.

    • Check properties of class instances or arrays: instanceof, checkcast.

    检查类实例或数组的属性：instanceof，checkcast.

    */



```