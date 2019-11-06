### 翻译WeakHashMap注释

```java

   /**
    * Hash table based implementation of the <tt>Map</tt> interface, with
    * <em>weak keys</em>.
    * An entry in a <tt>WeakHashMap</tt> will automatically be removed when
    * its key is no longer in ordinary use.  More precisely, the presence of a
    * mapping for a given key will not prevent the key from being discarded by the
    * garbage collector, that is, made finalizable, finalized, and then reclaimed.
    * When a key has been discarded its entry is effectively removed from the map,
    * so this class behaves somewhat differently from other <tt>Map</tt>
    * implementations.

    基于哈希表的Map接口实现, 采用弱键.
    当它的键不在使用时，WeakHashMap中的节点会自动被移除. 更确切地说，指定键的映射关系的存在不会阻止该键被垃圾回收器丢弃、终结、回收.
    键被丢弃后，它的节点也将被移除. 因此该类的行为不同于其他实现.

    * <p> Both null values and the null key are supported. This class has
    * performance characteristics similar to those of the <tt>HashMap</tt>
    * class, and has the same efficiency parameters of <em>initial capacity</em>
    * and <em>load factor</em>.
    
    键值对允许存放null. 该类的性能特征类似于HashMap, 并且具有相同的效率参数：初始容量与加载因子.

    * <p> Like most collection classes, this class is not synchronized.
    * A synchronized <tt>WeakHashMap</tt> may be constructed using the
    * {@link Collections#synchronizedMap Collections.synchronizedMap}
    * method.
    
    和大多数集合类一样，该类是非线程安全.
    使用Collections#synchronizedMap方法构造一个线程安全的WeakHashMap.

    * <p> This class is intended primarily for use with key objects whose
    * <tt>equals</tt> methods test for object identity using the
    * <tt>==</tt> operator.  Once such a key is discarded it can never be
    * recreated, so it is impossible to do a lookup of that key in a
    * <tt>WeakHashMap</tt> at some later time and be surprised that its entry
    * has been removed.  This class will work perfectly well with key objects
    * whose <tt>equals</tt> methods are not based upon object identity, such
    * as <tt>String</tt> instances.  With such recreatable key objects,
    * however, the automatic removal of <tt>WeakHashMap</tt> entries whose
    * keys have been discarded may prove to be confusing.
    
    该类主要用于键对象，equals方法使用 == 操作来测试对象的相等性.
    一旦某个键被丢弃，就无法在重新创建, 因此后续无法在WeakHashMap中查找该键，并且感到惊讶的是它的节点已被删除.
    该类与键对象完美配合使用，equals方法不基于上面的相等性，例如string实例.
    有了可再生的键对象, 然而, WeakHashMap自动移除其键已经被废弃的节点可能会造成混乱.

    * <p> The behavior of the <tt>WeakHashMap</tt> class depends in part upon
    * the actions of the garbage collector, so several familiar (though not
    * required) <tt>Map</tt> invariants do not hold for this class.  Because
    * the garbage collector may discard keys at any time, a
    * <tt>WeakHashMap</tt> may behave as though an unknown thread is silently
    * removing entries.  In particular, even if you synchronize on a
    * <tt>WeakHashMap</tt> instance and invoke none of its mutator methods, it
    * is possible for the <tt>size</tt> method to return smaller values over
    * time, for the <tt>isEmpty</tt> method to return <tt>false</tt> and
    * then <tt>true</tt>, for the <tt>containsKey</tt> method to return
    * <tt>true</tt> and later <tt>false</tt> for a given key, for the
    * <tt>get</tt> method to return a value for a given key but later return
    * <tt>null</tt>, for the <tt>put</tt> method to return
    * <tt>null</tt> and the <tt>remove</tt> method to return
    * <tt>false</tt> for a key that previously appeared to be in the map, and
    * for successive examinations of the key set, the value collection, and
    * the entry set to yield successively smaller numbers of elements.
    
    WeakHashMap的行为部分取决于垃圾回收器的动作, 一些熟悉的Map不变量不同于该类.
    因为垃圾回收器可能会在任意时间内丢弃键, WeakHashMap可能表现得像某个线程正在默默移除节点.
    尤其是, 即使你给WeakHashMap对象加上同步, 不调用它的变异方法, 随着时间的推移size方法可能返回较小的值, isEmpty方法返回false或true, containKey根据指定键返回true和后续返回false
    get方法根据指定键返回值，但是后续返回null， put方法返回null，remove方法由于某个键之前存在于map中故返回false
    键设置连续坚持，值集合，节点设置为连续生成较少数量的元素。

    * <p> Each key object in a <tt>WeakHashMap</tt> is stored indirectly as
    * the referent of a weak reference.  Therefore a key will automatically be
    * removed only after the weak references to it, both inside and outside of the
    * map, have been cleared by the garbage collector.
    
    WeakHashMap中每一个键对象都作为弱引用的对象来间接存储. 因此map内外部的弱引用被垃圾回收器清除后, 它的键将会自动删除.

    * <p> <strong>Implementation note:</strong> The value objects in a
    * <tt>WeakHashMap</tt> are held by ordinary strong references.  Thus care
    * should be taken to ensure that value objects do not strongly refer to their
    * own keys, either directly or indirectly, since that will prevent the keys
    * from being discarded.  Note that a value object may refer indirectly to its
    * key via the <tt>WeakHashMap</tt> itself; that is, a value object may
    * strongly refer to some other key object whose associated value object, in
    * turn, strongly refers to the key of the first value object.  If the values
    * in the map do not rely on the map holding strong references to them, one way
    * to deal with this is to wrap values themselves within
    * <tt>WeakReferences</tt> before
    * inserting, as in: <tt>m.put(key, new WeakReference(value))</tt>,
    * and then unwrapping upon each <tt>get</tt>.
    
    实现说明：WeakHashMap中的值对象由普通的强引用所持有. 因此，应确保值对象不会直接或间接地引用其自身的键，因为这会导致键无法被丢弃.
    请注意，值对象可能通过WeakHashMap本身间接引用其键；也就是说，值对象可能引用与其关联的值对象相关的某些其他键对象, 反过来，引用第一个值对象的键.
    如果map中的值不依赖于map来维持自身的强引用, 一种解决方法是在插入之前将值本身包装在WeakReferences中，像这样：m.put(key, new WeakReference(value)), 然后再每次获取时展开.

    * <p>The iterators returned by the <tt>iterator</tt> method of the collections
    * returned by all of this class's "collection view methods" are
    * <i>fail-fast</i>: if the map is structurally modified at any time after the
    * iterator is created, in any way except through the iterator's own
    * <tt>remove</tt> method, the iterator will throw a {@link
    * ConcurrentModificationException}.  Thus, in the face of concurrent
    * modification, the iterator fails quickly and cleanly, rather than risking
    * arbitrary, non-deterministic behavior at an undetermined time in the future.
    
    迭代器是快速失败：当迭代器被创建后的任何时间内修改结构，除了自身remove方法以外（iterator#remove）的任何方式，迭代器将会抛出ConcurrentModificationException异常（该说法很像ArrayList）.
    因此，面对并发修改，迭代器更加快速失败，而不是在未来的未确定时刻（多线程）上随意冒险、作不确定的行为.

    * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
    * as it is, generally speaking, impossible to make any hard guarantees in the
    * presence of unsynchronized concurrent modification.  Fail-fast iterators
    * throw <tt>ConcurrentModificationException</tt> on a best-effort basis.
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