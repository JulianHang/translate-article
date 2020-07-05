### 翻译Java虚拟机规范第三章(JSR-337/Java8)

主要内容：`第三章的概述`


```java

    /**

    3 Compiling for the Java Virtual Machine

    Java虚拟机的编译

    THE Java Virtual Machine machine is designed to support the Java programming
    language. Oracle's JDK software contains a compiler from source code written
    in the Java programming language to the instruction set of the Java Virtual
    Machine, and a run-time system that implements the Java Virtual Machine itself.
    Understanding how one compiler utilizes the Java Virtual Machine is useful to the
    prospective compiler writer, as well as to one trying to understand the Java Virtual
    Machine itself. The numbered sections in this chapter are not normative.
    
    Java虚拟机机器旨在支持Java编程语言. Oracle的JDK软件包含一个编译器，从使用Java编程语言编写的源代码到Java虚拟机的指令集，以及一个实现Java虚拟机本身的运行时系统.
    理解一个编译器如何利用Java虚拟机对于准编译器作者以及试图理解Java虚拟机本身的人都是有用的. 
    本章中的编号部分不是规范性的.

    Note that the term "compiler" is sometimes used when referring to a translator from
    the instruction set of a Java Virtual Machine to the instruction set of a specific
    CPU. One example of such a translator is a just-in-time (JIT) code generator, which
    generates platform-specific instructions only after Java Virtual Machine code has
    been loaded. This chapter does not address issues associated with code generation,
    only those associated with compiling source code written in the Java programming
    language to Java Virtual Machine instructions.

    注意，当指从Java虚拟机的指令集到特定CPU的指令集的转换器时，有时会使用术语"编译器". 
    这种翻译器的一个示例是即时（JIT）代码生成器，该生成器仅在加载Java虚拟机代码后才生成特定于平台的指令.
    本章不解决与代码生成相关的问题，仅解决以Java编程语言编写的源代码编译为Java虚拟机指令相关的问题.

    */



```