### 翻译ScheduledThreadPoolExecutor注释

```java

   /**
    * A {@link ThreadPoolExecutor} that can additionally schedule
    * commands to run after a given delay, or to execute
    * periodically. This class is preferable to {@link java.util.Timer}
    * when multiple worker threads are needed, or when the additional
    * flexibility or capabilities of {@link ThreadPoolExecutor} (which
    * this class extends) are required.
    
    ThreadPoolExecutor可以另外安排命令在给定延迟后运行，或定期执行. 当需要多个工作线程时，或者需要ThreadPoolExecutor（此类扩展）的附加灵活性或功能时，此类比Timer更可取.

    * <p>Delayed tasks execute no sooner than they are enabled, but
    * without any real-time guarantees about when, after they are
    * enabled, they will commence. Tasks scheduled for exactly the same
    * execution time are enabled in first-in-first-out (FIFO) order of
    * submission.
    
    延迟的任务执行不早于启用的时间，但是对于启用后的时间，它们没有任何实时保证. 按照提交的先进先出（FIFO）顺序启用计划执行时间完全相同的任务.（队列中的所有任务使用同一个延迟时间）

    * <p>When a submitted task is cancelled before it is run, execution
    * is suppressed. By default, such a cancelled task is not
    * automatically removed from the work queue until its delay
    * elapses. While this enables further inspection and monitoring, it
    * may also cause unbounded retention of cancelled tasks. To avoid
    * this, set {@link #setRemoveOnCancelPolicy} to {@code true}, which
    * causes tasks to be immediately removed from the work queue at
    * time of cancellation.
    
    当提交的任务在运行之前被取消时，执行被抑制. 默认情况下，已取消的任务在延迟时间到达之前不会自动从工作队列中删除.
    虽然这可以进行进一步的检查和监视，但也可能导致无限期地保留已取消的任务. 
    为避免这种情况，请将setRemoveOnCancelPolicy设置为true，这将导致在取消时立即从工作队列中删除任务.

    * <p>Successive executions of a task scheduled via
    * {@code scheduleAtFixedRate} or
    * {@code scheduleWithFixedDelay} do not overlap. While different
    * executions may be performed by different threads, the effects of
    * prior executions <a
    * href="package-summary.html#MemoryVisibility"><i>happen-before</i></a>
    * those of subsequent ones.
    
    通过scheduleAtFixedRate或scheduleWithFixedDelay调度的任务的连续执行不会重叠. 
    尽管可以由不同的线程去执行，但是先前执行的效果发生在后续执行之前.（happen-before的含义，后续会去了解）

    * <p>While this class inherits from {@link ThreadPoolExecutor}, a few
    * of the inherited tuning methods are not useful for it. In
    * particular, because it acts as a fixed-sized pool using
    * {@code corePoolSize} threads and an unbounded queue, adjustments
    * to {@code maximumPoolSize} have no useful effect. Additionally, it
    * is almost never a good idea to set {@code corePoolSize} to zero or
    * use {@code allowCoreThreadTimeOut} because this may leave the pool
    * without threads to handle tasks once they become eligible to run.
    
    尽管此类从ThreadPoolExecutor继承，但是一些继承的调整方法对此没有用. 特别是，由于它使用核心线程个数的线程和无限制队列充当固定大小的池，因此对最大线程数的调整没有任何作用.
    此外，将corePoolSize设置为零或使用allowCoreThreadTimeOut几乎不是一个好主意，因为一旦有资格运行任务，这可能会使池中没有线程来处理任务.

    * <p><b>Extension notes:</b> This class overrides the
    * {@link ThreadPoolExecutor#execute(Runnable) execute} and
    * {@link AbstractExecutorService#submit(Runnable) submit}
    * methods to generate internal {@link ScheduledFuture} objects to
    * control per-task delays and scheduling.  To preserve
    * functionality, any further overrides of these methods in
    * subclasses must invoke superclass versions, which effectively
    * disables additional task customization.  However, this class
    * provides alternative protected extension method
    * {@code decorateTask} (one version each for {@code Runnable} and
    * {@code Callable}) that can be used to customize the concrete task
    * types used to execute commands entered via {@code execute},
    * {@code submit}, {@code schedule}, {@code scheduleAtFixedRate},
    * and {@code scheduleWithFixedDelay}.  By default, a
    * {@code ScheduledThreadPoolExecutor} uses a task type extending
    * {@link FutureTask}. However, this may be modified or replaced using
    * subclasses of the form:
    
    扩展说明：此类重写ThreadPoolExecutor＃execute和AbstractExecutorService＃submit方法，以生成内部ScheduledFuture对象，以控制每个任务的延迟和调度.
    为了保留功能，子类中对这些方法的任何进一步覆写都必须调用超类版本，这将有效地禁用其他任务自定义.
    但是，此类提供了可选择的受保护的扩展方法decorateTask，可用于自定义具体任务类型，任务类型通过execute、submit用来执行命令.
    默认情况下，ScheduledThreadPoolExecutor使用的任务类型扩展为FutureTask.
    但是，可以使用以下形式的子类对其进行修改或替换：

    *  <pre> {@code
    * public class CustomScheduledExecutor extends ScheduledThreadPoolExecutor {
    *
    *   static class CustomTask<V> implements RunnableScheduledFuture<V> { ... }
    *
    *   protected <V> RunnableScheduledFuture<V> decorateTask(
    *                Runnable r, RunnableScheduledFuture<V> task) {
    *       return new CustomTask<V>(r, task);
    *   }
    *
    *   protected <V> RunnableScheduledFuture<V> decorateTask(
    *                Callable<V> c, RunnableScheduledFuture<V> task) {
    *       return new CustomTask<V>(c, task);
    *   }
    *   // ... add constructors, etc.
    * }}</pre>

```