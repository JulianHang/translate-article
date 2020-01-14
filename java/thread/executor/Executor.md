### 翻译Executor注释

```java


   /**
    * An object that executes submitted {@link Runnable} tasks. This
    * interface provides a way of decoupling task submission from the
    * mechanics of how each task will be run, including details of thread
    * use, scheduling, etc.  An {@code Executor} is normally used
    * instead of explicitly creating threads. For example, rather than
    * invoking {@code new Thread(new(RunnableTask())).start()} for each
    * of a set of tasks, you might use:
    
    执行已提交的Runnable任务的对象. 该接口提供了一种将任务提交与每个任务如何运行的机制分离的方法，包括线程使用、调度等详细信息.
    通常使用Executor代替显示创建线程. 例如，而不是为每一个任务都调用new Thread，你可以使用：

    * <pre>
    * Executor executor = <em>anExecutor</em>;
    * executor.execute(new RunnableTask1());
    * executor.execute(new RunnableTask2());
    * ...
    * </pre>
    *
    * However, the {@code Executor} interface does not strictly
    * require that execution be asynchronous. In the simplest case, an
    * executor can run the submitted task immediately in the caller's
    * thread:
    
    但是，Executor接口并不严格要求执行是异步的. 在最简单的情况下，执行器可以在调用者的线程中立即运行提交的任务.

    *  <pre> {@code
    * class DirectExecutor implements Executor {
    *   public void execute(Runnable r) {
    *     r.run();
    *   }
    * }}</pre>
    *
    * More typically, tasks are executed in some thread other
    * than the caller's thread.  The executor below spawns a new thread
    * for each task.
    
    更典型地，任务在调用者线程之外的其他线程中执行. 下面的执行器为每个任务生成一个新线程.

    *  <pre> {@code
    * class ThreadPerTaskExecutor implements Executor {
    *   public void execute(Runnable r) {
    *     new Thread(r).start();
    *   }
    * }}</pre>
    *
    * Many {@code Executor} implementations impose some sort of
    * limitation on how and when tasks are scheduled.  The executor below
    * serializes the submission of tasks to a second executor,
    * illustrating a composite executor.
    
    许多Executor实现对安排任务的方式和时间施加了某种限制. 下面的执行程序将任务提交序列化到第二个执行程序，说明了一个复合执行程序.

    *  <pre> {@code
    * class SerialExecutor implements Executor {
    *   final Queue<Runnable> tasks = new ArrayDeque<Runnable>();
    *   final Executor executor;
    *   Runnable active;
    *
    *   SerialExecutor(Executor executor) {
    *     this.executor = executor;
    *   }
    *
    *   public synchronized void execute(final Runnable r) {
    *     tasks.offer(new Runnable() {
    *       public void run() {
    *         try {
    *           r.run();
    *         } finally {
    *           scheduleNext();
    *         }
    *       }
    *     });
    *     if (active == null) {
    *       scheduleNext();
    *     }
    *   }
    *
    *   protected synchronized void scheduleNext() {
    *     if ((active = tasks.poll()) != null) {
    *       executor.execute(active);
    *     }
    *   }
    * }}</pre>
    *
    * The {@code Executor} implementations provided in this package
    * implement {@link ExecutorService}, which is a more extensive
    * interface.  The {@link ThreadPoolExecutor} class provides an
    * extensible thread pool implementation. The {@link Executors} class
    * provides convenient factory methods for these Executors.
    
    此软件包中提供了Executor实现ExecutorService，这是一个更广泛的接口. ThreadPoolExecutor类提供了可扩展的线程池实现. Executors类为这些Executor提供了方便的工厂方法.

    * <p>Memory consistency effects: Actions in a thread prior to
    * submitting a {@code Runnable} object to an {@code Executor}
    * <a href="package-summary.html#MemoryVisibility"><i>happen-before</i></a>
    * its execution begins, perhaps in another thread.

    内存一致性影响：在一个线程中提交一个Runnable对象给Executor先于开始执行发生（开始执行之前发生），可能在另外一个线程.

```