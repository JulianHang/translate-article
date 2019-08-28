### 翻译Spliterator注释

第一次认识Spliterator，尝试性地翻译下，请见谅！

```java

    /**
    * An object for traversing and partitioning elements of a source.  The source
    * of elements covered by a Spliterator could be, for example, an array, a
    * {@link Collection}, an IO channel, or a generator function.

    一个用于遍历和切割资源元素的对象.
    分割迭代器所涵盖的资源元素有：数组、集合、IO、生成器函数.

    * <p>A Spliterator may traverse elements individually ({@link
    * #tryAdvance tryAdvance()}) or sequentially in bulk
    * ({@link #forEachRemaining forEachRemaining()}).

    分割迭代器可以单个或顺序（多个）遍历元素.

    * <p>A Spliterator may also partition off some of its elements (using
    * {@link #trySplit}) as another Spliterator, to be used in
    * possibly-parallel operations.  Operations using a Spliterator that
    * cannot split, or does so in a highly imbalanced or inefficient
    * manner, are unlikely to benefit from parallelism.  Traversal
    * and splitting exhaust elements; each Spliterator is useful for only a single
    * bulk computation.

    分割迭代器可分割它的部分元素作为另外一个分割迭代器，通常用来做并行操作.
    使用无法分割的分割迭代器或以高度不平衡或低效的方式操作，不太可能从并行中受益.
    遍历和分割所有元素.
    每个分割迭代器仅作用于单批元素计算（每个分割迭代器只负责自己范围内的元素）

    * <p>A Spliterator also reports a set of {@link #characteristics()} of its
    * structure, source, and elements from among {@link #ORDERED},
    * {@link #DISTINCT}, {@link #SORTED}, {@link #SIZED}, {@link #NONNULL},
    * {@link #IMMUTABLE}, {@link #CONCURRENT}, and {@link #SUBSIZED}. These may
    * be employed by Spliterator clients to control, specialize or simplify
    * computation.  For example, a Spliterator for a {@link Collection} would
    * report {@code SIZED}, a Spliterator for a {@link Set} would report
    * {@code DISTINCT}, and a Spliterator for a {@link SortedSet} would also
    * report {@code SORTED}.  Characteristics are reported as a simple unioned bit
    * set.

    分割迭代器也会报告它的结构、资源、来自于ORDERED、DISTINCT、SORTED、SIZED、NONNULL
    IMMUTABLE、CONCURRENT、SUBSIZED当中的元素的一组特征（通过这些属性来标识类的状态）
    分割迭代器使用这些（特征）来控制、专门化（专门处理某一特征）、简化计算.
    例如，集合的分割迭代器会报告SIZED属性的特征，Set接口的分割迭代器会报告DISTINCT属性的特征，
    SortedSet接口的分割迭代器也会报告SORTED属性的特征（按我的理解应该是会报告DISTINCT与SORTED属性）
    被报告的特征是一组简单合并的位集（多个属性合并在一起）.

    * Some characteristics additionally constrain method behavior; for example if
    * {@code ORDERED}, traversal methods must conform to their documented ordering.
    * New characteristics may be defined in the future, so implementors should not
    * assign meanings to unlisted values.

    附加的特征说明了方法的行为.
    比如针对ORDERED属性，遍历方法必须符合记录的顺序.
    未来可能会定义新特征，所以实现者不要分配给未列出的值.

    * <p><a name="binding">A Spliterator that does not report {@code IMMUTABLE} or
    * {@code CONCURRENT} is expected to have a documented policy concerning:
    * when the spliterator <em>binds</em> to the element source; and detection of
    * structural interference of the element source detected after binding.</a>  A
    * <em>late-binding</em> Spliterator binds to the source of elements at the
    * point of first traversal, first split, or first query for estimated size,
    * rather than at the time the Spliterator is created.  A Spliterator that is
    * not <em>late-binding</em> binds to the source of elements at the point of
    * construction or first invocation of any method.  Modifications made to the
    * source prior to binding are reflected when the Spliterator is traversed.
    * After binding a Spliterator should, on a best-effort basis, throw
    * {@link ConcurrentModificationException} if structural interference is
    * detected.  Spliterators that do this are called <em>fail-fast</em>.  The
    * bulk traversal method ({@link #forEachRemaining forEachRemaining()}) of a
    * Spliterator may optimize traversal and check for structural interference
    * after all elements have been traversed, rather than checking per-element and
    * failing immediately.

    绑定中的分割迭代器不会报告IMMUTABLE与CONCURRENT，预计一个有关的记录策略：
    当分割迭代器绑定到元素上;
    绑定后资源的元素会启动结构冲突的检测.
    后期绑定的分割迭代器会在第一次遍历、第一次分割、第一次查询大小时绑定到元素上，而不是在创建分割迭代器时就绑定.
    非后期绑定的分割迭代器在第一次调用任何方法时绑定到元素上.
    遍历分割迭代器时会反映在绑定之前对元素的修改.
    绑定分割迭代后，应尽力而为，若检测到结构冲突则抛出ConcurrentModificationException异常.
    执行此行为的分割迭代器称为快速失败.
    分割迭代器的遍历方法forEachRemaining可以优化遍历（行为），在遍历完所有元素后才检查结构冲突，而不是检查每一个元素并立即失败.

    * <p>Spliterators can provide an estimate of the number of remaining elements
    * via the {@link #estimateSize} method.  Ideally, as reflected in characteristic
    * {@link #SIZED}, this value corresponds exactly to the number of elements
    * that would be encountered in a successful traversal.  However, even when not
    * exactly known, an estimated value value may still be useful to operations
    * being performed on the source, such as helping to determine whether it is
    * preferable to split further or traverse the remaining elements sequentially.

    分割迭代器通过调用estimateSize方法能够获取到剩余元素的数量的估计值.
    理想情况下，正如特征SIZED所反映的，该值对应着成功遍历时获取的元素的数量
    然而，即使不知道精确值，估计值仍然对在资源上执行的操作有用，例如，有助于确定是否可能进一步切割或顺序遍历其余元素.

    * <p>Despite their obvious utility in parallel algorithms, spliterators are not
    * expected to be thread-safe; instead, implementations of parallel algorithms
    * using spliterators should ensure that the spliterator is only used by one
    * thread at a time.  This is generally easy to attain via <em>serial
    * thread-confinement</em>, which often is a natural consequence of typical
    * parallel algorithms that work by recursive decomposition.  A thread calling
    * {@link #trySplit()} may hand over the returned Spliterator to another thread,
    * which in turn may traverse or further split that Spliterator.  The behaviour
    * of splitting and traversal is undefined if two or more threads operate
    * concurrently on the same spliterator.  If the original thread hands a
    * spliterator off to another thread for processing, it is best if that handoff
    * occurs before any elements are consumed with {@link #tryAdvance(Consumer)
    * tryAdvance()}, as certain guarantees (such as the accuracy of
    * {@link #estimateSize()} for {@code SIZED} spliterators) are only valid before
    * traversal has begun.

    尽管它们在并行算法中有明显的实用性，分割迭代器不属于线程安全;相反，并行算法的实现中，使用分割迭代器应确保一直只有一个线程在使用分割迭代器.
    这通常通过串行线程很容易达到，这通常是通过递归分解工作的典型并行算法的自然结果.
    调用trySplit方法的线程可以将返回的分割迭代器传递到另外一个线程，该线程可以顺序遍历或进一步拆分分割迭代器.
    如果有两个或多个线程同时操作同一个分割迭代器，则拆分和遍历的结果是不确定的.
    如果原始线程将分割迭代器传递到另外一个线程进行处理，那么最好在使用tryAdvance消耗任何元素之前进行传递，因为某些担保（例如estimateSize对具有SIZED特征的分割迭代器的准确性）仅在遍历开始之前有效.

    * <p>Primitive subtype specializations of {@code Spliterator} are provided for
    * {@link OfInt int}, {@link OfLong long}, and {@link OfDouble double} values.
    * The subtype default implementations of
    * {@link Spliterator#tryAdvance(java.util.function.Consumer)}
    * and {@link Spliterator#forEachRemaining(java.util.function.Consumer)} box
    * primitive values to instances of their corresponding wrapper class.  Such
    * boxing may undermine any performance advantages gained by using the primitive
    * specializations.  To avoid boxing, the corresponding primitive-based methods
    * should be used.  For example,
    * {@link Spliterator.OfInt#tryAdvance(java.util.function.IntConsumer)}
    * and {@link Spliterator.OfInt#forEachRemaining(java.util.function.IntConsumer)}
    * should be used in preference to
    * {@link Spliterator.OfInt#tryAdvance(java.util.function.Consumer)} and
    * {@link Spliterator.OfInt#forEachRemaining(java.util.function.Consumer)}.
    * Traversal of primitive values using boxing-based methods
    * {@link #tryAdvance tryAdvance()} and
    * {@link #forEachRemaining(java.util.function.Consumer) forEachRemaining()}
    * does not affect the order in which the values, transformed to boxed values,
    * are encountered.

    分割迭代器提供了专门处理基本类型的接口，OfInt -> int OfLong -> long OfDouble -> double
    tryAdvance与forEachRemaining的类型默认实现将原始值设置为其对应包装类的实例(int -> Integer)
    这种装箱可能会破坏使用基本类型所带来的任何性能上的优势.
    为了避免装箱，应使用对应的基本类型方法.
    例如，tryAdvance(IntConsumer)与forEachRemaining(IntConsumer)应优先于tryAdvance(Consumer)与forEachRemaining(Consumer)使用.
    使用基于装箱的方法遍历原始值不会影响值的顺序，而转换成包装类型的值则会有所影响.

    * @apiNote
    * <p>Spliterators, like {@code Iterator}s, are for traversing the elements of
    * a source.  The {@code Spliterator} API was designed to support efficient
    * parallel traversal in addition to sequential traversal, by supporting
    * decomposition as well as single-element iteration.  In addition, the
    * protocol for accessing elements via a Spliterator is designed to impose
    * smaller per-element overhead than {@code Iterator}, and to avoid the inherent
    * race involved in having separate methods for {@code hasNext()} and
    * {@code next()}.

    分割迭代器用于遍历资源中元素.
    除了顺序遍历之外，分割迭代器接口被设计成支持有效的并行遍历，通过支持分解及单元素迭代.
    除此之外，通过分割迭代器获取元素的方式被设计成比Iterator获取每个元素时具有更小的开销，同时避免在hasNext与next中使用分割方法所带来的固有竞争.

    * <p>For mutable sources, arbitrary and non-deterministic behavior may occur if
    * the source is structurally interfered with (elements added, replaced, or
    * removed) between the time that the Spliterator binds to its data source and
    * the end of traversal.  For example, such interference will produce arbitrary,
    * non-deterministic results when using the {@code java.util.stream} framework.

    针对易变资源，如果资源在分割迭代器绑定到数据源上和遍历结束之间的时间上发生结构冲突（添加、代替、删除），可能发生任意的、非确定性的行为.
    例如，当使用stream框架时冲突将会发生任意的、非确定性结果.

    * <p>Structural interference of a source can be managed in the following ways
    * (in approximate order of decreasing desirability):
    * <ul>
    * <li>The source cannot be structurally interfered with.
    * <br>For example, an instance of
    * {@link java.util.concurrent.CopyOnWriteArrayList} is an immutable source.
    * A Spliterator created from the source reports a characteristic of
    * {@code IMMUTABLE}.</li>
    * <li>The source manages concurrent modifications.
    * <br>For example, a key set of a {@link java.util.concurrent.ConcurrentHashMap}
    * is a concurrent source.  A Spliterator created from the source reports a
    * characteristic of {@code CONCURRENT}.</li>
    * <li>The mutable source provides a late-binding and fail-fast Spliterator.
    * <br>Late binding narrows the window during which interference can affect
    * the calculation; fail-fast detects, on a best-effort basis, that structural
    * interference has occurred after traversal has commenced and throws
    * {@link ConcurrentModificationException}.  For example, {@link ArrayList},
    * and many other non-concurrent {@code Collection} classes in the JDK, provide
    * a late-binding, fail-fast spliterator.</li>
    * <li>The mutable source provides a non-late-binding but fail-fast Spliterator.
    * <br>The source increases the likelihood of throwing
    * {@code ConcurrentModificationException} since the window of potential
    * interference is larger.</li>
    * <li>The mutable source provides a late-binding and non-fail-fast Spliterator.
    * <br>The source risks arbitrary, non-deterministic behavior after traversal
    * has commenced since interference is not detected.
    * </li>
    * <li>The mutable source provides a non-late-binding and non-fail-fast
    * Spliterator.
    * <br>The source increases the risk of arbitrary, non-deterministic behavior
    * since non-detected interference may occur after construction.
    * </li>
    * </ul>

    通过以下方式防止资源的结构冲突：
    资源不会发生结构冲突，例如，CopyOnWriteArrayList对象是一个不可变资源.该资源创建的分割迭代器报告了IMMUTABLE特征.

    资源管理并发修改，例如，ConcurrentHashMap集合属于并发资源.该资源创建的分割迭代器报告了CONCURRENT特征.

    可变资源提供了后期绑定和快速失败的分割迭代器.
    后期绑定缩小了使用范围，在冲突期间会影响计算;

    在遍历开始并抛出异常之后，在尽力而为的基础上，检测到结构冲突已经发生.
    例如，在JDK中的ArrayList和许多其他非线程安全的集合类，提供了后期绑定与快速失败的分割迭代器.

    可变资源提供了非后期绑定与快速失败的分割迭代器.
    由于潜在接口的范围过大，会增加抛出ConcurrentModificationException的可能性.

    可变资源提供了非后期绑定与快速失败的分割迭代器.
    由于接口未检测到结构冲突，在遍历开始之后，资源会发生任意的、非确定性的行为.

    可变资源提供了非后期绑定与非快速失败的分割迭代器.
    在构造对象后可能发生未检测到结构冲突，资源发生任意的、非确定性的行为会增加.

    * <p><b>Example.</b> Here is a class (not a very useful one, except
    * for illustration) that maintains an array in which the actual data
    * are held in even locations, and unrelated tag data are held in odd
    * locations. Its Spliterator ignores the tags.

    有一个类（并不是那么有用，除了插图）内部维护了一个数组，实际数据被存储在偶数位置上，不相关的标签数据被存储在奇数位置
    它的分割迭代器会忽略标签数据.

    省略...

    * <p>As an example how a parallel computation framework, such as the
    * {@code java.util.stream} package, would use Spliterator in a parallel
    * computation, here is one way to implement an associated parallel forEach,
    * that illustrates the primary usage idiom of splitting off subtasks until
    * the estimated amount of work is small enough to perform
    * sequentially. Here we assume that the order of processing across
    * subtasks doesn't matter; different (forked) tasks may further split
    * and process elements concurrently in undetermined order.  This
    * example uses a {@link java.util.concurrent.CountedCompleter};
    * similar usages apply to other parallel task constructions.

    以一个并行计算框架为例，例如stream包，在并行计算中使用分割迭代器，这是实现关联并行forEach的一种方式，这说明了分离子任务的主要用法，直到估计的工作量小到足以按顺序执行.
    这里我们假设处理子任务的顺序无关紧要，不同的任务以不确定的顺序进一步拆分和处理元素.这个例子使用了CountedCompleter.类似的用法适用于其他并行任务结构.
    
    省略...

    * @implNote
    * If the boolean system property {@code org.openjdk.java.util.stream.tripwire}
    * is set to {@code true} then diagnostic warnings are reported if boxing of
    * primitive values occur when operating on primitive subtype specializations.

    如果系统属性设置为true，在对基本类型进行操作时，如果出现原始值的装箱，则会报告诊断警告.

```

