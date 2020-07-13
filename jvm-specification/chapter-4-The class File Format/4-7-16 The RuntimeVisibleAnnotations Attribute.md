### 翻译Java虚拟机规范第四章第七节-第十六节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单介绍RuntimeVisibleAnnotations属性`


```java

    /**

    4.7.16 The RuntimeVisibleAnnotations Attribute
    
    RuntimeVisibleAnnotations属性

    The RuntimeVisibleAnnotations attribute is a variable-length attribute in the
    attributes table of a ClassFile, field_info, or method_info structure (§4.1,
    §4.5, §4.6). The RuntimeVisibleAnnotations attribute records run-time visible
    annotations on the declaration of the corresponding class, field, or method. The
    Java Virtual Machine must make these annotations available so they can be
    returned by the appropriate reflective APIs.

    Deprecated属性是ClassFile结构的属性表的可变长度属性. RuntimeVisibleAnnotations属性记录在对应类、字段、方法上的运行时可见注解.
    Java虚拟机必须使这些注释可用，以便可以通过适当的反射API返回它们.

    */



```