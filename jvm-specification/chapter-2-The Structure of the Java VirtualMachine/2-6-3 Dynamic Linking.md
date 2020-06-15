### 翻译Java虚拟机规范第二章第六节-第三节(JSR-337/Java8)

主要内容：`介绍动态连接`

```java

    /**

    2.6.3 Dynamic Linking

    动态连接

    Each frame (§2.6) contains a reference to the run-time constant pool (§2.5.5) for
    the type of the current method to support dynamic linking of the method code.
    The class file code for a method refers to methods to be invoked and variables
    to be accessed via symbolic references. Dynamic linking translates these symbolic
    method references into concrete method references, loading classes as necessary to
    resolve as-yet-undefined symbols, and translates variable accesses into appropriate
    offsets in storage structures associated with the run-time location of these variables.

    每个栈帧（第2.6节）都包含对当前方法类型的运行时常量池（第2.5.5节）的引用，以支持方法代码的动态链接. 
    方法的类文件代码是指要调用的方法和要通过符号引用访问的变量. 
    动态链接将这些符号方法引用转换为具体的方法引用，根据需要加载类以解析尚未定义的符号，并将变量访问转换为与运行时变量的位置相关联的存储结构中的适当偏移量.

    This late binding of the methods and variables makes changes in other classes that
    a method uses less likely to break this code.

    方法和变量的后期绑定使在其他类中方法使用的更改不太可能破坏此代码.

    */

```