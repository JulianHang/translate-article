### 翻译AbstractExecutorService注释

```java


   /**
    * Provides default implementations of {@link ExecutorService}
    * execution methods. This class implements the {@code submit},
    * {@code invokeAny} and {@code invokeAll} methods using a
    * {@link RunnableFuture} returned by {@code newTaskFor}, which defaults
    * to the {@link FutureTask} class provided in this package.  For example,
    * the implementation of {@code submit(Runnable)} creates an
    * associated {@code RunnableFuture} that is executed and
    * returned. Subclasses may override the {@code newTaskFor} methods
    * to return {@code RunnableFuture} implementations other than
    * {@code FutureTask}.
    
    提供ExecutorService执行方法的默认实现. 此类使用newTaskFor返回的RunnableFuture来实现Submit，invokeAny和invokeAll方法，默认情况下，该方法是包中提供的FutureTask类. 例如Submit的实现会创建一个关联的RunnableFuture，用于被执行并返回。 子类可以重写newTaskFor方法，以返回FutureTask以外的RunnableFuture实现.

    * <p><b>Extension example</b>. Here is a sketch of a class
    * that customizes {@link ThreadPoolExecutor} to use
    * a {@code CustomTask} class instead of the default {@code FutureTask}:
    *  <pre> {@code
    * public class CustomThreadPoolExecutor extends ThreadPoolExecutor {
    *
    *   static class CustomTask<V> implements RunnableFuture<V> {...}
    *
    *   protected <V> RunnableFuture<V> newTaskFor(Callable<V> c) {
    *       return new CustomTask<V>(c);
    *   }
    *   protected <V> RunnableFuture<V> newTaskFor(Runnable r, V v) {
    *       return new CustomTask<V>(r, v);
    *   }
    *   // ... add constructors, etc.
    * }}</pre>

    扩展说明：自定义ThreadPoolExecutor类使用CustomTask代替默认的FutureTask

```