### 翻译Thread注释

```java

   /**
    * A <i>thread</i> is a thread of execution in a program. The Java
    * Virtual Machine allows an application to have multiple threads of
    * execution running concurrently.
    * <p>

    线程是程序中的执行线程. JVM允许应用具有多个并发运行的执行线程.

    * Every thread has a priority. Threads with higher priority are
    * executed in preference to threads with lower priority. Each thread
    * may or may not also be marked as a daemon. When code running in
    * some thread creates a new <code>Thread</code> object, the new
    * thread has its priority initially set equal to the priority of the
    * creating thread, and is a daemon thread if and only if the
    * creating thread is a daemon.
    
    每一个线程都有一个优先级. 高优先级的线程优先于低优先级的线程执行. 每个线程可能也可能不被标记为守护线程.
    当在某个线程中运行的代码创建新的Thread对象时，新线程的优先级最初设置为等于创建线程的优先级（当前线程）, 并且当且仅当创建线程是守护线程时，该线程才是守护线程

    * When a Java Virtual Machine starts up, there is usually a single
    * non-daemon thread (which typically calls the method named
    * <code>main</code> of some designated class). The Java Virtual
    * Machine continues to execute threads until either of the following
    * occurs:

    Jvm启动时，通常只有一个非守护线程（通常会调用某些指定类的名为main的方法）.
    JVM将继续执行线程直到发生以下任何一种情况：

    * <ul>
    * <li>The <code>exit</code> method of class <code>Runtime</code> has been
    *     called and the security manager has permitted the exit operation
    *     to take place.

          调用了Runtime类的exit方法，并且安全管理器允许进行exit操作

    * <li>All threads that are not daemon threads have died, either by
    *     returning from the call to the <code>run</code> method or by
    *     throwing an exception that propagates beyond the <code>run</code>
    *     method.

          所有非守护线程的线程都已死亡，要么通过从调用run方法处返回，要么通过抛出传播到run方法之外的异常  

    * </ul>
    * <p>
    * There are two ways to create a new thread of execution. One is to
    * declare a class to be a subclass of <code>Thread</code>. This
    * subclass should override the <code>run</code> method of class
    * <code>Thread</code>. An instance of the subclass can then be
    * allocated and started. For example, a thread that computes primes
    * larger than a stated value could be written as follows:
    * <hr><blockquote><pre>
    *     class PrimeThread extends Thread {
    *         long minPrime;
    *         PrimeThread(long minPrime) {
    *             this.minPrime = minPrime;
    *         }
    *
    *         public void run() {
    *             // compute primes larger than minPrime
    *             &nbsp;.&nbsp;.&nbsp;.
    *         }
    *     }
    * </pre></blockquote><hr>
    * <p>

    有两个方式创建一个新的执行线程. 一种是将一个类声明为Thread的子类. 该子类应该覆写Thread类的run方法.
    然后可以分配并启动子类的实例. 计算大于指定值的质数的线程可以编写如下: class PrimeThread extends Thread

    * The following code would then create a thread and start it running:
    * <blockquote><pre>
    *     PrimeThread p = new PrimeThread(143);
    *     p.start();
    * </pre></blockquote>
    * <p>

    下面的代码将创建一个线程并开始运行：

    * The other way to create a thread is to declare a class that
    * implements the <code>Runnable</code> interface. That class then
    * implements the <code>run</code> method. An instance of the class can
    * then be allocated, passed as an argument when creating
    * <code>Thread</code>, and started. The same example in this other
    * style looks like the following:
    * <hr><blockquote><pre>
    *     class PrimeRun implements Runnable {
    *         long minPrime;
    *         PrimeRun(long minPrime) {
    *             this.minPrime = minPrime;
    *         }
    *
    *         public void run() {
    *             // compute primes larger than minPrime
    *             &nbsp;.&nbsp;.&nbsp;.
    *         }
    *     }
    * </pre></blockquote><hr>
    * <p>

    创建线程的另一种方法是声明一个实现Runnable接口的类. 该类实现了run方法.
    然后可以分配该类的实例，在创建Thread时将其作为参数传递，然后启动. 其他样式的相同示例如下所示：class PrimeRun implements Runnable

    * The following code would then create a thread and start it running:
    * <blockquote><pre>
    *     PrimeRun p = new PrimeRun(143);
    *     new Thread(p).start();
    * </pre></blockquote>
    * <p>

    下面的代码将创建一个线程并开始运行：

    * Every thread has a name for identification purposes. More than
    * one thread may have the same name. If a name is not specified when
    * a thread is created, a new name is generated for it.
    * <p>

    每个线程都有一个名称供识别. 一个以上的线程可能具有相同的名称. 如果在创建线程时未指定名称，则会为其生成一个新名称.

    * Unless otherwise noted, passing a {@code null} argument to a constructor
    * or method in this class will cause a {@link NullPointerException} to be
    * thrown.

    除非另有说明，否则将null参数传递给Thread的构造函数或方法将导致抛出空指针异常

```