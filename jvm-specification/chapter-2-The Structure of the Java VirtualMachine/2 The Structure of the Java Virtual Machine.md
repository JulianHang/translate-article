### 翻译Java虚拟机规范第二章(JSR-337/Java8)

主要内容：`第二章的概述，Java虚拟机规范并不会限制实现者的思路，只要能读取类文件格式并正确执行操作就行`


```java

    /**

    2 The Structure of the Java Virtual Machine

    Java虚拟机的结构

    THIS document specifies an abstract machine. It does not describe any particular
    implementation of the Java Virtual Machine.

    该文档指定了抽象机器. 它没有描述Java虚拟机的任何特定实现.

    To implement the Java Virtual Machine correctly, you need only be able to
    read the class file format and correctly perform the operations specified therein.
    Implementation details that are not part of the Java Virtual Machine's specification
    would unnecessarily constrain the creativity of implementors. For example, the
    memory layout of run-time data areas, the garbage-collection algorithm used, and
    any internal optimization of the Java Virtual Machine instructions (for example,
    translating them into machine code) are left to the discretion of the implementor.

    为了正确实现Java虚拟机，您仅需要能够读取类文件格式并正确执行其中指定的操作. 不是Java虚拟机规范的部分的实现细节将不必要地限制实现者的创造力.
    例如，运行时数据区的内存布局，使用的垃圾收集算法以及Java虚拟机指令的任何内部优化（例如，将它们转换为机器代码）都由实现者自行决定.

    All references to Unicode in this specification are given with respect to The
    Unicode Standard, Version 6.0.0, available at http://www.unicode.org/.

    本规范中对Unicode的所有引用均相对于Unicode标准版本6.0.0（可从http://www.unicode.org/获得）给出.


    */



```