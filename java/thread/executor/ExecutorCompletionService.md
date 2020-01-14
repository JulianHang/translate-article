### 翻译ExecutorCompletionService注释

```java

    /**
    * A {@link CompletionService} that uses a supplied {@link Executor}
    * to execute tasks.  This class arranges that submitted tasks are,
    * upon completion, placed on a queue accessible using {@code take}.
    * The class is lightweight enough to be suitable for transient use
    * when processing groups of tasks.
    
    CompletionService使用Executor去执行任务. 该类安排已提交的任务，任务完成会被放入到队列中，使用take可访问该队列.
    该类足够轻巧，适合在处理任务组时短暂使用.

    * <p>
    *
    * <b>Usage Examples.</b>
    
    使用实例.

    * Suppose you have a set of solvers for a certain problem, each
    * returning a value of some type {@code Result}, and would like to
    * run them concurrently, processing the results of each of them that
    * return a non-null value, in some method {@code use(Result r)}. You
    * could write this as:
    
    假设您有一组解决某个问题的求解器，每个求解器都返回某个类型为code Result的值，并且希望同时运行它们，并对每个返回非空值的结果进行处理. 你可以这样写.

    * <pre> {@code
    * void solve(Executor e, Collection<Callable<Result>> solvers) throws InterruptedException, ExecutionException {
    *     CompletionService<Result> ecs = new ExecutorCompletionService<Result>(e);
    *     for (Callable<Result> s : solvers)
    *         ecs.submit(s);
    *     int n = solvers.size();
    *     for (int i = 0; i < n; ++i) {
    *         Result r = ecs.take().get();
    *         if (r != null)
    *             use(r);
    *     }
    * }}</pre>
    *
    * Suppose instead that you would like to use the first non-null result
    * of the set of tasks, ignoring any that encounter exceptions,
    * and cancelling all other tasks when the first one is ready:
    
    相反，假设您想使用任务集的第一个非空结果，忽略遇到异常的任何结果，并在第一个任务就绪时取消所有其他任务：

    * <pre> {@code
    * void solve(Executor e, Collection<Callable<Result>> solvers) throws InterruptedException {
    *     CompletionService<Result> ecs = new ExecutorCompletionService<Result>(e);
    *     int n = solvers.size();
    *     List<Future<Result>> futures = new ArrayList<Future<Result>>(n);
    *     Result result = null;
    *     try {
    *         for (Callable<Result> s : solvers)
    *             futures.add(ecs.submit(s));
    *         for (int i = 0; i < n; ++i) {
    *             try {
    *                 Result r = ecs.take().get();
    *                 if (r != null) {
    *                     result = r;
    *                     break;
    *                 }
    *             } catch (ExecutionException ignore) {}
    *         }
    *     }
    *     finally {
    *         for (Future<Result> f : futures)
    *             f.cancel(true);
    *     }
    *
    *     if (result != null)
    *         use(result);
    * }}</pre>
    */


```