### 翻译Spliterator方法

```java

    /**
     * 若存在剩余元素，则对其执行指定动作，并返回true
     * 若不存在剩余元素，则返回false
     * 若分割迭代器具有ORDERED特征，按顺序获取到的下一个元素会执行指定动作，类似于Iterator中的hasNext与next的结合体
     * @param action 若存在剩余元素，执行指定动作
     * @return 是否存在剩余元素
     */
    boolean tryAdvance(Consumer<? super T> action);

    /**
     * 遍历剩余元素，并执行指定动作
     * 若分割迭代器具有ORDERED特征，按顺序获取到的下一个元素会执行指定动作
     * 这是分割迭代器的默认方法，应该尽可能的覆写该方法
     * @param action 若存在剩余元素，执行指定动作
     */
    default void forEachRemaining(Consumer<? super T> action) {
        do { } while (tryAdvance(action));
    }

    /**
     * 若切割迭代器1是可以拆分的，会返回新的一个切割迭代器2，两个迭代器平分原来迭代器1中的元素
     * 若切割迭代器无法拆分，则返回nul
     * @return 新的切割迭代器或null
     */
    Spliterator<T> trySplit();

    /**
     * 获取元素数量的估计值
     * 该值与未拆分的切割迭代器中调用forEachRemaining遍历所有剩余元素的个数相同，
     * 若元素数量无穷大、未定义则返回MAX_VALUE
     * 若切割迭代器已拆分，则该值是不正确的（大约是父元素数量的一半）
     * @return 元素数量的估计值或MAX_VALUE
     */
    long estimateSize();

    /**
     * 获取元素的确切数量
     * 若切割迭代器具有SIZED特征，则返回元素数量的估计值，否则返回-1
     * @return 元素数量或-1
     */
    default long getExactSizeIfKnown() {
        return (characteristics() & SIZED) == 0 ? -1L : estimateSize();
    }

    /**
     * 获取切割迭代器的特征值
     * 即使切割迭代器被多次拆分，特征值始终保持不变
     * @return 切割迭代器的特征值
     */
    int characteristics();

    /**
     * 切割迭代器中是否包含指定特征值
     * @param characteristics 指定特征值
     * @return 是否包含指定特征值
     */
    default boolean hasCharacteristics(int characteristics) {
        return (characteristics() & characteristics) == characteristics;
    }

    /**
     * 获取比较器
     * 若切割迭代器具有SORTED特征，按照指定顺序排序，则返回指定比较器
     * 若切割迭代器具有SORTED特征，按照自然顺序排序，则返回null
     * 若切割迭代器不具有SORTED特征，则抛出异常
     * @return 比较器或null
     */
    default Comparator<? super T> getComparator() {
        throw new IllegalStateException();
    }

```

### 翻译Spliterator属性

```java

    /**
     * 表示元素是有序的
     * 切割迭代器保证拆分与遍历会按照此顺序
     */
    public static final int ORDERED = 0x00000010;

    /**
     * 表示元素不能重复
     */
    public static final int DISTINCT = 0x00000001;

    /**
     * 表示元素是按指定规则进行排列（有指定比较器）
     */
    public static final int SORTED = 0x00000004;

    /**
     * 表示一个有限的大小值
     */
    public static final int SIZED = 0x00000040;

    /**
     * 表示没有null元素
     */
    public static final int NONNULL = 0x00000100;

    /**
     * 表示元素不可变，在遍历时不能进行添加、删除、替换
     */
    public static final int IMMUTABLE = 0x00000400;

    /**
     * 表示元素可以被多个线程并发修改而不需要外部的同步
     */
    public static final int CONCURRENT = 0x00001000;

    /**
     * 表示子切割迭代器具有SIZED特性
     */
    public static final int SUBSIZED = 0x00004000;

```

剩余的是切割迭代器针对基本类型的衍生接口，差别不大，不作重复翻译！