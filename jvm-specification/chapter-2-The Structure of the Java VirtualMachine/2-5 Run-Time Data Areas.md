### 翻译Java虚拟机规范第二章第五节-(JSR-337/Java8)

主要内容：`Java虚拟机定义了运行时数据区域`

```java

    /**

    2.5 Run-Time Data Areas

    运行时数据区

    The Java Virtual Machine defines various run-time data areas that are used during
    execution of a program. Some of these data areas are created on Java Virtual
    Machine start-up and are destroyed only when the Java Virtual Machine exits.
    Other data areas are per thread. Per-thread data areas are created when a thread is
    created and destroyed when the thread exits.

    Java虚拟机定义了在程序执行期间使用的各种运行时数据区域.
    其中一些数据区域是在Java虚拟机启动时创建的，仅在Java虚拟机退出时才被销毁.
    其他数据区域是每个线程的. 在创建线程时创建每个线程的数据区域，并在线程退出时销毁每个数据区域.

    */

```