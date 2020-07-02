### 翻译Java虚拟机规范第三章第八节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机对构造函数所采用的指令`

```java

    /**

    3.8 Working with Class Instances

    使用类实例

    Java Virtual Machine class instances are created using the Java Virtual Machine's
    new instruction. Recall that at the level of the Java Virtual Machine, a constructor
    appears as a method with the compiler-supplied name <init>. This specially
    named method is known as the instance initialization method (§2.9). Multiple
    instance initialization methods, corresponding to multiple constructors, may exist
    for a given class. Once the class instance has been created and its instance variables,
    including those of the class and all of its superclasses, have been initialized to
    their default values, an instance initialization method of the new class instance is
    invoked. For example:

    Java虚拟机类实例是使用Java虚拟机的新指令创建的. 回想一下，在Java虚拟机级别上，构造函数显示为带有编译器提供的名称<init>的方法.
    这种特别命名的方法称为实例初始化方法（第2.9节）. 对于给定的类，可能存在对应于多个构造函数的多个实例初始化方法.
    一旦创建了类实例，并将其实例变量（包括该类及其所有超类的实例变量）初始化为其默认值，便会调用新类实例的实例初始化方法.
    例如：

        Object create() {
            return new Object();
        }

    compiles to:

        Method java.lang.Object create()
        0 new #1 // Class java.lang.Object
        3 dup
        4 invokespecial #4 // Method java.lang.Object.<init>()V
        7 areturn
    
    Class instances are passed and returned (as reference types) very much like
    numeric values, although type reference has its own complement of instructions,
    for example:

    类实例的传递和返回（作为引用类型）与数值非常相似，尽管类型引用具有自己的指令补全，例如：

        int i; // An instance variable
        MyObj example() {
            MyObj o = new MyObj();
            return silly(o);
        }
        MyObj silly(MyObj o) {
            if (o != null) {
                return o;
            } else {
                return o;
            }
        }

    becomes:

        Method MyObj example()
        0 new #2 // Class MyObj
        3 dup
        4 invokespecial #5 // Method MyObj.<init>()V
        7 astore_1
        8 aload_0
        9 aload_1
        10 invokevirtual #4 // Method Example.silly(LMyObj;)LMyObj;
        13 areturn
        Method MyObj silly(MyObj)
        0 aload_1
        1 ifnull 6
        4 aload_1
        5 areturn
        6 aload_1
        7 areturn

    The fields of a class instance (instance variables) are accessed using the getfield
    and putfield instructions. If i is an instance variable of type int, the methods setIt
    and getIt, defined as:

    使用getfield和putfield指令访问类实例的字段（实例变量）. 如果i是int类型的实例变量，则方法setIt和getIt定义为：

        void setIt(int value) {
            i = value;
        }
        int getIt() {
            return i;
        }
    
    become:

        Method void setIt(int)
        0 aload_0
        1 iload_1
        2 putfield #4 // Field Example.i I
        5 return
        Method int getIt()
        0 aload_0
        1 getfield #4 // Field Example.i I
        4 ireturn
        
    As with the operands of method invocation instructions, the operands of the putfield
    and getfield instructions (the run-time constant pool index #4) are not the offsets
    of the fields in the class instance. The compiler generates symbolic references to
    the fields of an instance, which are stored in the run-time constant pool. Those run
    time constant pool items are resolved at run-time to determine the location of the
    field within the referenced object.

    与方法调用指令的操作数一样，putfield和getfield指令的操作数（运行时常量池索引＃4）也不是类实例中字段的偏移量.
    编译器生成对实例字段的符号引用，这些符号引用存储在运行时常量池中.
    这些运行时常量池项在运行时被解析，以确定字段在引用对象内的位置.

    */



```