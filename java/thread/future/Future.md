### 翻译Future注释

```java

   /**
    * A {@code Future} represents the result of an asynchronous
    * computation.  Methods are provided to check if the computation is
    * complete, to wait for its completion, and to retrieve the result of
    * the computation.  The result can only be retrieved using method
    * {@code get} when the computation has completed, blocking if
    * necessary until it is ready.  Cancellation is performed by the
    * {@code cancel} method.  Additional methods are provided to
    * determine if the task completed normally or was cancelled. Once a
    * computation has completed, the computation cannot be cancelled.
    * If you would like to use a {@code Future} for the sake
    * of cancellability but not provide a usable result, you can
    * declare types of the form {@code Future<?>} and
    * return {@code null} as a result of the underlying task.
    
    Future表示异步计算的结果. 提供了一些方法来检查计算是否完成，等待其完成以及检索计算结果. 计算完成后，只能使用方法get检索结果，并在必要时阻塞直到准备好.
    提供了其他方法来确定任务是正常完成还是被取消. 一旦计算完成，就无法取消计算.
    如果出于可取消性的目的而使用Future而不提供可用的结果，则可以声明Future <?>形式的类型，并返回null作为任务的结果.

    * <p>
    * <b>Sample Usage</b> (Note that the following classes are all
    * made-up.)
    * <pre> {@code
    * interface ArchiveSearcher { String search(String target); }
    * class App {
    *   ExecutorService executor = ...
    *   ArchiveSearcher searcher = ...
    *   void showSearch(final String target)
    *       throws InterruptedException {
    *     Future<String> future
    *       = executor.submit(new Callable<String>() {
    *         public String call() {
    *             return searcher.search(target);
    *         }});
    *     displayOtherThings(); // do other things while searching
    *     try {
    *       displayText(future.get()); // use future
    *     } catch (ExecutionException ex) { cleanup(); return; }
    *   }
    * }}</pre>
    *
    * The {@link FutureTask} class is an implementation of {@code Future} that
    * implements {@code Runnable}, and so may be executed by an {@code Executor}.
    * For example, the above construction with {@code submit} could be replaced by:

    FutureTask是实现Runnable与Future, 因为可以被Executor执行. 例如，上面的submit结构可替换为：

    *  <pre> {@code
    * FutureTask<String> future =
    *   new FutureTask<String>(new Callable<String>() {
    *     public String call() {
    *       return searcher.search(target);
    *   }});
    * executor.execute(future);}</pre>
    *
    * <p>Memory consistency effects: Actions taken by the asynchronous computation
    * <a href="package-summary.html#MemoryVisibility"> <i>happen-before</i></a>
    * actions following the corresponding {@code Future.get()} in another thread.

    内存一致性影响：异步计算采取的操作发生在另一个线程中相应Future.get之后的操作之前.

```