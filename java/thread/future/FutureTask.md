### 翻译FutureTask注释

```java

   /**
    * A cancellable asynchronous computation.  This class provides a base
    * implementation of {@link Future}, with methods to start and cancel
    * a computation, query to see if the computation is complete, and
    * retrieve the result of the computation.  The result can only be
    * retrieved when the computation has completed; the {@code get}
    * methods will block if the computation has not yet completed.  Once
    * the computation has completed, the computation cannot be restarted
    * or cancelled (unless the computation is invoked using
    * {@link #runAndReset}).
    
    可取消的异步计算. 此类提供Future的基本实现，其中包含启动和取消计算，查询以查看计算是否完成以及检索计算结果的方法.
    只有在计算完成后才能检索结果; 如果计算尚未完成，则get方法将阻塞.
    一旦计算完成，就不能重新开始或取消计算（除非在计算中调用runAndReset）

    * <p>A {@code FutureTask} can be used to wrap a {@link Callable} or
    * {@link Runnable} object.  Because {@code FutureTask} implements
    * {@code Runnable}, a {@code FutureTask} can be submitted to an
    * {@link Executor} for execution.
    
    FutureTask可用于包装Callable或Runnable对象. 因为FutureTask实现了Runnable，FutureTask可以提交给Executor进行执行.

    * <p>In addition to serving as a standalone class, this class provides
    * {@code protected} functionality that may be useful when creating
    * customized task classes.

    除了用作独立类，该类还提供protected功能，这些功能在创建自定义任务类时可能会很有用.

    */

```