### 翻译Reference注释

```java

   /**
    * Abstract base class for reference objects.  This class defines the
    * operations common to all reference objects.  Because reference objects are
    * implemented in close cooperation with the garbage collector, this class may
    * not be subclassed directly.
    
    引用对象的抽象基类. 该类定义了所有引用对象的通用操作. 因为引用对象通过与垃圾回收器的密切合作来实现, 该类不可能直接作为子类.

    /* A Reference instance is in one of four possible internal states:
    
    一个引用对象处于四种可能的内部状态之一：

    *     Active: Subject to special treatment by the garbage collector.  Some
    *     time after the collector detects that the reachability of the
    *     referent has changed to the appropriate state, it changes the
    *     instance's state to either Pending or Inactive, depending upon
    *     whether or not the instance was registered with a queue when it was
    *     created.  In the former case it also adds the instance to the
    *     pending-Reference list.  Newly-created instances are Active.
    
          活跃：受到垃圾回收器的特殊处理.
          收集器检测到引用的可达性已经转换为适当的状态后的一段时间, 它转换对象的状态为待定或不活跃，取决于对象在创建时是否注册到队列中.
          在前一种情况下它还被添加到待定引用列表中. 新创建的对象是活跃的.

    *     Pending: An element of the pending-Reference list, waiting to be
    *     enqueued by the Reference-handler thread.  Unregistered instances
    *     are never in this state.

          待定：待定引用列表中的元素， 等待引用处理线程将其放入队列中. 未注册到队列的对象不会处于此状态.

    *     Enqueued: An element of the queue with which the instance was
    *     registered when it was created.  When an instance is removed from
    *     its ReferenceQueue, it is made Inactive.  Unregistered instances are
    *     never in this state.

          入队：对象在创建时注册的队列中的元素. 当对象从它的引用队列中移除时，它被转换为不活跃.
          未注册到队列的对象不会处于此状态.

    *     Inactive: Nothing more to do.  Once an instance becomes Inactive its
    *     state will never change again.
    
          不活跃：没事做. 一旦对象成为不活跃，它的状态将不会在转换.

    * The state is encoded in the queue and next fields as follows:
    
    队列中的状态和下一个字段编码如下：

    *     Active: queue = ReferenceQueue with which instance is registered, or
    *     ReferenceQueue.NULL if it was not registered with a queue; next =
    *     null.
    
          活跃：对象注册时：queue = ReferenceQueue, 对象未注册： queue = ReferenceQueue.NULL ； next = null

    *     Pending: queue = ReferenceQueue with which instance is registered;
    *     next = this

          待定：对象注册时：queue = ReferenceQueue ; next = this;

    *     Enqueued: queue = ReferenceQueue.ENQUEUED; next = Following instance
    *     in queue, or this if at end of list.
    
          入队：queue = ReferenceQueue.ENQUEUED  

    *     Inactive: queue = ReferenceQueue.NULL; next = this.

          不活跃： queue = ReferenceQueue.NULL; next = this.

    * With this scheme the collector need only examine the next field in order
    * to determine whether a Reference instance requires special treatment: If
    * the next field is null then the instance is active; if it is non-null,
    * then the collector should treat the instance normally.
    
    采用此方案，收集器只需要检查下一个字段即可确定引用对象是否需要特殊处理：
    如果next = null 说明对象是活跃的； 如果next != null 说明收集器正常处理对象

    * To ensure that a concurrent collector can discover active Reference
    * objects without interfering with application threads that may apply
    * the enqueue() method to those objects, collectors should link
    * discovered objects through the discovered field. The discovered
    * field is also used for linking Reference objects in the pending list.
    *

    为了确保并发的收集器可以发现活动的引用对象而不会干扰可能将enqueue方法应用于这些对象的应用程序线程，收集器应通过发现字段链接发现的对象.
    发现字段还用于链接挂起列表中的引用对象。

```