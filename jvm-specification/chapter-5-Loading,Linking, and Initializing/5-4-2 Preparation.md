### 翻译Java虚拟机规范第五章-第四节-第二节(JSR-337/Java8)

> `筛选重点内容进行翻译`

主要内容：`简单描述Java虚拟机的准备`


```java

    /**

    5.4.2 Preparation
    
    准备

    Preparation involves creating the static fields for a class or interface and initializing
    such fields to their default values (§2.3, §2.4). This does not require the execution
    of any Java Virtual Machine code; explicit initializers for static fields are executed
    as part of initialization (§5.5), not preparation.

    准备工作包括为类或接口创建静态字段，并将这些字段初始化为其默认值（第2.3节，第2.4节）.
    这不需要执行任何Java虚拟机代码.
    静态字段的显式初始化程序是初始化（第5.5节）的一部分，而不是准备工作.

    */



```