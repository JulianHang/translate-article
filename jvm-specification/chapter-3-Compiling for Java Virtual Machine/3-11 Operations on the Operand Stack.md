### 翻译Java虚拟机规范第三章第十一节(JSR-337/Java8)

主要内容：`简单说明操作数栈的含义`

```java

    /**

    3.11 Operations on the Operand Stack

    在操作数栈上操作

    The Java Virtual Machine has a large complement of instructions that manipulate
    the contents of the operand stack as untyped values. These are useful because of
    the Java Virtual Machine's reliance on deft manipulation of its operand stack. For
    instance:

    Java虚拟机具有大量指令，这些指令将操作数堆栈的内容作为未类型化的值进行操作（就是只有值没有类型，类型通过不同的指令来区分）.
    由于Java虚拟机依赖于它的操作数栈的灵巧操作，因此它们非常有用. 例如：

        public long nextIndex() {
            return index++;
        }
        private long index = 0;

    is compiled to:

    Method long nextIndex()

        0 aload_0 // Push this
        1 dup // Make a copy of it
        2 getfield #4 // One of the copies of this is consumed
        // pushing long field index,
        // above the original this
        5 dup2_x1 // The long on top of the operand stack is
        // inserted into the operand stack below the
        // original this
        6 lconst_1 // Push long constant 1
        7 ladd // The index value is incremented...
        8 putfield #4 // ...and the result stored in the field
        11 lreturn // The original value of index is on top of
        // the operand stack, ready to be returned

    Note that the Java Virtual Machine never allows its operand stack manipulation
    instructions to modify or break up individual values on the operand stack.

    请注意，Java虚拟机从不允许其操作数栈操作指令修改或破坏操作数栈上的各个值.

    */



```