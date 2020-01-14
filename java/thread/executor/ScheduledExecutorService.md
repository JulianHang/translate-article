### 翻译ScheduledExecutorService翻译

```java

   /**
    * An {@link ExecutorService} that can schedule commands to run after a given
    * delay, or to execute periodically.
    
    ExecutorService可以安排命令在给定的延迟后运行或定期执行.

    * <p>The {@code schedule} methods create tasks with various delays
    * and return a task object that can be used to cancel or check
    * execution. The {@code scheduleAtFixedRate} and
    * {@code scheduleWithFixedDelay} methods create and execute tasks
    * that run periodically until cancelled.
    
    schedule方法创建具有各种延迟的任务，并返回可用于取消或检查执行的任务对象. scheduleAtFixedRate和scheduleWithFixedDelay方法创建并执行定期运行的任务，直到被取消.

    * <p>Commands submitted using the {@link Executor#execute(Runnable)}
    * and {@link ExecutorService} {@code submit} methods are scheduled
    * with a requested delay of zero. Zero and negative delays (but not
    * periods) are also allowed in {@code schedule} methods, and are
    * treated as requests for immediate execution.
    
    使用Executor＃execute和ExecutorServiceSubmit方法提交的命令的计划延迟为零（立即执行）. schedule方法中还允许零延迟和负延迟（但不包括周期），它们被视为立即执行的请求.

    * <p>All {@code schedule} methods accept <em>relative</em> delays and
    * periods as arguments, not absolute times or dates. It is a simple
    * matter to transform an absolute time represented as a {@link
    * java.util.Date} to the required form. For example, to schedule at
    * a certain future {@code date}, you can use: {@code schedule(task,
    * date.getTime() - System.currentTimeMillis(),
    * TimeUnit.MILLISECONDS)}. Beware however that expiration of a
    * relative delay need not coincide with the current {@code Date} at
    * which the task is enabled due to network time synchronization
    * protocols, clock drift, or other factors.
    
    所有schedule方法都接受相对延迟和周期作为参数，而不是绝对时间或日期. 将表示为Date的绝对时间转换为所需的形式很简单. 
    例如，安排在某个将来的日期，你可以使用date.getTime - System.currentTimeMillis. 但是请注意，由于网络时间同步协议，时钟漂移或其他因素，相对延迟的到期时间不必与当前启用任务的Date一致.

    * <p>The {@link Executors} class provides convenient factory methods for
    * the ScheduledExecutorService implementations provided in this package.
    
    Executors类为此程序包中提供的ScheduledExecutorService实现提供了方便的工厂方法.（通过Executors创建ScheduledExecutorService实现类）

    * <h3>Usage Example</h3>
    *
    * Here is a class with a method that sets up a ScheduledExecutorService
    * to beep every ten seconds for an hour:
    *
    *  <pre> {@code
    * import static java.util.concurrent.TimeUnit.*;
    * class BeeperControl {
    *   private final ScheduledExecutorService scheduler =
    *     Executors.newScheduledThreadPool(1);
    *
    *   public void beepForAnHour() {
    *     final Runnable beeper = new Runnable() {
    *       public void run() { System.out.println("beep"); }
    *     };
    *     final ScheduledFuture<?> beeperHandle =
    *       scheduler.scheduleAtFixedRate(beeper, 10, 10, SECONDS);
    *     scheduler.schedule(new Runnable() {
    *       public void run() { beeperHandle.cancel(true); }
    *     }, 60 * 60, SECONDS);
    *   }
    * }}</pre>


```