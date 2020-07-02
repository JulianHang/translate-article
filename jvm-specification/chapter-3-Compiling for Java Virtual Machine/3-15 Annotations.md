### 翻译Java虚拟机规范第三章第十五节(JSR-337/Java8)

主要内容：`简单说明Java虚拟机对注解的处理`

```java

    /**

    3.15 Annotations

    注解

    The representation of annotations in class files is described in §4.7.16-§4.7.22.
    These sections make it clear how to represent annotations on declarations of
    classes, interfaces, fields, methods, method parameters, and type parameters, as
    well as annotations on types used in those declarations. Annotations on package
    declarations require additional rules, given here.

    §4.7.16-§4.7.22中描述了类文件中注释的表示. 这些部分使如何清楚地表示关于类、接口、字段、方法、方法参数和类型参数的声明的注释，以及在这些声明中使用的类型的注解.
    包声明的注解需要此处提供的其他规则.

    When the compiler encounters an annotated package declaration that must be made
    available at run time, it emits a class file with the following properties:

    当编译器遇到必须在运行时提供带注解的程序包声明时，它将发出具有以下属性的类文件：

    • The class file represents an interface, that is, the ACC_INTERFACE and
    ACC_ABSTRACT flags of the ClassFile structure are set (§4.1).

    该类文件表示一个接口，即，设置了ClassFile结构的ACC_INTERFACE和ACC_ABSTRACT标志（第4.1节）.

    • If the class file version number is less than 50.0, then the ACC_SYNTHETIC flag is
    unset; if the class file version number is 50.0 or above, then the ACC_SYNTHETIC
    flag is set.

    如果类文件版本号小于50.0，则不设置ACC_SYNTHETIC标志. 如果类文件版本号为50.0或更高，则设置ACC_SYNTHETIC标志.

    • The interface has package access (JLS §6.6.1).

    该接口具有程序包访问权限（JLS§6.6.1）.

    • The interface's name is the internal form (§4.2.1) of package-name.packageinfo.

    接口的名称是package-name.packageinfo的内部形式（第4.2.1节）.

    • The interface has no superinterfaces.

    该接口没有超级接口.

    • The interface's only members are those implied by The Java Language
    Specification, Java SE 8 Edition (JLS §9.2).

    该接口的唯一成员是Java语言规范Java SE 8 Edition（JLS§9.2）所隐含的成员.

    • The annotations on the package declaration are stored as
    RuntimeVisibleAnnotations and RuntimeInvisibleAnnotations attributes
    in the attributes table of the ClassFile structure.

    程序包声明中的注解存储为ClassFile结构的属性表中的RuntimeVisibleAnnotations和RuntimeInvisibleAnnotations属性.


    */



```