### 翻译Java虚拟机规范第二章第三节-第四节-(JSR-337/Java8)

主要内容：`简单介绍returnAddress的概念`


```java

    /**

    2.3.4 The boolean Type

    boolean类型

    Although the Java Virtual Machine defines a boolean type, it only provides
    very limited support for it. There are no Java Virtual Machine instructions solely
    dedicated to operations on boolean values. Instead, expressions in the Java
    programming language that operate on boolean values are compiled to use values
    of the Java Virtual Machine int data type.

    尽管Java虚拟机定义了布尔类型，但它仅提供了非常有限的支持. 没有专门用于布尔值操作的Java虚拟机指令.
    相反，在Java程序语言中对布尔值进行操作的表达式被编译成使用Java虚拟机int数据类型的值.

    The Java Virtual Machine does directly support boolean arrays. Its newarray
    instruction (§newarray) enables creation of boolean arrays. Arrays of type
    boolean are accessed and modified using the byte array instructions baload and
    bastore (§baload, §bastore).

    Java虚拟机确实直接支持布尔数组. 它的newarray指令可以创建布尔数组. 使用字节数组指令baload和bastore访问和修改boolean类型的数组.

    In Oracle’s Java Virtual Machine implementation, boolean arrays in the Java
    programming language are encoded as Java Virtual Machine byte arrays, using 8 bits per
    boolean element.

    在Oracle的Java虚拟机实现中，Java编程语言中的布尔数组被编码为Java虚拟机字节数组，每个布尔元素使用8位.

    The Java Virtual Machine encodes boolean array components using 1 to represent
    true and 0 to represent false. Where Java programming language boolean values
    are mapped by compilers to values of Java Virtual Machine type int, the compilers
    must use the same encoding.

    Java虚拟机使用1表示true和0表示false来对布尔数组组件进行编码. 
    在Java程序语言布尔值由编译器映射到Java虚拟机int类型的值的情况下，编译器必须使用相同的编码.

    */



```