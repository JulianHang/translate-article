### 翻译Java虚拟机规范第二章第五节-第一节(JSR-337/Java8)

主要内容：`简单介绍程序计数器`

```java

    /**

    2.5.1 The pc Register

    程序计数器（pc寄存器）

    The Java Virtual Machine can support many threads of execution at once (JLS
    §17). Each Java Virtual Machine thread has its own pc (program counter) register.
    At any point, each Java Virtual Machine thread is executing the code of a single
    method, namely the current method (§2.6) for that thread. If that method is not
    native, the pc register contains the address of the Java Virtual Machine instruction
    currently being executed. If the method currently being executed by the thread is
    native, the value of the Java Virtual Machine's pc register is undefined. The Java
    Virtual Machine's pc register is wide enough to hold a returnAddress or a native
    pointer on the specific platform.

    Java虚拟机可以支持多个执行线程. 每个Java虚拟机线程都有自己的pc寄存器（程序计数器）. 在任何时候，每个Java虚拟机线程都在执行单个方法的代码，即该线程的当前方法. 
    如果该方法不是本机方法，则pc寄存器包含当前正在执行的Java虚拟机指令的地址. 
    如果线程当前正在执行的方法是本地方法，则Java虚拟机的pc寄存器的值是未定义的.
    Java虚拟机的pc寄存器是足够宽的，可以在特定平台上保存returnAddress或本机指针.

    */

```