### 翻译ThreadPoolExecutor注释

```java

   /**
    * An {@link ExecutorService} that executes each submitted task using
    * one of possibly several pooled threads, normally configured
    * using {@link Executors} factory methods.
    
    一个ExecutorService，它使用线程池中的其中一个线程来执行每一个已提交的任务，通常使用Executors工厂方法进行配置.

    * <p>Thread pools address two different problems: they usually
    * provide improved performance when executing large numbers of
    * asynchronous tasks, due to reduced per-task invocation overhead,
    * and they provide a means of bounding and managing the resources,
    * including threads, consumed when executing a collection of tasks.
    * Each {@code ThreadPoolExecutor} also maintains some basic
    * statistics, such as the number of completed tasks.
    
    线程池解决了两个不同的问题：由于减少了每个任务的调用开销，它们通常在执行大量异步任务时提供改进的性能，并且它们提供了一种绑定和管理在执行任务集合时消耗的资源（包括线程）的方法.
    每个ThreadPoolExecutor还维护一些基本统计信息，例如已完成任务的数量。

    * <p>To be useful across a wide range of contexts, this class
    * provides many adjustable parameters and extensibility
    * hooks. However, programmers are urged to use the more convenient
    * {@link Executors} factory methods {@link
    * Executors#newCachedThreadPool} (unbounded thread pool, with
    * automatic thread reclamation), {@link Executors#newFixedThreadPool}
    * (fixed size thread pool) and {@link
    * Executors#newSingleThreadExecutor} (single background thread), that
    * preconfigure settings for the most common usage
    * scenarios. Otherwise, use the following guide when manually
    * configuring and tuning this class:
    
    为了在广泛的上下文中有用，该类提供了许多可调整的参数和可扩展性钩子. 但是，敦促程序员使用更方便的Executors工厂方法
    Executor#newCachedThreadPool：无限制线程池，具有自动线程回收
    Executor#newFixedThreadPool：固定线程池大小
    Executor#newSingleThreadExecutor：单个后台线程
    针对最常见的使用场景进行预配置.
    否则，在手动配置和调整此类时，请使用以下指南：

    * <dl>
    *
    * <dt>Core and maximum pool sizes</dt>
    
    核心线程数、最大线程数

    * <dd>A {@code ThreadPoolExecutor} will automatically adjust the
    * pool size (see {@link #getPoolSize})
    * according to the bounds set by
    * corePoolSize (see {@link #getCorePoolSize}) and
    * maximumPoolSize (see {@link #getMaximumPoolSize}).
    
    ThreadPoolExecutor将会按照设置好的核心线程数与最大线程数的边界进行自动调整线程池大小.

    * When a new task is submitted in method {@link #execute(Runnable)},
    * and fewer than corePoolSize threads are running, a new thread is
    * created to handle the request, even if other worker threads are
    * idle.  If there are more than corePoolSize but less than
    * maximumPoolSize threads running, a new thread will be created only
    * if the queue is full.  By setting corePoolSize and maximumPoolSize
    * the same, you create a fixed-size thread pool. By setting
    * maximumPoolSize to an essentially unbounded value such as {@code
    * Integer.MAX_VALUE}, you allow the pool to accommodate an arbitrary
    * number of concurrent tasks. Most typically, core and maximum pool
    * sizes are set only upon construction, but they may also be changed
    * dynamically using {@link #setCorePoolSize} and {@link
    * #setMaximumPoolSize}. </dd>
    
    当一个新任务已经提交，并且少于核心线程数的线程正在运行，即使其他工作线程处于空闲状态，也会创建一个新线程来处理请求.（ size < corePoolSize ）
    如果有大于核心线程数且小于最大线程数的线程正在运行，则仅在队列已满时才会创建新线程.（ corePoolSize < x < maximumPoolSize，队列是用来存储已提交待执行的任务）
    通过将corePoolSize和maximumPoolSize设置为相同，可以创建固定大小的线程池.
    通过将maximumPoolSize设置为一个本质上不受限制的值，例如Integer.MAX_VALUE，可以允许线程池容纳任意数量的并发任务.
    通常，核心线程数和最大线程数仅在构造时设置，但也可以使用setCorePoolSize和setMaximumPoolSize动态更改.

    * <dt>On-demand construction</dt>
    
    按需构造

    * <dd>By default, even core threads are initially created and
    * started only when new tasks arrive, but this can be overridden
    * dynamically using method {@link #prestartCoreThread} or {@link
    * #prestartAllCoreThreads}.  You probably want to prestart threads if
    * you construct the pool with a non-empty queue. </dd>
    
    默认情况下，甚至最初也只有在有新任务到达时才创建和启动核心线程，但是可以使用方法prestartCoreThread或prestartAllCoreThreads动态地覆写它.
    如果使用非空队列构造线程池，则可能要预启动线程.

    * <dt>Creating new threads</dt>
    
    创建新线程

    * <dd>New threads are created using a {@link ThreadFactory}.  If not
    * otherwise specified, a {@link Executors#defaultThreadFactory} is
    * used, that creates threads to all be in the same {@link
    * ThreadGroup} and with the same {@code NORM_PRIORITY} priority and
    * non-daemon status. By supplying a different ThreadFactory, you can
    * alter the thread's name, thread group, priority, daemon status,
    * etc. If a {@code ThreadFactory} fails to create a thread when asked
    * by returning null from {@code newThread}, the executor will
    * continue, but might not be able to execute any tasks. Threads
    * should possess the "modifyThread" {@code RuntimePermission}. If
    * worker threads or other threads using the pool do not possess this
    * permission, service may be degraded: configuration changes may not
    * take effect in a timely manner, and a shutdown pool may remain in a
    * state in which termination is possible but not completed.</dd>
    
    使用ThreadFactory创建新线程. 如果没有另外指定，则使用Executors＃defaultThreadFactory来创建所有线程，它们都位于同一个ThreadGroup中，并且具有相同的NORM_PRIORITY优先级和非守护线程状态.
    通过应用不同的ThreadFactory，你可以改变线程的名字、线程组、优先级、守护线程等等.
    如果ThreadFactory通过从newThread返回null来询问未能创建线程，则执行器将继续执行，但可能无法执行任何任务.
    线程应该具备"modifyThread".
    如果使用该池的工作线程或其他线程不具有此权限，则服务可能会降级：配置更改可能无法及时生效，并且关闭池可能仍处于可能终止但未完成的状态.

    * <dt>Keep-alive times</dt>
    
    保活时间

    * <dd>If the pool currently has more than corePoolSize threads,
    * excess threads will be terminated if they have been idle for more
    * than the keepAliveTime (see {@link #getKeepAliveTime(TimeUnit)}).
    * This provides a means of reducing resource consumption when the
    * pool is not being actively used. If the pool becomes more active
    * later, new threads will be constructed. This parameter can also be
    * changed dynamically using method {@link #setKeepAliveTime(long,
    * TimeUnit)}.  Using a value of {@code Long.MAX_VALUE} {@link
    * TimeUnit#NANOSECONDS} effectively disables idle threads from ever
    * terminating prior to shut down. By default, the keep-alive policy
    * applies only when there are more than corePoolSize threads. But
    * method {@link #allowCoreThreadTimeOut(boolean)} can be used to
    * apply this time-out policy to core threads as well, so long as the
    * keepAliveTime value is non-zero. </dd>
    
    如果当前池中的线程数超过核心线程数，则多余的线程将在空闲时间超过keepAliveTime时终止. 当未积极使用线程池时，这提供了减少资源消耗的方法.
    如果线程池稍后变得更加活动，则将构建新线程. 这个参数也能使用setKeepAliveTime方法来动态改变. 使用Long.MAX_VALUE, TimeUnit＃NANOSECONDS的值可以有效地使空闲线程永远不会在关闭之前终止.
    默认情况下，仅当存在大于核心线程数的线程时，保持活动策略才应用. 但是方法allowCoreThreadTimeOut也可以用于将此超时策略应用于核心线程，只要keepAliveTime值不为零即可.

    * <dt>Queuing</dt>
    
    队列

    * <dd>Any {@link BlockingQueue} may be used to transfer and hold
    * submitted tasks.  The use of this queue interacts with pool sizing:
    
    任何BlockingQueue均可用于传输和保留提交的任务. 此队列的使用与线程池大小交互：

    * <ul>
    *
    * <li> If fewer than corePoolSize threads are running, the Executor
    * always prefers adding a new thread
    * rather than queuing.</li>
    
    如果正在运行的线程少于核心线程数，则执行程序总是喜欢添加新线程，而不是放入队列.

    * <li> If corePoolSize or more threads are running, the Executor
    * always prefers queuing a request rather than adding a new
    * thread.</li>

    如果正在运行的线程等于或大于核心线程数，则执行程序总是喜欢对请求进行排队，而不是添加新线程.

    * <li> If a request cannot be queued, a new thread is created unless
    * this would exceed maximumPoolSize, in which case, the task will be
    * rejected.</li>
    
    如果无法将请求放入队列中，则将创建一个新线程，除非线程数超过了最大线程数，在这种情况下，该任务将被拒绝.

    * </ul>
    *
    * There are three general strategies for queuing:
    * <ol>
    
    有三种一般的排队策略：

    * <li> <em> Direct handoffs.</em> A good default choice for a work
    * queue is a {@link SynchronousQueue} that hands off tasks to threads
    * without otherwise holding them. Here, an attempt to queue a task
    * will fail if no threads are immediately available to run it, so a
    * new thread will be constructed. This policy avoids lockups when
    * handling sets of requests that might have internal dependencies.
    * Direct handoffs generally require unbounded maximumPoolSizes to
    * avoid rejection of new submitted tasks. This in turn admits the
    * possibility of unbounded thread growth when commands continue to
    * arrive on average faster than they can be processed.  </li>
    
    直接交接. SynchronousQueue是工作队列的一个很好的默认选择，它可以将任务移交给线程，而无需另外持有它们. 在这里，如果没有立即可用的线程来运行任务，则将任务排队的尝试将失败，因此将构造一个新线程.
    在处理可能具有内部依赖项的请求集时，此策略避免了锁定. 直接切换通常需要无限制的最大线程数以避免拒绝新提交的任务.
    反过来，当平均而言，命令继续以超出其处理速度的速度到达时，这可能会带来无限线程增长的可能性.（当任务提交比任务的处理速度还快时，线程的数量将会无限增长）

    * <li><em> Unbounded queues.</em> Using an unbounded queue (for
    * example a {@link LinkedBlockingQueue} without a predefined
    * capacity) will cause new tasks to wait in the queue when all
    * corePoolSize threads are busy. Thus, no more than corePoolSize
    * threads will ever be created. (And the value of the maximumPoolSize
    * therefore doesn't have any effect.)  This may be appropriate when
    * each task is completely independent of others, so tasks cannot
    * affect each others execution; for example, in a web page server.
    * While this style of queuing can be useful in smoothing out
    * transient bursts of requests, it admits the possibility of
    * unbounded work queue growth when commands continue to arrive on
    * average faster than they can be processed.  </li>
    
    无界队列. 当所有核心线程都忙时，使用无界队列（例如，没有预定义容量的LinkedBlockingQueue）将导致新任务在队列中等待. 因此，将仅创建核心线程数的线程个数.（因此，最大线程数的值没有任何作用）
    当每个任务完全独立于其他任务时，这可能是适当的，因此任务不会影响彼此的执行. 例如，在网页服务器中. 
    尽管这种排队方式对于消除短暂的请求突发很有用，但它承认就平均而言当命令继续以比处理命令更快的速度到达时，工作队列可能会无限增长.（当任务提交比任务的处理速度还快时，队列的长度将会无限增长）

    * <li><em>Bounded queues.</em> A bounded queue (for example, an
    * {@link ArrayBlockingQueue}) helps prevent resource exhaustion when
    * used with finite maximumPoolSizes, but can be more difficult to
    * tune and control.  Queue sizes and maximum pool sizes may be traded
    * off for each other: Using large queues and small pools minimizes
    * CPU usage, OS resources, and context-switching overhead, but can
    * lead to artificially low throughput.  If tasks frequently block (for
    * example if they are I/O bound), a system may be able to schedule
    * time for more threads than you otherwise allow. Use of small queues
    * generally requires larger pool sizes, which keeps CPUs busier but
    * may encounter unacceptable scheduling overhead, which also
    * decreases throughput.  </li>
    
    有界队列. 与有限的最大线程数一起使用时，有界队列（例如，ArrayBlockingQueue）有助于防止资源耗尽，但调优和控制起来会更加困难. 
    队列大小和最大线程池大小可能会相互折衷：使用大队列和小x线程池可最大程度地减少CPU使用率，操作系统资源和上下文切换开销，但可能导致不自然的低吞吐量.
    如果任务频繁阻塞，则系统可能安排比原先允许的线程更多的时间（加大CPU时间片？？？）.
    使用小队列通常需要更大的池大小，这会使CPU繁忙，但可能会遇到不可接受的调度开销，这也会降低吞吐量.

    * </ol>
    *
    * </dd>
    *
    * <dt>Rejected tasks</dt>
    
    拒绝任务

    * <dd>New tasks submitted in method {@link #execute(Runnable)} will be
    * <em>rejected</em> when the Executor has been shut down, and also when
    * the Executor uses finite bounds for both maximum threads and work queue
    * capacity, and is saturated.  In either case, the {@code execute} method
    * invokes the {@link
    * RejectedExecutionHandler#rejectedExecution(Runnable, ThreadPoolExecutor)}
    * method of its {@link RejectedExecutionHandler}.  Four predefined handler
    * policies are provided:
    
    在执行器关闭后，并且在执行器对最大线程数和工作队列容量使用有限范围且已饱和时，将拒绝在方法execute（Runnable）中提交的新任务.
    无论哪种情况，execute方法都会调用其RejectedExecutionHandler＃rejectedExecution（Runnable，ThreadPoolExecutor）方法.
    提供了四个预定义的处理程序策略：

    * <ol>
    *
    * <li> In the default {@link ThreadPoolExecutor.AbortPolicy}, the
    * handler throws a runtime {@link RejectedExecutionException} upon
    * rejection. </li>
    
    在默认的ThreadPoolExecutor.AbortPolicy中，处理程序在拒绝时抛出运行时RejectedExecutionException异常.

    * <li> In {@link ThreadPoolExecutor.CallerRunsPolicy}, the thread
    * that invokes {@code execute} itself runs the task. This provides a
    * simple feedback control mechanism that will slow down the rate that
    * new tasks are submitted. </li>
    
    在ThreadPoolExecutor.CallerRunsPolicy中，调用execute的线程本身运行任务. 这提供了一种简单的反馈控制机制，将降低新任务的提交速度.

    * <li> In {@link ThreadPoolExecutor.DiscardPolicy}, a task that
    * cannot be executed is simply dropped.  </li>
    
    在ThreadPoolExecutor.DiscardPolicy中，简单地删除了被拒绝的任务.

    * <li>In {@link ThreadPoolExecutor.DiscardOldestPolicy}, if the
    * executor is not shut down, the task at the head of the work queue
    * is dropped, and then execution is retried (which can fail again,
    * causing this to be repeated.) </li>
    
    在ThreadPoolExecutor.DiscardOldestPolicy中，如果未关闭执行器，则将丢弃工作队列开头的任务（最久的任务），然后重试执行（当前任务）（该操作可能再次失败，导致重复执行此操作）

    * </ol>
    *
    * It is possible to define and use other kinds of {@link
    * RejectedExecutionHandler} classes. Doing so requires some care
    * especially when policies are designed to work only under particular
    * capacity or queuing policies. </dd>
    
    可以定义和使用其他类型的RejectedExecutionHandler类. 这样做需要格外小心，尤其是在设计策略仅在特定容量或排队策略下工作时.

    * <dt>Hook methods</dt>
    
    钩子方法

    * <dd>This class provides {@code protected} overridable
    * {@link #beforeExecute(Thread, Runnable)} and
    * {@link #afterExecute(Runnable, Throwable)} methods that are called
    * before and after execution of each task.  These can be used to
    * manipulate the execution environment; for example, reinitializing
    * ThreadLocals, gathering statistics, or adding log entries.
    * Additionally, method {@link #terminated} can be overridden to perform
    * any special processing that needs to be done once the Executor has
    * fully terminated.
    
    该类提供了可覆写的方法，方法在每个任务的执行前/后被调用. 这些可以用来操纵执行环境. 例如，重新初始化ThreadLocals，收集统计信息或添加日志.
    此外，一旦执行程序完全终止，方法terminated可以被覆盖以执行需要执行的任何特殊处理.

    * <p>If hook or callback methods throw exceptions, internal worker
    * threads may in turn fail and abruptly terminate.</dd>
    
    如果钩子或回调方法引发异常，内部工作线程可能反过来失败并突然终止.

    * <dt>Queue maintenance</dt>
    
    队列保持

    * <dd>Method {@link #getQueue()} allows access to the work queue
    * for purposes of monitoring and debugging.  Use of this method for
    * any other purpose is strongly discouraged.  Two supplied methods,
    * {@link #remove(Runnable)} and {@link #purge} are available to
    * assist in storage reclamation when large numbers of queued tasks
    * become cancelled.</dd>
    
    方法getQueue允许访问工作队列，以进行监视和调试. 强烈建议不要将此方法用于其他目的. 提供的两种方法remove和purge可用于在取消大量排队任务时协助回收存储.

    * <dt>Finalization</dt>
    
    定案？结局？

    * <dd>A pool that is no longer referenced in a program <em>AND</em>
    * has no remaining threads will be {@code shutdown} automatically. If
    * you would like to ensure that unreferenced pools are reclaimed even
    * if users forget to call {@link #shutdown}, then you must arrange
    * that unused threads eventually die, by setting appropriate
    * keep-alive times, using a lower bound of zero core threads and/or
    * setting {@link #allowCoreThreadTimeOut(boolean)}.  </dd>
    
    程序中不再引用且没有剩余线程的线程池将自动关闭. 
    如果即使在用户忘记调用shutdown的情况下也要确保收回未引用的线程池，则必须通过使用零核心线程数的下限来设置适当的保活时间，以安排未使用的线程最终死掉或设置allowCoreThreadTimeOut

    * </dl>
    *
    * <p><b>Extension example</b>. Most extensions of this class
    * override one or more of the protected hook methods. For example,
    * here is a subclass that adds a simple pause/resume feature:
    
    扩展示例. 此类的大多数扩展都覆写一个或多个受保护的钩子方法. 例如，以下是一个子类，它添加了一个简单的暂停/继续功能：

    *  <pre> {@code
    * class PausableThreadPoolExecutor extends ThreadPoolExecutor {
    *   private boolean isPaused;
    *   private ReentrantLock pauseLock = new ReentrantLock();
    *   private Condition unpaused = pauseLock.newCondition();
    *
    *   public PausableThreadPoolExecutor(...) { super(...); }
    *
    *   protected void beforeExecute(Thread t, Runnable r) {
    *     super.beforeExecute(t, r);
    *     pauseLock.lock();
    *     try {
    *       while (isPaused) unpaused.await();
    *     } catch (InterruptedException ie) {
    *       t.interrupt();
    *     } finally {
    *       pauseLock.unlock();
    *     }
    *   }
    *
    *   public void pause() {
    *     pauseLock.lock();
    *     try {
    *       isPaused = true;
    *     } finally {
    *       pauseLock.unlock();
    *     }
    *   }
    *
    *   public void resume() {
    *     pauseLock.lock();
    *     try {
    *       isPaused = false;
    *       unpaused.signalAll();
    *     } finally {
    *       pauseLock.unlock();
    *     }
    *   }
    * }}</pre>
    *

```

