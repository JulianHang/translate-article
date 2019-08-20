### 翻译Vector注释

```java

   /**
    * The {@code Vector} class implements a growable array of
    * objects. Like an array, it contains components that can be
    * accessed using an integer index. However, the size of a
    * {@code Vector} can grow or shrink as needed to accommodate
    * adding and removing items after the {@code Vector} has been created.

    Vector实现了一个可增长的对象数组.
    像数组一样，使用索引来访问它包含的元素.
    但是，Vector的容量大小可以根据需要增大或缩小，以便在创建Vector对象后调节添加和删除元素.

    * <p>Each vector tries to optimize storage management by maintaining a
    * {@code capacity} and a {@code capacityIncrement}. The
    * {@code capacity} is always at least as large as the vector
    * size; it is usually larger because as components are added to the
    * vector, the vector's storage increases in chunks the size of
    * {@code capacityIncrement}. An application can increase the
    * capacity of a vector before inserting a large number of
    * components; this reduces the amount of incremental reallocation.

    Vector通过维护capacity（容量）和capacityIncrement（容量增大速率）来尽可能地完善存储机制.
    capacity始终与Vector容量一样大（capacity就是Vector的容量大小）
    当元素被添加到Vector时，它（capacity）通常更大，Vector的容量增长速度以capacityIncrement大小为准.
    在添加许多元素之前，Vector的容量大小会被增大（判断是否需要扩容）.
    它减少了增量在分配的次数（一次性增加更大的容量，减少了多次增加相对小的容量，提高了效率，不过有可能浪费资源）.

    * <p><a name="fail-fast">
    * The iterators returned by this class's {@link #iterator() iterator} and
    * {@link #listIterator(int) listIterator} methods are <em>fail-fast</em></a>:
    * if the vector is structurally modified at any time after the iterator is
    * created, in any way except through the iterator's own
    * {@link ListIterator#remove() remove} or
    * {@link ListIterator#add(Object) add} methods, the iterator will throw a
    * {@link ConcurrentModificationException}.  Thus, in the face of
    * concurrent modification, the iterator fails quickly and cleanly, rather
    * than risking arbitrary, non-deterministic behavior at an undetermined
    * time in the future.  The {@link Enumeration Enumerations} returned by
    * the {@link #elements() elements} method are <em>not</em> fail-fast.

    iterator、listIterator方法返回iterators（迭代器）
    迭代器被称为fail-fast（快速失败）：当迭代器被创建后的任何时间内修改结构，除了自身add或remove方法外（itertor#add）的任何方式，迭代器将会抛出ConcurrentModificationException异常（在获取迭代器后，只能通过iterator#add或iterator#remove操作元素，要是用Vector#remove移除元素后，在接着使用迭代器则会抛出错误）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.
    通过elements方法返回Enumerations不属于快速失败.

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

    * <p>As of the Java 2 platform v1.2, this class was retrofitted to
    * implement the {@link List} interface, making it a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.  Unlike the new collection
    * implementations, {@code Vector} is synchronized.  If a thread-safe
    * implementation is not needed, it is recommended to use {@link
    * ArrayList} in place of {@code Vector}.

    从Java2平台v1.2开始，Vector被改成了实现List接口，使它称为Java Collections Framework的一员.
    与新的集合实现不同，Vector属于线程安全.
    如果线程安全是不需要的，推荐使用ArrayList代替Vector.

```