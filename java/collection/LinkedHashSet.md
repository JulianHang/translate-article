### 翻译LinkedHashSet注释

```java

   /**
    * <p>Hash table and linked list implementation of the <tt>Set</tt> interface,
    * with predictable iteration order.  This implementation differs from
    * <tt>HashSet</tt> in that it maintains a doubly-linked list running through
    * all of its entries.  This linked list defines the iteration ordering,
    * which is the order in which elements were inserted into the set
    * (<i>insertion-order</i>).  Note that insertion order is <i>not</i> affected
    * if an element is <i>re-inserted</i> into the set.  (An element <tt>e</tt>
    * is reinserted into a set <tt>s</tt> if <tt>s.add(e)</tt> is invoked when
    * <tt>s.contains(e)</tt> would return <tt>true</tt> immediately prior to
    * the invocation.)

    基于哈希表与链表的Set接口实现, 可预测的迭代顺序.
    此实现与HashSet的不同之处在于，它维护了一个遍历所有节点的双向链表.
    此链表定义了迭代顺序，该顺序通常是键插入.
    注意，如果键重复插入到表中，则插入顺序不会受到影响.

    * <p>This implementation spares its clients from the unspecified, generally
    * chaotic ordering provided by {@link HashSet}, without incurring the
    * increased cost associated with {@link TreeSet}.  It can be used to
    * produce a copy of a set that has the same order as the original, regardless
    * of the original set's implementation:
    * <pre>
    *     void foo(Set s) {
    *         Set copy = new LinkedHashSet(s);
    *         ...
    *     }
    * </pre>
    * This technique is particularly useful if a module takes a set on input,
    * copies it, and later returns results whose order is determined by that of
    * the copy.  (Clients generally appreciate having things returned in the same
    * order they were presented.)
    
    此实现将它的客户从HashSet提供的未被指定的、通常混乱的顺序中解救出来，而不会发生与TreeSet相关的成本增加.
    无论原始set的实现如何，它都可以被用来生成与原始set具有相同顺序的set副本
    如果模块对set进行输入，复制它，然后返回结果的顺序由副本决定，此技术是特别有用.
    客户通常喜欢按照它们添加的顺序来呈现结果（顺序一致）

    * <p>This class provides all of the optional <tt>Set</tt> operations, and
    * permits null elements.  Like <tt>HashSet</tt>, it provides constant-time
    * performance for the basic operations (<tt>add</tt>, <tt>contains</tt> and
    * <tt>remove</tt>), assuming the hash function disperses elements
    * properly among the buckets.  Performance is likely to be just slightly
    * below that of <tt>HashSet</tt>, due to the added expense of maintaining the
    * linked list, with one exception: Iteration over a <tt>LinkedHashSet</tt>
    * requires time proportional to the <i>size</i> of the set, regardless of
    * its capacity.  Iteration over a <tt>HashSet</tt> is likely to be more
    * expensive, requiring time proportional to its <i>capacity</i>.
    
    该类提供了set的所有可选操作，允许空节点.
    与HashSet一样，假定哈希函数适当地分散桶中的元素，那对于基本的操作它消耗了恒定的时间.
    性能可能会略低于HashSet，因为额外的维护了一个链表，有一个例外：LinkedHashSet的迭代所需的时间与哈希表中节点的数量成比例，而不管它的容量.
    在HashSet上进行迭代可能会更昂贵，需要的时间与其容量成比例.

    * <p>A linked hash set has two parameters that affect its performance:
    * <i>initial capacity</i> and <i>load factor</i>.  They are defined precisely
    * as for <tt>HashSet</tt>.  Note, however, that the penalty for choosing an
    * excessively high value for initial capacity is less severe for this class
    * than for <tt>HashSet</tt>, as iteration times for this class are unaffected
    * by capacity.
    
    LinkedHashSet有两个参数会影响它的性能：初始容量与加载因子.它们的定义与在HashSet中一样.
    但是请注意，与HashSet相比，此类对于初始容量选择过高的惩罚并不严重，因为它的迭代时间并不会被容量所影响.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a linked hash set concurrently, and at least
    * one of the threads modifies the set, it <em>must</em> be synchronized
    * externally.  This is typically accomplished by synchronizing on some
    * object that naturally encapsulates the set.
    
    该实现是非线程安全.
    如果多线程同时访问同一个LinkedHashSet，至少一个线程会修改LinkedHashSet结构，它必须在外部保持同步.
    一般通过包装LinkedHashSet的对象来完成同步.

    * If no such object exists, the set should be "wrapped" using the
    * {@link Collections#synchronizedSet Collections.synchronizedSet}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the set: <pre>
    *   Set s = Collections.synchronizedSet(new LinkedHashSet(...));</pre>
    
    如果外部对象不存在，应该使用Collections.synchronizedSet来包裹LinkedHashSet对象.
    该方法最好在LinkedHashSet创建时调用，为了防止偶然性的非线程安全访问LinkedHashSet.

    * <p>The iterators returned by this class's <tt>iterator</tt> method are
    * <em>fail-fast</em>: if the set is modified at any time after the iterator
    * is created, in any way except through the iterator's own <tt>remove</tt>
    * method, the iterator will throw a {@link ConcurrentModificationException}.
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
    * exception for its correctness:   <i>the fail-fast behavior of iterators
    * should be used only to detect bugs.</i>
    
    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```