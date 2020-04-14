### 翻译JDK1.8 ConcurrentHashMap注释

```java

   /**
    * A hash table supporting full concurrency of retrievals and
    * high expected concurrency for updates. This class obeys the
    * same functional specification as {@link java.util.Hashtable}, and
    * includes versions of methods corresponding to each method of
    * {@code Hashtable}. However, even though all operations are
    * thread-safe, retrieval operations do <em>not</em> entail locking,
    * and there is <em>not</em> any support for locking the entire table
    * in a way that prevents all access.  This class is fully
    * interoperable with {@code Hashtable} in programs that rely on its
    * thread safety but not on its synchronization details.
    
    哈希表支持检索的完全并发性和更新的高期望并发性. 此类遵循与Hashtable相同的功能规范，并且包括与Hashtable的每个方法相对应的方法版本.
    但是，即使所有操作都是线程安全的，检索操作也不需要进行锁定，并且不支持以阻止所有访问的方式来锁定整个表.
    在依赖于其线程安全性但不依赖于其同步详细信息的程序中，此类可以与Hashtable完全互操作.

    * <p>Retrieval operations (including {@code get}) generally do not
    * block, so may overlap with update operations (including {@code put}
    * and {@code remove}). Retrievals reflect the results of the most
    * recently <em>completed</em> update operations holding upon their
    * onset. (More formally, an update operation for a given key bears a
    * <em>happens-before</em> relation with any (non-null) retrieval for
    * that key reporting the updated value.)  For aggregate operations
    * such as {@code putAll} and {@code clear}, concurrent retrievals may
    * reflect insertion or removal of only some entries.  Similarly,
    * Iterators, Spliterators and Enumerations return elements reflecting the
    * state of the hash table at some point at or since the creation of the
    * iterator/enumeration.  They do <em>not</em> throw {@link
    * java.util.ConcurrentModificationException ConcurrentModificationException}.
    * However, iterators are designed to be used by only one thread at a time.
    * Bear in mind that the results of aggregate status methods including
    * {@code size}, {@code isEmpty}, and {@code containsValue} are typically
    * useful only when a map is not undergoing concurrent updates in other threads.
    * Otherwise the results of these methods reflect transient states
    * that may be adequate for monitoring or estimation purposes, but not
    * for program control.
    
    检索操作（get）通常不会被阻止，因此可能与更新操作（包括put和remove）重叠. 检索反映了刚发生时最新完成的更新操作的结果. 
    更正式地说，指定键的更新操作happens-before于任何获取该键来报告更新的值. 对于putAll和clear之类的汇总操作，并发检索可能仅反映某些节点的插入或删除.
    类似地，Iterators、Spliterators、Enumerations返回的元素反映了在创建迭代器/枚举时或此后某个时刻哈希表的状态.
    它们不会抛出ConcurrentModificationException. 但是，迭代器被设计为一次只能由一个线程使用.
    请记住，包括size、isEmpty和containsValue在内的聚合状态方法的结果通常仅在map未在其他线程中进行并发更新时才有用.
    否则，这些方法的结果将反映可能足以用于监视或估计目的但不适用于程序控制的瞬态.

    * <p>The table is dynamically expanded when there are too many
    * collisions (i.e., keys that have distinct hash codes but fall into
    * the same slot modulo the table size), with the expected average
    * effect of maintaining roughly two bins per mapping (corresponding
    * to a 0.75 load factor threshold for resizing). There may be much
    * variance around this average as mappings are added and removed, but
    * overall, this maintains a commonly accepted time/space tradeoff for
    * hash tables.  However, resizing this or any other kind of hash
    * table may be a relatively slow operation. When possible, it is a
    * good idea to provide a size estimate as an optional {@code
    * initialCapacity} constructor argument. An additional optional
    * {@code loadFactor} constructor argument provides a further means of
    * customizing initial table capacity by specifying the table density
    * to be used in calculating the amount of space to allocate for the
    * given number of elements.  Also, for compatibility with previous
    * versions of this class, constructors may optionally specify an
    * expected {@code concurrencyLevel} as an additional hint for
    * internal sizing.  Note that using many keys with exactly the same
    * {@code hashCode()} is a sure way to slow down performance of any
    * hash table. To ameliorate impact, when keys are {@link Comparable},
    * this class may use comparison order among keys to help break ties.
    
    当发生太多冲突时，该表将动态展开，预期的平均效果是每个映射大约维护两个bin. 添加和删除映射时，此平均值附近可能会有很大差异，但是总的来说，这保持了哈希表的公认的时间/空间权衡.
    但是，调整此哈希表或任何其他类型的哈希表的大小可能是一个相对较慢的操作. 可能的话，建议将大小估算值作为可选的initialCapacity构造函数参数提供. 
    附加的可选loadFactor构造函数参数提供了另一种自定义初始表容量的方法，通过执行表密度来计算要为给定数量的元素分配的空间.
    另外，为了与此类的早期版本兼容，构造函数可以选择指定期望的concurrencyLevel作为内部调整的附加提示.
    请注意，使用完全相同hashCode的许多键是会降低任何哈希表性能.
    为了改善影响，当键为Comparable时，此类可以使用键之间的比较顺序来帮助打破平局.

    * <p>A {@link Set} projection of a ConcurrentHashMap may be created
    * (using {@link #newKeySet()} or {@link #newKeySet(int)}), or viewed
    * (using {@link #keySet(Object)} when only keys are of interest, and the
    * mapped values are (perhaps transiently) not used or all take the
    * same mapping value.

    可以创建ConcurrentHashMap的Set集合（使用newKeySet或newKeySet（int）），当只关注键或不使用映射的值，使用keySet（Object）

    * <p>A ConcurrentHashMap can be used as scalable frequency map (a
    * form of histogram or multiset) by using {@link
    * java.util.concurrent.atomic.LongAdder} values and initializing via
    * {@link #computeIfAbsent computeIfAbsent}. For example, to add a count
    * to a {@code ConcurrentHashMap<String,LongAdder> freqs}, you can use
    * {@code freqs.computeIfAbsent(k -> new LongAdder()).increment();}
    
    通过使用LongAdder值和computeIfAbsent进行初始化，ConcurrentHashMap可用作可伸缩的频率图.
    例如，要将计数添加到ConcurrentHashMap <String，LongAdder> freqs，可以使用freqs.computeIfAbsent(k-> new LongAdder()).increment()

    * <p>This class and its views and iterators implement all of the
    * <em>optional</em> methods of the {@link Map} and {@link Iterator}
    * interfaces.
    
    此类及其视图和迭代器实现Map和Iterator接口的所有可选方法.

    * <p>Like {@link Hashtable} but unlike {@link HashMap}, this class
    * does <em>not</em> allow {@code null} to be used as a key or value.
    
    与Hashtable相似，但与HashMap不同的是，此类不允许null用作键或值.

    * <p>ConcurrentHashMaps support a set of sequential and parallel bulk
    * operations that, unlike most {@link Stream} methods, are designed
    * to be safely, and often sensibly, applied even with maps that are
    * being concurrently updated by other threads; for example, when
    * computing a snapshot summary of the values in a shared registry.
    * There are three kinds of operation, each with four forms, accepting
    * functions with Keys, Values, Entries, and (Key, Value) arguments
    * and/or return values. Because the elements of a ConcurrentHashMap
    * are not ordered in any particular way, and may be processed in
    * different orders in different parallel executions, the correctness
    * of supplied functions should not depend on any ordering, or on any
    * other objects or values that may transiently change while
    * computation is in progress; and except for forEach actions, should
    * ideally be side-effect-free. Bulk operations on {@link java.util.Map.Entry}
    * objects do not support method {@code setValue}.
    
    ConcurrentHashMaps支持一组顺序和并行的批量操作，与大多数Stream方法不同，该操作被设计为即使其他线程并发更新也可以安全且通常明智地应用.
    例如，当计算共享注册表中值的快照摘要时. 共有三种操作，每种都有四种形式，它们接受带有键、值、节点和(键, 值)参数和/或返回值的函数.
    因为ConcurrentHashMap的元素没有以任何特定的方式排序，并且可以在不同的并行执行中以不同的顺序进行处理，所以提供的函数的正确性不应该依赖于任何顺序，也不应该依赖于任何其他对象或值，这些对象或值在计算过程中可能会瞬时改变.
    并且除了forEach动作外，理想情况下应该没有副作用.
    Entry对象上的批量操作不支持setValue方法.

    * <ul>
    * <li> forEach: Perform a given action on each element.
    * A variant form applies a given transformation on each element
    * before performing the action.</li>
    
    forEach: 对每个元素执行给定的操作. 变体形式在执行操作之前对每个元素应用给定的转换.

    * <li> search: Return the first available non-null result of
    * applying a given function on each element; skipping further
    * search when a result is found.</li>
    
    search: 

    * <li> reduce: Accumulate each element.  The supplied reduction
    * function cannot rely on ordering (more formally, it should be
    * both associative and commutative).  There are five variants:
    
    reduce:

    * <ul>
    *
    * <li> Plain reductions. (There is not a form of this method for
    * (key, value) function arguments since there is no corresponding
    * return type.)</li>
    *
    * <li> Mapped reductions that accumulate the results of a given
    * function applied to each element.</li>
    *
    * <li> Reductions to scalar doubles, longs, and ints, using a
    * given basis value.</li>
    *
    * </ul>
    * </li>
    * </ul>
    *
    * <p>These bulk operations accept a {@code parallelismThreshold}
    * argument. Methods proceed sequentially if the current map size is
    * estimated to be less than the given threshold. Using a value of
    * {@code Long.MAX_VALUE} suppresses all parallelism.  Using a value
    * of {@code 1} results in maximal parallelism by partitioning into
    * enough subtasks to fully utilize the {@link
    * ForkJoinPool#commonPool()} that is used for all parallel
    * computations. Normally, you would initially choose one of these
    * extreme values, and then measure performance of using in-between
    * values that trade off overhead versus throughput.

    这些批量操作接受parallelismThreshold参数. 如果CurrentHashMap的容量大小小于给定的阈值，则方法将按顺序进行. 
    使用Long.MAX_VALUE值抑制所有并行性. 
    使用1值划分为足够的子任务来充分利用用于所有并行计算的ForkJoinPool＃commonPool来实现最大的并行度.
    通常，您最初会选择这些极值之一，然后使用权衡开销与吞吐量的中间值来衡量性能.

    * <p>The concurrency properties of bulk operations follow
    * from those of ConcurrentHashMap: Any non-null result returned
    * from {@code get(key)} and related access methods bears a
    * happens-before relation with the associated insertion or
    * update.  The result of any bulk operation reflects the
    * composition of these per-element relations (but is not
    * necessarily atomic with respect to the map as a whole unless it
    * is somehow known to be quiescent).  Conversely, because keys
    * and values in the map are never null, null serves as a reliable
    * atomic indicator of the current lack of any result.  To
    * maintain this property, null serves as an implicit basis for
    * all non-scalar reduction operations. For the double, long, and
    * int versions, the basis should be one that, when combined with
    * any other value, returns that other value (more formally, it
    * should be the identity element for the reduction). Most common
    * reductions have these properties; for example, computing a sum
    * with basis 0 or a minimum with basis MAX_VALUE.
    
    批量操作的并发属性遵循ConcurrentHashMap的并发属性: 从get(key)和相关访问方法返回的任何非空结果都具有与关联的插入或更新相关的happens-before关系.
    任何批量操作的结果都反映了每个元素关系的组成. 相反，由于映射中的键和值永远不会为null，因此null可作为当前缺少任何结果的可靠原子指示符.
    为了保持此属性，null用作所有非标量归约操作的隐式基础. 对于double，long和int版本，基数应为当与任何其他值组合时返回该其他值的基数.
    最常见的还原具有这些特性. 例如，计算基数为0的总和或基数为MAX_VALUE的最小值.

    * <p>Search and transformation functions provided as arguments
    * should similarly return null to indicate the lack of any result
    * (in which case it is not used). In the case of mapped
    * reductions, this also enables transformations to serve as
    * filters, returning null (or, in the case of primitive
    * specializations, the identity basis) if the element should not
    * be combined. You can create compound transformations and
    * filterings by composing them yourself under this "null means
    * there is nothing there now" rule before using them in search or
    * reduce operations.
    
    作为参数提供的搜索和转换函数应类似地返回null以指示缺少任何结果. 对于映射的约简，这还使转换能够用作过滤器，如果不应组合元素，则返回null.
    您可以通过在"null表示现在没有内容"规则下将它们自己合成来创建复合转换和过滤，然后再在搜索或归约运算中使用它们.

    * <p>Methods accepting and/or returning Entry arguments maintain
    * key-value associations. They may be useful for example when
    * finding the key for the greatest value. Note that "plain" Entry
    * arguments can be supplied using {@code new
    * AbstractMap.SimpleEntry(k,v)}.
    
    接受和/或返回Entry参数的方法维护键值关联. 例如，当找到最大价值的key时，它们可能会很有用.
    请注意，可以使用new AbstractMap.SimpleEntry(k，v)提供"普通"节点参数

    * <p>Bulk operations may complete abruptly, throwing an
    * exception encountered in the application of a supplied
    * function. Bear in mind when handling such exceptions that other
    * concurrently executing functions could also have thrown
    * exceptions, or would have done so if the first exception had
    * not occurred.
    
    批量操作可能会突然完成，从而引发所提供函数的应用程序抛出异常. 处理此类异常时请记住，其他并发执行的函数也可能抛出异常，或者如果没有发生第一个异常，则可能会抛出异常.

    * <p>Speedups for parallel compared to sequential forms are common
    * but not guaranteed.  Parallel operations involving brief functions
    * on small maps may execute more slowly than sequential forms if the
    * underlying work to parallelize the computation is more expensive
    * than the computation itself.  Similarly, parallelization may not
    * lead to much actual parallelism if all processors are busy
    * performing unrelated tasks.
    
    与顺序形式相比，并行加速是常见的，但不能保证. 如果并行化计算的基础工作比计算本身更昂贵，则涉及CuccrentHashMap上简短功能的并行操作的执行速度可能会比顺序形式慢.
    同样，如果所有处理器都在忙于执行无关的任务，那么并行化可能不会导致很多实际的并行性.

    * <p>All arguments to all task methods must be non-null.
    
    所有任务方法的所有参数都必须为非空.

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.

    */

```

