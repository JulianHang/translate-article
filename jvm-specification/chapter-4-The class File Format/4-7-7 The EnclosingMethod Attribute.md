### 翻译Java虚拟机规范第四章第七节-第七节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单介绍EnclosingMethod属性`


```java

    /**

    4.7.7 The EnclosingMethod Attribute
    
    EnclosingMethod属性

    The EnclosingMethod attribute is a fixed-length attribute in the attributes table
    of a ClassFile structure (§4.1). A class must have an EnclosingMethod attribute
    if and only if it represents a local class or an anonymous class (JLS §14.3, JLS
    §15.9.5).

    InnerClasses属性是ClassFile结构的属性表的固定长度属性. 当且仅当表示局部类或匿名类时有一个EnclosingMethod属性.


    */



```