### 线程池的状态

```java

   /**
    * The main pool control state, ctl, is an atomic integer packing
    * two conceptual fields
    *   workerCount, indicating the effective number of threads
    *   runState,    indicating whether running, shutting down etc

    线程池控制状态ctl是一个原子整数，它包装了两个概念字段：workerCount，指示有效线程数的数量，runState，指示是否运行，关闭等.

    * In order to pack them into one int, we limit workerCount to
    * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2
    * billion) otherwise representable. If this is ever an issue in
    * the future, the variable can be changed to be an AtomicLong,
    * and the shift/mask constants below adjusted. But until the need
    * arises, this code is a bit faster and simpler using an int.
    
    为了将它们打包为一个int，我们将workerCount限制为（2^29）-1个线程，而不是（2^31）-1个可以表示的线程. 如果将来有问题，可以将该变量更改为AtomicLong，并在以下调整shift / mask常数.
    但是在需要之前，使用int可以使此代码更快，更简单.

    * The workerCount is the number of workers that have been
    * permitted to start and not permitted to stop.  The value may be
    * transiently different from the actual number of live threads,
    * for example when a ThreadFactory fails to create a thread when
    * asked, and when exiting threads are still performing
    * bookkeeping before terminating. The user-visible pool size is
    * reported as the current size of the workers set.
    
    workerCount是已被允许启动但不允许停止的工人数. 该值可能与活动线程的实际数量暂时不同，例如，当ThreadFactory在被询问时未能创建线程，并且退出线程仍在终止之前执行簿记操作时.
    用户可见的线程池大小报告为工作集的当前大小.

    * The runState provides the main lifecycle control, taking on values:
    
    runState提供主要的生命周期控制，并具有以下值：

    *   RUNNING:  Accept new tasks and process queued tasks
                
                接受新任务并处理排队的任务

    *   SHUTDOWN: Don't accept new tasks, but process queued tasks

                不接受新任务，而是处理排队的任务

    *   STOP:     Don't accept new tasks, don't process queued tasks,
    *             and interrupt in-progress tasks

                不接受新任务，不处理排队任务以及中断进行中的任务

    *   TIDYING:  All tasks have terminated, workerCount is zero,
    *             the thread transitioning to state TIDYING
    *             will run the terminated() hook method

                所有任务都已终止，workerCount为零，转换为状态TIDYING的线程将运行terminated钩子方法.

    *   TERMINATED: terminated() has completed

                terminated方法执行完成
    *
    * The numerical order among these values matters, to allow
    * ordered comparisons. The runState monotonically increases over
    * time, but need not hit each state. The transitions are:
    
    这些值之间的数字顺序很重要，可以进行有序的比较. runState随时间单调增加，但不必达到每个状态. 过渡是：

    * RUNNING -> SHUTDOWN
    *    On invocation of shutdown(), perhaps implicitly in finalize()

        shutdown：RUNNING -> SHUTDOWN

    * (RUNNING or SHUTDOWN) -> STOP
    *    On invocation of shutdownNow()

        shutdownNow：RUNNING -> STOP 或 SHUTDOWN -> STOP

    * SHUTDOWN -> TIDYING
    *    When both queue and pool are empty

        当队列和线程池是空的

    * STOP -> TIDYING
    *    When pool is empty

        当线程池是空的

    * TIDYING -> TERMINATED
    *    When the terminated() hook method has completed
    
        当terminated钩子方法调用完成

    * Threads waiting in awaitTermination() will return when the
    * state reaches TERMINATED.

    状态达到TERMINATED时，在awaitTermination中等待的线程将返回

    * Detecting the transition from SHUTDOWN to TIDYING is less
    * straightforward than you'd like because the queue may become
    * empty after non-empty and vice versa during SHUTDOWN state, but
    * we can only terminate if, after seeing that it is empty, we see
    * that workerCount is 0 (which sometimes entails a recheck -- see
    * below).

    检测从SHUTDOWN到TIDYING的转换并不像您想要的那样简单，因为在SHUTDOWN状态期间，队列在非空之后可能会变空，反之亦然，但是只有在看到队列为空后看到workerCount为0时，我们才能终止（有时需要重新检查-参见下文）

    */   

```