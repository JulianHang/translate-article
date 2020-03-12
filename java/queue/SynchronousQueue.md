#### 翻译SynchronousQueue注释

```java

   /**
    * A {@linkplain BlockingQueue blocking queue} in which each insert
    * operation must wait for a corresponding remove operation by another
    * thread, and vice versa.  A synchronous queue does not have any
    * internal capacity, not even a capacity of one.  You cannot
    * {@code peek} at a synchronous queue because an element is only
    * present when you try to remove it; you cannot insert an element
    * (using any method) unless another thread is trying to remove it;
    * you cannot iterate as there is nothing to iterate.  The
    * <em>head</em> of the queue is the element that the first queued
    * inserting thread is trying to add to the queue; if there is no such
    * queued thread then no element is available for removal and
    * {@code poll()} will return {@code null}.  For purposes of other
    * {@code Collection} methods (for example {@code contains}), a
    * {@code SynchronousQueue} acts as an empty collection.  This queue
    * does not permit {@code null} elements.
    
    阻塞队列，其中每个插入操作必须等待另一个线程进行相应的删除操作，反之亦然. 同步队列没有任何内部容量，甚至没有一个容量. 你不能在同步队列中peek，因为仅当您尝试删除它时，该元素才存在.
    你不能插入元素（使用任何方法），除非另一个线程试图将其删除. 您无法迭代，因为没有什么可迭代的. 队列的头部是第一个排队的插入线程试图添加到队列中的元素. 
    如果没有这样的排队线程，则没有元素可用于删除，并且poll将返回null. 出于其他Collection方法的目的，SynchronousQueue用作空集合.
    该队列不允许null元素.

    * <p>Synchronous queues are similar to rendezvous channels used in
    * CSP and Ada. They are well suited for handoff designs, in which an
    * object running in one thread must sync up with an object running
    * in another thread in order to hand it some information, event, or
    * task.

    同步队列类似于CSP和Ada中使用的集合通道. 它们非常适合切换设计，在该设计中，在一个线程中运行的对象必须与在另一个线程中运行的对象同步，以便向其传递一些信息，事件或任务.

    * <p>This class supports an optional fairness policy for ordering
    * waiting producer and consumer threads.  By default, this ordering
    * is not guaranteed. However, a queue constructed with fairness set
    * to {@code true} grants threads access in FIFO order.
    
    此类支持可选的公平性策略，用于排列正在等待的生产者和使用者线程. 默认情况下，不保证此排序. 但是，将公平性设置为true构造的队列以FIFO顺序授予线程访问权限.

    * <p>This class and its iterator implement all of the
    * <em>optional</em> methods of the {@link Collection} and {@link
    * Iterator} interfaces.
    
    该类和它的迭代器实现了Collection和Iterator接口的所有可选方法.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.
    
    该类是Java集合框架的一个成员

    */

```