### 翻译Java虚拟机规范第二章第十二节(JSR-337/Java8)

主要内容：`简单说明Java虚拟机对类库的支持`

```java

    /**

    2.12 Class Libraries

    类库

    The Java Virtual Machine must provide sufficient support for the implementation
    of the class libraries of the Java SE platform. Some of the classes in these libraries
    cannot be implemented without the cooperation of the Java Virtual Machine.

    Java虚拟机必须为Java SE平台的类库的实现提供足够的支持. 没有Java虚拟机的合作，这些库中的某些类将无法实现.

    Classes that might require special support from the Java Virtual Machine include
    those that support:

    可能需要Java虚拟机特殊支持的类包括那些支持以下内容的类:
    
    • Reflection, such as the classes in the package java.lang.reflect and the class
    Class.

    反射，例如包java.lang.reflect中的类和类Class.

    • Loading and creation of a class or interface. The most obvious example is the
    class ClassLoader.

    加载和创建类或接口。 最明显的例子是ClassLoader类.

    • Linking and initialization of a class or interface. The example classes cited above
    fall into this category as well.

    类或接口的链接和初始化。 上面引用的示例类也属于此类.

    • Security, such as the classes in the package java.security and other classes
    such as SecurityManager.

    安全性，例如包java.security中的类，以及其他类，例如SecurityManager.

    • Multithreading, such as the class Thread.

    多线程，例如Thread类.

    • Weak references, such as the classes in the package java.lang.ref.

    弱引用，例如包java.lang.ref中的类.

    The list above is meant to be illustrative rather than comprehensive. An exhaustive
    list of these classes or of the functionality they provide is beyond the scope of
    this specification. See the specifications of the Java SE platform class libraries for
    details.

    上面的列表是说明性的，而不是全面的。 这些类或它们提供的功能的详尽列表超出了本规范的范围.
    有关详细信息，请参见Java SE平台类库的规范.

    */



```