### 翻译LinkedHashMap注释

```java

   /**
    * <p>Hash table and linked list implementation of the <tt>Map</tt> interface,
    * with predictable iteration order.  This implementation differs from
    * <tt>HashMap</tt> in that it maintains a doubly-linked list running through
    * all of its entries.  This linked list defines the iteration ordering,
    * which is normally the order in which keys were inserted into the map
    * (<i>insertion-order</i>).  Note that insertion order is not affected
    * if a key is <i>re-inserted</i> into the map.  (A key <tt>k</tt> is
    * reinserted into a map <tt>m</tt> if <tt>m.put(k, v)</tt> is invoked when
    * <tt>m.containsKey(k)</tt> would return <tt>true</tt> immediately prior to
    * the invocation.)

    哈希表与链表的Map接口实现，可预测的迭代顺序.
    此实现与HashMap的不同之处在于，它维护了一个遍历所有节点的双向链表.
    此链表定义了迭代顺序，该顺序通常是键插入.
    注意，如果键重复插入到表中，则插入顺序不会受到影响.

    * <p>This implementation spares its clients from the unspecified, generally
    * chaotic ordering provided by {@link HashMap} (and {@link Hashtable}),
    * without incurring the increased cost associated with {@link TreeMap}.  It
    * can be used to produce a copy of a map that has the same order as the
    * original, regardless of the original map's implementation:
    * <pre>
    *     void foo(Map m) {
    *         Map copy = new LinkedHashMap(m);
    *         ...
    *     }
    * </pre>
    * This technique is particularly useful if a module takes a map on input,
    * copies it, and later returns results whose order is determined by that of
    * the copy.  (Clients generally appreciate having things returned in the same
    * order they were presented.)

    此实现将它的客户从HashMap和Hashtable提供的未被指定的、通常混乱的顺序中解救出来，而不会发生与TreeMap相关的成本增加.
    无论原始map的实现如何，它都可以被用来生成与原始map具有相同顺序的map副本
    如果模块对map进行输入，复制它，然后返回结果的顺序由副本决定，此技术是特别有用.
    客户通常喜欢按照它们添加的顺序来呈现结果（顺序一致）

    * <p>A special {@link #LinkedHashMap(int,float,boolean) constructor} is
    * provided to create a linked hash map whose order of iteration is the order
    * in which its entries were last accessed, from least-recently accessed to
    * most-recently (<i>access-order</i>).  This kind of map is well-suited to
    * building LRU caches.  Invoking the {@code put}, {@code putIfAbsent},
    * {@code get}, {@code getOrDefault}, {@code compute}, {@code computeIfAbsent},
    * {@code computeIfPresent}, or {@code merge} methods results
    * in an access to the corresponding entry (assuming it exists after the
    * invocation completes). The {@code replace} methods only result in an access
    * of the entry if the value is replaced.  The {@code putAll} method generates one
    * entry access for each mapping in the specified map, in the order that
    * key-value mappings are provided by the specified map's entry set iterator.
    * <i>No other methods generate entry accesses.</i>  In particular, operations
    * on collection-views do <i>not</i> affect the order of iteration of the
    * backing map.

    提供构造器来创建LinkedHashMap对象，其迭代顺序是它的节点被最后访问的顺序，从最少访问到最多访问.
    这种map非常适合构建LRU缓存.调用put、putIfAbsent、get、getOrDefault、compute、computeIfAbsent、computeIfPresent、merge方法导致对应节点的访问.
    如果值被替换了，replace方法才算是导致了节点的访问.
    putAll为指定map中的每一个键值对生成一次节点访问，按照指定map中提供的键值对顺序来设置迭代器.
    其他的方法不会生成节点访问.特别是，在集合视图上的操作不会影响map的迭代顺序.

    * <p>The {@link #removeEldestEntry(Map.Entry)} method may be overridden to
    * impose a policy for removing stale mappings automatically when new mappings
    * are added to the map.

    removeEldestEntry方法可能被覆写用来实施当新的键值对被添加到map时会自动移除过时的键值对的策略（通过覆写removeEldestEntry可以实现自定义的LRU缓存策略）

    * <p>This class provides all of the optional <tt>Map</tt> operations, and
    * permits null elements.  Like <tt>HashMap</tt>, it provides constant-time
    * performance for the basic operations (<tt>add</tt>, <tt>contains</tt> and
    * <tt>remove</tt>), assuming the hash function disperses elements
    * properly among the buckets.  Performance is likely to be just slightly
    * below that of <tt>HashMap</tt>, due to the added expense of maintaining the
    * linked list, with one exception: Iteration over the collection-views
    * of a <tt>LinkedHashMap</tt> requires time proportional to the <i>size</i>
    * of the map, regardless of its capacity.  Iteration over a <tt>HashMap</tt>
    * is likely to be more expensive, requiring time proportional to its
    * <i>capacity</i>.

    该类提供了map的所有可选操作，允许空节点.
    与HashMap一样，假定哈希函数适当地分散桶中的元素，那对于基本的操作它消耗了恒定的时间.
    性能可能会略低于HashMap，因为额外的维护了一个链表，有一个例外：LinkedHashMap的迭代所需的时间与哈希表中节点的数量成比例，而不管它的容量.
    在HashMap上进行迭代可能会更昂贵，需要的时间与其容量成比例.

    * <p>A linked hash map has two parameters that affect its performance:
    * <i>initial capacity</i> and <i>load factor</i>.  They are defined precisely
    * as for <tt>HashMap</tt>.  Note, however, that the penalty for choosing an
    * excessively high value for initial capacity is less severe for this class
    * than for <tt>HashMap</tt>, as iteration times for this class are unaffected
    * by capacity.

    LinkedHashMap有两个参数会影响它的性能：初始容量与加载因子.它们的定义与在HashMap中一样.
    但是请注意，与HashMap相比，此类对于初始容量选择过高的惩罚并不严重，因为它的迭代时间并不会被容量所影响.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a linked hash map concurrently, and at least
    * one of the threads modifies the map structurally, it <em>must</em> be
    * synchronized externally.  This is typically accomplished by
    * synchronizing on some object that naturally encapsulates the map.

    该实现是非线程安全.
    如果多线程同时访问同一个LinkedHashMap，至少一个线程会修改LinkedHashMap结构，它必须在外部保持同步.
    一般通过包装LinkedHashMap的对象来完成同步.
    
    * If no such object exists, the map should be "wrapped" using the
    * {@link Collections#synchronizedMap Collections.synchronizedMap}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the map:<pre>
    *   Map m = Collections.synchronizedMap(new LinkedHashMap(...));</pre>
    
    如果外部对象不存在，应该使用Collections.synchronizedMap来包裹LinkedHashMap对象.
    该方法最好在LinkedHashMap创建时调用，为了防止偶然性的非线程安全访问LinkedHashMap.

    * A structural modification is any operation that adds or deletes one or more
    * mappings or, in the case of access-ordered linked hash maps, affects
    * iteration order.  In insertion-ordered linked hash maps, merely changing
    * the value associated with a key that is already contained in the map is not
    * a structural modification.  <strong>In access-ordered linked hash maps,
    * merely querying the map with <tt>get</tt> is a structural modification.
    * </strong>)

    结构修改指的是添加或删除一个或多个键值对的任何一个操作，对于按照访问顺序的LinkedHashMap来说，会影响它的迭代顺序.
    对于按照插入顺序的LinkedHashMap来说，仅仅改变对象已存在的键所对应的值不属于结构修改.
    对于按照访问顺序的LinkedHashMap来说，仅仅使用get来查询表是一种结构修改.

    * <p>The iterators returned by the <tt>iterator</tt> method of the collections
    * returned by all of this class's collection view methods are
    * <em>fail-fast</em>: if the map is structurally modified at any time after
    * the iterator is created, in any way except through the iterator's own
    * <tt>remove</tt> method, the iterator will throw a {@link
    * ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than risking
    * arbitrary, non-deterministic behavior at an undetermined time in the future.
    
    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw <tt>ConcurrentModificationException</tt> on a best-effort basis.
    * Therefore, it would be wrong to write a program that depended on this
    * exception for its correctness:   <i>the fail-fast behavior of iterators
    * should be used only to detect bugs.</i>

    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug

    * <p>The spliterators returned by the spliterator method of the collections
    * returned by all of this class's collection view methods are
    * <em><a href="Spliterator.html#binding">late-binding</a></em>,
    * <em>fail-fast</em>, and additionally report {@link Spliterator#ORDERED}.
    
    可分割迭代器属于后期绑定、快速失败、有序.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.
    
    该类是Java Collections Framework的一员.

```

