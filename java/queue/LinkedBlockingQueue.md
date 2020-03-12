### 翻译LinkedBlockingQueue注释

```java

   /**
    * An optionally-bounded {@linkplain BlockingQueue blocking queue} based on
    * linked nodes.

    基于链表的有界/无界队列（有界与无界可通过容量设置）

    * This queue orders elements FIFO (first-in-first-out).
    * The <em>head</em> of the queue is that element that has been on the
    * queue the longest time.
    * The <em>tail</em> of the queue is that element that has been on the
    * queue the shortest time. New elements
    * are inserted at the tail of the queue, and the queue retrieval
    * operations obtain elements at the head of the queue.
    * Linked queues typically have higher throughput than array-based queues but
    * less predictable performance in most concurrent applications.
    
    队列按照先进先出的顺序访问. 队列的头部元素是在队列中停留时间最长的元素. 队列的尾部元素是在队列中停留时间最短的元素. 新元素被插入到队列的尾部，队列检索操作获取队列头部的元素.
    链接队列通常比基于阵列的队列具有更高的吞吐量，但在大多数并发应用程序中，可预测的性能较差.

    * <p>The optional capacity bound constructor argument serves as a
    * way to prevent excessive queue expansion. The capacity, if unspecified,
    * is equal to {@link Integer#MAX_VALUE}.  Linked nodes are
    * dynamically created upon each insertion unless this would bring the
    * queue above capacity.
    
    可选的capacity作为构造函数的入参用作防止队列过度扩展的一种方法. 如果未指定capacity，默认是Integer.MAX_VALUE. 每次插入时都会动态创建链接节点，除非这会使队列超出容量.

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