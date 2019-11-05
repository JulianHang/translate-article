### 翻译Hashtable注释

```java

   /**
    * This class implements a hash table, which maps keys to values. Any
    * non-<code>null</code> object can be used as a key or as a value. <p>
    *

    该类实现了哈希表，哈希表关联键值对. 键值对不允许null.

    * To successfully store and retrieve objects from a hashtable, the
    * objects used as keys must implement the <code>hashCode</code>
    * method and the <code>equals</code> method. <p>

    为了成功地在哈希表中存储和获取对象, 使用对象作为键必须实现hashCode与equals方法.
    
    * An instance of <code>Hashtable</code> has two parameters that affect its
    * performance: <i>initial capacity</i> and <i>load factor</i>.  The
    * <i>capacity</i> is the number of <i>buckets</i> in the hash table, and the
    * <i>initial capacity</i> is simply the capacity at the time the hash table
    * is created.  Note that the hash table is <i>open</i>: in the case of a "hash
    * collision", a single bucket stores multiple entries, which must be searched
    * sequentially.  The <i>load factor</i> is a measure of how full the hash
    * table is allowed to get before its capacity is automatically increased.
    * The initial capacity and load factor parameters are merely hints to
    * the implementation.  The exact details as to when and whether the rehash
    * method is invoked are implementation-dependent.<p>
    
    Hashtable对象有两个参数会影响它的性能：初始容量与加载因子.
    初始容量指的是在哈希表中桶的数量，初始容量就是创建哈希表时的容量.
    哈希表是开阔的：如是哈希碰撞，单个桶存储多个节点，必须按顺序搜索.
    加载因子是一种在容量自动扩容前，允许哈希表获取填满程度的措施（哈希表中元素填满的程度，比如填满了75%）
    初始容量与加载因子参数仅是实现的提示.
    有关何时及是否调用重新散列方法的详细信息取决于实现.

    * Generally, the default load factor (.75) offers a good tradeoff between
    * time and space costs.  Higher values decrease the space overhead but
    * increase the time cost to look up an entry (which is reflected in most
    * <tt>Hashtable</tt> operations, including <tt>get</tt> and <tt>put</tt>).<p>
    
    通常，默认加载因子（0.75）提供了一个在时间与空间代价上的权衡.
    较高的加载因子会减少空间开销，但是增加了查找的成本（在Hashtable的大部分操作中都反映了这一点，包括get和put）.

    * The initial capacity controls a tradeoff between wasted space and the
    * need for <code>rehash</code> operations, which are time-consuming.
    * No <code>rehash</code> operations will <i>ever</i> occur if the initial
    * capacity is greater than the maximum number of entries the
    * <tt>Hashtable</tt> will contain divided by its load factor.  However,
    * setting the initial capacity too high can waste space.<p>
    
    初始容量控制了空间浪费与重新散列的需要，重新散列是耗时的.
    如果初始容量大于哈希表将包含的最大条目数除以其负载因子（阈值），则将不会进行任何重新散列操作  
    但是，初始容量过高会造成空间浪费

    * If many entries are to be made into a <code>Hashtable</code>,
    * creating it with a sufficiently large capacity may allow the
    * entries to be inserted more efficiently than letting it perform
    * automatic rehashing as needed to grow the table. <p>
    *
    * This example creates a hashtable of numbers. It uses the names of
    * the numbers as keys:
    * <pre>   {@code
    *   Hashtable<String, Integer> numbers
    *     = new Hashtable<String, Integer>();
    *   numbers.put("one", 1);
    *   numbers.put("two", 2);
    *   numbers.put("three", 3);}</pre>
    *
    * <p>To retrieve a number, use the following code:
    * <pre>   {@code
    *   Integer n = numbers.get("two");
    *   if (n != null) {
    *     System.out.println("two = " + n);
    *   }}</pre>
    
    假设很多键值对要存储在Hashtable中，相对于执行自动重新构建来扩充哈希表的容量来说，创建一个初始容量足够大的Hashtable能更高效地存储键值对.
    本示例创造了数字哈希表.

    * <p>The iterators returned by the <tt>iterator</tt> method of the collections
    * returned by all of this class's "collection view methods" are
    * <em>fail-fast</em>: if the Hashtable is structurally modified at any time
    * after the iterator is created, in any way except through the iterator's own
    * <tt>remove</tt> method, the iterator will throw a {@link
    * ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than risking
    * arbitrary, non-deterministic behavior at an undetermined time in the future.
    * The Enumerations returned by Hashtable's keys and elements methods are
    * <em>not</em> fail-fast.

    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常（该说法很像ArrayList）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.
    通过keys、elements方法返回Enumerations不属于快速失败.

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

    * <p>As of the Java 2 platform v1.2, this class was retrofitted to
    * implement the {@link Map} interface, making it a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    *
    * Java Collections Framework</a>.  Unlike the new collection
    * implementations, {@code Hashtable} is synchronized.  If a
    * thread-safe implementation is not needed, it is recommended to use
    * {@link HashMap} in place of {@code Hashtable}.  If a thread-safe
    * highly-concurrent implementation is desired, then it is recommended
    * to use {@link java.util.concurrent.ConcurrentHashMap} in place of
    * {@code Hashtable}.

    该类是Java Collections Framework的一员.
    与新的集合实现不同，Hashtable是线程安全.
    如果线程安全是不需要的，推荐使用HashMap代替Hashtable.
    如果高并发的线程安全，推荐使用ConcurrentHashMap代替Hashtable.

```