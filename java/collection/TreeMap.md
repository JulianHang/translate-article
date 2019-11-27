### 翻译TreeMap注释

```java

   /**
    * A Red-Black tree based {@link NavigableMap} implementation.
    * The map is sorted according to the {@linkplain Comparable natural
    * ordering} of its keys, or by a {@link Comparator} provided at map
    * creation time, depending on which constructor is used.
    
    基于红黑树的NavigableMap实现.它的键是按照自然顺序进行排序，或者在创建对象时提供Comparator进行排序，具体取决于所使用的构造函数.

    * <p>This implementation provides guaranteed log(n) time cost for the
    * {@code containsKey}, {@code get}, {@code put} and {@code remove}
    * operations.  Algorithms are adaptations of those in Cormen, Leiserson, and
    * Rivest's <em>Introduction to Algorithms</em>.

    该实现为containsKey、get、put、remove操作提供了保证的log(n)时间成本.
    算法是对Cormen，Leiserson和Rivest算法简介中的改编.

    * <p>Note that the ordering maintained by a tree map, like any sorted map, and
    * whether or not an explicit comparator is provided, must be <em>consistent
    * with {@code equals}</em> if this sorted map is to correctly implement the
    * {@code Map} interface.  (See {@code Comparable} or {@code Comparator} for a
    * precise definition of <em>consistent with equals</em>.)  This is so because
    * the {@code Map} interface is defined in terms of the {@code equals}
    * operation, but a sorted map performs all key comparisons using its {@code
    * compareTo} (or {@code compare}) method, so two keys that are deemed equal by
    * this method are, from the standpoint of the sorted map, equal.  The behavior
    * of a sorted map <em>is</em> well-defined even if its ordering is
    * inconsistent with {@code equals}; it just fails to obey the general contract
    * of the {@code Map} interface.
    
    请注意，如果有序map正确实现Map接口，则它所维护的顺序（与任何排序的映射一样）以及是否提供显式比较器必须与equals一致.(有关equals一致性的精确定义，请参见Comparable或Comparator)
    之所以如此，是因为Map接口是根据equals操作定义的，但是有序map是使用它的compareTo或compare方法来执行所有键的比较，因此从有序map的角度来看，该方法相等则认为两个键相等.
    即使它的有序性与equals是不一致的，有序map的行为也是明确定义的.它只是不遵守map接口的一般规定.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a map concurrently, and at least one of the
    * threads modifies the map structurally, it <em>must</em> be synchronized
    * externally.  (A structural modification is any operation that adds or
    * deletes one or more mappings; merely changing the value associated
    * with an existing key is not a structural modification.)  This is
    * typically accomplished by synchronizing on some object that naturally
    * encapsulates the map.
    * If no such object exists, the map should be "wrapped" using the
    * {@link Collections#synchronizedSortedMap Collections.synchronizedSortedMap}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the map: <pre>
    *   SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));</pre>

    该实现是非线程安全.
    如果多线程同时访问同一个TreeMap，至少一个线程会修改TreehMap结构，它必须在外部保持同步.
    (结构修改指的是添加或删除一个或多个键值对的任何一个操作;仅仅改变对象已存在的键所对应的值不属于结构修改)
    一般通过包装TreeMap的对象来完成同步.
    如果外部对象不存在，应该使用Collections.synchronizedSortedMap来包裹TreeMap对象.
    该方法最好在TreeMap创建时调用，为了防止偶然性的非线程安全访问TreeMap.

    * <p>The iterators returned by the {@code iterator} method of the collections
    * returned by all of this class's "collection view methods" are
    * <em>fail-fast</em>: if the map is structurally modified at any time after
    * the iterator is created, in any way except through the iterator's own
    * {@code remove} method, the iterator will throw a {@link
    * ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than risking
    * arbitrary, non-deterministic behavior at an undetermined time in the future.
    
    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常（该说法很像ArrayList）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw {@code ConcurrentModificationException} on a best-effort basis.
    * Therefore, it would be wrong to write a program that depended on this
    * exception for its correctness:   <em>the fail-fast behavior of iterators
    * should be used only to detect bugs.</em>

    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug

    * <p>All {@code Map.Entry} pairs returned by methods in this class
    * and its views represent snapshots of mappings at the time they were
    * produced. They do <strong>not</strong> support the {@code Entry.setValue}
    * method. (Note however that it is possible to change mappings in the
    * associated map using {@code put}.)
    
    此类中的方法返回的所有键值对及视图均表示生成映射时的快照.
    它们不支持setValue方法.但是可以使用put来修改映射关系.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```