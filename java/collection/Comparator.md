### 翻译Comparator注释

```java

   /**
    * A comparison function, which imposes a <i>total ordering</i> on some
    * collection of objects.  Comparators can be passed to a sort method (such
    * as {@link Collections#sort(List,Comparator) Collections.sort} or {@link
    * Arrays#sort(Object[],Comparator) Arrays.sort}) to allow precise control
    * over the sort order.  Comparators can also be used to control the order of
    * certain data structures (such as {@link SortedSet sorted sets} or {@link
    * SortedMap sorted maps}), or to provide an ordering for collections of
    * objects that don't have a {@link Comparable natural ordering}.<p>

    比较功能，对某些对象集合施加总排序. Comparator传递给排序方法, 允许对排序顺序进行精确控制
    Comparator也能用于控制数据结构的顺序, 或为没有自然顺序的对象集合提供顺序.

    * The ordering imposed by a comparator <tt>c</tt> on a set of elements
    * <tt>S</tt> is said to be <i>consistent with equals</i> if and only if
    * <tt>c.compare(e1, e2)==0</tt> has the same boolean value as
    * <tt>e1.equals(e2)</tt> for every <tt>e1</tt> and <tt>e2</tt> in
    * <tt>S</tt>.<p>
    
    当前仅当c.compare(e1, e2) == 0的结果与在Set集合中每个e1、e2的e1.equals(e2)相同的结果时，在Set集合上通过比较器c施加的顺序被称为一致性相等.

    * Caution should be exercised when using a comparator capable of imposing an
    * ordering inconsistent with equals to order a sorted set (or sorted map).
    * Suppose a sorted set (or sorted map) with an explicit comparator <tt>c</tt>
    * is used with elements (or keys) drawn from a set <tt>S</tt>.  If the
    * ordering imposed by <tt>c</tt> on <tt>S</tt> is inconsistent with equals,
    * the sorted set (or sorted map) will behave "strangely."  In particular the
    * sorted set (or sorted map) will violate the general contract for set (or
    * map), which is defined in terms of <tt>equals</tt>.<p>
    
    当使用equals不一致的排序比较器时，应谨慎行事. 假设带有比较器c的集合与从集合S提取的元素（或键）一起使用. 如果在集合S上通过c施加的顺序是不一致的话，该集合将表现为奇怪.
    特别是，集合将违反集合的一般合同，该合同以等式定义.

    * For example, suppose one adds two elements {@code a} and {@code b} such that
    * {@code (a.equals(b) && c.compare(a, b) != 0)}
    * to an empty {@code TreeSet} with comparator {@code c}.
    * The second {@code add} operation will return
    * true (and the size of the tree set will increase) because {@code a} and
    * {@code b} are not equivalent from the tree set's perspective, even though
    * this is contrary to the specification of the
    * {@link Set#add Set.add} method.<p>
    
    例如，假设添加两个元素ab到一个带有比较c的空集合.
    第二次添加操作将返回true，因为a和b是不相等, 即使这与添加方法的规范相反.

    * Note: It is generally a good idea for comparators to also implement
    * <tt>java.io.Serializable</tt>, as they may be used as ordering methods in
    * serializable data structures (like {@link TreeSet}, {@link TreeMap}).  In
    * order for the data structure to serialize successfully, the comparator (if
    * provided) must implement <tt>Serializable</tt>.<p>
    
    比较器最好也实现Serializable是一个好主意, 因为它们可用作可序列化数据结构中的排序方法.
    为了数据结构序列化成功， Comparator必须实现Serializable

    * For the mathematically inclined, the <i>relation</i> that defines the
    * <i>imposed ordering</i> that a given comparator <tt>c</tt> imposes on a
    * given set of objects <tt>S</tt> is:<pre>
    *       {(x, y) such that c.compare(x, y) &lt;= 0}.
    * </pre> The <i>quotient</i> for this total order is:<pre>
    *       {(x, y) such that c.compare(x, y) == 0}.
    * </pre>
    
    对于数学倾向, 定义指定比较器c施加在指定对象S上的强制顺序的关系为：

    * It follows immediately from the contract for <tt>compare</tt> that the
    * quotient is an <i>equivalence relation</i> on <tt>S</tt>, and that the
    * imposed ordering is a <i>total order</i> on <tt>S</tt>.  When we say that
    * the ordering imposed by <tt>c</tt> on <tt>S</tt> is <i>consistent with
    * equals</i>, we mean that the quotient for the ordering is the equivalence
    * relation defined by the objects' {@link Object#equals(Object)
    * equals(Object)} method(s):<pre>
    *     {(x, y) such that x.equals(y)}. </pre>
    
    略

    * <p>Unlike {@code Comparable}, a comparator may optionally permit
    * comparison of null arguments, while maintaining the requirements for
    * an equivalence relation.
    
    不像Comparable，Comparator可以选择允许比较空参数, 同时保持对等关系的要求.

    * <p>This interface is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```