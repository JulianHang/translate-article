### 翻译HashMap注释

```java

   /**
    * Hash table based implementation of the <tt>Map</tt> interface.  This
    * implementation provides all of the optional map operations, and permits
    * <tt>null</tt> values and the <tt>null</tt> key.  (The <tt>HashMap</tt>
    * class is roughly equivalent to <tt>Hashtable</tt>, except that it is
    * unsynchronized and permits nulls.)  This class makes no guarantees as to
    * the order of the map; in particular, it does not guarantee that the order
    * will remain constant over time.

    基于哈希表的Map接口实现. 它提供了map的所有可选操作，key与value允许存放null.
    HashMap大体上等同于Hashtable，除了它是非线程安全和允许null.
    该类不能保证map的顺序；特别是，它不能保证随着时间的推移顺序保持不变.

    * <p>This implementation provides constant-time performance for the basic
    * operations (<tt>get</tt> and <tt>put</tt>), assuming the hash function
    * disperses the elements properly among the buckets.  Iteration over
    * collection views requires time proportional to the "capacity" of the
    * <tt>HashMap</tt> instance (the number of buckets) plus its size (the number
    * of key-value mappings).  Thus, it's very important not to set the initial
    * capacity too high (or the load factor too low) if iteration performance is
    * important.

    假定哈希函数适当地分散桶中的元素,那么该实现对于执行基本的操作（get、put）会消耗恒定的时间.
    迭代所需要的时间与HashMapd的容量加上它的大小（键值的映射）成比例.
    因此，若迭代性能很重要，对于设置初始容量不能太高（加载因子太低）.

    * <p>An instance of <tt>HashMap</tt> has two parameters that affect its
    * performance: <i>initial capacity</i> and <i>load factor</i>.  The
    * <i>capacity</i> is the number of buckets in the hash table, and the initial
    * capacity is simply the capacity at the time the hash table is created.  The
    * <i>load factor</i> is a measure of how full the hash table is allowed to
    * get before its capacity is automatically increased.  When the number of
    * entries in the hash table exceeds the product of the load factor and the
    * current capacity, the hash table is <i>rehashed</i> (that is, internal data
    * structures are rebuilt) so that the hash table has approximately twice the
    * number of buckets.

    一个HashMap对象有两个参数会影响它的性能：初始容量与加载因子.
    初始容量指的是在哈希表中桶的数量，初始容量就是创建哈希表时的容量.
    加载因子是一种在容量自动扩容前，允许哈希表获取填满程度的措施（哈希表中元素填满的程度，比如填满了75%）
    当哈希表中的元素数量超过加载因子与当前容量的乘积时，哈希表会重新构建（内部数据结构重建）
    因此，哈希表大约是桶的数量的两倍.

    * <p>As a general rule, the default load factor (.75) offers a good
    * tradeoff between time and space costs.  Higher values decrease the
    * space overhead but increase the lookup cost (reflected in most of
    * the operations of the <tt>HashMap</tt> class, including
    * <tt>get</tt> and <tt>put</tt>).  The expected number of entries in
    * the map and its load factor should be taken into account when
    * setting its initial capacity, so as to minimize the number of
    * rehash operations.  If the initial capacity is greater than the
    * maximum number of entries divided by the load factor, no rehash
    * operations will ever occur.

    通常的规则是，默认加载因子（0.75）提供了一个在时间与空间代价上的权衡.
    较高的加载因子会减少空间开销，但是增加了查找的成本（在HashMap的大部分操作中都反映了这一点，包括get和put）.

    插入题外话---->为什么较高的加载因子会减少空间开销，增加了查找的成本？
    加载因子表示元素填满的程度，在容量不变的情况下，随着加载因子越大，填满的元素就越多，空间利用率变大了，
    简单来说，容量固定值为16
    A. 加载因子0.75，该数组在不扩容的情况下最多可填充0.75 * 16 = 12     
    B. 加载因子1，该数组在不扩容的情况下最多可填充1 * 16 = 16
    所以我们说空间利用率变大了，同样的，元素多了，查找的成本也就自然增加了.

    设置初始容量时应该考虑预期的元素数量与加载因子，以便最大限度地减少重新构建的操作次数.
    如果初始容量大于元素数量最大值除以加载因子的值，则重新构建操作将不会发生.

    * <p>If many mappings are to be stored in a <tt>HashMap</tt>
    * instance, creating it with a sufficiently large capacity will allow
    * the mappings to be stored more efficiently than letting it perform
    * automatic rehashing as needed to grow the table.  Note that using
    * many keys with the same {@code hashCode()} is a sure way to slow
    * down performance of any hash table. To ameliorate impact, when keys
    * are {@link Comparable}, this class may use comparison order among
    * keys to help break ties.

    假设很多键值对要存储在HashMap中，相对于执行自动重新构建来扩充哈希表的容量来说，创建一个初始容量足够大的HashMap更高效地存储键值对.
    注意，使用相同hashCode的键是降低任何哈希表性能的一种方式.
    为了改善碰撞，当键是Comparable时，该类可以使用键之间的比较规则来帮助打破平局.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a hash map concurrently, and at least one of
    * the threads modifies the map structurally, it <i>must</i> be
    * synchronized externally.  (A structural modification is any operation
    * that adds or deletes one or more mappings; merely changing the value
    * associated with a key that an instance already contains is not a
    * structural modification.)  This is typically accomplished by
    * synchronizing on some object that naturally encapsulates the map.

    注意，此实现不是同步的.
    如果多线程同时访问同一个HashMap，至少一个线程会修改HashMap结构，它必须在外部保持同步.
    (结构修改指的是添加或删除一个或多个键值对的任何一个操作;仅仅改变对象已存在的键所对应的值不属于结构修改)
    一般通过包装HashMap的对象来完成同步.

    * If no such object exists, the map should be "wrapped" using the
    * {@link Collections#synchronizedMap Collections.synchronizedMap}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the map:<pre>
    *   Map m = Collections.synchronizedMap(new HashMap(...));</pre>

    如果外部对象不存在，应该使用Collections.synchronizedMap来包裹HashMap对象.
    该方法最好在HashMap创建时调用，为了防止偶然性的非线程安全访问HashMap.

    * <p>The iterators returned by all of this class's "collection view methods"
    * are <i>fail-fast</i>: if the map is structurally modified at any time after
    * the iterator is created, in any way except through the iterator's own
    * <tt>remove</tt> method, the iterator will throw a
    * {@link ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than risking
    * arbitrary, non-deterministic behavior at an undetermined time in the
    * future.

    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常（该说法很像ArrayList）.
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