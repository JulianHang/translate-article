### 翻译ArrayBlockingQueue注释

```java

   /**
    * A bounded {@linkplain BlockingQueue blocking queue} backed by an
    * array.  This queue orders elements FIFO (first-in-first-out).  The
    * <em>head</em> of the queue is that element that has been on the
    * queue the longest time.  The <em>tail</em> of the queue is that
    * element that has been on the queue the shortest time. New elements
    * are inserted at the tail of the queue, and the queue retrieval
    * operations obtain elements at the head of the queue.
    
    由数组支持的有界阻塞队列. 该队列按照先进先出顺序. 队列的头部元素是在队列中停留时间最长的元素. 队列的尾部元素是在队列中停留时间最短的元素. 新元素被插入到队列的尾部，队列检索操作获取队列头部的元素.

    * <p>This is a classic &quot;bounded buffer&quot;, in which a
    * fixed-sized array holds elements inserted by producers and
    * extracted by consumers.  Once created, the capacity cannot be
    * changed.  Attempts to {@code put} an element into a full queue
    * will result in the operation blocking; attempts to {@code take} an
    * element from an empty queue will similarly block.
    
    这是经典的有界缓冲区，其中固定大小的数组包含生产者插入并由消费者提取的元素. 尝试将元素放入饱满的队列中将导致操作阻塞. 尝试从空队列中获取元素也会类似地阻塞.

    * <p>This class supports an optional fairness policy for ordering
    * waiting producer and consumer threads.  By default, this ordering
    * is not guaranteed. However, a queue constructed with fairness set
    * to {@code true} grants threads access in FIFO order. Fairness
    * generally decreases throughput but reduces variability and avoids
    * starvation.
    
    此类支持可选的公平性策略，用于排列正在等待的生产者和消费者线程. 默认情况下，并不保证排序顺序. 但是，将公平性设置为true的队列按FIFO顺序授予线程访问权限. 公平通常会降低吞吐量，但会减少可变性并避免饥饿.

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