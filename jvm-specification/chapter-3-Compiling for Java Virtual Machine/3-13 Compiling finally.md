### 翻译Java虚拟机规范第三章第十三节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机对try-finally/try-catch-finally的处理，可以了解到try块中return语句和finally块的执行情况`

```java

    /**

    3.13 Compiling finally

    编译finally

    (This section assumes a compiler generates class files with version number 50.0
    or below, so that the jsr instruction may be used. See also §4.10.2.5.)

    （本节假定编译器生成版本号为50.0或更低的类文件，以便可以使用jsr指令。另请参见§4.10.2.5.）

    Compilation of a try-finally statement is similar to that of try-catch. Prior to
    transferring control outside the try statement, whether that transfer is normal or
    abrupt, because an exception has been thrown, the finally clause must first be
    executed. For this simple example:

    try-finally语句的编译与try-catch相似. 在try语句之外进行控制转移之前，无论该转移是正常转移还是突然转移，由于引发了异常，必须首先执行finally子句.
    对于这个简单的例子：

        void tryFinally() {
            try {
                tryItOut();
            } finally {
                wrapItUp();
            }
        }

    the compiled code is:
        
        Method void tryFinally()
        0 aload_0 // Beginning of try block
        1 invokevirtual #6 // Method Example.tryItOut()V
        4 jsr 14 // Call finally block
        7 return // End of try block
        8 astore_1 // Beginning of handler for any throw
        9 jsr 14 // Call finally block
        12 aload_1 // Push thrown value
        13 athrow // ...and rethrow value to the invoker
        14 astore_2 // Beginning of finally block
        15 aload_0 // Push this
        16 invokevirtual #5 // Method Example.wrapItUp()V
        19 ret 2 // Return from finally block

        Exception table:
        From To Target Type
        0     4   8    any

    There are four ways for control to pass outside of the try statement: by falling
    through the bottom of that block, by returning, by executing a break or continue
    statement, or by raising an exception. If tryItOut returns without raising an
    exception, control is transferred to the finally block using a jsr instruction. The
    jsr 14 instruction at index 4 makes a "subroutine call" to the code for the finally
    block at index 14 (the finally block is compiled as an embedded subroutine).
    When the finally block completes, the ret 2 instruction returns control to the
    instruction following the jsr instruction at index 4.

    有四种方法可以使控制传递到try语句之外：落入该块的底部、返回、执行break或Continue语句或引发异常. 
    如果tryItOut返回而没有引发异常，则使用jsr指令将控制权转移到finally块. 
    索引4处的jsr 14指令对索引14处的finally块的代码进行"子例程调用"（finally块被编译为嵌入式子例程）.
    当finally块完成时，ret 2指令将控制权返回到索引4处jsr指令之后的指令.

    In more detail, the subroutine call works as follows: The jsr instruction pushes
    the address of the following instruction (return at index 7) onto the operand stack
    before jumping. The astore_2 instruction that is the jump target stores the address
    on the operand stack into local variable 2. The code for the finally block (in
    this case the aload_0 and invokevirtual instructions) is run. Assuming execution of
    that code completes normally, the ret instruction retrieves the address from local
    variable 2 and resumes execution at that address. The return instruction is executed,
    and tryFinally returns normally.

    更详细地讲，子例程调用的工作方式如下：jsr指令在跳转之前将以下指令的地址（从索引7返回）压入操作数栈.
    作为跳转目标的astore_2指令将操作数栈上的地址存储到局部变量2中. 运行finally块的代码（在本例中为aload_0和invokevirtual指令）.
    假设该代码的执行正常完成，则ret指令从局部变量2检索地址，并在该地址处恢复执行. 
    返回指令已执行，tryFinally正常返回.

    A try statement with a finally clause is compiled to have a special exception
    handler, one that can handle any exception thrown within the try statement. If
    tryItOut throws an exception, the exception table for tryFinally is searched for
    an appropriate exception handler. The special handler is found, causing execution
    to continue at index 8. The astore_1 instruction at index 8 stores the thrown value
    into local variable 1. The following jsr instruction does a subroutine call to the
    code for the finally block. Assuming that code returns normally, the aload_1
    instruction at index 12 pushes the thrown value back onto the operand stack, and
    the following athrow instruction rethrows the value.

    带有finally子句的try语句被编译为具有特殊的异常处理程序，该异常处理程序可以处理try语句内引发的任何异常.
    如果tryItOut引发异常，则在tryFinally的异常表中搜索适当的异常处理程序.
    找到特殊处理程序，使执行在索引8处继续.
    索引8处的astore_1指令将抛出的值存储到局部变量1中.
    以下jsr指令对finally块的代码进行子例程调用.
    
    Compiling a try statement with both a catch clause and a finally clause is more
    complex:
        void tryCatchFinally() {
            try {
                tryItOut();
            } catch (TestExc e) {
                handleExc(e);
            } finally {
                wrapItUp();
            }
        }

    becomes:
        
        Method void tryCatchFinally()
        0 aload_0 // Beginning of try block
        1 invokevirtual #4 // Method Example.tryItOut()V
        4 goto 16 // Jump to finally block
        7 astore_3 // Beginning of handler for TestExc;
        // Store thrown value in local var 3
        8 aload_0 // Push this
        9 aload_3 // Push thrown value
        10 invokevirtual #6 // Invoke handler method:
        // Example.handleExc(LTestExc;)V
        13 goto 16 // This goto is unnecessary, but was
        // generated by javac in JDK 1.0.2
        16 jsr 26 // Call finally block
        19 return // Return after handling TestExc
        20 astore_1 // Beginning of handler for exceptions
        // other than TestExc, or exceptions
        // thrown while handling TestExc
        21 jsr 26 // Call finally block
        24 aload_1 // Push thrown value...
        25 athrow // ...and rethrow value to the invoker
        26 astore_2 // Beginning of finally block
        27 aload_0 // Push this
        28 invokevirtual #5 // Method Example.wrapItUp()V
        31 ret 2 // Return from finally block

        Exception table:
        From To Target Type
        0    4   7     Class TestExc
        0    16  20    any

    If the try statement completes normally, the goto instruction at index 4 jumps
    to the subroutine call for the finally block at index 16. The finally block at
    index 26 is executed, control returns to the return instruction at index 19, and
    tryCatchFinally returns normally.

    如果try语句正常完成，则索引4处的goto指令跳转到索引16处的finally块的子例程调用.
    执行索引26处的finally块，控制返回到索引19处的return指令，tryCatchFinally正常返回.

    If tryItOut throws an instance of TestExc, the first (innermost) applicable
    exception handler in the exception table is chosen to handle the exception. The
    code for that exception handler, beginning at index 7, passes the thrown value to
    handleExc and on its return makes the same subroutine call to the finally block
    at index 26 as in the normal case. If an exception is not thrown by handleExc,
    tryCatchFinally returns normally.

    如果tryItOut抛出一个TestExc实例，则选择异常表中的第一个（最内部）适用的异常处理程序来处理该异常.
    该异常处理程序的代码从索引7开始，将抛出的值传递给handleExc，并在返回时对索引26处的finally块进行与常规情况相同的子例程调用.
    如果handleExc没有引发异常，则tryCatchFinally将正常返回.

    If tryItOut throws a value that is not an instance of TestExc or if handleExc itself
    throws an exception, the condition is handled by the second entry in the exception
    table, which handles any value thrown between indices 0 and 16. That exception
    handler transfers control to index 20, where the thrown value is first stored in local
    variable 1. The code for the finally block at index 26 is called as a subroutine. If it
    returns, the thrown value is retrieved from local variable 1 and rethrown using the
    athrow instruction. If a new value is thrown during execution of the finally clause,
    the finally clause aborts, and tryCatchFinally returns abruptly, throwing the
    new value to its invoker.

    如果tryItOut抛出的值不是TestExc的实例，或者handleExc本身抛出异常，则条件由异常表中的第二个条目处理，该表处理在索引0和16之间引发的任何值.
    该异常处理程序将控制权转移到索引20，在该索引中首先将引发的值存储在局部变量1中. 索引26处的finally块的代码称为子例程.
    如果返回，则从局部变量1中检索抛出的值，并使用athrow指令重新抛出. 
    如果在执行finally子句的过程中抛出了新值，则finally子句将中止，tryCatchFinally突然返回，并将新值抛出给其调用者.

    */



```