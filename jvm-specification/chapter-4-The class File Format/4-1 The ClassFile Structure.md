### 翻译Java虚拟机规范第四章第一节(JSR-337/Java8)

主要内容：`简要介绍类文件结构中每一项的含义`


```java

    /**

    4.1 The ClassFile Structure

    类文件结构

    A class file consists of a single ClassFile structure:

    一个类文件由单个ClassFile结构组成:

    ClassFile {
        u4 magic;
        u2 minor_version;
        u2 major_version;
        u2 constant_pool_count;
        cp_info constant_pool[constant_pool_count-1];
        u2 access_flags;
        u2 this_class;
        u2 super_class;
        u2 interfaces_count;
        u2 interfaces[interfaces_count];
        u2 fields_count;
        field_info fields[fields_count];
        u2 methods_count;
        method_info methods[methods_count];
        u2 attributes_count;
        attribute_info attributes[attributes_count];
    }

    The items in the ClassFile structure are as follows:

    ClassFile结构中的项目如下:

    magic

    魔数

        The magic item supplies the magic number identifying the class file format;
        it has the value 0xCAFEBABE.

    魔数项提供用于标识类文件格式的魔术数字；其值为0xCAFEBABE.

    minor_version, major_version

    次版本号，主版本号

        The values of the minor_version and major_version items are the minor and
        major version numbers of this class file. Together, a major and a minor version
        number determine the version of the class file format. If a class file has major
        version number M and minor version number m, we denote the version of its
        class file format as M.m. Thus, class file format versions may be ordered
        lexicographically, for example, 1.5 < 2.0 < 2.1.

        minor_version和major_version项目的值是类文件的次要版本号和主要版本号. 主版本号和次版本号共同决定了类文件格式的版本.
        如果类文件的主要版本号为M，次要版本号为m，则将其类文件格式的版本表示为M.m. 
        因此，可以按字典顺序对类文件格式版本进行排序，例如1.5 <2.0 <2.1.

        A Java Virtual Machine implementation can support a class file format of
        version v if and only if v lies in some contiguous range Mi.0 ≤ v ≤ Mj.m.
        The release level of the Java SE platform to which a Java Virtual Machine
        implementation conforms is responsible for determining the range.

        当且仅当v处于Mi.0≤v≤Mj.m的某个连续范围内时，Java虚拟机实现才能支持版本v的类文件格式. 
        Java虚拟机实现所遵循的Java SE平台的发行级别负责确定范围.

        Oracle's Java Virtual Machine implementation in JDK release 1.0.2 supports class file
        format versions 45.0 through 45.3 inclusive. JDK releases 1.1.* support class file format
        versions in the range 45.0 through 45.65535 inclusive. For k ≥ 2, JDK release 1.k supports
        class file format versions in the range 45.0 through 44+k.0 inclusive.

        JDK 1.0.2版中的Oracle Java虚拟机实现支持类文件格式版本45.0至45.3（含）. 
        JDK发行的1.1.*支持类文件格式版本范围为45.0到45.65535（含）.
        对于k≥2，JDK版本1.k支持45.0到44 + k.0（含）范围内的类文件格式版本.

    constant_pool_count

    常量的数量

        The value of the constant_pool_count item is equal to the number of entries
        in the constant_pool table plus one. A constant_pool index is considered
        valid if it is greater than zero and less than constant_pool_count, with the
        exception for constants of type long and double noted in §4.4.5.
    
        constant_pool_count项的值等于constant_pool表中的条目数加一.
        如果constant_pool索引大于零且小于constant_pool_count，则该索引被视为有效，但§4.4.5中指出的long和double类型的常量除外.

    constant_pool[]

    常量池

        The constant_pool is a table of structures (§4.4) representing various string
        constants, class and interface names, field names, and other constants that are
        referred to within the ClassFile structure and its substructures. The format of
        each constant_pool table entry is indicated by its first "tag" byte.
        The constant_pool table is indexed from 1 to constant_pool_count - 1.

        constant_pool是一个结构表（第4.4节），表示各种字符串常量、类接口名称、字段名称以及在ClassFile结构及其子结构中引用的其他常量.
        每个constant_pool表条目的格式由其第一个"tag"字节指示.
        constant_pool表的索引从1到constant_pool_count-1.
    
    access_flags

    权限标识

        The value of the access_flags item is a mask of flags used to denote access
        permissions to and properties of this class or interface. The interpretation of
        each flag, when set, is specified in Table 4.1-A.

        access_flags项的值是标志的掩码，用于表示对该类或接口的访问权限及其属性. 设置后，每个标志的解释在表4.1-A中指定.

    ACC_PUBLIC         0x0001         Declared public; may be accessed from outside its package.  声明public
    ACC_FINAL          0x0010         Declared final; no subclasses allowed.  声明final
    ACC_SUPER          0x0020         Treat superclass methods specially when invoked by the invokespecial instruction.  当被invokespecial指令调用时，特别处理超类方法.
    ACC_INTERFACE      0x0200         Is an interface, not a class.  声明接口
    ACC_ABSTRACT       0x0400         Declared abstract; must not be instantiated.  声明抽象
    ACC_SYNTHETIC      0x1000         Declared synthetic; not present in the source code.  声明合成的，源代码中不存在
    ACC_ANNOTATION     0x2000         Declared as an annotation type.  声明注解
    ACC_ENUM           0x4000         Declared as an enum type.  声明枚举

    An interface is distinguished by the ACC_INTERFACE flag being set. If the
    ACC_INTERFACE flag is not set, this class file defines a class, not an interface.

    接口通过设置ACC_INTERFACE标志来区分. 如果未设置ACC_INTERFACE标志，则此类文件定义一个类，而不是一个接口.

    If the ACC_INTERFACE flag is set, the ACC_ABSTRACT flag must also be set, and
    the ACC_FINAL, ACC_SUPER, and ACC_ENUM flags set must not be set.

    如果设置了ACC_INTERFACE标志，也必须设置ACC_ABSTRACT标志，并且必须不设置ACC_FINAL、ACC_SUPER和ACC_ENUM标志.

    If the ACC_INTERFACE flag is not set, any of the other flags in Table 4.1-A may
    be set except ACC_ANNOTATION. However, such a class file must not have both
    its ACC_FINAL and ACC_ABSTRACT flags set (JLS §8.1.1.2).

    如果未设置ACC_INTERFACE标志，则可以设置表4.1-A中除ACC_ANNOTATION以外的任何其他标志. 
    但是，此类类文件不能同时设置ACC_FINAL和ACC_ABSTRACT标志（JLS§8.1.1.2）.

    The ACC_SUPER flag indicates which of two alternative semantics is to be
    expressed by the invokespecial instruction (§invokespecial) if it appears in
    this class or interface. Compilers to the instruction set of the Java Virtual
    Machine should set the ACC_SUPER flag. In Java SE 8 and above, the Java
    Virtual Machine considers the ACC_SUPER flag to be set in every class file,
    regardless of the actual value of the flag in the class file and the version of
    the class file.

    如果ACC_SUPER标志出现在此类或接口中，则该标志指示将由invokespecial指令（§invokespecial）表示两种替代语义中的一种.
    Java虚拟机指令集的编译器应设置ACC_SUPER标志.
    在Java SE 8和更高版本中，Java虚拟机认为将在每个类文件中设置ACC_SUPER标志，而不管该标志在该类文件中的实际值和类文件的版本如何.

    The ACC_SUPER flag exists for backward compatibility with code compiled by older
    compilers for the Java programming language. In JDK releases prior to 1.0.2, the compiler
    generated access_flags in which the flag now representing ACC_SUPER had no assigned
    meaning, and Oracle's Java Virtual Machine implementation ignored the flag if it was set.

    存在ACC_SUPER标志是为了与旧的编译器为Java编程语言编译的代码向后兼容.
    在1.0.2之前的JDK版本中，编译器生成了access_flags，其中现在表示ACC_SUPER的标志没有分配的含义，如果设置了该标志，Oracle的Java Virtual Machine实现将忽略该标志.

    The ACC_SYNTHETIC flag indicates that this class or interface was generated by
    a compiler and does not appear in source code.

    ACC_SYNTHETIC标志指示该类或接口是由编译器生成的，并且未出现在源代码中.

    An annotation type must have its ACC_ANNOTATION flag set. If the
    ACC_ANNOTATION flag is set, the ACC_INTERFACE flag must also be set.
    The ACC_ENUM flag indicates that this class or its superclass is declared as an
    enumerated type.

    注解类型必须设置其ACC_ANNOTATION标志. 如果设置了ACC_ANNOTATION标志，则还必须设置ACC_INTERFACE标志.
    ACC_ENUM标志指示该类或其超类被声明为枚举类型.

    All bits of the access_flags item not assigned in Table 4.1-A are reserved for
    future use. They should be set to zero in generated class files and should be
    ignored by Java Virtual Machine implementations.

    表4.1-A中未分配的access_flags项目的所有位均保留供将来使用. 在生成的类文件中，应将它们设置为零，并且Java虚拟机实现应将其忽略.

    this_class

    当前类

        The value of the this_class item must be a valid index into the
        constant_pool table. The constant_pool entry at that index must be a
        CONSTANT_Class_info structure (§4.4.1) representing the class or interface
        defined by this class file.

        this_class项的值必须是constant_pool表中的有效索引. 
        该索引处的constant_pool条目必须是代表此类文件定义的类或接口的CONSTANT_Class_info结构（第4.4.1节）.

    super_class

    父类

        For a class, the value of the super_class item either must be zero or
        must be a valid index into the constant_pool table. If the value of the
        super_class item is nonzero, the constant_pool entry at that index must
        be a CONSTANT_Class_info structure representing the direct superclass of the
        class defined by this class file. Neither the direct superclass nor any of its
        superclasses may have the ACC_FINAL flag set in the access_flags item of its
        ClassFile structure.

        对于一个类，super_class项的值必须为零或必须是constant_pool表中的有效索引.
        如果super_class项的值不为零，则该索引处的constant_pool条目必须为CONSTANT_Class_info结构，该结构表示此类文件定义的类的直接超类.
        直接超类或其任何超类都不能在其ClassFile结构的access_flags项中设置ACC_FINAL标志.

    If the value of the super_class item is zero, then this class file must represent
    the class Object, the only class or interface without a direct superclass.

    如果super_class项的值为零，则该类文件必须表示类Object，这是没有直接超类的唯一类或接口.

    For an interface, the value of the super_class item must always be a valid
    index into the constant_pool table. The constant_pool entry at that index
    must be a CONSTANT_Class_info structure representing the class Object.

    对于接口，super_class项的值必须始终是constant_pool表中的有效索引. 
    该索引处的constant_pool条目必须是代表类Object的CONSTANT_Class_info结构.

    interfaces_count

    接口的数量

        The value of the interfaces_count item gives the number of direct
        superinterfaces of this class or interface type.

        interfaces_count项的值给出了此类或接口类型的直接超接口的数量.

    interfaces[]

    接口表

        Each value in the interfaces array must be a valid index into
        the constant_pool table. The constant_pool entry at each value
        of interfaces[i], where 0 ≤ i < interfaces_count, must be a
        CONSTANT_Class_info structure representing an interface that is a direct
        superinterface of this class or interface type, in the left-to-right order given in
        the source for the type.

        interfaces数组中的每个值都必须是constant_pool表中的有效索引.
        interfaces[i]的每个值（其中0 ≤i <interfaces_count）上的constant_pool条目必须是CONSTANT_Class_info结构，该结构代表该类或接口类型的直接超接口，并按类型的来源从左到右的顺序给出.

    fields_count

    字段的数量

        The value of the fields_count item gives the number of field_info
        structures in the fields table. The field_info structures represent all fields,
        both class variables and instance variables, declared by this class or interface
        type.

        fields_count项目的值给出了fields表中field_info结构的数量.
        field_info结构代表此类或接口类型声明的所有字段，包括类变量和实例变量.

    fields[]

    字段表

        Each value in the fields table must be a field_info structure (§4.5) giving
        a complete description of a field in this class or interface. The fields table
        includes only those fields that are declared by this class or interface. It does
        not include items representing fields that are inherited from superclasses or
        superinterfaces.

        字段表中的每个值都必须是field_info结构（第4.5节），以提供对该类或接口中字段的完整描述.
        字段表仅包含由此类或接口声明的字段.
        它不包括表示从超类或超接口继承的字段项.

    methods_count

    方法的数量

        The value of the methods_count item gives the number of method_info
        structures in the methods table.

        methods_count项的值给出了methods表中method_info结构的数量.

    methods[]

        Each value in the methods table must be a method_info structure (§4.6) giving
        a complete description of a method in this class or interface. If neither of the
        ACC_NATIVE and ACC_ABSTRACT flags are set in the access_flags item of a
        method_info structure, the Java Virtual Machine instructions implementing
        the method are also supplied.

        方法表中的每个值都必须是method_info结构（第4.6节），以提供对该类或接口中方法的完整描述.
        如果在method_info结构的access_flags项中均未设置ACC_NATIVE和ACC_ABSTRACT标志，则还将提供实现该方法的Java虚拟机指令.

        The method_info structures represent all methods declared by this class
        or interface type, including instance methods, class methods, instance
        initialization methods (§2.9), and any class or interface initialization method
        (§2.9). The methods table does not include items representing methods that are
        inherited from superclasses or superinterfaces.

        method_info结构表示此类或接口类型声明的所有方法，包括实例方法、类方法、实例初始化方法（第2.9节）以及任何类或接口初始化的方法（第2.9节）.
        方法表不包括表示从超类或超接口继承的方法项.

    attributes_count

    属性的数量

        The value of the attributes_count item gives the number of attributes in the
        attributes table of this class.

        attribute_count项目的值提供此类的属性表中的属性数量.

    attributes[]

        Each value of the attributes table must be an attribute_info structure(§4.7).

        属性表的每个值都必须是attribute_info结构（第4.7节）.

        The attributes defined by this specification as appearing in the attributes
        table of a ClassFile structure are listed in Table 4.7-C.

        表4.7-C列出了此规范定义的，显示在ClassFile结构的属性表中的属性.

        The rules concerning attributes defined to appear in the attributes table of a
        ClassFile structure are given in §4.7.

        在§4.7中给出了有关定义为出现在ClassFile结构的属性表中的属性的规则.

        The rules concerning non-predefined attributes in the attributes table of a
        ClassFile structure are given in §4.7.1.

        第4.7.1节中给出了有关ClassFile结构的属性表中非预定义属性的规则.

    */



```