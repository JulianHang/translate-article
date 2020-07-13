### 翻译Java虚拟机规范第四章第四节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单提及常量池的格式`


```java

    /**

    4.4 The Constant Pool
    
    常量池

    All constant_pool table entries have the following general format:
    
    所有常量池表项有以下常规格式：

        cp_info {
            u1 tag;
            u1 info[];
        }

    常量类型                    值

    CONSTANT_Class              7
    CONSTANT_Fieldref           9
    CONSTANT_Methodref          10
    CONSTANT_InterfaceMethodref 11
    CONSTANT_String             8
    CONSTANT_Integer            3
    CONSTANT_Float              4
    CONSTANT_Long               5
    CONSTANT_Double             6
    CONSTANT_NameAndType        12
    CONSTANT_Utf8               1
    CONSTANT_MethodHandle       15
    CONSTANT_MethodType         16
    CONSTANT_InvokeDynamic      18

    The contents of the info array vary with the value of tag.

    info数组的内容随tag的值而变化.

    */



```