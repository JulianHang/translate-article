### 翻译ExecutorSerivce注释

```java

   /**
    * An {@link Executor} that provides methods to manage termination and
    * methods that can produce a {@link Future} for tracking progress of
    * one or more asynchronous tasks.
    
    Executor提供管理终止的方法和可以产生用于跟踪一个或多个异步任务进度的Future方法.

    * <p>An {@code ExecutorService} can be shut down, which will cause
    * it to reject new tasks.  Two different methods are provided for
    * shutting down an {@code ExecutorService}. The {@link #shutdown}
    * method will allow previously submitted tasks to execute before
    * terminating, while the {@link #shutdownNow} method prevents waiting
    * tasks from starting and attempts to stop currently executing tasks.
    * Upon termination, an executor has no tasks actively executing, no
    * tasks awaiting execution, and no new tasks can be submitted.  An
    * unused {@code ExecutorService} should be shut down to allow
    * reclamation of its resources.
    
    ExecutorService可以关闭，将导致它拒绝新任务. 提供了两种不同的方法来关闭ExecutorService.
    shutdown方法允许先前提交的任务在终止之前执行（将提交的所有任务执行完毕后在终止），shutdownNow方法阻止等待的任务启动并尝试停止当前执行中的任务.
    终止后，执行器将没有正在执行的任务，没有正在等待执行的任务，并且无法提交新任务.
    应该关闭未使用的ExecutorService以便回收其资源.

    * <p>Method {@code submit} extends base method {@link
    * Executor#execute(Runnable)} by creating and returning a {@link Future}
    * that can be used to cancel execution and/or wait for completion.
    * Methods {@code invokeAny} and {@code invokeAll} perform the most
    * commonly useful forms of bulk execution, executing a collection of
    * tasks and then waiting for at least one, or all, to
    * complete. (Class {@link ExecutorCompletionService} can be used to
    * write customized variants of these methods.)
    
    submit方法通过创建并返回可以用来取消执行和/或等待完成的Future来扩展基本方法Executor＃execute（Runnable）.
    invokeAny和invokeAll方法执行最常用的批量执行形式，执行一组任务，然后等待至少一个或全部完成.
    ExecutorCompletionService可用于编写这些方法的自定义变体.

    * <p>The {@link Executors} class provides factory methods for the
    * executor services provided in this package.
    
    Executors类为此包中提供的执行程序服务提供了工厂方法.

    * <h3>Usage Examples</h3>
    
    使用示例

    * Here is a sketch of a network service in which threads in a thread
    * pool service incoming requests. It uses the preconfigured {@link
    * Executors#newFixedThreadPool} factory method:
    
    这是网络服务的示意图，其中线程池中的线程服务传入的请求. 它使用预配置的Executors＃newFixedThreadPool工厂方法.

    *  <pre> {@code
    * class NetworkService implements Runnable {
    *   private final ServerSocket serverSocket;
    *   private final ExecutorService pool;
    *
    *   public NetworkService(int port, int poolSize)
    *       throws IOException {
    *     serverSocket = new ServerSocket(port);
    *     pool = Executors.newFixedThreadPool(poolSize);
    *   }
    *
    *   public void run() { // run the service
    *     try {
    *       for (;;) {
    *         pool.execute(new Handler(serverSocket.accept()));
    *       }
    *     } catch (IOException ex) {
    *       pool.shutdown();
    *     }
    *   }
    * }
    *
    * class Handler implements Runnable {
    *   private final Socket socket;
    *   Handler(Socket socket) { this.socket = socket; }
    *   public void run() {
    *     // read and service request on socket
    *   }
    * }}</pre>
    *
    * The following method shuts down an {@code ExecutorService} in two phases,
    * first by calling {@code shutdown} to reject incoming tasks, and then
    * calling {@code shutdownNow}, if necessary, to cancel any lingering tasks:
    
    以下方法分两个阶段关闭ExecutorService，首先通过调用shutdown拒绝传入的任务，然后在必要时调用shutdownNow来取消所有未完成的任务.

    *  <pre> {@code
    * void shutdownAndAwaitTermination(ExecutorService pool) {
    *   pool.shutdown(); // Disable new tasks from being submitted
    *   try {
    *     // Wait a while for existing tasks to terminate
    *     if (!pool.awaitTermination(60, TimeUnit.SECONDS)) {
    *       pool.shutdownNow(); // Cancel currently executing tasks
    *       // Wait a while for tasks to respond to being cancelled
    *       if (!pool.awaitTermination(60, TimeUnit.SECONDS))
    *           System.err.println("Pool did not terminate");
    *     }
    *   } catch (InterruptedException ie) {
    *     // (Re-)Cancel if current thread also interrupted
    *     pool.shutdownNow();
    *     // Preserve interrupt status
    *     Thread.currentThread().interrupt();
    *   }
    * }}</pre>
    *
    * <p>Memory consistency effects: Actions in a thread prior to the
    * submission of a {@code Runnable} or {@code Callable} task to an
    * {@code ExecutorService}
    * <a href="package-summary.html#MemoryVisibility"><i>happen-before</i></a>
    * any actions taken by that task, which in turn <i>happen-before</i> the
    * result is retrieved via {@code Future.get()}.

    内存一致性影响：在将Runnable或Callable任务提交给ExecutorService之前，线程中的操作发生在该任务执行的任何操作之前，而该操作又发生在通过Future.get检索结果之前.

```