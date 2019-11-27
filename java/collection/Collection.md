### 翻译Collection注释

```java

   /**
    * The root interface in the <i>collection hierarchy</i>.  A collection
    * represents a group of objects, known as its <i>elements</i>.  Some
    * collections allow duplicate elements and others do not.  Some are ordered
    * and others unordered.  The JDK does not provide any <i>direct</i>
    * implementations of this interface: it provides implementations of more
    * specific subinterfaces like <tt>Set</tt> and <tt>List</tt>.  This interface
    * is typically used to pass collections around and manipulate them where
    * maximum generality is desired.

    集合继承结构的基类. 集合表示为一组对象，称为其元素. 一些集合允许重复的元素而另外一些不允许. 一些集合是有序的而另外一些是无序的
    JDK没有提供任何该接口的直接实现类：它提供了更多指定子接口的实现，如Set、List
    该接口通常用于传递集合并在需要最大通用性的地方操作它们.

    * <p><i>Bags</i> or <i>multisets</i> (unordered collections that may contain
    * duplicate elements) should implement this interface directly.
    
    Bags或multisets（无序集合可能包含重复元素）应该直接实现该接口.

    * <p>All general-purpose <tt>Collection</tt> implementation classes (which
    * typically implement <tt>Collection</tt> indirectly through one of its
    * subinterfaces) should provide two "standard" constructors: a void (no
    * arguments) constructor, which creates an empty collection, and a
    * constructor with a single argument of type <tt>Collection</tt>, which
    * creates a new collection with the same elements as its argument.  In
    * effect, the latter constructor allows the user to copy any collection,
    * producing an equivalent collection of the desired implementation type.
    * There is no way to enforce this convention (as interfaces cannot contain
    * constructors) but all of the general-purpose <tt>Collection</tt>
    * implementations in the Java platform libraries comply.
    
    所有通用集合实现类通常通过它的子接口之一来间接实现该集合，它的子接口应该提供两个标准构造器：无参构造器，创建一个空的集合；单个参数类型为Collection的有参构造器，创建包含参数中所有元素的新集合.
    后者的构造器允许开发者拷贝任何集合， 产生所需实现类型的等效集合. 没有强制执行此约定的方式（接口没有包含构造器）, 但是Java平台库中的所有通用集合实现类都遵守.

    * <p>The "destructive" methods contained in this interface, that is, the
    * methods that modify the collection on which they operate, are specified to
    * throw <tt>UnsupportedOperationException</tt> if this collection does not
    * support the operation.  If this is the case, these methods may, but are not
    * required to, throw an <tt>UnsupportedOperationException</tt> if the
    * invocation would have no effect on the collection.  For example, invoking
    * the {@link #addAll(Collection)} method on an unmodifiable collection may,
    * but is not required to, throw the exception if the collection to be added
    * is empty.
    
    如果此集合不支持该操作，则指定该接口中包含的“破坏性”方法（即修改其操作的集合的方法）引发UnsupportedOperationException
    在这种情况下，如果调用对集合没有影响，则这些方法可能会（但不是必需）引发UnsupportedOperationException
    例如，如果要添加的集合为空，则对不可修改的集合调用addAll方法可能（但并非必须）引发异常.

    * <p><a name="optional-restrictions">
    * Some collection implementations have restrictions on the elements that
    * they may contain.</a>  For example, some implementations prohibit null elements,
    * and some have restrictions on the types of their elements.  Attempting to
    * add an ineligible element throws an unchecked exception, typically
    * <tt>NullPointerException</tt> or <tt>ClassCastException</tt>.  Attempting
    * to query the presence of an ineligible element may throw an exception,
    * or it may simply return false; some implementations will exhibit the former
    * behavior and some will exhibit the latter.  More generally, attempting an
    * operation on an ineligible element whose completion would not result in
    * the insertion of an ineligible element into the collection may throw an
    * exception or it may succeed, at the option of the implementation.
    * Such exceptions are marked as "optional" in the specification for this
    * interface.
    
    一些集合实现类对它们可能包含的元素有限制. 例如，一些实现类禁止空元素，一些集合实现类对它们的元素类型有限制.
    尝试添加不合格的元素会抛出非受查异常, 通常是空指针或类型转换异常. 尝试查询不合格元素是否存在可能抛出异常，或者可能简单返回false;
    一些实现类会显示前者行为和一些实现类会显示后者. 更一般地，尝试对不合格元素进行操作，该操作的完成不会导致将不合格元素插入集合中，这可能会引发异常，也可能会成功，具体取决于实现方式.
    此类异常在此接口的规范中标记为“可选”

    * <p>It is up to each collection to determine its own synchronization
    * policy.  In the absence of a stronger guarantee by the
    * implementation, undefined behavior may result from the invocation
    * of any method on a collection that is being mutated by another
    * thread; this includes direct invocations, passing the collection to
    * a method that might perform invocations, and using an existing
    * iterator to examine the collection.
    
    由每个集合决定自己的同步策略. 在没有强有力的执行保证的情况下, 调用另一个线程正在变异的集合上的任何方法都可能导致未定义的行为; 这包括直接调用，将集合传递给可能执行调用的方法，以及使用现有的迭代器检查集合.

    * <p>Many methods in Collections Framework interfaces are defined in
    * terms of the {@link Object#equals(Object) equals} method.  For example,
    * the specification for the {@link #contains(Object) contains(Object o)}
    * method says: "returns <tt>true</tt> if and only if this collection
    * contains at least one element <tt>e</tt> such that
    * <tt>(o==null ? e==null : o.equals(e))</tt>."  This specification should
    * <i>not</i> be construed to imply that invoking <tt>Collection.contains</tt>
    * with a non-null argument <tt>o</tt> will cause <tt>o.equals(e)</tt> to be
    * invoked for any element <tt>e</tt>.  Implementations are free to implement
    * optimizations whereby the <tt>equals</tt> invocation is avoided, for
    * example, by first comparing the hash codes of the two elements.  (The
    * {@link Object#hashCode()} specification guarantees that two objects with
    * unequal hash codes cannot be equal.)  More generally, implementations of
    * the various Collections Framework interfaces are free to take advantage of
    * the specified behavior of underlying {@link Object} methods wherever the
    * implementor deems it appropriate.
    
    集合框架接口中的许多方法都是根据equals方法定义的. 例如，contains方法的规范：当且仅当集合中包含至少一个元素时返回true. 该规范并不是说调用Collecion.contains传入不为空的参数会调用任何元素的equals.
    实现可以自由执行优化，从而避免了equals调用, 例如，首先比较两个元素的哈希值.(Object#hashCode规范保证两个不相等的对象哈希值不会相等)
    一般而言，无论实现者认为如何合适，各种集合框架接口的实现都可以自由利用底层Object方法的指定行为.

    * <p>Some collection operations which perform recursive traversal of the
    * collection may fail with an exception for self-referential instances where
    * the collection directly or indirectly contains itself. This includes the
    * {@code clone()}, {@code equals()}, {@code hashCode()} and {@code toString()}
    * methods. Implementations may optionally handle the self-referential scenario,
    * however most current implementations do not do so.
    
    某些执行集合递归遍历的集合操作可能会失败，对于自引用实例，其中直接或间接包含集合本身的情况除外. 包括clone、equals、hashCode、toString方法.
    实现可以有选择地处理自引用场景，但是大多数当前实现不这样做.

    * <p>This interface is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.
    
    该类是Java Collections Framework的一员.

```