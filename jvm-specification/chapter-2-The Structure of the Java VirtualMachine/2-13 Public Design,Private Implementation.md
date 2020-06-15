### 翻译Java虚拟机规范第二章第十三节(JSR-337/Java8)

主要内容：`简单说明Java虚拟机实现并不需要严格遵守规范，只要能够读取类文件并准确实现语义就可以以任何方式实现`

```java

    /**

    2.13 Public Design,Private Implementation

    公共设计，私有实现

    Thus far this specification has sketched the public view of the Java Virtual
    Machine: the class file format and the instruction set. These components are vital
    to the hardware-, operating system-, and implementation-independence of the Java
    Virtual Machine. The implementor may prefer to think of them as a means to
    securely communicate fragments of programs between hosts each implementing
    the Java SE platform, rather than as a blueprint to be followed exactly.

    到目前为止，该规范已勾勒出Java虚拟机的公众视野：类文件格式和指令集. 这些组件对于Java虚拟机的硬件，操作系统和实现独立性至关重要.
    实现者可能更愿意将它们视为在各实现Java SE平台的主机之间安全地传递程序片段的一种手段，而不是要严格遵循的蓝图.

    It is important to understand where the line between the public design and the
    private implementation lies. A Java Virtual Machine implementation must be
    able to read class files and must exactly implement the semantics of the Java
    Virtual Machine code therein. One way of doing this is to take this document
    as a specification and to implement that specification literally. But it is also
    perfectly feasible and desirable for the implementor to modify or optimize the
    implementation within the constraints of this specification. So long as the class file
    format can be read and the semantics of its code are maintained, the implementor
    may implement these semantics in any way. What is "under the hood" is the
    implementor's business, as long as the correct external interface is carefully
    maintained.

    重要的是要了解公共设计和私有实现之间的界线在哪里.
    Java虚拟机实现必须能够读取类文件，并且必须在其中准确实现Java虚拟机代码的语义.
    一种实现方法是将该文档作为规范，并按字面实现该规范. 但是，实现者在本规范的约束内修改或优化实现也是完全可行和理想的. 
    只要可以读取类文件格式并维护其代码的语义，实现者就可以以任何方式实现这些语义.
    只要精心维护正确的外部接口，"内幕"是实施者的工作.

    There are some exceptions: debuggers, profilers, and just-in-time code generators can each
    require access to elements of the Java Virtual Machine that are normally considered to
    be “under the hood.” Where appropriate, Oracle works with other Java Virtual Machine
    implementors and with tool vendors to develop common interfaces to the Java Virtual
    Machine for use by such tools, and to promote those interfaces across the industry.

    有一些例外：调试器，事件探查器和即时代码生成器每个都可能需要访问Java虚拟机的元素，这些元素通常被认为是"幕后".
    在适当的情况下，Oracle与其他Java虚拟机实现者和工具供应商合作，开发Java虚拟机的通用接口，以供此类工具使用，并在整个行业中推广这些接口.

    The implementor can use this flexibility to tailor Java Virtual Machine
    implementations for high performance, low memory use, or portability. What
    makes sense in a given implementation depends on the goals of that
    implementation. The range of implementation options includes the following:

    实现者可以使用这种灵活性来定制Java虚拟机实现，以实现高性能，低内存使用或可移植性.
    在给定的实现方案中，什么才有意义取决于该实施方案的目标.
    实现选项的范围包括：

    • Translating Java Virtual Machine code at load-time or during execution into the
    instruction set of another virtual machine.

    在加载时或执行期间将Java虚拟机代码转换为另一个虚拟机的指令集.

    • Translating Java Virtual Machine code at load-time or during execution into the
    native instruction set of the host CPU (sometimes referred to as just-in-time, or
    JIT, code generation).

    在加载时或执行期间将Java虚拟机代码转换为主机CPU的本机指令集（有时称为即时代码或JIT代码生成）.

    The existence of a precisely defined virtual machine and object file format need not
    significantly restrict the creativity of the implementor. The Java Virtual Machine is
    designed to support many different implementations, providing new and interesting
    solutions while retaining compatibility between implementations.

    精确定义的虚拟机和目标文件格式的存在不必严格限制实现者的创造力.
    Java虚拟机旨在支持许多不同的实现，在保留实现之间的兼容性的同时，提供了新的有趣的解决方案.

    */

```