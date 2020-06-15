### 翻译Java虚拟机规范第二章第六节-第四节(JSR-337/Java8)

主要内容：`阐述方法正常调用完成的栈帧变化`

```java

    /**

    2.6.4 Normal Method Invocation Completion

    方法正常调用完成

    A method invocation completes normally if that invocation does not cause an
    exception (§2.10) to be thrown, either directly from the Java Virtual Machine or as
    a result of executing an explicit throw statement. If the invocation of the current
    method completes normally, then a value may be returned to the invoking method.
    This occurs when the invoked method executes one of the return instructions
    (§2.11.8), the choice of which must be appropriate for the type of the value being
    returned (if any).

    如果该调用不会直接从Java虚拟机或由于执行显式throw语句而导致引发异常（第2.10节），则该方法调用将正常完成. 如果当前方法的调用正常完成，则可以将值返回给调用方法.
    当被调用的方法执行其中一个返回指令（第2.11.8节）时，就会发生这种情况，对返回指令的选择必须适合于要返回的值的类型（如果有）.

    The current frame (§2.6) is used in this case to restore the state of the invoker,
    including its local variables and operand stack, with the program counter of the
    invoker appropriately incremented to skip past the method invocation instruction.
    Execution then continues normally in the invoking method's frame with the
    returned value (if any) pushed onto the operand stack of that frame.

    在这种情况下，使用当前栈帧（第2.6节）恢复调用程序的状态，包括其局部变量和操作数栈，并适当增加调用程序的程序计数器以跳过方法调用指令.
    在将返回值推入到操作数栈的调用方法栈帧中继续正常执行.

    */


```
