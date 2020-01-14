### 翻译Executors注释

```java

   /**
    * Factory and utility methods for {@link Executor}, {@link
    * ExecutorService}, {@link ScheduledExecutorService}, {@link
    * ThreadFactory}, and {@link Callable} classes defined in this
    * package. This class supports the following kinds of methods:
    
    Executor、ExecutorService、ScheduledExecutorService、ThreadFactory的工厂和实用方法, Callable类定义在该包中. 此类支持以下方法：

    * <ul>
    *   <li> Methods that create and return an {@link ExecutorService}
    *        set up with commonly useful configuration settings.

            使用常用的配置来创建和返回ExecutorSerive的方法.

    *   <li> Methods that create and return a {@link ScheduledExecutorService}
    *        set up with commonly useful configuration settings.

            使用常用的配置来创建和返回ScheduleExecutorSerive的方法.

    *   <li> Methods that create and return a "wrapped" ExecutorService, that
    *        disables reconfiguration by making implementation-specific methods
    *        inaccessible.

             创建并返回包装的ExecutorService的方法，该方法通过使特定实现的方法不可访问来禁用重新配置.

    *   <li> Methods that create and return a {@link ThreadFactory}
    *        that sets newly created threads to a known state.

             创建并返回ThreadFactory的方法，该方法将新创建的线程设置为已经状态.

    *   <li> Methods that create and return a {@link Callable}
    *        out of other closure-like forms, so they can be used
    *        in execution methods requiring {@code Callable}.

             从其他类似闭包的形式创建和返回Callable的方法，因此它们可用于需要Callable（参数）的执行方法中.

    * </ul>


    /

```