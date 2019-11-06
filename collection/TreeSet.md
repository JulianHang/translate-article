### 翻译TreeSet注释

```java

   /**
    * A {@link NavigableSet} implementation based on a {@link TreeMap}.
    * The elements are ordered using their {@linkplain Comparable natural
    * ordering}, or by a {@link Comparator} provided at set creation
    * time, depending on which constructor is used.

    基于TreeMap的NavigableSet实现类. 元素按照自然顺序排列或创建时提供比较器, 取决于使用哪个构造器.

    * <p>This implementation provides guaranteed log(n) time cost for the basic
    * operations ({@code add}, {@code remove} and {@code contains}).
    
    该实现为add、remove、contains操作提供了保证的log(n)时间成本.

    * <p>Note that the ordering maintained by a set (whether or not an explicit
    * comparator is provided) must be <i>consistent with equals</i> if it is to
    * correctly implement the {@code Set} interface.  (See {@code Comparable}
    * or {@code Comparator} for a precise definition of <i>consistent with
    * equals</i>.)  This is so because the {@code Set} interface is defined in
    * terms of the {@code equals} operation, but a {@code TreeSet} instance
    * performs all element comparisons using its {@code compareTo} (or
    * {@code compare}) method, so two elements that are deemed equal by this method
    * are, from the standpoint of the set, equal.  The behavior of a set
    * <i>is</i> well-defined even if its ordering is inconsistent with equals; it
    * just fails to obey the general contract of the {@code Set} interface.
    
    注意，如果它正确实现Set接口，则它所维护的顺序（不管是否提供比较器）必须与equals一致. (有关equals一致性的精确定义，请参见Comparable或Comparator)
    之所以如此，是因为Set接口是根据equals操作定义的，但是TreeMap是使用它的compareTo或compare方法来执行所有键的比较，因此从set的角度来看，该方法相等则认为两个键相等.
    即使它的有序性与equals是不一致的, set的行为也是明确定义的.它只是不遵守Set接口的一般规定.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a tree set concurrently, and at least one
    * of the threads modifies the set, it <i>must</i> be synchronized
    * externally.  This is typically accomplished by synchronizing on some
    * object that naturally encapsulates the set.
    * If no such object exists, the set should be "wrapped" using the
    * {@link Collections#synchronizedSortedSet Collections.synchronizedSortedSet}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the set: <pre>
    *   SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...));</pre>
    
    该实现是非线程安全.
    如果多线程同时访问同一个TreeMap，至少一个线程会修改TreehMap结构，它必须在外部保持同步.
    一般通过包装TreeMap的对象来完成同步.
    如果外部对象不存在，应该使用Collections.synchronizedSortedSet来包裹TreeSet对象.
    该方法最好在TreeSet创建时调用，为了防止偶然性的非线程安全访问TreeSet.

    * <p>The iterators returned by this class's {@code iterator} method are
    * <i>fail-fast</i>: if the set is modified at any time after the iterator is
    * created, in any way except through the iterator's own {@code remove}
    * method, the iterator will throw a {@link ConcurrentModificationException}.
    * Thus, in the face of concurrent modification, the iterator fails quickly
    * and cleanly, rather than risking arbitrary, non-deterministic behavior at
    * an undetermined time in the future.
    
    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常（该说法很像ArrayList）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw {@code ConcurrentModificationException} on a best-effort basis.
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