### 翻译SoftReference注释

``` java

   /**
    * Soft reference objects, which are cleared at the discretion of the garbage
    * collector in response to memory demand.  Soft references are most often used
    * to implement memory-sensitive caches.

    软引用对象, 垃圾回收器根据内存需要酌情清除对象. 软引用最常用于实现对内存敏感的缓存.

    * <p> Suppose that the garbage collector determines at a certain point in time
    * that an object is <a href="package-summary.html#reachability">softly
    * reachable</a>.  At that time it may choose to clear atomically all soft
    * references to that object and all soft references to any other
    * softly-reachable objects from which that object is reachable through a chain
    * of strong references.  At the same time or at some later time it will
    * enqueue those newly-cleared soft references that are registered with
    * reference queues.
    
    假设垃圾收集器在某个时间点确定对象是软可达. 那时，它可以选择自动清除对该对象的所有软引用，
    以及对所有其他软可达对象的所有软引用，这些对象是通过一系列强引用变成可达的
    同时或在以后的某个时间，它将那些在引用队列中注册的新清除的软引用放入队列。

    * <p> All soft references to softly-reachable objects are guaranteed to have
    * been cleared before the virtual machine throws an
    * <code>OutOfMemoryError</code>.  Otherwise no constraints are placed upon the
    * time at which a soft reference will be cleared or the order in which a set
    * of such references to different objects will be cleared.  Virtual machine
    * implementations are, however, encouraged to bias against clearing
    * recently-created or recently-used soft references.
    
    虚拟机抛出内存溢出之前，确保已清除所有对软可达对象的软引用.
    否则，清除软引用的时间或清除对不同对象的一组此类引用的时间将不受任何限制.
    但是，鼓励虚拟机实现偏向于清除最近创建或最近使用的软引用.

    * <p> Direct instances of this class may be used to implement simple caches;
    * this class or derived subclasses may also be used in larger data structures
    * to implement more sophisticated caches.  As long as the referent of a soft
    * reference is strongly reachable, that is, is actually in use, the soft
    * reference will not be cleared.  Thus a sophisticated cache can, for example,
    * prevent its most recently used entries from being discarded by keeping
    * strong referents to those entries, leaving the remaining entries to be
    * discarded at the discretion of the garbage collector.

    该类的实例可用于实现简单的缓存.
    此类或派生的子类也可以在较大的数据结构中使用，以实现更复杂的缓存.
    只要软引用是强可达，已在使用中，软引用将不会被清除.
    因此，复杂的高速缓存可以通过保持对那些对象的强引用来防止其最近使用的条目被丢弃，而其余条目则由垃圾收集器来决定丢弃.

```