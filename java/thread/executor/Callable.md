### 翻译Callable注释-类似于Runnable

```java

    /**
    * A task that returns a result and may throw an exception.
    * Implementors define a single method with no arguments called
    * {@code call}.
    
    返回任务的结果和可能抛出异常
    该实现定义了没有参数的call方法.

    * <p>The {@code Callable} interface is similar to {@link
    * java.lang.Runnable}, in that both are designed for classes whose
    * instances are potentially executed by another thread.  A
    * {@code Runnable}, however, does not return a result and cannot
    * throw a checked exception.
    
    Callable接口类似于Runnable，两者都是针对其实例可能被另一个线程执行而设计.
    然而，Runnable不能返回结果也不能抛出异常

    * <p>The {@link Executors} class contains utility methods to
    * convert from other common forms to {@code Callable} classes.

    Executors类包含实用程序方法，可从其他常见形式转换为Callable类.

    * /



```