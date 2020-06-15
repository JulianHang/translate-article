### 翻译Java虚拟机规范第二章第一节-(JSR-337/Java8)

主要内容：`简单说明什么是类文件格式（字节码）`

```java

    /**

    The class File Format

    类文件格式

    Compiled code to be executed by the Java Virtual Machine is represented using
    a hardware- and operating system-independent binary format, typically (but not
    necessarily) stored in a file, known as the class file format. The class file format
    precisely defines the representation of a class or interface, including details such
    as byte ordering that might be taken for granted in a platform-specific object file
    format.

    Java虚拟机要执行的已编译代码使用独立于硬件和操作系统的二进制格式表示，通常（但不一定）存储在称为类文件格式的文件中.
    类文件格式精确地定义了类或接口的表示形式，包括诸如字节顺序之类的细节，这些细节可能在特定于平台的对象文件格式中被视为理所当然.

    Chapter 4, "The class File Format", covers the class file format in detail.

    第4章"类文件格式"详细介绍了类文件格式.


     */
```