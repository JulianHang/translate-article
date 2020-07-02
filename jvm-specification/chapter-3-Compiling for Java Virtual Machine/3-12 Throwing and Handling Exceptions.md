### 翻译Java虚拟机规范第三章第十二节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机对异常的处理`

```java

    /**

    3.12 Throwing and Handling Exceptions

    抛出和处理异常

    Exceptions are thrown from programs using the throw keyword. Its compilation
    is simple:

    使用throw关键字从程序中引发异常. 它的编译很简单：

        void cantBeZero(int i) throws TestExc {
            if (i == 0) {
                throw new TestExc();
            }
        }

    becomes:

        Method void cantBeZero(int)
        0 iload_1 // Push argument 1 (i)
        1 ifne 12 // If i==0, allocate instance and throw
        4 new #1 // Create instance of TestExc
        7 dup // One reference goes to its constructor
        8 invokespecial #7 // Method TestExc.<init>()V
        11 athrow // Second reference is thrown
        12 return // Never get here if we threw TestExc

    Compilation of try-catch constructs is straightforward. For example:

    try-catch构造的编译非常简单. 例如：

        void catchOne() {
            try {
                tryItOut();
            } catch (TestExc e) {
                handleExc(e);
            }
        }

    is compiled as:

        Method void catchOne()
        0 aload_0 // Beginning of try block
        1 invokevirtual #6 // Method Example.tryItOut()V
        4 return // End of try block; normal return
        5 astore_1 // Store thrown value in local var 1
        6 aload_0 // Push this
        7 aload_1 // Push thrown value
        8 invokevirtual #5 // Invoke handler method:
        // Example.handleExc(LTestExc;)V
        11 return // Return after handling TestExc
        Exception table:
        From To Target Type
        0 4 5 Class TestExc

    Looking more closely, the try block is compiled just as it would be if the try were
    not present:

    仔细观察，try块将被编译，就像没有try一样：

        Method void catchOne()
        0 aload_0 // Beginning of try block
        1 invokevirtual #6 // Method Example.tryItOut()V
        4 return // End of try block; normal return

    If no exception is thrown during the execution of the try block, it behaves as though
    the try were not there: tryItOut is invoked and catchOne returns.

    如果在try块执行期间未引发任何异常，则其行为就好像try不存在：tryItOut被调用，catchOne返回.

    Following the try block is the Java Virtual Machine code that implements the
    single catch clause:

    try块之后是实现单个catch子句的Java虚拟机代码：

        5 astore_1 // Store thrown value in local var 1
        6 aload_0 // Push this
        7 aload_1 // Push thrown value
        8 invokevirtual #5 // Invoke handler method:
        // Example.handleExc(LTestExc;)V
        11 return // Return after handling TestExc

        Exception table:
        From To Target Type
        0    4   5     Class TestExc

    The invocation of handleExc, the contents of the catch clause, is also compiled
    like a normal method invocation. However, the presence of a catch clause causes
    the compiler to generate an exception table entry (§2.10, §4.7.3). The exception
    table for the catchOne method has one entry corresponding to the one argument (an
    instance of class TestExc) that the catch clause of catchOne can handle. If some
    value that is an instance of TestExc is thrown during execution of the instructions
    between indices 0 and 4 in catchOne, control is transferred to the Java Virtual
    Machine code at index 5, which implements the block of the catch clause. If the
    value that is thrown is not an instance of TestExc, the catch clause of catchOne
    cannot handle it. Instead, the value is rethrown to the invoker of catchOne.
    A try may have multiple catch clauses:

    catchExc（即catch子句的内容）的调用也像普通方法调用一样进行编译. 但是，catch子句的存在会导致编译器生成异常表条目（第2.10节，第4.7.3节）.
    catchOne方法的异常表具有一个条目，该条目对应于catchOne的catch子句可以处理的一个参数（类TestExc的实例）.
    如果在catchOne的索引0和4之间的指令执行期间抛出了一些TestExc实例的值，则将控制权转移到索引5处的Java虚拟机代码，该代码实现catch子句的块.
    如果抛出的值不是TestExc的实例，则catchOne的catch子句无法处理它.
    取而代之的是，该值将重新引发给catchOne的调用者.
    一次尝试可能具有多个catch子句：

        void catchTwo() {
            try {
                tryItOut();
            } catch (TestExc1 e) {
                handleExc(e);
            } catch (TestExc2 e) {
                handleExc(e);
            }
        }

    Multiple catch clauses of a given try statement are compiled by simply appending
    the Java Virtual Machine code for each catch clause one after the other and adding
    entries to the exception table, as shown:

    指定try语句的多个catch子句可通过简单地为每个catch子句一个接一个地追加Java虚拟机代码并将其添加到异常表中来编译，如下所示：

        Method void catchTwo()
        0 aload_0 // Begin try block
        1 invokevirtual #5 // Method Example.tryItOut()V
        4 return // End of try block; normal return
        5 astore_1 // Beginning of handler for TestExc1;
        // Store thrown value in local var 1
        6 aload_0 // Push this
        7 aload_1 // Push thrown value
        8 invokevirtual #7 // Invoke handler method:
        // Example.handleExc(LTestExc1;)V
        11 return // Return after handling TestExc1
        12 astore_1 // Beginning of handler for TestExc2;
        // Store thrown value in local var 1
        13 aload_0 // Push this
        14 aload_1 // Push thrown value
        15 invokevirtual #7 // Invoke handler method:
        // Example.handleExc(LTestExc2;)V
        18 return // Return after handling TestExc2

        Exception table:
        From To Target Type
        0    4   5     Class TestExc1
        0    4   12    Class TestExc2

    If during the execution of the try clause (between indices 0 and 4) a value is thrown
    that matches the parameter of one or more of the catch clauses (the value is an
    instance of one or more of the parameters), the first (innermost) such catch clause
    is selected. Control is transferred to the Java Virtual Machine code for the block of
    that catch clause. If the value thrown does not match the parameter of any of the
    catch clauses of catchTwo, the Java Virtual Machine rethrows the value without
    invoking code in any catch clause of catchTwo.

    如果在try子句执行期间（在索引0和4之间）抛出一个或多个与catch子句的参数匹配的值（该值是一个或多个参数的实例），则第一个（最内部））atch子句被选中.
    将控制权转移到该catch子句的Java虚拟机代码. 
    如果抛出的值与catchTwo的任何catch子句的参数都不匹配，则Java虚拟机将在不调用catchTwo的任何catch子句的情况下重新抛出该值.

    Nested try-catch statements are compiled very much like a try statement with
    multiple catch clauses:

    嵌套的try-catch语句的编译非常类似于带有多个try语句的try语句：

        void nestedCatch() {
            try {
                try {
                    tryItOut();
                } catch (TestExc1 e) {
                    handleExc1(e);
                }
            } catch (TestExc2 e) {
                handleExc2(e);
            }
        }

    becomes:

        Method void nestedCatch()
        0 aload_0 // Begin try block
        1 invokevirtual #8 // Method Example.tryItOut()V
        4 return // End of try block; normal return
        5 astore_1 // Beginning of handler for TestExc1;
        // Store thrown value in local var 1
        6 aload_0 // Push this
        7 aload_1 // Push thrown value
        8 invokevirtual #7 // Invoke handler method:
        // Example.handleExc1(LTestExc1;)V
        11 return // Return after handling TestExc1
        12 astore_1 // Beginning of handler for TestExc2;
        // Store thrown value in local var 1
        13 aload_0 // Push this
        14 aload_1 // Push thrown value
        15 invokevirtual #6 // Invoke handler method:
        // Example.handleExc2(LTestExc2;)V
        18 return // Return after handling TestExc2

        Exception table:
        From To Target Type
        0    4  5      Class TestExc1
        0    12 12     Class TestExc2

    The nesting of catch clauses is represented only in the exception table. The Java
    Virtual Machine does not enforce nesting of or any ordering of the exception table
    entries (§2.10). However, because try-catch constructs are structured, a compiler
    can always order the entries of the exception handler table such that, for any thrown
    exception and any program counter value in that method, the first exception handler
    that matches the thrown exception corresponds to the innermost matching catch
    clause.    

    catch子句的嵌套仅在异常表中表示. Java虚拟机不强制异常表条目的嵌套或任何顺序（第2.10节）. 
    但是，由于try-catch构造是结构化的，因此编译器始终可以对异常处理程序表的条目进行排序，以便用于方法中任何引发的异常和程序计数器值，引发的异常匹配的第一个异常处理程序对应于最里面匹配的catch子句.

    For instance, if the invocation of tryItOut (at index 1) threw an instance of
    TestExc1, it would be handled by the catch clause that invokes handleExc1. This
    is so even though the exception occurs within the bounds of the outer catch clause
    (catching TestExc2) and even though that outer catch clause might otherwise have
    been able to handle the thrown value.

    例如，如果tryItOut的调用（在索引1处）抛出了TestExc1的实例，则它将由调用handleExc1的catch子句处理.
    即使在外部catch子句（捕获TestExc2）的范围内发生异常，即使该外部catch子句可能能够处理抛出的值，也是如此.

    As a subtle point, note that the range of a catch clause is inclusive on the "from"
    end and exclusive on the "to" end (§4.7.3). Thus, the exception table entry for the
    catch clause catching TestExc1 does not cover the return instruction at offset 4.
    However, the exception table entry for the catch clause catching TestExc2 does
    cover the return instruction at offset 11. Return instructions within nested catch
    clauses are included in the range of instructions covered by nesting catch clauses.

    作为一个微妙的一点，请注意catch子句的范围在"from"端是包含的，而在"to"端是排斥的（第4.7.3节）.
    因此，catch子句捕获TestExc1的异常表条目不涵盖偏移量4处的返回指令.
    但是，catch子句捕获TestExc2的异常表条目确实涵盖偏移量11处的返回指令.
    嵌套catch子句中的返回指令包含在嵌套catch子句涵盖的指令范围内.

    */



```