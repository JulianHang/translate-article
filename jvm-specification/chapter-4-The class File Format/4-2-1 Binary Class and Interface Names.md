### 翻译Java虚拟机规范第四章第二节-第一节(JSR-337/Java8)

主要内容：`简单说明接口与类的表示形式，在代码层面使用java.lang.Thread表示，而在Java虚拟机层面使用java/lang/Thread表示`


```java

    /**

    4.2.1 Binary Class and Interface Names

    二进制类和接口名

    Class and interface names that appear in class file structures are always
    represented in a fully qualified form known as binary names (JLS §13.1).
    Such names are always represented as CONSTANT_Utf8_info structures (§4.4.7)
    and thus may be drawn, where not further constrained, from the entire
    Unicode codespace. Class and interface names are referenced from those
    CONSTANT_NameAndType_info structures (§4.4.6) which have such names as part
    of their descriptor (§4.3), and from all CONSTANT_Class_info structures (§4.4.1).

    出现在类文件结构中的类和接口名称始终以完全合格的形式表示，即二进制名称（JLS§13.1）.
    这样的名称始终表示为CONSTANT_Utf8_info结构（第4.4.7节），因此可以从整个Unicode代码空间中提取（不受进一步限制）.
    类和接口名称是从那些具有这样的名称作为其描述符的一部分（第4.3.4节）的CONSTANT_NameAndType_info结构（第4.4.6节）以及所有CONSTANT_Class_info结构（第4.4.1节）中引用的.

    For historical reasons, the syntax of binary names that appear in class file
    structures differs from the syntax of binary names documented in JLS §13.1. In this
    internal form, the ASCII periods (.) that normally separate the identifiers which
    make up the binary name are replaced by ASCII forward slashes (/). The identifiers
    themselves must be unqualified names (§4.2.2).

    由于历史原因，出现在类文件结构中的二进制名称的语法与JLS§13.1中记录的二进制名称的语法不同.
    在这种内部形式中，通常分隔组成二进制名称的标识符的ASCII句点（.）被ASCII正斜杠（/）代替.
    标识符本身必须是不合格的名称（第4.2.2节）.

    For example, the normal binary name of class Thread is java.lang.Thread. In the
    internal form used in descriptors in the class file format, a reference to the name of class
    Thread is implemented using a CONSTANT_Utf8_info structure representing the string
    java/lang/Thread.

    例如，类Thread的常规二进制名称是java.lang.Thread. 
    在类文件格式的描述符中使用的内部形式中，使用表示字符串java/lang/Thread的CONSTANT_Utf8_info结构实现对类Thread名称的引用.

    */



```