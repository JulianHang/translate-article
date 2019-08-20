### 翻译ArrayList类注释

```java

   /**
    * Resizable-array implementation of the <tt>List</tt> interface.  Implements
    * all optional list operations, and permits all elements, including
    * <tt>null</tt>.  In addition to implementing the <tt>List</tt> interface,
    * this class provides methods to manipulate the size of the array that is
    * used internally to store the list.  (This class is roughly equivalent to
    * <tt>Vector</tt>, except that it is unsynchronized.)

    实现List接口的可变数组类.实现List接口的所有可选操作，允许存放所有元素，包括null.
    除了实现List接口外，该类还提供了操作数组大小的方法，内部实现通过数组来存储列表。该类大体上和Vector相似，除了它是线程不安全之外.

    * <p>The <tt>size</tt>, <tt>isEmpty</tt>, <tt>get</tt>, <tt>set</tt>,
    * <tt>iterator</tt>, and <tt>listIterator</tt> operations run in constant
    * time.  The <tt>add</tt> operation runs in <i>amortized constant time</i>,
    * that is, adding n elements requires O(n) time.  All of the other operations
    * run in linear time (roughly speaking).  The constant factor is low compared
    * to that for the <tt>LinkedList</tt> implementation.

    size、isEmpty、get、set、iterator、listIterator操作的时间复杂度是个常量O(1).
    add操作的时间复杂度是O(n).
    其他操作的时间复杂度呈现线性.
    恒定因子，也就是上面提到的常量比LinkedList类还低（说明修改/获取效率比LinkedList高）.

    * <p>Each <tt>ArrayList</tt> instance has a <i>capacity</i>.  The capacity is
    * the size of the array used to store the elements in the list.  It is always
    * at least as large as the list size.  As elements are added to an ArrayList,
    * its capacity grows automatically.  The details of the growth policy are not
    * specified beyond the fact that adding an element has constant amortized
    * time cost.
    
    每个ArrayList对象都有一个容量.
    该容量是用来存储元素的数组大小.
    它始终和列表大小一样大.
    当有元素被添加到ArrayList时，它的容量会自动增加.
    除了添加元素会花费时间成本这个事实外，并没有具体说明增长策略的详情.

    * <p>An application can increase the capacity of an <tt>ArrayList</tt> instance
    * before adding a large number of elements using the <tt>ensureCapacity</tt>
    * operation.  This may reduce the amount of incremental reallocation.

    应用可以在添加许多元素之前通过调用ensureCapacity方法来增加ArrayList对象的容量.
    它可能会减少增量再分配的次数（一次性增加更大的容量，减少了多次增加相对小的容量，提高了效率，不过有可能浪费资源）

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access an <tt>ArrayList</tt> instance concurrently,
    * and at least one of the threads modifies the list structurally, it
    * <i>must</i> be synchronized externally.  (A structural modification is
    * any operation that adds or deletes one or more elements, or explicitly
    * resizes the backing array; merely setting the value of an element is not
    * a structural modification.)  This is typically accomplished by
    * synchronizing on some object that naturally encapsulates the list.

    注意该实现不是线程安全.
    如果多个线程同时访问一个ArrayList对象，至少会有一个线程会修改列表结构，所以它必须在外部控制线程安全.
    结构修改是指添加或删除一个或多个元素，或者明确的调整数组大小，不过设置元素的值不属于结构修改.
    一般通过包装list的对象来完成线程安全.

    * If no such object exists, the list should be "wrapped" using the
    * {@link Collections#synchronizedList Collections.synchronizedList}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the list:<pre>
    *   List list = Collections.synchronizedList(new ArrayList(...));</pre>

    如果外部对象不存在，应该使用Collections.synchronizedList来包裹ArrayList对象.
    该方法最好在ArrayList创建时调用，为了防止偶然性的非线程安全访问ArrayList

    * <p><a name="fail-fast">
    * The iterators returned by this class's {@link #iterator() iterator} and
    * {@link #listIterator(int) listIterator} methods are <em>fail-fast</em>:</a>
    * if the list is structurally modified at any time after the iterator is
    * created, in any way except through the iterator's own
    * {@link ListIterator#remove() remove} or
    * {@link ListIterator#add(Object) add} methods, the iterator will throw a
    * {@link ConcurrentModificationException}.  Thus, in the face of
    * concurrent modification, the iterator fails quickly and cleanly, rather
    * than risking arbitrary, non-deterministic behavior at an undetermined
    * time in the future.

    iterator、listIterator方法返回iterators（迭代器）
    迭代器被称为fail-fast（快速失败）：当迭代器被创建后的任何时间内修改结构，除了自身add或remove方法外（itertor#add）的任何方式，迭代器将会抛出ConcurrentModificationException异常（在获取迭代器后，只能通过iterator#add或iterator#remove操作元素，要是用ArrayList#remove移除元素后，在接着使用迭代器则会抛出错误）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw {@code ConcurrentModificationException} on a best-effort basis.
    * Therefore, it would be wrong to write a program that depended on this
    * exception for its correctness:  <i>the fail-fast behavior of iterators
    * should be used only to detect bugs.</i>

    注意iterator（迭代器）无法保证快速失败，通俗来说，在非同步的并发修改的场景下无法做确定的担保.
    迭代器会尽可能的抛出ConcurrentModificationException.
    因此，编写一个依赖于此异常的程序是错误的，对于它的正确性应该是：迭代器的快速失败行为仅用于检测bug

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```