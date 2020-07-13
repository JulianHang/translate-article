### 翻译Java虚拟机规范第四章第七节-第三节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单介绍Code属性`


```java

    /**

    4.7.3 The Code Attribute
    
    Code属性

    The Code attribute is a variable-length attribute in the attributes table of
    a method_info structure (§4.6). A Code attribute contains the Java Virtual
    Machine instructions and auxiliary information for a method, including an instance
    initialization method or a class or interface initialization method (§2.9).

    Code属性是method_info结构属性表中的可变长度. Code属性包含Java虚拟机指令和方法的附属信息，包含对象或类或接口的初始化方法.

    max_stack

        The value of the max_stack item gives the maximum depth of the operand
        stack of this method (§2.6.2) at any point during execution of the method.

    max_stack的值是方法执行期间的任意点上，该方法的操作数栈的指定最大深度

    max_locals

        The value of the max_locals item gives the number of local variables in
        the local variable array allocated upon invocation of this method (§2.6.1),
        including the local variables used to pass parameters to the method on its
        invocation.

    max_locals的值是在调用此方法（第2.6.1节）时分配的局部变量表中的局部变量数，包括用于在调用该方法时将参数传递给该方法的局部变量.

    */



```