```java

   /*
    * Implementation note.  A previous version of this class was
    * internally structured a little differently. Because superclass
    * HashMap now uses trees for some of its nodes, class
    * LinkedHashMap.Entry is now treated as intermediary node class
    * that can also be converted to tree form. The name of this
    * class, LinkedHashMap.Entry, is confusing in several ways in its
    * current context, but cannot be changed.  Otherwise, even though
    * it is not exported outside this package, some existing source
    * code is known to have relied on a symbol resolution corner case
    * rule in calls to removeEldestEntry that suppressed compilation
    * errors due to ambiguous usages. So, we keep the name to
    * preserve unmodified compilability.
    
    实施说明.该类的先前版本在内部结构上有所不同.因为父类HashMap现在在一些节点上使用了树，LinkedHashMap.Entry现在被当作中间节点，也可以被转换为树形.
    LinkedHashMap.Entry在当前上下文中的多种形式是令人困惑，但是无法更改.
    否则，即使未将其导出到包之外，也已知某些现有源代码在对removeEldestEntry的调用中依赖符号解析的特殊情况规则，该规则抑制了由于用法不明确引起的编译错误.
    因此，我们保留名称以维持未修改的可编译性.

   /*
    * The changes in node classes also require using two fields
    * (head, tail) rather than a pointer to a header node to maintain
    * the doubly-linked before/after list. This class also
    * previously used a different style of callback methods upon
    * access, insertion, and removal.
    *

    节点类的更改需要使用两个字段，而不是指向头节点以维护双向链表的前后列表
    此类在访问、插入、移除时也曾使用过不同风格的回调方法.


```