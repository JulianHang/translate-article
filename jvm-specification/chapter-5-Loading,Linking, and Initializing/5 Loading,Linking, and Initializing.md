### 翻译Java虚拟机规范第五章(JSR-337/Java8)

> `筛选重点内容进行翻译`

主要内容：`简单描述Java虚拟机的加载、链接、初始化过程`


```java

    /**

    5 Loading, Linking, and Initializing
    
    加载，链接，初始化

    THE Java Virtual Machine dynamically loads, links and initializes classes and
    interfaces. Loading is the process of finding the binary representation of a class
    or interface type with a particular name and creating a class or interface from
    that binary representation. Linking is the process of taking a class or interface and
    combining it into the run-time state of the Java Virtual Machine so that it can be
    executed. Initialization of a class or interface consists of executing the class or
    interface initialization method <clinit> (§2.9).

    Java虚拟机动态加载，链接和初始化类和接口. 加载是查找具有特定名称的类或接口类型的二进制表示形式并从该二进制表示形式创建类或接口的过程.
    链接是获取类或接口并将其组合到Java虚拟机的运行时状态以便可以执行的过程.
    类或接口的初始化包括执行类或接口的初始化方法<clinit>（第2.9节）.    

    */



```