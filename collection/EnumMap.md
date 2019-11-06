### 翻译EnumMap注释

```java

   /**
    * A specialized {@link Map} implementation for use with enum type keys.  All
    * of the keys in an enum map must come from a single enum type that is
    * specified, explicitly or implicitly, when the map is created.  Enum maps
    * are represented internally as arrays.  This representation is extremely
    * compact and efficient.
    
    专门用于枚举类型的键的Map实现. EnumMap中的所有键必须来自EnumMap被创建时显式或隐式地指定单个m枚举类型.
    EnumMap在内部表示为数组. 该代表是非常严谨、有效率的.

    * <p>Enum maps are maintained in the <i>natural order</i> of their keys
    * (the order in which the enum constants are declared).  This is reflected
    * in the iterators returned by the collections views ({@link #keySet()},
    * {@link #entrySet()}, and {@link #values()}).
    
    EnumMap将它的键按自然顺序进行维护(枚举常量的声明顺序). 通过keySet、entrySet、values返回的迭代器体现出来.

    * <p>Iterators returned by the collection views are <i>weakly consistent</i>:
    * they will never throw {@link ConcurrentModificationException} and they may
    * or may not show the effects of any modifications to the map that occur while
    * the iteration is in progress.
    
    迭代器是弱一致性：它们将不会抛出ConcurrentModificationException，它们可能会显示也可能不会显示在进行迭代时对EnumMap进行的任何修改的效果.

    * <p>Null keys are not permitted.  Attempts to insert a null key will
    * throw {@link NullPointerException}.  Attempts to test for the
    * presence of a null key or to remove one will, however, function properly.
    * Null values are permitted.

    不允许空键. 尝试插入空间会抛出空指针. 尝试测试是否存在空键或移除一个空键将正常运行.
    允许空值.

    * <P>Like most collection implementations <tt>EnumMap</tt> is not
    * synchronized. If multiple threads access an enum map concurrently, and at
    * least one of the threads modifies the map, it should be synchronized
    * externally.  This is typically accomplished by synchronizing on some
    * object that naturally encapsulates the enum map.  If no such object exists,
    * the map should be "wrapped" using the {@link Collections#synchronizedMap}
    * method.  This is best done at creation time, to prevent accidental
    * unsynchronized access:
    *
    * <pre>
    *     Map&lt;EnumKey, V&gt; m
    *         = Collections.synchronizedMap(new EnumMap&lt;EnumKey, V&gt;(...));
    * </pre>
    
    EnumMap是非线程安全.
    如果多线程同时访问同一个EnumMap，至少一个线程会修改EnumMap结构，它必须在外部保持同步.
    (结构修改指的是添加或删除一个或多个键值对的任何一个操作;仅仅改变对象已存在的键所对应的值不属于结构修改)
    一般通过包装EnumMap的对象来完成同步.
    如果外部对象不存在，应该使用Collections.synchronizedMap来包裹EnumMap对象.
    该方法最好在EnumMap创建时调用，为了防止偶然性的非线程安全访问EnumMap.

    * <p>Implementation note: All basic operations execute in constant time.
    * They are likely (though not guaranteed) to be faster than their
    * {@link HashMap} counterparts.
    
    实现说明：所有的基础操作执行消耗恒定的时间. 它可能比HashMap还快.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```