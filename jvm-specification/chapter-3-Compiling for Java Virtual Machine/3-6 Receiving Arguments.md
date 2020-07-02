### 翻译Java虚拟机规范第三章第六节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机在实例方法与类方法中参数传递的区别`

```java

    /**

    3.6 Receiving Arguments

    接收参数

    If n arguments are passed to an instance method, they are received, by convention,
    in the local variables numbered 1 through n of the frame created for the new method
    invocation. The arguments are received in the order they were passed. For example:

    如果将n个参数传递给实例方法，则按照惯例，它们在为新方法调用而创建的栈帧中编号为1到n的局部变量所接收的.
    参数按传递顺序接收. 例如：

        int addTwo(int i, int j) {
            return i + j;
        }

    compiles to:

        Method int addTwo(int,int)

        0 iload_1 // Push value of local variable 1 (i)
        1 iload_2 // Push value of local variable 2 (j)
        2 iadd // Add; leave int result on operand stack
        3 ireturn // Return int result

    By convention, an instance method is passed a reference to its instance in local
    variable 0. In the Java programming language the instance is accessible via the
    this keyword.

    按照约定，实例方法将在本地变量0中传递对其实例的引用. 在Java编程语言中，可通过this关键字访问该实例.

    Class (static) methods do not have an instance, so for them this use of local
    variable 0 is unnecessary. A class method starts using local variables at index 0. If
    the addTwo method were a class method, its arguments would be passed in a similar
    way to the first version:

    类（静态）方法没有实例，因此对于它们而言，不需要使用局部变量0.
    一个类方法从索引0开始使用局部变量. 如果addTwo方法是一个类方法，则其参数将以与第一个版本相似的方式传递：

        static int addTwoStatic(int i, int j) {
            return i + j;
        }

    compiles to:

        Method int addTwoStatic(int,int)
        0 iload_0
        1 iload_1
        2 iadd
        3 ireturn

    The only difference is that the method arguments appear starting in local variable
    0 rather than 1.

    唯一的区别是方法参数从局部变量0而不是1开始出现.

    */



```