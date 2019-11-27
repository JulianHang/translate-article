### 翻译Comparable注释

```java

   /**
    * This interface imposes a total ordering on the objects of each class that
    * implements it.  This ordering is referred to as the class's <i>natural
    * ordering</i>, and the class's <tt>compareTo</tt> method is referred to as
    * its <i>natural comparison method</i>.<p>
    
    该接口对每个实现类施加了总体排序。 此顺序称为类的自然顺序，而该类的compareTo方法被称为其自然比较方法.

    * Lists (and arrays) of objects that implement this interface can be sorted
    * automatically by {@link Collections#sort(List) Collections.sort} (and
    * {@link Arrays#sort(Object[]) Arrays.sort}).  Objects that implement this
    * interface can be used as keys in a {@linkplain SortedMap sorted map} or as
    * elements in a {@linkplain SortedSet sorted set}, without the need to
    * specify a {@linkplain Comparator comparator}.<p>
    
    对象列表实现该接口通过Collections.sort会自动排序. 实现该接口的对象可以用作键或元素，不需要指定Comparator.

    * The natural ordering for a class <tt>C</tt> is said to be <i>consistent
    * with equals</i> if and only if <tt>e1.compareTo(e2) == 0</tt> has
    * the same boolean value as <tt>e1.equals(e2)</tt> for every
    * <tt>e1</tt> and <tt>e2</tt> of class <tt>C</tt>.  Note that <tt>null</tt>
    * is not an instance of any class, and <tt>e.compareTo(null)</tt> should
    * throw a <tt>NullPointerException</tt> even though <tt>e.equals(null)</tt>
    * returns <tt>false</tt>.<p>
    
    当前仅当e1.compareTo(e2) == 0的结果与在C类中每个e1、e2的e1.equals(e2)相同的结果时，C类施加的自然顺序被称为一致性相等.
    请注意，null不是任何类的实例, e.compareTo(null)应该抛出空指针，即使e.equals(null)返回false.

    * It is strongly recommended (though not required) that natural orderings be
    * consistent with equals.  This is so because sorted sets (and sorted maps)
    * without explicit comparators behave "strangely" when they are used with
    * elements (or keys) whose natural ordering is inconsistent with equals.  In
    * particular, such a sorted set (or sorted map) violates the general contract
    * for set (or map), which is defined in terms of the <tt>equals</tt>
    * method.<p>
    
    自然顺序一致性相等是强烈推荐. 因为有序集合没有明确的比较器，当它们使用比较器后元素的自然顺序具有不一致时，比较器表现为奇怪的.
    特别是，集合违反集合的一般合同，该合同以等式定义.

    * For example, if one adds two keys <tt>a</tt> and <tt>b</tt> such that
    * {@code (!a.equals(b) && a.compareTo(b) == 0)} to a sorted
    * set that does not use an explicit comparator, the second <tt>add</tt>
    * operation returns false (and the size of the sorted set does not increase)
    * because <tt>a</tt> and <tt>b</tt> are equivalent from the sorted set's
    * perspective.<p>
    
    例如，假设添加两个元素ab到一个没有比较器的有序集合，第二次添加将返回false. 因为a和b是相等的

    * Virtually all Java core classes that implement <tt>Comparable</tt> have natural
    * orderings that are consistent with equals.  One exception is
    * <tt>java.math.BigDecimal</tt>, whose natural ordering equates
    * <tt>BigDecimal</tt> objects with equal values and different precisions
    * (such as 4.0 and 4.00).<p>
    
    几乎所有实现Comparable的Java核心类都具有与equals一致的自然顺序. 一种例外情况是BigDecimal，BigDecimal自然顺序相等是指值相等、不同精度的BigDecimal对象.

    * For the mathematically inclined, the <i>relation</i> that defines
    * the natural ordering on a given class C is:<pre>
    *       {(x, y) such that x.compareTo(y) &lt;= 0}.
    * </pre> The <i>quotient</i> for this total order is: <pre>
    *       {(x, y) such that x.compareTo(y) == 0}.
    * </pre>
    
    对于数学倾向, 定义指定比较器c施加在指定对象S上的强制顺序的关系为：

    * It follows immediately from the contract for <tt>compareTo</tt> that the
    * quotient is an <i>equivalence relation</i> on <tt>C</tt>, and that the
    * natural ordering is a <i>total order</i> on <tt>C</tt>.  When we say that a
    * class's natural ordering is <i>consistent with equals</i>, we mean that the
    * quotient for the natural ordering is the equivalence relation defined by
    * the class's {@link Object#equals(Object) equals(Object)} method:<pre>
    *     {(x, y) such that x.equals(y)}. </pre><p>
    
    略

    * This interface is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

```