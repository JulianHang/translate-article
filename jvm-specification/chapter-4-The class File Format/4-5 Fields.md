### 翻译Java虚拟机规范第四章第五节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单介绍常量池的field_info结构`


```java

    /**

    4.5 Fields
    
    字段

    Each field is described by a field_info structure

    使用field_info结构来描述字段

    标志名               值             解释

    ACC_PUBLIC           0x0001         Declared public; may be accessed from outside its package.  
    ACC_PRIVATE          0x0002         Declared private; usable only within the defining class.
    ACC_PROTECTED        0x0004         Declared protected; may be accessed within subclasses.
    ACC_STATIC           0x0008         Declared static.
    ACC_FINAL            0x0010         Declared final; never directly assigned to after object construction (JLS §17.5).
    ACC_VOLATILE         0x0040         Declared volatile; cannot be cached.
    ACC_TRANSIENT        0x0080         Declared transient; not written or read by a persistent object manager.
    ACC_SYNTHETIC        0x1000         Declared synthetic; not present in the source code.
    ACC_ENUM             0x4000         Declared as an element of an enum.

    The ACC_SYNTHETIC flag indicates that this field was generated by a compiler and does not appear in source code

    ACC_SYNTHETIC标志声明该字段是由编译器生成，不会出现在源码中.


    */



```