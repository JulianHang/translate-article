### 翻译Java虚拟机规范第二章第三节-第三节-(JSR-337/Java8)

主要内容：`简单介绍returnAddress的概念`


```java

    /**

    2.3.3 The returnAddress Types and Values

    returnAddress类型和值

    The returnAddress type is used by the Java Virtual Machine's jsr, ret, and jsr_w
    instructions (§jsr, §ret, §jsr_w). The values of the returnAddress type are pointers
    to the opcodes of Java Virtual Machine instructions. Unlike the numeric primitive
    types, the returnAddress type does not correspond to any Java programming
    language type and cannot be modified by the running program.

    通过Java虚拟机的jsr、ret和jsr_w指令来使用returnAddress类型. returnAddress类型的值是指向Java虚拟机指令的操作码指针.
    与数字基本类型不同，returnAddress类型不与任何Java编程语言类型相对应，并且不能由正在运行的程序进行修改.

    */



```