### 翻译JDK1.8 ConcurrentHashMap类中的注释

```java

   /*
    * Overview:

    概述：

    * The primary design goal of this hash table is to maintain
    * concurrent readability (typically method get(), but also
    * iterators and related methods) while minimizing update
    * contention. Secondary goals are to keep space consumption about
    * the same or better than java.util.HashMap, and to support high
    * initial insertion rates on an empty table by many threads.
    
    此哈希表的主要设计目标是保持并发可读性，同时最大程度地减少更新争用. 次要目标是使空间消耗保持与HashMap相同或更好，并支持多线程在空表上的高初始插入率.

    * This map usually acts as a binned (bucketed) hash table.  Each
    * key-value mapping is held in a Node.  Most nodes are instances
    * of the basic Node class with hash, key, value, and next
    * fields. However, various subclasses exist: TreeNodes are
    * arranged in balanced trees, not lists.  TreeBins hold the roots
    * of sets of TreeNodes. ForwardingNodes are placed at the heads
    * of bins during resizing. ReservationNodes are used as
    * placeholders while establishing values in computeIfAbsent and
    * related methods.  The types TreeBin, ForwardingNode, and
    * ReservationNode do not hold normal user keys, values, or
    * hashes, and are readily distinguishable during search etc
    * because they have negative hash fields and null key and value
    * fields. (These special nodes are either uncommon or transient,
    * so the impact of carrying around some unused fields is
    * insignificant.)
    
    该映射通常用作装箱(存储桶)的哈希表. 每个键值映射都保存在一个节点中. 大多数节点是基本节点类的实例，具有hash、key、value、next属性.
    但是，存在各种子类：TreeNode以平衡树而不是列表的形式排列. TreeBins拥有TreeNode集合的根. 在调整大小期间，ForwardingNodes放置在bins的头部. 
    在computeIfAbsent和相关方法中建立值时，ReservationNode用作占位符. 
    TreeBin、ForwardingNode和ReservationNode类型不包含常规用户key、value或hash，并且在搜索等过程中易于区分，因为它们具有负的hash以及空key和value字段.
    这些特殊的节点是不常见的或瞬态的，因此携带一些未使用的字段的影响是微不足道的.

    * The table is lazily initialized to a power-of-two size upon the
    * first insertion.  Each bin in the table normally contains a
    * list of Nodes (most often, the list has only zero or one Node).
    * Table accesses require volatile/atomic reads, writes, and
    * CASes.  Because there is no other way to arrange this without
    * adding further indirections, we use intrinsics
    * (sun.misc.Unsafe) operations.
    
    该表在第一次插入时被延迟初始化为2的幂次方. 表中的每个bin通常包含一个Node列表. 哈希表访问要求valatile/原子性读、写和CAS.
    由于没有其他方法可以在不增加其他间接调用的情况下进行安排，因此我们使用内部函数(sun.misc.Unsafe).

    * We use the top (sign) bit of Node hash fields for control
    * purposes -- it is available anyway because of addressing
    * constraints.  Nodes with negative hash fields are specially
    * handled or ignored in map methods.
    
    我们将Node哈希字段的最高（符号）位用于控制目的. 由于地址限制，它仍然可用. 带有负哈希字段的节点在map方法中被特殊处理或忽略.

    * Insertion (via put or its variants) of the first node in an
    * empty bin is performed by just CASing it to the bin.  This is
    * by far the most common case for put operations under most
    * key/hash distributions.  Other update operations (insert,
    * delete, and replace) require locks.  We do not want to waste
    * the space required to associate a distinct lock object with
    * each bin, so instead use the first node of a bin list itself as
    * a lock. Locking support for these locks relies on builtin
    * "synchronized" monitors.
    
    将第一个节点插入(通过put或其变体)到空容器中，只需将其CAS到容器中即可（第一个节点的插入使用CAS）.
    到目前为止，这是大多数键/哈希分布下的put操作的最常见情况. 其他更新操作（插入，删除和替换）需要锁. 
    我们不想浪费空间，要求每个bin关联独特的锁，因此可以将bin列表本身的第一个节点用作锁. 对这些锁的锁定支持依赖于内置的“同步"监视器（使用同步代码块）

    * Using the first node of a list as a lock does not by itself
    * suffice though: When a node is locked, any update must first
    * validate that it is still the first node after locking it, and
    * retry if not. Because new nodes are always appended to lists,
    * once a node is first in a bin, it remains first until deleted
    * or the bin becomes invalidated (upon resizing).
    
    但是，将列表的第一个节点用作锁并不能满足自身：当一个节点被锁定时，任何更新都必须在锁定它之后首先验证它仍然是第一个节点，如果不是就要重试.
    由于新节点总是添加到列表中，因此一旦节点首次位于bin中，它将一直保持到删除或bin无效（调整大小后）.

    * The main disadvantage of per-bin locks is that other update
    * operations on other nodes in a bin list protected by the same
    * lock can stall, for example when user equals() or mapping
    * functions take a long time.  However, statistically, under
    * random hash codes, this is not a common problem.  Ideally, the
    * frequency of nodes in bins follows a Poisson distribution
    * (http://en.wikipedia.org/wiki/Poisson_distribution) with a
    * parameter of about 0.5 on average, given the resizing threshold
    * of 0.75, although with a large variance because of resizing
    * granularity. Ignoring variance, the expected occurrences of
    * list size k are (exp(-0.5) * pow(0.5, k) / factorial(k)). The
    * first values are:
    
    每个bin锁的主要缺点是，受相同锁保护的bin列表中其他节点上的其他更新操作可能会停止, 例如当用户equals或mappin函数花费很长时间时.
    但是，从统计学上讲，在随机哈希码下，这不是一个普遍的问题. 
    理想情况下，在给定调整大小阈值0.75的情况下，哈希表中节点的分布遵循泊松分布，平均参数约为0.5，尽管由于调整大小粒度而存在较大差异.

    * 0:    0.60653066
    * 1:    0.30326533
    * 2:    0.07581633
    * 3:    0.01263606
    * 4:    0.00157952
    * 5:    0.00015795
    * 6:    0.00001316
    * 7:    0.00000094
    * 8:    0.00000006
    * more: less than 1 in ten million
    *
    * Lock contention probability for two threads accessing distinct
    * elements is roughly 1 / (8 * #elements) under random hashes.
    
    在随机哈希下，两个线程访问不同元素的锁争用概率约为1 /（8 * elements）.

    * Actual hash code distributions encountered in practice
    * sometimes deviate significantly from uniform randomness.  This
    * includes the case when N > (1<<30), so some keys MUST collide.
    * Similarly for dumb or hostile usages in which multiple keys are
    * designed to have identical hash codes or ones that differs only
    * in masked-out high bits. So we use a secondary strategy that
    * applies when the number of nodes in a bin exceeds a
    * threshold. These TreeBins use a balanced tree to hold nodes (a
    * specialized form of red-black trees), bounding search time to
    * O(log N).  Each search step in a TreeBin is at least twice as
    * slow as in a regular list, but given that N cannot exceed
    * (1<<64) (before running out of addresses) this bounds search
    * steps, lock hold times, etc, to reasonable constants (roughly
    * 100 nodes inspected per operation worst case) so long as keys
    * are Comparable (which is very common -- String, Long, etc).
    * TreeBin nodes (TreeNodes) also maintain the same "next"
    * traversal pointers as regular nodes, so can be traversed in
    * iterators in the same way.
    
    在实践中遇到的实际哈希码分布有时会明显偏离均匀随机性. 这包括N > (1 << 30)的情况, 因此某些键必定发生冲突.
    类似地，对于愚蠢或恶意的用法，其中多个密钥被设计为具有相同的哈希码或仅在被掩盖的高位上有所不同的哈希码.
    因此，我们使用了二级策略，该策略在bin中的节点数超过阈值时适用. 
    这些TreeBins使用平衡树来保存节点（红黑树的一种特殊形式），将查找时间限制为O（log N）.
    TreeBin中的每个查找步骤至少比常规列表中的速度慢两倍，但鉴于N不能超过（1 << 64）（在用完地址之前），这会合理地限制查找步骤，锁定保持时间，只要键是可比较的，就可以使用常量（每次操作最多检查100个节点）.
    TreeBin节点（TreeNodes）还维护与常规节点相同的"下一个"遍历指针，因此可以在迭代器中以相同的方式遍历.

    * The table is resized when occupancy exceeds a percentage
    * threshold (nominally, 0.75, but see below).  Any thread
    * noticing an overfull bin may assist in resizing after the
    * initiating thread allocates and sets up the replacement array.
    * However, rather than stalling, these other threads may proceed
    * with insertions etc.  The use of TreeBins shields us from the
    * worst case effects of overfilling while resizes are in
    * progress.  Resizing proceeds by transferring bins, one by one,
    * from the table to the next table. However, threads claim small
    * blocks of indices to transfer (via field transferIndex) before
    * doing so, reducing contention.  A generation stamp in field
    * sizeCtl ensures that resizings do not overlap. Because we are
    * using power-of-two expansion, the elements from each bin must
    * either stay at same index, or move with a power of two
    * offset. We eliminate unnecessary node creation by catching
    * cases where old nodes can be reused because their next fields
    * won't change.  On average, only about one-sixth of them need
    * cloning when a table doubles. The nodes they replace will be
    * garbage collectable as soon as they are no longer referenced by
    * any reader thread that may be in the midst of concurrently
    * traversing table.  Upon transfer, the old table bin contains
    * only a special forwarding node (with hash field "MOVED") that
    * contains the next table as its key. On encountering a
    * forwarding node, access and update operations restart, using
    * the new table.

    当占用率超过百分比阈值（标称值为0.75，但请参见下文）时，将调整表的大小（发生扩容）.
    在启动线程分配并设置替换数组之后，任何注意到bin过多的线程都可以帮助调整大小. 但是，这些其他线程可能会进行插入等操作，而不是停滞不前. 
    使用TreeBins可以防止我们在调整大小时发生过度填充的最坏情况. 通过将bin从旧哈希表转移到另一个哈希表来调整大小.
    但是，在这样做之前线程要声明小索引块要进行传输（通过字段transferIndex）（表示还剩下多少区间供其他线程帮助迁移），从而减少了争用（每个线程都会分配一个迁移数组的区间）. 
    sizeCtl字段中的世代标记可确保调整大小不重叠（多个线程不会发生同时扩容数组，只会帮助迁移节点）.
    因为我们使用2的幂次方来扩展，所以每个bin中的元素必须保持相同的索引或以2个偏移量的幂移动.
    我们通过捕获旧节点可以重复使用的情况来消除不必要的节点创建，因为它们的下一个字段不会更改.
    平均而言，当表加倍时，只有大约六分之一需要克隆. 
    一旦它们被并发遍历表中的任何读取器线程不再引用，它们替换的节点将立即被垃圾回收.
    传输后，旧哈希表中仅包含一个特殊的转发节点（哈希字段为"MOVED"），该节点包含下一个表作为其关键字. 遇到转发节点时，将使用新表重新启动访问和更新操作.

    * Each bin transfer requires its bin lock, which can stall
    * waiting for locks while resizing. However, because other
    * threads can join in and help resize rather than contend for
    * locks, average aggregate waits become shorter as resizing
    * progresses.  The transfer operation must also ensure that all
    * accessible bins in both the old and new table are usable by any
    * traversal.  This is arranged in part by proceeding from the
    * last bin (table.length - 1) up towards the first.  Upon seeing
    * a forwarding node, traversals (see class Traverser) arrange to
    * move to the new table without revisiting nodes.  To ensure that
    * no intervening nodes are skipped even when moved out of order,
    * a stack (see class TableStack) is created on first encounter of
    * a forwarding node during a traversal, to maintain its place if
    * later processing the current table. The need for these
    * save/restore mechanics is relatively rare, but when one
    * forwarding node is encountered, typically many more will be.
    * So Traversers use a simple caching scheme to avoid creating so
    * many new TableStack nodes. (Thanks to Peter Levart for
    * suggesting use of a stack here.)
    
    每个bin转移都需要其bin对应的锁，该bin在调整大小时可能会暂停等待锁. 但是，由于其他线程可以加入并帮助调整大小而不是争夺锁，因此随着调整大小的进行，平均聚合等待时间会缩短.
    转移操作还必须确保任何遍历都可以使用旧表和新表中的所有可访问bin. 这是通过从最后一个bin（table.length-1）到first进行部分安排的.
    看到转发节点后，遍历（请参见Traverser类）安排在不重新访问节点的情况下移至新表.
    为了确保即使在无序移动时也不会跳过中间节点，在遍历过程中第一次遇到转发节点时会创建一个堆栈（请参见TableStack类），为了在以后处理当前表时保持其位置.
    对这些保存/恢复机制的需求相对很少，但是当遇到一个转发节点时，通常会更多. 
    因此，遍历器使用一种简单的缓存方案来避免创建许多新的TableStack节点.

    * The traversal scheme also applies to partial traversals of
    * ranges of bins (via an alternate Traverser constructor)
    * to support partitioned aggregate operations.  Also, read-only
    * operations give up if ever forwarded to a null table, which
    * provides support for shutdown-style clearing, which is also not
    * currently implemented.
    
    遍历方案也适用于部分范围的bin（通过备用Traverser构造函数），以支持分区聚合操作. 
    此外，只读操作如果转发到空表也将放弃，该操作提供对关机样式清除的支持，该功能目前也未实现.

    * Lazy table initialization minimizes footprint until first use,
    * and also avoids resizings when the first operation is from a
    * putAll, constructor with map argument, or deserialization.
    * These cases attempt to override the initial capacity settings,
    * but harmlessly fail to take effect in cases of races.
    
    延迟表初始化可最大程度地减少首次使用之前的占用空间，并且当第一个操作来自putAll，带有map参数的构造函数或反序列化时，还避免了调整大小.
    这些情况试图超越初始容量设置，但在比赛中未能生效.

    * The element count is maintained using a specialization of
    * LongAdder. We need to incorporate a specialization rather than
    * just use a LongAdder in order to access implicit
    * contention-sensing that leads to creation of multiple
    * CounterCells.  The counter mechanics avoid contention on
    * updates but can encounter cache thrashing if read too
    * frequently during concurrent access. To avoid reading so often,
    * resizing under contention is attempted only upon adding to a
    * bin already holding two or more nodes. Under uniform hash
    * distributions, the probability of this occurring at threshold
    * is around 13%, meaning that only about 1 in 8 puts check
    * threshold (and after resizing, many fewer do so).
    
    元素计数使用LongAdder的特殊化来维护. 我们需要合并一个专业化对象，而不仅仅是使用LongAdder来访问隐式竞争感应，从而导致创建多个CounterCell.
    计数器机制避免了更新争用，但如果在并发访问期间读取得太频繁，则可能会遇到缓存崩溃的情况. 
    为避免经常读取，仅在添加到已容纳两个或更多节点的容器中后，才尝试在竞争下调整大小.
    在统一的哈希分布下，在阈值处发生这种情况的可能性约为13％，这意味着只有八分之一的位置放置了检查阈值（并且在调整大小之后，这样做的人要少得多）.

    * TreeBins use a special form of comparison for search and
    * related operations (which is the main reason we cannot use
    * existing collections such as TreeMaps). TreeBins contain
    * Comparable elements, but may contain others, as well as
    * elements that are Comparable but not necessarily Comparable for
    * the same T, so we cannot invoke compareTo among them. To handle
    * this, the tree is ordered primarily by hash value, then by
    * Comparable.compareTo order if applicable.  On lookup at a node,
    * if elements are not comparable or compare as 0 then both left
    * and right children may need to be searched in the case of tied
    * hash values. (This corresponds to the full list search that
    * would be necessary if all elements were non-Comparable and had
    * tied hashes.) On insertion, to keep a total ordering (or as
    * close as is required here) across rebalancings, we compare
    * classes and identityHashCodes as tie-breakers. The red-black
    * balancing code is updated from pre-jdk-collections
    * (http://gee.cs.oswego.edu/dl/classes/collections/RBCell.java)
    * based in turn on Cormen, Leiserson, and Rivest "Introduction to
    * Algorithms" (CLR).
    
    TreeBins使用一种特殊的比较形式来进行查找和相关操作（这是我们不能使用TreeMap等现有集合的主要原因）.
    TreeBins包含Comparable元素，但可能包含其他元素，以及对于同一个T是可比较但不一定要比较的元素，因此我们无法在其中调用compareTo.
    为了解决这个问题，树主要是按哈希值排序，然后按Comparable.compareTo排序（如果适用）.
    在节点上查找时，如果元素不可比较或比较为0，则在绑定哈希值的情况下，可能需要同时搜索左右子节点.
    （如果所有元素都是不可比较的并且具有散列，则这对应于完整列表搜索.）
    插入时，为了保持重新平衡之间的总体排序（或此处要求的接近程度），我们将类和identityHashCodes作为决胜局进行比较.
    红黑平衡代码从jdk之前的收藏集更新而来，依次基于Cormen、Leiserson和Rivest算法（CLR）.

    * TreeBins also require an additional locking mechanism.  While
    * list traversal is always possible by readers even during
    * updates, tree traversal is not, mainly because of tree-rotations
    * that may change the root node and/or its linkages.  TreeBins
    * include a simple read-write lock mechanism parasitic on the
    * main bin-synchronization strategy: Structural adjustments
    * associated with an insertion or removal are already bin-locked
    * (and so cannot conflict with other writers) but must wait for
    * ongoing readers to finish. Since there can be only one such
    * waiter, we use a simple scheme using a single "waiter" field to
    * block writers.  However, readers need never block.  If the root
    * lock is held, they proceed along the slow traversal path (via
    * next-pointers) until the lock becomes available or the list is
    * exhausted, whichever comes first. These cases are not fast, but
    * maximize aggregate expected throughput.
    
    TreeBins还需要其他锁定机制. 尽管即使在更新过程中，读者始终可以遍历列表，但无法遍历树，主要是因为树的旋转可能会更改根节点和/或其链接.
    TreeBins包括寄生在主要bin同步策略上的简单读写锁定机制：与插入或删除相关的结构调整已被bin锁定（因此不能与其他编写器发生冲突），但必须等待正在进行的读取器完成.
    由于只能有一个这样的服务员，因此我们使用一个简单的方案，即使用单个"服务员"字段来阻止作者.
    但是，读者永远不需要阻塞。 如果持有根锁，则它们将沿着慢速遍历路径（通过下一个指针）前进，直到锁可用或列表用完为止，以先到者为准.
    这些情况不是很快，但是会最大化合计预期吞吐量.

    * Maintaining API and serialization compatibility with previous
    * versions of this class introduces several oddities. Mainly: We
    * leave untouched but unused constructor arguments refering to
    * concurrencyLevel. We accept a loadFactor constructor argument,
    * but apply it only to initial table capacity (which is the only
    * time that we can guarantee to honor it.) We also declare an
    * unused "Segment" class that is instantiated in minimal form
    * only when serializing.
    
    与此类的早期版本保持API和序列化兼容性会带来一些奇怪的问题. 主要是：我们保持不变，但未使用的构造函数参数引用了concurrencyLevel.
    我们接受一个loadFactor构造函数参数，但仅将其应用于初始表容量（这是我们唯一可以保证兑现它的时间。）我们还声明了一个未使用的"Segment"类，该类仅在序列化时以最小形式实例化.

    * Also, solely for compatibility with previous versions of this
    * class, it extends AbstractMap, even though all of its methods
    * are overridden, so it is just useless baggage.
    
    同样，仅出于与此类以前版本的兼容性的考虑，它扩展了AbstractMap，即使其所有方法都被覆盖，因此也只是无济于事.

    * This file is organized to make things a little easier to follow
    * while reading than they might otherwise: First the main static
    * declarations and utilities, then fields, then main public
    * methods (with a few factorings of multiple public methods into
    * internal ones), then sizing methods, trees, traversers, and
    * bulk operations.

    该文件的结构使阅读时的操作比其他情况容易一些：首先是主要的静态声明和实用程序，然后是字段，然后是主要的公共方法（将多个公共方法分解成一些内部方法），然后调整方法、树、遍历器和批量操作的大小.

    */


```