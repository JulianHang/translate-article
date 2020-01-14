### 翻译CompletionService注释

```java

   /**
    * A service that decouples the production of new asynchronous tasks
    * from the consumption of the results of completed tasks.  Producers
    * {@code submit} tasks for execution. Consumers {@code take}
    * completed tasks and process their results in the order they
    * complete.  A {@code CompletionService} can for example be used to
    * manage asynchronous I/O, in which tasks that perform reads are
    * submitted in one part of a program or system, and then acted upon
    * in a different part of the program when the reads complete,
    * possibly in a different order than they were requested.
    
    该服务将新异步任务的产生与完成任务的结果的消耗分开. 生产者执行提交的任务. 消费者获取已完成的任务，并按完成顺序处理结果.
    CompletionService可以用于管理异步I/O，在异步I/O中，执行任务的读取在程序或系统的一部分中提交，然后在读取完成时在程序的另一部分中进行操作，顺序可能与要求的顺序不同.

    * <p>Typically, a {@code CompletionService} relies on a separate
    * {@link Executor} to actually execute the tasks, in which case the
    * {@code CompletionService} only manages an internal completion
    * queue. The {@link ExecutorCompletionService} class provides an
    * implementation of this approach.

    通常，CompletionService依赖于单独的Executor去执行任务，在这种情况下，CompletionService仅管理内部完成队列. ExecutorCompletionService类提供了此方法的实现.

    * <p>Memory consistency effects: Actions in a thread prior to
    * submitting a task to a {@code CompletionService}
    * <a href="package-summary.html#MemoryVisibility"><i>happen-before</i></a>
    * actions taken by that task, which in turn <i>happen-before</i>
    * actions following a successful return from the corresponding {@code take()}.

    内存一致性影响：将任务提交给该任务执行的CompletionService操作之前，线程中的操作，这些操作依次发生在从相应的take成功返回之后的操作之前.

    */



```