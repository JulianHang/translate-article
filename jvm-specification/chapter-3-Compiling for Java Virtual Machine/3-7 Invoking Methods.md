### 翻译Java虚拟机规范第三章第七节(JSR-337/Java8)

主要内容：`通过示例说明在调用不同方法时Java虚拟机采用指令的区别`

```java

    /**

    3.7 Invoking Methods

    调用方法

    The normal method invocation for a instance method dispatches on the runtime type of the object. 
    (They are virtual, in C++ terms.) Such an invocation is
    implemented using the invokevirtual instruction, which takes as its argument an
    index to a run-time constant pool entry giving the internal form of the binary name
    of the class type of the object, the name of the method to invoke, and that method's
    descriptor (§4.3.3). To invoke the addTwo method, defined earlier as an instance
    method, we might write:

    实例方法的常规方法调用根据对象的运行时类型调度. （使用C ++术语，它们是虚拟的.）
    这种调用是使用invokevirtual指令实现的，该指令以运行时常量池条目的索引作为参数，给出了对象类类型的二进制名称的内部形式，要调用方法的名称以及该方法的描述符（第4.3.3节）.
    要调用先前定义为实例方法的addTwo方法，我们可以编写：

        int add12and13() {
            return addTwo(12, 13);
        }

    This compiles to:

        Method int add12and13()
        0 aload_0 // Push local variable 0 (this)
        1 bipush 12 // Push int constant 12
        3 bipush 13 // Push int constant 13
        5 invokevirtual #4 // Method Example.addtwo(II)I
        8 ireturn // Return int on top of operand stack;
        // it is the int result of addTwo()

    The invocation is set up by first pushing a reference to the current instance, this,
    on to the operand stack. The method invocation's arguments, int values 12 and 13,
    are then pushed. When the frame for the addTwo method is created, the arguments
    passed to the method become the initial values of the new frame's local variables.
    That is, the reference for this and the two arguments, pushed onto the operand
    stack by the invoker, will become the initial values of local variables 0, 1, and 2
    of the invoked method.

    首先将对当前实例的引用推入操作数栈来建立调用. 然后将方法调用的参数int值12和13推入. 创建addTwo方法的栈帧时，传递给该方法的参数将成为新栈帧的局部变量的初始值.
    也就是说，this参数和两个参数的引用，由调用程序推入操作数栈将成为被调用方法的局部变量0、1和2的初始值.

    Finally, addTwo is invoked. When it returns, its int return value is pushed onto
    the operand stack of the frame of the invoker, the add12and13 method. The return
    value is thus put in place to be immediately returned to the invoker of add12and13.
    The return from add12and13 is handled by the ireturn instruction of add12and13.

    最后，调用addTwo. 当它返回时，其int返回值被压入调用程序add12and13方法的栈帧的操作数栈中.
    因此，将返回值放在适当的位置以立即返回给add12and13的调用者.
    add12and13的返回由add12and13的ireturn指令处理.

    The ireturn instruction takes the int value returned by addTwo, on the operand
    stack of the current frame, and pushes it onto the operand stack of the frame of
    the invoker. It then returns control to the invoker, making the invoker's frame
    current. The Java Virtual Machine provides distinct return instructions for many of
    its numeric and reference data types, as well as a return instruction for methods
    with no return value. The same set of return instructions is used for all varieties
    of method invocations.

    ireturn指令在当前栈帧的操作数栈上采用addTwo返回的int值，并将其压入调用程序的栈帧的操作数栈.
    然后，它将控制权返回给调用者，使调用者的栈帧成为当前栈帧.
    Java虚拟机为许多数字和引用数据类型提供了不同的返回指令，并为没有返回值的方法提供了返回指令.
    所有方法调用都使用相同的返回指令集.

    The operand of the invokevirtual instruction (in the example, the run-time constant
    pool index #4) is not the offset of the method in the class instance. The compiler
    does not know the internal layout of a class instance. Instead, it generates symbolic
    references to the methods of an instance, which are stored in the run-time constant
    pool. Those run-time constant pool items are resolved at run-time to determine
    the actual method location. The same is true for all other Java Virtual Machine
    instructions that access class instances.

    invokevirtual指令的操作数（在示例中为运行时常量池索引＃4）不是该方法在类实例中的偏移量. 编译器不知道类实例的内部布局.
    相反，它生成对实例方法的符号引用，这些符号引用存储在运行时常量池中.
    这些运行时常量池项在运行时被解析，以确定实际的方法位置. 
    对于访问类实例的所有其他Java虚拟机指令，也是如此.

    Invoking addTwoStatic, a class (static) variant of addTwo, is similar, as shown:

        int add12and13() {
            return addTwoStatic(12, 13);
        }

    although a different Java Virtual Machine method invocation instruction is used:

        Method int add12and13()
        0 bipush 12
        2 bipush 13
        4 invokestatic #3 // Method Example.addTwoStatic(II)I
        7 ireturn

    Compiling an invocation of a class (static) method is very much like compiling
    an invocation of an instance method, except this is not passed by the invoker. The
    method arguments will thus be received beginning with local variable 0 (§3.6). The
    invokestatic instruction is always used to invoke class methods.


    静态方法的调用与实例方法的调用非常相似，不同之处在于调用者未传递this. 
    因此，将从局部变量0（第3.6节）开始接收方法参数.
    invokestatic指令始终用于调用类方法. 

    The invokespecial instruction must be used to invoke instance initialization
    methods (§3.8). It is also used when invoking methods in the superclass (super)
    and when invoking private methods. For instance, given classes Near and Far
    declared as:

    必须使用invokespecial指令来调用实例初始化方法（第3.8节）. 当调用超类（super）中的方法以及调用私有方法时，也使用它.
    例如，给定的类Near和Far声明为：

        class Near {
            int it;
            public int getItNear() {
                return getIt();
            }
            private int getIt() {
                return it;
            }
        }
        class Far extends Near {
            int getItFar() {
                return super.getItNear();
            }
        }
    
    the method Near.getItNear (which invokes a private method) becomes:

        Method int getItNear()
        0 aload_0
        1 invokespecial #5 // Method Near.getIt()I
        4 ireturn

    The method Far.getItFar (which invokes a superclass method) becomes:

        Method int getItFar()
        0 aload_0
        1 invokespecial #4 // Method Near.getItNear()I
        4 ireturn

    Note that methods called using the invokespecial instruction always pass this to
    the invoked method as its first argument. As usual, it is received in local variable 0.

    请注意，使用invokespecial指令调用的方法始终将this作为第一个参数传递给被调用的方法. 与往常一样，它是在局部变量0中接收的.

    To invoke the target of a method handle, a compiler must form a method descriptor
    that records the actual argument and return types. A compiler may not perform
    method invocation conversions on the arguments; instead, it must push them on
    the stack according to their own unconverted types. The compiler arranges for
    a reference to the method handle object to be pushed on the stack before the
    arguments, as usual. The compiler emits an invokevirtual instruction that references
    a descriptor which describes the argument and return types. By special arrangement
    with method resolution (§5.4.3.3), an invokevirtual instruction which invokes
    the invokeExact or invoke methods of java.lang.invoke.MethodHandle will
    always link, provided the method descriptor is syntactically well-formed and the
    types named in the descriptor can be resolved.

    要调用方法句柄的目标，编译器必须形成一个记录实际参数和返回类型的方法描述符. 编译器可能不会对参数执行方法调用转换.
    相反，它必须根据它们自己未转换的类型将它们压入堆栈. 像往常一样，编译器安排对方法句柄对象的引用，以在参数之前将其压入堆栈.
    编译器会发出一个invokevirtual指令，该指令引用一个描述参数和返回类型的描述符. 
    通过特殊的安排和方法解析（第5.4.3.3节），只要方法描述符在语法上格式正确并且在方法描述符中的类型名称能被解析
    将始终链接invokevirtual指令对invokeExact或java.lang.invoke.MethodHandle的invoke方法的调用.

    */



```