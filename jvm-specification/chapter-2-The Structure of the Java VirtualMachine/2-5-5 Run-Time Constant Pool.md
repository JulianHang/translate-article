### 翻译Java虚拟机规范第二章第五节-第五节(JSR-337/Java8)

主要内容：`简单介绍运行时常量池，其内存由方法区分配，会发生OOM`

```java

    /**

    2.5.5 Run-Time Constant Pool

    运行时常量池

    A run-time constant pool is a per-class or per-interface run-time representation
    of the constant_pool table in a class file (§4.4). It contains several kinds of
    constants, ranging from numeric literals known at compile-time to method and field
    references that must be resolved at run-time. The run-time constant pool serves a
    function similar to that of a symbol table for a conventional programming language,
    although it contains a wider range of data than a typical symbol table.

    运行时常量池是类文件（第4.4节）中constant_pool表的类或接口的运行时表示. 它包含多种常量，范围从编译时已知的数字文字到必须在运行时解析的方法和字段引用.
    运行时常量池的功能类似于常规编程语言的符号表，尽管它包含的数据范围比典型的符号表还大.

    Each run-time constant pool is allocated from the Java Virtual Machine's method
    area (§2.5.4). The run-time constant pool for a class or interface is constructed
    when the class or interface is created (§5.3) by the Java Virtual Machine.

    每个运行时常量池都是由Java虚拟机的方法区（第2.5.4节）分配的. Java虚拟机创建类或接口（第5.3节）时，将构造该类或接口的运行时常量池.

    The following exceptional condition is associated with the construction of the runtime constant pool for a class or interface:

    以下异常条件与类或接口的运行时常量池的构造相关联：

    • When creating a class or interface, if the construction of the run-time constant
    pool requires more memory than can be made available in the method area of the
    Java Virtual Machine, the Java Virtual Machine throws an OutOfMemoryError.

    创建类或接口时，如果运行时常量池的构造所需的内存超过Java虚拟机的方法区中可用的内存，则Java虚拟机将引发OutOfMemoryError.

    See §5 (Loading, Linking, and Initializing) for information about the construction of the
    run-time constant pool.

    有关运行时常量池构造的信息，请参见§5（加载，链接和初始化）.

    */


```