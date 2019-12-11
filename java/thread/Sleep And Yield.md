### 翻译sleep、yield关键词，引入Java Specification（Java说明书），可自行Google下载针对版本

```java

    /**

     Thread#sleep使当前正在执行的线程在指定的持续时间内进入睡眠状态（暂时停止执行），这取决于系统计时器和调度程序的精度和准确性. 线程不会失去任何锁的所有权（不会释放锁），执行的恢复将取决于调度和在其上执行线程的处理器的可用性.

     重要的是要注意Thread#sleep和Thread#yield都没有任何同步语义（这两个方法都可以在非同步控制方法或非同步代码块中执行，因为没有操作锁，都没有释放锁）. 特别是，编译器不必在调用Thread.sleep或Thread.yield之前将缓存在寄存器中的写操作刷新到共享内存中，也不必在调用Thread.sleep或Thread之后重新加载缓存在寄存器中的值.

        例如，在下面的代码片段中，假定this.done是非volatile修饰的布尔字段：

        while (!this.done)
            Thread.sleep(1000);
        
        编译器可以自由读取一次this.done字段，并在每次循环执行中重用缓存的值. 这意味着即使另一个线程更改了this.done的值，循环也将永远不会终止（缓存一致性）.

    /

```