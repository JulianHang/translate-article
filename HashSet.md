### 翻译HashSet注释

```java

   /**
    * This class implements the <tt>Set</tt> interface, backed by a hash table
    * (actually a <tt>HashMap</tt> instance).  It makes no guarantees as to the
    * iteration order of the set; in particular, it does not guarantee that the
    * order will remain constant over time.  This class permits the <tt>null</tt>
    * element.

    该类实现了Set接口，底层通过哈希表（确切来说是一个HashMap实例）.
    它不能保证集合的迭代顺序;特别是，它不能保证随着时间的推移顺序保持不变.
    该类允许存放null元素.

    * <p>This class offers constant time performance for the basic operations
    * (<tt>add</tt>, <tt>remove</tt>, <tt>contains</tt> and <tt>size</tt>),
    * assuming the hash function disperses the elements properly among the
    * buckets.  Iterating over this set requires time proportional to the sum of
    * the <tt>HashSet</tt> instance's size (the number of elements) plus the
    * "capacity" of the backing <tt>HashMap</tt> instance (the number of
    * buckets).  Thus, it's very important not to set the initial capacity too
    * high (or the load factor too low) if iteration performance is important.

    假定哈希函数适当地分散桶中的元素，该类对于执行基本的操作（add、remove、contains、size）消耗恒定的时间.
    迭代所需要的时间与HashSet的大小加上HashMap的容量成比例.
    因此，若迭代性能很重要，对于设置初始容量不能太高（加载因子太低）.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a hash set concurrently, and at least one of
    * the threads modifies the set, it <i>must</i> be synchronized externally.
    * This is typically accomplished by synchronizing on some object that
    * naturally encapsulates the set.
    
    HashSet是非线程安全.
    如果多线程同时访问同一个HasSet，至少一个线程会修改HashSet结构，它必须在外部保持同步.
    一般通过包装HashMap的对象来完成同步.

    * If no such object exists, the set should be "wrapped" using the
    * {@link Collections#synchronizedSet Collections.synchronizedSet}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the set:<pre>
    *   Set s = Collections.synchronizedSet(new HashSet(...));</pre>
    
    如果外部对象不存在，应该使用Collections.synchronizedSet来包裹HashSet对象.
    该方法最好在HashSet创建时调用，为了防止偶然性的非线程安全访问HashSet.

    * <p>The iterators returned by this class's <tt>iterator</tt> method are
    * <i>fail-fast</i>: if the set is modified at any time after the iterator is
    * created, in any way except through the iterator's own <tt>remove</tt>
    * method, the Iterator throws a {@link ConcurrentModificationException}.
    * Thus, in the face of concurrent modification, the iterator fails quickly
    * and cleanly, rather than risking arbitrary, non-deterministic behavior at
    * an undetermined time in the future.
    
    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw <tt>ConcurrentModificationException</tt> on a best-effort basis.
    * Therefore, it would be wrong to write a program that depended on this
    * exception for its correctness: <i>the fail-fast behavior of iterators
    * should be used only to detect bugs.</i>
    
    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```