### 翻译PriorityBlockingQueue注释

```java

   /**
    * An unbounded {@linkplain BlockingQueue blocking queue} that uses
    * the same ordering rules as class {@link PriorityQueue} and supplies
    * blocking retrieval operations.  While this queue is logically
    * unbounded, attempted additions may fail due to resource exhaustion
    * (causing {@code OutOfMemoryError}). This class does not permit
    * {@code null} elements.  A priority queue relying on {@linkplain
    * Comparable natural ordering} also does not permit insertion of
    * non-comparable objects (doing so results in
    * {@code ClassCastException}).
    
    无界的BlockingQueue阻塞队列，其使用与类PriorityQueue相同的排序规则，并提供阻塞检索操作. 
    尽管此队列在逻辑上是不受限制的，但是尝试添加可能会由于资源耗尽而失败(导致OutOfMemoryError)
    该类不允许null元素. 优先级队列依赖比较器，不允许插入不可比较的对象(会导致{@code ClassCastException}).

    * <p>This class and its iterator implement all of the
    * <em>optional</em> methods of the {@link Collection} and {@link
    * Iterator} interfaces.  The Iterator provided in method {@link
    * #iterator()} is <em>not</em> guaranteed to traverse the elements of
    * the PriorityBlockingQueue in any particular order. If you need
    * ordered traversal, consider using
    * {@code Arrays.sort(pq.toArray())}.  Also, method {@code drainTo}
    * can be used to <em>remove</em> some or all elements in priority
    * order and place them in another collection.
    
    该类和它的迭代器实现了Collection和Iterator接口的所有可选方法. 方法iterator中提供的Iterator不保证以任何特定顺序遍历PriorityBlockingQueue的元素(不保证顺序)
    如果你需要顺序遍历，可以使用Arrays.sort(pq.toArray). 另外，方法drainTo可用于按优先级顺序删除某些或所有元素并将它们放置在另一个集合中.

    * <p>Operations on this class make no guarantees about the ordering
    * of elements with equal priority. If you need to enforce an
    * ordering, you can define custom classes or comparators that use a
    * secondary key to break ties in primary priority values.  For
    * example, here is a class that applies first-in-first-out
    * tie-breaking to comparable elements. To use it, you would insert a
    * {@code new FIFOEntry(anEntry)} instead of a plain entry object.
    
    此类的操作不能保证具有相同优先级的元素的顺序. 如果需要执行排序，则可以定义使用辅助键来打破主优先级值关系的自定义类或比较器. 
    例如，这是一个将先进先出打破僵局的应用于可比较元素的类. 为了使用它，应该插入一个FIFOEntry(anEntry)来代替FIFOEntry对象.

    *  <pre> {@code
    * class FIFOEntry<E extends Comparable<? super E>> implements Comparable<FIFOEntry<E>> {
    *   static final AtomicLong seq = new AtomicLong(0);
    *   final long seqNum;
    *   final E entry;
    *   public FIFOEntry(E entry) {
    *     seqNum = seq.getAndIncrement();
    *     this.entry = entry;
    *   }
    *   public E getEntry() { return entry; }
    *   public int compareTo(FIFOEntry<E> other) {
    *     int res = entry.compareTo(other.entry);
    *     if (res == 0 && other.entry != this.entry)
    *       res = (seqNum < other.seqNum ? -1 : 1);
    *     return res;
    *   }
    * }}</pre>
    *
    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.
    
    该类是Java集合框架的一个成员

    */

```