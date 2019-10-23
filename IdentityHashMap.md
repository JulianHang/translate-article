### 翻译IdentityHashMap注释

```java

   /**
    * This class implements the <tt>Map</tt> interface with a hash table, using
    * reference-equality in place of object-equality when comparing keys (and
    * values).  In other words, in an <tt>IdentityHashMap</tt>, two keys
    * <tt>k1</tt> and <tt>k2</tt> are considered equal if and only if
    * <tt>(k1==k2)</tt>.  (In normal <tt>Map</tt> implementations (like
    * <tt>HashMap</tt>) two keys <tt>k1</tt> and <tt>k2</tt> are considered equal
    * if and only if <tt>(k1==null ? k2==null : k1.equals(k2))</tt>.)

    该类使用哈希表实现了Map接口，当比较key时使用"引用相等"代替"对象相等".
    换句话说，当且仅当k1==k2时，两个键k1与k2才被认为是相等的.
    而在HashMap中，当且仅当k1.equals(k2) == true时，两个键k1与k2才被认为是相等的.

    * <p><b>This class is <i>not</i> a general-purpose <tt>Map</tt>
    * implementation!  While this class implements the <tt>Map</tt> interface, it
    * intentionally violates <tt>Map's</tt> general contract, which mandates the
    * use of the <tt>equals</tt> method when comparing objects.  This class is
    * designed for use only in the rare cases wherein reference-equality
    * semantics are required.</b>

    此类不是通用的Map实现.尽管此类实现了Map接口，但它有意违反Map的通用协定，该协定要求在比较对象时使用equals方法.
    此类仅设计用于在少数情况下需要引用相等的情况.

    * <p>A typical use of this class is <i>topology-preserving object graph
    * transformations</i>, such as serialization or deep-copying.  To perform such
    * a transformation, a program must maintain a "node table" that keeps track
    * of all the object references that have already been processed.  The node
    * table must not equate distinct objects even if they happen to be equal.
    * Another typical use of this class is to maintain <i>proxy objects</i>.  For
    * example, a debugging facility might wish to maintain a proxy object for
    * each object in the program being debugged.

    该类的典型用法是保留拓扑对象图转换，例如序列化或深拷贝.为了执行转换，程序必须维护一个节点表来跟踪已经处理的所有对象的引用.
    节点表不能等于不同的对象，即使它们恰好相等.
    该类的另外一个典型用法是维护一个代理对象.例如，调试工具可能希望为要调试的程序的每个对象维护一个代理.

    * <p>This class provides all of the optional map operations, and permits
    * <tt>null</tt> values and the <tt>null</tt> key.  This class makes no
    * guarantees as to the order of the map; in particular, it does not guarantee
    * that the order will remain constant over time.

    该类提供了map的所有可选操作，key与value允许存放null.该类无法保证map的顺序；特别是，它不能保证随着时间的推移顺序保持不变.

    * <p>This class provides constant-time performance for the basic
    * operations (<tt>get</tt> and <tt>put</tt>), assuming the system
    * identity hash function ({@link System#identityHashCode(Object)})
    * disperses elements properly among the buckets.

    假定身份哈希函数适当地分散桶中的元素,那么该类对于执行基本的操作（get、put）会消耗恒定的时间
    
    * <p>This class has one tuning parameter (which affects performance but not
    * semantics): <i>expected maximum size</i>.  This parameter is the maximum
    * number of key-value mappings that the map is expected to hold.  Internally,
    * this parameter is used to determine the number of buckets initially
    * comprising the hash table.  The precise relationship between the expected
    * maximum size and the number of buckets is unspecified.

    此类具有一个调整参数（这会影响性能，但不会影响语义）：预期的最大容量.这个参数是map能存储键值对的最大数目.
    在内部，该参数用于决定最初组成哈希表时桶的数量.预期的最大容量与桶的数量之间的关系未被指定

    * <p>If the size of the map (the number of key-value mappings) sufficiently
    * exceeds the expected maximum size, the number of buckets is increased.
    * Increasing the number of buckets ("rehashing") may be fairly expensive, so
    * it pays to create identity hash maps with a sufficiently large expected
    * maximum size.  On the other hand, iteration over collection views requires
    * time proportional to the number of buckets in the hash table, so it
    * pays not to set the expected maximum size too high if you are especially
    * concerned with iteration performance or memory usage.
    
    倘若map的容量远超过预期的最大容量，桶的数量会增大.增加桶的数量所消耗的成本很高，因此它需要创建具有足够大的预期容量的map.
    另一方面，集合视图上的迭代所需的时间与哈希表中桶的数量成比例，因此如果你是特别关注迭代性能或内存使用，则不必将预期的最大容量设置的过高.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access an identity hash map concurrently, and at
    * least one of the threads modifies the map structurally, it <i>must</i>
    * be synchronized externally.  (A structural modification is any operation
    * that adds or deletes one or more mappings; merely changing the value
    * associated with a key that an instance already contains is not a
    * structural modification.)  This is typically accomplished by
    * synchronizing on some object that naturally encapsulates the map.

    注意该实现是非线程安全.
    如果多线程同时访问同一个IdentityHashMap，至少一个线程会修改IdentityHashMap结构，它必须在外部保持同步.(结构修改指的是添加或删除一个或多个键值对的任何一个操作;仅仅改变对象已存在的键所对应的值不属于结构修改)
    一般通过包装IdentityHashMap的对象来完成同步.

    * If no such object exists, the map should be "wrapped" using the
    * {@link Collections#synchronizedMap Collections.synchronizedMap}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the map:<pre>
    *   Map m = Collections.synchronizedMap(new IdentityHashMap(...));</pre>

    如果外部对象不存在，应该使用Collections.synchronizedMap来包裹IdentityHashMap对象.
    该方法最好在IdentityHashMap创建时调用，为了防止偶然性的非线程安全访问IdentityHashMap.

    * <p>The iterators returned by the <tt>iterator</tt> method of the
    * collections returned by all of this class's "collection view
    * methods" are <i>fail-fast</i>: if the map is structurally modified
    * at any time after the iterator is created, in any way except
    * through the iterator's own <tt>remove</tt> method, the iterator
    * will throw a {@link ConcurrentModificationException}.  Thus, in the
    * face of concurrent modification, the iterator fails quickly and
    * cleanly, rather than risking arbitrary, non-deterministic behavior
    * at an undetermined time in the future.

    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.
    
    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw <tt>ConcurrentModificationException</tt> on a best-effort basis.
    * Therefore, it would be wrong to write a program that depended on this
    * exception for its correctness: <i>fail-fast iterators should be used only
    * to detect bugs.</i>

    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug
    
    * <p>Implementation note: This is a simple <i>linear-probe</i> hash table,
    * as described for example in texts by Sedgewick and Knuth.  The array
    * alternates holding keys and values.  (This has better locality for large
    * tables than does using separate arrays.)  For many JRE implementations
    * and operation mixes, this class will yield better performance than
    * {@link HashMap} (which uses <i>chaining</i> rather than linear-probing).
    
    实施说明：这是一个简单的线性探针哈希表，如Sedgewick和Knuth在文本中所描述的。
    数组轮流存储键与值（与使用单独的数组相比，对于大型表具有更好的局部性）
    对于许多JRE实现和操作组合，该类将产生比HashMap更好的性能（HashMap使用链表而不是线性探针）

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```