### 翻译Java虚拟机规范第五章-第二节(JSR-337/Java8)

> `筛选重点内容进行翻译`

主要内容：`简单描述Java虚拟机的启动过程`


```java

    /**

    5.2 Java Virtual Machine Startup
    
    启动Java虚拟机

    The Java Virtual Machine starts up by creating an initial class, which is specified
    in an implementation-dependent manner, using the bootstrap class loader (§5.3.1).
    The Java Virtual Machine then links the initial class, initializes it, and invokes
    the public class method void main(String[]).  The invocation of this method
    drives all further execution. Execution of the Java Virtual Machine instructions
    constituting the main method may cause linking (and consequently creation) of
    additional classes and interfaces, as well as invocation of additional methods

    Java虚拟机通过使用引导类加载器（第5.3.1节）创建一个初始类来启动，该类以与实现相关的方式指定.
    Java虚拟机将链接初始类，对其进行初始化，然后调用公共类方法void main（String []）.
    该方法的调用将推动所有进一步的执行.
    构成main方法的Java虚拟机指令的执行可能导致其他类和接口的链接（并因此创建），以及其他方法的调用.

    */



```