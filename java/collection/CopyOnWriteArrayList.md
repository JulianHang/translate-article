### 翻译CopyOnWriteArrayList类注释

```java

    /**
    * A thread-safe variant of {@link java.util.ArrayList} in which all mutative
    * operations ({@code add}, {@code set}, and so on) are implemented by
    * making a fresh copy of the underlying array.

    ArrayList中所有会变化的操作（add、set等等）的线程安全变体，通过生成底层数组的新副本来实现的.

    * <p>This is ordinarily too costly, but may be <em>more</em> efficient
    * than alternatives when traversal operations vastly outnumber
    * mutations, and is useful when you cannot or don't want to
    * synchronize traversals, yet need to preclude interference among
    * concurrent threads.  The "snapshot" style iterator method uses a
    * reference to the state of the array at the point that the iterator
    * was created. This array never changes during the lifetime of the
    * iterator, so interference is impossible and the iterator is
    * guaranteed not to throw {@code ConcurrentModificationException}.
    * The iterator will not reflect additions, removals, or changes to
    * the list since the iterator was created.  Element-changing
    * operations on iterators themselves ({@code remove}, {@code set}, and
    * {@code add}) are not supported. These methods throw
    * {@code UnsupportedOperationException}.

    这通常代价太大，但是当遍历操作超过变动时它可能比其他方法更有效，当你不能或不想要同步遍历，但仍需要在并发线程中排除冲突时非常有用.
    迭代器方法中的‘snapshot’变量在创建迭代器时将引用指向了当前数组.
    在迭代器的生命周期中数组永远不会被改变，因此冲突是不可能的，迭代器保证不会抛出异常.
    迭代器不会反映列表上的添加、删除、修改，因为它（迭代器）已经被创建了.（因为这些操作是在新副本上进行，所以不会被反映到）
    迭代器上的元素变化操作不支持remove、set、add.这些方法会抛出异常.

    * <p>All elements are permitted, including {@code null}.

    允许存放所有元素,包括null.

    * <p>Memory consistency effects: As with other concurrent
    * collections, actions in a thread prior to placing an object into a
    * {@code CopyOnWriteArrayList}
    * <a href="package-summary.html#MemoryVisibility"><i>happen-before</i></a>
    * actions subsequent to the access or removal of that element from
    * the {@code CopyOnWriteArrayList} in another thread.

    内存一致性效果:与其他并发集合一样,将对象放入CopyOnWriteArrayList之前执行线程中的操作,之后的操作是在另外一个线程中通过CopyOnWriteArrayList访问或删除元素.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```
