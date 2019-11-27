### 翻译LinkedList注释

```java

    /**
    * Doubly-linked list implementation of the {@code List} and {@code Deque}
    * interfaces.  Implements all optional list operations, and permits all
    * elements (including {@code null}).

    双向链表实现了List与Deque接口.实现List接口的所有可选操作，允许存放所有元素，包括null.

    * <p>All of the operations perform as could be expected for a doubly-linked
    * list.  Operations that index into the list will traverse the list from
    * the beginning or the end, whichever is closer to the specified index.

    对于双向链表，所有的操作都可以像预期那样执行.列表中索引的操作将会从头部或尾部遍历列表，直到接近指定索引.

    * <p><strong>Note that this implementation is not synchronized.</strong>
    * If multiple threads access a linked list concurrently, and at least
    * one of the threads modifies the list structurally, it <i>must</i> be
    * synchronized externally.  (A structural modification is any operation
    * that adds or deletes one or more elements; merely setting the value of
    * an element is not a structural modification.)  This is typically
    * accomplished by synchronizing on some object that naturally
    * encapsulates the list.

    注意该实现不是线程安全.
    如果多个线程同时访问一个LinkedList对象，至少会有一个线程会修改列表结构，所以它必须在外部控制线程安全.
    结构修改是指添加或删除一个或多个元素，不过设置元素的值不属于结构修改.
    一般通过包装list的对象来完成线程安全.

    * If no such object exists, the list should be "wrapped" using the
    * {@link Collections#synchronizedList Collections.synchronizedList}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access to the list:<pre>
    *   List list = Collections.synchronizedList(new LinkedList(...));</pre>

    如果外部对象不存在，应该使用Collections.synchronizedList来包裹LinkedList对象.
    该方法最好在LinkedList创建时调用，为了防止偶然性的非线程安全访问LinkedList.

    * <p>The iterators returned by this class's {@code iterator} and
    * {@code listIterator} methods are <i>fail-fast</i>: if the list is
    * structurally modified at any time after the iterator is created, in
    * any way except through the Iterator's own {@code remove} or
    * {@code add} methods, the iterator will throw a {@link
    * ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than
    * risking arbitrary, non-deterministic behavior at an undetermined
    * time in the future.

    iterator、listIterator方法返回iterators（迭代器）
    迭代器被称为fail-fast（快速失败）：当迭代器被创建后的任何时间内修改结构，除了自身add或remove方法外（iterator#add）的任何方式，迭代器将会抛出ConcurrentModificationException异常（在获取迭代器后，只能通过iterator#add或iterator#remove操作元素，要是用LinkedList#remove移除元素后，在接着使用迭代器则会抛出错误）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

      <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
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
为啥注释都和ArrayList差不多呢，直接拷贝了！