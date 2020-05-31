### 翻译Java虚拟机规范第一章第二节-(JSR-337/Java8)

主要内容：`简单地介绍了什么是Java虚拟机，它包含一组指令集并且能够操作各种内存区域，它不依赖于任何硬件或操作系统，这直接屏蔽了底层的差异性；Java虚拟机只认识字节码文件，对其有严格的约束，所以任何语言只要能够将源文件编译成符合要求的字节码文件则可以使用Java虚拟机作为媒介与底层交付`

```java

    /**
     
    1.2 The Java Virtual Machine

    Java虚拟机

    The Java Virtual Machine is the cornerstone of the Java platform. It is the
    component of the technology responsible for its hardware- and operating systemindependence, the small size of its compiled code, and its ability to protect users
    from malicious programs.

    Java虚拟机是Java平台的基石. 它是技术的部件，负责硬件和操作系统的独立性，编译代码的小尺寸以及保护用户免受恶意程序攻击的能力.

    The Java Virtual Machine is an abstract computing machine. Like a real computing
    machine, it has an instruction set and manipulates various memory areas at run time.
    It is reasonably common to implement a programming language using a virtual
    machine; the best-known virtual machine may be the P-Code machine of UCSD
    Pascal.

    Java虚拟机是一个抽象的计算机. 就像真正的计算机一样，它具有指令集和在运行时操作各种内存区域. 使用虚拟机实现编程语言是相当普遍的. 最知名的虚拟机可能是UCSD Pascal的P-Code计机器.

    The first prototype implementation of the Java Virtual Machine, done at Sun
    Microsystems, Inc., emulated the Java Virtual Machine instruction set in software
    hosted by a handheld device that resembled a contemporary Personal Digital
    Assistant (PDA). Oracle's current implementations emulate the Java Virtual
    Machine on mobile, desktop and server devices, but the Java Virtual Machine
    does not assume any particular implementation technology, host hardware, or
    host operating system. It is not inherently interpreted, but can just as well be
    implemented by compiling its instruction set to that of a silicon CPU. It may also
    be implemented in microcode or directly in silicon.

    由Sun Microsystems,Incs完成的Java虚拟机的第一个原型实现模拟了类似于现代个人数字助理（PDA）的手持设备托管软件中的Java虚拟机指令集.
    当前的Oracle实现在移动设备、桌面设备和服务器设备上模拟Java虚拟机，但是Java虚拟机不采用任何特定的实现技术，主机硬件或主机操作系统.
    它没有内在的解释，但是可以通过将其指令及编译成硅CPU的指令集来实现.  它也可以用微码或直接在硅中实现.

    The Java Virtual Machine knows nothing of the Java programming language, only
    of a particular binary format, the class file format. A class file contains Java
    Virtual Machine instructions (or bytecodes) and a symbol table, as well as other
    ancillary information.

    Java虚拟机不了解Java编程语言，只知道特定的二进制格式，类文件格式. 类文件包含Java虚拟机指令（或字节码）和一个符号表，以及其他辅助信息.

    For the sake of security, the Java Virtual Machine imposes strong syntactic and
    structural constraints on the code in a class file. However, any language with
    functionality that can be expressed in terms of a valid class file can be hosted by
    the Java Virtual Machine. Attracted by a generally available, machine-independent
    platform, implementors of other languages can turn to the Java Virtual Machine as
    a delivery vehicle for their languages.

    为了安全起见，Java虚拟机对类文件中的代码强加了语法和结构上的约束. 但是，Java虚拟机可以托管能够表示有效类文件的任何功能性语言.
    受到普遍可用的，与机器无关的平台的吸引，其他语言的实现者可以将Java虚拟机用作其语言的交付工具.

    The Java Virtual Machine specified here is compatible with the Java SE 8 platform,
    and supports the Java programming language specified in The Java Language
    Specification, Java SE 8 Edition.

    此处指定的Java虚拟机与Java SE 8平台兼容，并支持Java语言规范中指定的Java编程语言.


















     */







```