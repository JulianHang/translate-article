### 翻译Java虚拟机规范第四章第七节-第八节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单介绍Synthetic属性`


```java

    /**

    4.7.8 The Synthetic Attribute
    
    Synthetic属性

    The Synthetic attribute is a fixed-length attribute in the attributes table of
    a ClassFile, field_info, or method_info structure (§4.1, §4.5, §4.6).
    A class member that does not appear in the source code must be marked using a Synthetic, 
    or else it must have its ACC_SYNTHETIC flag set.

    Synthetic属性是ClassFile、field_info、method_info结构的属性表的固定长度属性. 类成员没有出现在源码中必须使用Synthetic标记，或者是设置ACC_SYNTHETIC标志


    */



```