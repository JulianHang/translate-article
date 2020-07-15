### 翻译Java虚拟机规范第一章第四节-(JSR-337/Java8)

主要内容：`对Java虚拟机规范的特殊表示作简要说明，比如斜体、粗体`


```java

    /**

    1.4 Notation

    符号

    Throughout this specification we refer to classes and interfaces drawn from the
    Java SE platform API. Whenever we refer to a class or interface (other than those
    declared in an example) using a single identifier N, the intended reference is to the
    class or interface named N in the package java.lang. We use the fully qualified
    name for classes or interfaces from packages other than java.lang.

    在整个规范中，我们指的是从Java SE平台API提取的类和接口. 每当我们使用单个标识符N引用类或接口时（而不是示例中声明的类），预期的引用就是包java.lang中名为N的类或接口.
    对于java.lang以外的包中的类或接口，我们使用完全限定的名称.

    Whenever we refer to a class or interface that is declared in the package java or
    any of its subpackages, the intended reference is to that class or interface as loaded
    by the bootstrap class loader (§5.3.1).

    每当我们引用在包java或其任何子包中声明的类或接口时，预期的引用就是由bootstrap类加载器加载的类或接口.

    Whenever we refer to a subpackage of a package named java, the intended
    reference is to that subpackage as determined by the bootstrap class loader.

    每当我们引用名为java的包的子包时，预期的引用就是由bootstrap类加载器确定的那个子包.

    The use of fonts in this specification is as follows:

    本规范中字体的使用如下：

    • A fixed width font is used for Java Virtual Machine data types, exceptions,
    errors, class file structures, Prolog code, and Java code fragments.

    固定宽度字体用于Java虚拟机数据类型、异常、错误、类文件结构、Prolog代码和Java代码片段.

    • Italic is used for Java Virtual Machine "assembly language", its opcodes and
    operands, as well as items in the Java Virtual Machine's run-time data areas. It
    is also used to introduce new terms and simply for emphasis.

    斜体用于Java虚拟机的"汇编语言"、其操作码和操作数，以及Java虚拟机的运行时数据区域中的项目. 它也用于引入新术语并仅用于强调.

    Non-normative information, designed to clarify the specification, is given in
    smaller, indented text.

    旨在澄清规范的非规范性信息以较小的缩进文本形式提供.

    This is non-normative information. It provides intuition, rationale, advice, examples, etc.

    这是非规范性信息. 它提供了直觉、理由、建议、示例等.

     */

```