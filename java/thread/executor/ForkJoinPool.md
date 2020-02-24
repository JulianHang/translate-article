### 翻译ForkJoin论文注释——由Doug Lea写的论文


```java
    /**
        ABSTRACT
            This paper describes the design, implementation, and
            performance of a Java framework for supporting a style of
            parallel programming in which problems are solved by
            (recursively) splitting them into subtasks that are solved in
            parallel, waiting for them to complete, and then composing
            results. The general design is a variant of the work−stealing
            framework devised for Cilk. The main implementation
            techniques surround efficient construction and management of
            tasks queues and worker threads. The measured performance
            shows good parallel speedups for most programs, but also
            suggests possible improvements.

            本文描述了用于支持并行编程样式的Java框架的设计，实现和性能，在Java框架中，通过（递归）将问题分解为并行解决的子任务，等待它们完成，然后合成结果来解决问题.
            大体设计是Cilk设计的工作窃取框架的变体. 主要实现技术围绕任务队列和工作线程的有效构建与管理. 对于大部分程序，性能测试显示具有良好的并行加速，但暗示了可能的改进.

        1. INTRODUCTION

            Fork/Join parallelism is among the simplest and most effective
            design techniques for obtaining good parallel performance.
            Fork/join algorithms are parallel versions of familiar divide−
            and−conquer algorithms, taking the typical form:

            Fork/Join并行性是获得良好并行性能的最简单、最有效的设计技术之一. Fork/Join算法是熟悉的分而治之算法的并行版本，采用典型形式：

                Result solve(Problem problem) {
                    if (problem is small)
                        directly solve problem
                    else {
                        split problem into independent parts
                        fork new subtasks to solve each part
                        join all subtasks
                        compose result from subresults
                    }
                }
            
            The fork operation starts a new parallel fork/join subtask. The
            join operation causes the current task not to proceed until the
            forked subtask has completed. Fork/join algorithms, like other
            divide−and−conquer algorithms, are nearly always recursive,
            repeatedly splitting subtasks until they are small enough to solve
            using simple, short sequential methods.

            fork操作启动一个新的并行fork/join子任务. join操作使当前任务不会继续运行直到子任务完成. 像其他分而治之算法一样，Fork/Join算法几乎总是递归的，反复拆分子任务，直到子任务足够小以至于可以使用简单的短顺序方法进行求解

            Some associated programming techniques and examples are
            discussed in section 4.4 of Concurrent Programming in Java,
            second edition [7]. This paper discusses the design (section 2),
            implementation (section 3), and performance (section 4) of
            FJTask, a JavaTM framework that supports this programming
            style. FJTask is available as part of the util.concurrent
            package from http://gee.cs.oswego.edu.

            第二版Java的并发编程中的4.4节讨论了一些相关的编程技术和示例. 本书讨论了设计、实现、FJTask的性能、支持这种编程风格的JavaTM框架. 

        2. DESIGN

            Fork/join programs can be run using any framework that
            supports construction of subtasks that are executed in parallel,
            along with a mechanism for waiting out their completion.
            However, the java.lang.Thread class (as well as POSIX
            pthreads, upon which Java threads are often based) are
            suboptimal vehicles for supporting fork/join programs:

            可以使用任何支持并行执行子任务构建的框架以及等待其完成的机制来运行Fork/Join程序. 但是，java.lang.Thread类（以及Java线程通常基于的POSIX pthreads）对于支持fork/join程序不是最佳的工具.

                • Fork/join tasks have simple and regular synchronization and
                management requirements. The computation graphs
                produced by fork/join tasks admit much more efficient
                scheduling tactics than needed for general−purpose threads.
                For example, fork/join tasks never need to block except to
                wait out subtasks. Thus, the overhead and bookkeeping
                necessary for tracking blocked general−purpose threads are
                wasted.

                Fork/join任务具有简单且定期的同步和管理要求. 由fork/join任务产生的计算图比通用线程所需要的调度策略有效得多. 例如，fork/join任务不需要阻塞除了等待子任务. 因此，跟踪阻塞的通用线程所需的开销和簿记是浪费的.

                • Given reasonable base task granularities, the cost of
                constructing and managing a thread can be greater than the
                computation time of the task itself. While granularities can
                and should be subject to tuning when running programs on
                particular platforms, the extremely coarse granularities
                necessary to outweigh thread overhead limits opportunities
                for exploiting parallelism.

                给定合理的基本任务粒度，构造和管理线程的成本可能大于任务本身的计算时间. 当在特定平台上运行程序时应该对粒度进行调整，但是超过线程开销所必需的极其粗糙的粒度限制了利用并行性的机会.

            In short, standard thread frameworks are just too heavy to
            support most fork/join programs. But since threads form the
            basis of many other styles of concurrent and parallel
            programming as well, it is impossible (or at least impractical) to
            remove overhead or tune scheduling of threads themselves just
            for the sake of supporting this style.

            简而言之，标准线程框架太重了，无法支持大多数fork/join程序. 但是，由于线程也是并发和并行编程的许多其他样式的基础，因此仅为了支持这种样式就不可能（或者至少不切实际）消除开销或调整线程本身的调度.

            While the ideas surely have a longer heritage, the first published
            framework to offer systematic solutions to these problems was
            Cilk[5]. Cilk and other lightweight executable frameworks layer
            special−purpose fork/join support on top of an operating
            system’s basic thread or process mechanisms. This tactic applies
            equally well to Java, even though Java threads are in turn
            layered onto lower−level OS capabilities. The main advantage
            of creating such a Java lightweight execution framework is to
            enable fork/join programs to be written in a more portable
            fashion and to run on the wide range of systems supporting
            JVMs.

            虽然这些想法肯定具有较长的传承，但第一个为这些问题提供系统解决方案的已发布框架是Cilk. Cilk和其他轻量级可执行框架在操作系统的基本线程或进程机制之上提供了特殊的fork/join支持.
            即使Java线程依次被置于较低级别的OS功能上，该策略也同样适用于Java. 创建这样的Java轻量级执行框架的主要优点是可以使fork/join程序以更可移植的方式编写，并且可以在支持JVM的各种系统上运行.

            The FJTask framework is based on a variant of the design
            used in Cilk. Other variants are seen in Hood[4], Filaments[8],
            stackthreads[10], and related systems relying on lightweight executable tasks. 
            All of these frameworks map tasks to threads
            in about the same way that operating systems map threads to
            CPUs, but exploit the simplicity, regularity, and constraints of
            fork/join programs in performing the mapping. While all of
            these frameworks can accommodate (to varying extents) parallel
            programs written in different styles, they optimize for fork/join
            designs:

            FJTask框架基于Cilk中使用的设计变体. 其他变形见于Hood, Filaments, stackthreads, 以及依赖轻量级可执行任务的相关系统.
            所有这些框架都以与操作系统将线程映射到CPU相同的方式将任务映射到线程，但是在执行映射时利用了fork/join程序的简单性，规则性和约束.
            尽管所有这些框架都可以（在不同程度上）容纳以不同样式编写的并行程序，但它们针对fork/join设计进行了优化.

                • A pool of worker threads is established. Each worker thread
                is a standard ("heavy") thread (here, an instance of Thread
                subclass FJTaskRunner) that processes tasks held in
                queues. Normally, there are as many worker threads as there
                are CPUs on a system. In native frameworks such as Cilk,
                these are mapped to kernel threads or lightweight processes,
                and in turn to CPUs. In Java, the JVM and OS must be
                trusted to map these threads to CPUs. However, this is a
                very simple task for the OS, since these threads are
                computationally intensive. Any reasonable mapping strategy
                will map these threads to different CPUs.

                建立工作线程池. 每个工作线程都是一个标准（重载）线程（此处是Thread子类FJTaskRunner的实例），用于处理队列中保存的任务. 通常，工作线程与系统上的CPU一样多.
                在诸如Cilk之类的本机框架中，这些映射到内核线程或轻量级进程，然后映射到CPU. 在Java中，必须信任JVM和OS才能将这些线程映射到CPU. 但是，这对于OS是非常简单的任务，因为这些线程的计算量很大.
                任何合理的映射策略都会将这些线程映射到不同的CPU.

                • All fork/join tasks are instances of a lightweight executable
                class, not instances of threads. In Java, independently
                executable tasks must implement interface Runnable and
                define a run method. In the FJTask framework, these
                tasks subclass FJTask rather than subclassing Thread,
                both of which implement Runnable. (In both cases, a class
                can alternatively implement Runnable and then supply
                instances to be run within executing tasks or threads.
                Because tasks operate under restricted rules supported by
                FJTask methods, it is much more convenient to subclass
                FJTask, so as to be able to directly invoke them.)

                所有的fork/join任务都是轻量级可执行类的实例，而不是线程的实例. 在Java中，独立的可执行任务必须实现Runnable接口并定义run方法. 在FJTask框架中，这些任务是FJTask的子类，而不是Thread的子类，两者均实现Runnable.
                在这两种情况下，类都可以替代地实现Runnable，然后提供要在执行任务或线程中运行的实例. 因为任务是在FJTask方法支持的受限规则下运行的，所以子类FJTask更加方便，以便能够直接调用它们.

                • A special purpose queuing and scheduling discipline is used
                to manage tasks and execute them via the worker threads
                (see section 2.1). These mechanics are triggered by those
                few methods provided in the task class: principally fork,
                join, isDone (a completion status indicator), and some
                convenience methods such as coInvoke that forks then
                joins two or more tasks.

                特殊目的排队和调度规则用于管理任务并通过工作线程执行它们. 这些机制是由任务类中提供的少量方法触发的：
                主要是fork, join, isDone（完成状态指示器）, 以及一些方便的方法，例如coInvoke，fork然后联接将两个或多个任务.

                • A simple control and management facility (here,
                FJTaskRunnerGroup) sets up worker pools and initiates
                execution of a given fork/join task when invoked from a
                normal thread (such as the one performing main in a Java
                program).
                
                当从普通线程（例如Java程序中执行main的线程）调用时，一个简单的控制和管理工具（这里为FJTaskRunnerGroup）将设置工作池并启动给定fork/join任务的执行.
            
            As the standard example of how this framework appears to the programmer, here is a class computing the Fibonacci function:

            作为该框架在程序员看来的标准示例，这是一个计算斐波那契函数的类：

            This version runs at least thirty times faster than an equivalent
            program in which each new task is run in a new
            java.lang.Thread on the platforms described in section 4.
            It does so while maintaining the intrinsic portability of
            multithreaded Java programs. There are only two tuning
            parameters of typical interest to programmers:

            这个版本的运行速度至少是同等程序的三十倍，在同等程序中，每个新任务都在第4节所述的平台上的新java.lang.Thread中运行. 这样做是在保持多线程Java程序固有的可移植性.
            程序员通常只有两个调优参数：

                • The number of worker threads to construct, which should
                ordinarily correspond to the number of CPUs available on a
                platform (or fewer, to reserve processing for other unrelated
                purposes, or occasionally more, to soak up non−
                computational slack).

                要构造的工作线程数，通常应与平台上可用的CPU数相对应（或更少，以保留用于其他不相关目的的处理，或偶尔更多，以吸收非计算松弛）

                • A granularity parameter that represents the point at which
                the overhead of generating tasks outweighs potential
                parallelism benefits. This parameter is typically more
                algorithm−dependent than platform−dependent. It is
                normally possible to settle on a threshold that achieves good
                results when run on a uniprocessor, yet still exploits
                multiple CPUs when they are present. As a side−benefit, this
                approach meshes well with JVM dynamic compilation
                mechanics that optimize small methods better than
                monolithic procedures. This, along with data locality
                advantages, can cause fork/join algorithms to outperform
                other kinds of algorithms even on uniprocessors.

                粒度参数表示生成任务的开销超过潜在的并行性收益的时间点. 此参数通常比算法依赖于平台. 通常可以确定一个阈值，使其在单处理器上运行时可以达到良好的结果，然而当存在多个CPU时仍然可以利用它们.
                作为附带好处，此方法与JVM动态编译机制很好地结合在一起，该机制比单一过程更好地优化了小型方法. 
                随着数据本地化的优势，甚至在单处理器上，也能导致fork/join算法优于其他类型的算法.

        2.1 Work−Stealing

            The heart of a fork/join framework lies in its lightweight
            scheduling mechanics. FJTask adapts the basic tactics
            pioneered in the Cilk work−stealing scheduler:

            fork/join框架的核心在于其轻量级的调度机制. FJTask改编了Cilk工作窃取调度程序中率先采用的基本策略.

                • Each worker thread maintains runnable tasks in its own
                scheduling queue.

                每个工作线程在其自己的调度队列中维护可运行任务.

                • Queues are maintained as double−ended queues (i.e.,
                deques, usually pronounced "decks"), supporting both LIFO
                push and pop operations, as well as a FIFO take
                operation.

                队列维护为双端队列，支持先进后出和先进先出.

                • Subtasks generated in tasks run by a given worker thread
                are pushed onto that workers own deque.

                由给定工作线程运行的任务中生成的子任务将推入该工作线程自己的双端队列.

                • Worker threads process their own deques in LIFO
                (youngest−first) order, by popping tasks.

                工作者线程通过弹出任务以LIFO（最先）顺序处理自己的双端队列.

                • When a worker thread has no local tasks to run, it attempts
                to take ("steal") a task from another randomly chosen
                worker, using a FIFO (oldest first) rule.

                当工作线程没有本地任务要运行时，它尝试使用FIFO（最早的时间）规则从另一个随机选择的工作线程中接走（窃取）任务

                • When a worker thread encounters a join operation, it
                processes other tasks, if available, until the target task is
                noticed to have completed (via isDone). All tasks
                otherwise run to completion without blocking.

                当工作线程遇到联接操作时，它将处理其他任务（如果可用），直到注意到目标任务已完成（通过isDone）. 否则所有任务都将运行完成而不会阻塞

                • When a worker thread has no work and fails to steal any
                from others, it backs off (via yields, sleeps, and/or priority
                adjustment − see section 3) and tries again later unless all
                workers are known to be similarly idle, in which case they
                all block until another task is invoked from top−level.

                当工作线程没有工作并且无法从其他线程窃取任何东西时，它会退出（通过yield，sleep和/或优先级调整-参见第3节），然后再试一次，除非已知所有工作线程都类似地处于空闲状态，否则他们都阻塞，直到从顶层调用另一个任务.

            As discussed in more detail in [5], the use of LIFO rules for
            each thread processing its own tasks, but FIFO rules for stealing
            other tasks is optimal for a wide class of recursive fork/join
            designs. Less formally, the scheme offers two basic advantages:

            正如在[5]中更详细讨论的那样，每个线程都使用LIFO规则处理自己的任务，但是对于大量的递归fork/join设计，用于窃取其他任务的FIFO规则是最佳的. 非正式地讲，该方案具有两个基本优点:

                • It reduces contention by having stealers operate on the opposite
                side of the deque as owners. It also exploits the property of
                recursive divide−and−conquer algorithms of generating "large"
                tasks early. Thus, an older stolen task is likely to provide a
                larger unit of work, leading to further recursive decompositions
                by the stealing thread.

                通过让盗窃者在双端队列的另一侧操作，可以减少竞争. 它还利用了递归分治算法的特性，可以尽早生成大任务. 因此，较旧的被盗任务可能会提供较大的工作单元，从而导致窃取线程进一步递归分解.

                • As one consequence of these rules, programs that employ
                relatively small task granularities for base actions tend to run
                faster than those that only use coarse−grained partitioning or
                those that do not use recursive decomposition. Even though
                relatively few tasks are stolen in most fork/join programs,
                creating many fine−grained tasks means that a task is likely to
                be available whenever a worker thread is ready to run it.

                这些规则的结果是，相对于仅使用粗粒度分区的程序或不使用递归分解的程序，对基本操作采用相对较小的任务粒度的程序往往运行得更快. 
                即使在大多数fork/join程序中失窃的任务相对较少，但是创建许多细粒度的任务意味着只要工作线程准备运行该任务，该任务就有可能可用.

        
        3. IMPLEMENTATION

            The framework has been implemented in about 800 lines of pure
            Java code, mainly in class FJTaskRunner, a subclass of
            java.lang.Thread. FJTasks themselves maintain only a
            boolean completion status, and perform all other operations via
            delegation to their current worker threads. The
            FJTaskRunnerGroup class serves to construct worker
            threads, maintains some shared state (for example, the identities
            of all worker threads, needed for steal operations), and helps
            coordinate startup and shutdown.

            该框架已用大约800行纯Java代码实现, 主要是Thread的子类FJTaskRunner. FJTasks本身仅维护布尔值完成状态，并通过委派其当前工作线程来执行所有其他操作.
            FJTaskRunnerGroup类用于构造工作线程，维护某些共享状态（例如，窃取操作所需的所有工作线程的标识）, 并有助于协调启动和关闭.

            More detailed implementation documentation is available inside
            the util.concurrent package. This section discusses only
            two sets of problems and solutions encountered when
            implementing this framework: Supporting efficient deque
            operations (push, pop, and take), and managing the steal
            protocol by which threads obtain new work.

            util.concurrent软件包中提供了更详细的实现文档. 本节仅讨论实现此框架时遇到的两组问题和解决方案：支持有效的双端队列操作（推送，弹出和获取），以及管理线程用来获取新工作的窃取协议.

        3.1 Deques

            To enable efficient and scalable execution, task management
            must be made as fast as possible. Creating, pushing, and later
            popping (or, much less frequently, taking) tasks are analogs of
            procedure call overhead in sequential programs. Lower overhead
            enables programmers to adopt smaller task granularities, and in
            turn better exploit parallelism.

            为了实现高效且可扩展的执行，必须尽快执行任务管理. 创建，推入和稍后弹出（或减少执行频率）任务是顺序程序中过程调用开销的类似物. 较低的开销使程序员可以采用较小的任务粒度，从而更好地利用并行性

            Task allocation itself is the responsibility of the JVM. Java
            garbage collection relieves us of needing to create a special−
            purpose memory allocator to maintain tasks. This substantially
            reduces the complexity and lines of code needed to implement
            FJTasks compared to similar frameworks in other languages.

            任务分配本身是JVM的责任. Java垃圾回收使我们无需创建专用内存分配器来维护任务. 与其他语言中的类似框架相比，这大大降低了实现FJTasks所需的复杂性和代码行.

            The basic structure of the deque employs the common scheme
            of using a single (although resizable) array per deque, along
            with two indices: The top index acts just like an array−based
            stack pointer, changing upon push and pop. The base index
            is modified only by take. Since FJTaskRunner operations
            are all intimately tied to the concrete details of the deque (for
            example, fork simply invokes push), this data structure is
            directly embedded in the class rather than being defined as a
            separate component.

            双端队列的基本结构采用了常见的方案，即每个双端队列使用单个（尽管可调整大小）数组以及两个索引：顶部索引的行为就像基于数组的堆栈指针一样，在push和pop时发生变化. 基本索引只能通过take修改.
            由于FJTaskRunner操作都与双端队列的具体细节密切相关（例如，fork仅调用push），因此此数据结构直接嵌入到类中，而不是被定义为单独的组件.

            Because the deque array is accessed by multiple threads,
            sometimes without full synchronization (see below), yet
            individual Java array elements cannot be declared as
            volatile, each array element is actually a fixed reference to a
            little forwarding object maintaining a single volatile
            reference. This decision was made originally to ensure
            conformance with Java memory rules, but the level of
            indirection that it entails turns out to improve performance on
            tested platforms, presumably by reducing cache contention due
            to accesses of nearby elements, which are spread out a bit more
            in memory due to the indirection.

            因为双端队列数组是由多个线程访问的，有时没有完全同步（请参见下文），但是单个Java数组元素不能声明为volatile，因此每个数组元素实际上是对少量转发对象的固定引用，并维护单个volatile引用.
            最初做出此决定是为了确保符合Java内存规则，但是事实证明，它所涉及的间接级别可以提高经过测试的平台上的性能，大概是通过减少由于访问附近元素而导致的缓存争用，由于相邻元素的访问，这些元素在内存中的分布更多.

            The main challenges in deque implementation surround
            synchronization and its avoidance. Even on JVMs with
            optimized synchronization facilities[2], the need to obtain locks
            for every push and pop operation becomes a bottleneck.
            However, adaptations of tactics taken in Cilk[5] provide a
            solution based on the following observations:

            双端队列实现中的主要挑战在于同步及其避免。 即使在具有优化的同步工具的JVM上，也需要为每个push和pop操作获取锁而成为瓶颈. 但是，Cilk采用的策略改编提供了基于以下观察结果的解决方案:

                • The push and pop operations are only invoked by owner
                threads.

                push和pop操作仅有自身线程调用.

                • Access to the take operation can easily be confined to one
                stealing thread at a time via an entry lock on take. (This
                deque lock also serves to disable take operations when
                necessary.) Thus, interference control is reduced to a two−
                party synchronization problem.

                通过在take中上锁，可以轻松地将take操作限制在一个窃取线程内. (双端队列锁定还可以在必要时禁用take操作) 因此，干扰控制被简化为两方同步问题！！！

                • The pop and take operations can only interfere if the
                deque is about to become empty. Otherwise they are
                guaranteed to operate on disjoint elements of the array.

                仅当双端队列即将变空时，pop和take操作才会干扰（只有一个节点）. 否则，保证它们可以对数组的不相交元素进行操作.

            Defining the top and base indices as volatile ensures that
            a pop and take can proceed without locking if the deque is
            sure to have more than one element. This is done via a Dekker−
            like algorithm in which push pre−decrements top:
            if (−−top >= base) ...
            and take pre−increments base:
            if (++base < top) ...

            如果将双端队列确定有多个元素，则将顶部索引和基础索引定义为volatile可以确保在不锁定的情况下进行pop和take操作. 
            这是通过类似Dekker的算法完成的，其中push预减：
            if (−−top >= base) ...
            take预增
            if (++base < top) ...

            In each case they must then check to see if this could have
            caused the deque to become empty by comparing the two
            indices. An asymmetric rule is used upon potential conflict: pop
            rechecks state and tries to continue after obtaining the deque
            lock (the same one as held by take), backing off only if the
            deque is truly empty. A take operation instead just backs off
            immediately, typically then trying to steal from a different
            victim. This asymmetry represents the only significant departure
            from the otherwise similar THE protocol used in Cilk.

            在每种情况下，他们必须通过比较两个索引来检查是否可能导致双端队列为空. 在发生潜在冲突时使用非对称规则：pop在获得双端队列锁（与take持有的锁相同）后重新检查状态并尝试继续操作，仅在双端队列确实为空时才回退.
            取而代之的是立即撤退，通常然后尝试从其他受害者那里窃取. 这种不对称性表示与Cilk中使用的其他类似协议唯一的重大偏离

            The use of volatile indices also enables the push operation
            to proceed without synchronization unless the deque array is
            about to overflow, in which case it must first obtain the deque
            lock to resize the array. Otherwise, simply ensuring that top is
            updated only after the deque array slot is filled in suppresses
            interference by any take.

            使用volatile索引还可以使push操作无需同步即可进行，除非双端队列数组即将溢出，在这种情况下，它必须首先获得双端队列锁才能调整数组大小. 否则，只需确保仅在填充双端队列阵列插槽后才更新top即可抑制任何take的干扰.

            Subsequent to initial implementation, it was discovered that
            several JVMs do not conform to the Java Memory Model [6]
            rule requiring accurate reads after writes of pairs of volatile
            fields. As a workaround, the criterion for pop to retry under
            lock was adjusted to trigger if there appear to be two or fewer
            elements, and the take operation added a secondary lock to
            ensure a memory barrier. This suffices as long as at most one
            index change is missed by the owner thread (which holds here
            for platforms that otherwise maintain proper memory order
            when reading volatile fields), and causes only a tiny
            slowdown in performance. 

            在最初实施之后，发现若干JVM不符合Java内存模型规则，该规则要求在写入volatile字段后进行准确的读取. 
            作为一种解决方法，调整了pop在锁定下重试的条件，以触发是否出现两个或更少的元素，并且take操作添加了第二个锁定以确保内存屏障.
            只要自身线程最多遗漏一个索引更改（对于在读取volatile字段时会保持适当的内存顺序的平台而言，此处保持不变），这就足够了，并且只会导致性能略有下降.

        3.2 Stealing and Idling

            Worker threads in work−stealing frameworks know nothing
            about the synchronization demands of the programs they are
            running. They simply generate, push, pop, take, manage the
            status of, and execute tasks. The simplicity of this scheme leads
            to efficient execution when there is plenty of work for all
            threads. However, this streamlining comes at the price of relying
            on heuristics when there is not enough work; i.e., during startup
            of a main task, upon its completion, and around global full−stop
            synchronization points employed in some fork/join algorithms.

            工作窃取框架中的工作线程对他们正在运行的程序的同步要求一无所知. 它们只是生成、push、pop、take、管理任务的状态和执行任务. 当所有线程都有大量工作时，此方案的简单性导致高效执行.
            但是，这种精简以牺牲工作时依靠启发式方法为代价；在主要任务启动期间，完成时以及在某些fork/join算法中使用的全局句点同步点附近（这语句翻译有点出入???）

            The main issue here is what to do when a worker thread has no
            local tasks and cannot steal one from any other thread. If the
            program is running on a dedicated multiprocessor, then one
            could make the case for relying on hard busy−wait spins looping
            to try to steal work. However, even here, attempted steals
            increase contention, which can slow down even those threads
            that are not idle (due to locking protocols in section 3.1).
            Additionally, in more typical usage contexts of this framework,
            the operating system should somehow be convinced to try to run
            other unrelated runnable processes or threads.

            这里的主要问题是当工作线程没有本地任务并且无法从其他线程中窃取任务时该怎么办. 如果程序在专用的多处理器上运行，则可能需要依靠自旋循环来尝试窃取工作.
            但是，即使在这里，尝试的窃取也会增加抢夺，即使是那些非空闲的线程也可能减慢抢夺. 另外，在此框架的更典型用法上下文中，应以某种方式说服操作系统尝试运行其他不相关的可运行进程或线程.

            The tools for achieving this in Java are weak, have no
            guarantees (see [6, 7]), but usually appear to be acceptable in
            practice (as do similar techniques described for Hood[3]). A
            thread that fails to obtain work from any other thread lowers its
            priority before attempting additional steals, performs
            Thread.yield between attempts, and registers itself as
            inactive in its FJTaskRunnerGroup. If all others become
            inactive, they all block waiting for additional main tasks.
            Otherwise, after a given number of additional spins, threads
            enter a sleeping phase, where they sleep (for up to 100ms) rather
            than yield between steal attempts. These imposed sleeps can
            cause artificial lags in programs that take a long time to split
            their tasks. But this appears to be the best general−purpose
            compromise. Future versions of the framework may supply
            additional control methods so that programmers can override
            defaults when they impact performance.

            用Java实现此目的的工具较弱，没有保证（请参见[6，7]），但实际上通常是可以接受的. 
            无法从任何其他线程获得工作的线程会在尝试其他窃取之前降低其优先级，在两次尝试之间执行Thread.yield，并在其FJTaskRunnerGroup中将其注册为不活跃
            如果所有其他线程都变得不活跃，它们都会阻塞等待其他主要任务. 否则，在给定数量的额外旋转之后，线程将进入休眠阶段，在该阶段它们进入休眠状态（最长100毫秒），而不是两次尝试之间都yield.
            这些强加的睡眠可能导致程序中人为的延迟，而这些程序需要很长时间才能分解其任务. 但这似乎是最佳的通用折衷方案. 
            框架的未来版本可能会提供其他控制方法，以便程序员在影响性能时可以覆盖默认值.


        4. PERFORMANCE

            In these times of nearly continuous performance improvements
            of compilers and JVMs, performance measurements are only of
            transient value. However, the metrics reported in this section
            reveal some basic properties of the framework.

            在编译器和JVM的性能几乎持续提高的这些时期，性能度量仅具有过渡价值. 但是，本节中报告的指标揭示了框架的一些基本属性.

            A collection of seven fork/join test programs are briefly
            described in the following table. These programs are adaptations
            of those available as demos in the util.concurrent
            package. They were selected to show some diversity in the kinds
            of problems that can be run within this framework, as well as to
            obtain results for some common parallel test programs.

            下表简要描述了七个fork/join测试程序的集合. 这些程序是util.concurrent包中作为演示可用的程序的改编. 选择它们是为了显示在此框架内可以运行的各种问题的多样性，以及获得一些常见的并行测试程序的结果.

            For the main tests, programs were run on a 30−CPU Sun
            Enterprise 10000 running the Solaris Production 1.2 JVM (an
            early version of the 1.2.2_05 release) on Solaris 7. The JVM
            was run with environment parameters selecting "bound threads"
            for thread mappings, and memory parameters discussed in
            section 4.2. A few additional measurements reported below were
            run on a 4−CPU Sun Enterprise 450. 

            对于主要测试，程序在Solaris 7上运行Solaris Production 1.2 JVM（1.2.2_05发行版的早期版本）的30-CPU Sun Enterprise 10000上运行.
            运行JVM时，环境参数选择用于线程映射的“绑定线程”，而内存参数则在第4.2节中讨论。 下面报告的一些其他测量是在4-CPU Sun Enterprise 450上运行的.

            Programs were run with very large input parameters in order to
            minimize timer granularity and JVM warm−up artifacts. Some
            other warm−up effects were avoided by running an initial
            problem set before starting timers. Most data are medians of
            three runs, but some (including most follow−up measurements
            in sections 4.2−4.4) only reflect single runs so are a bit noisy.

            程序使用非常大的输入参数运行，以最大程度地减少计时器粒度和JVM预热工件. 通过在启动计时器之前运行初始问题集可以避免其他一些预热效果. 
            大多数数据是三个运行的中值，但是某些数据（包括第4.2-4.4节中的大多数后续测量）仅反映了单个运行，因此有点嘈杂.

        4.1 Speedups

            Scalability measurements were obtained by running the same
            problem set using worker thread groups of size 1 ... 30. There is
            no way to know whether the JVM always mapped each thread to
            a different CPU when available. While there is no evidence
            about this either way, it is possible that the lags to map new
            threads to CPUs increased with numbers of threads and/or
            varied systematically across the different test programs.

            可伸缩性度量是通过使用大小为1 ... 30的工作线程组运行相同的问题集而获得的. 无法知道JVM是否在可用时始终将每个线程映射到不同的CPU. 
            尽管没有证据表明这两种方法都存在，但是将新线程映射到CPU的滞后可能会随着线程数量的增加和/或在不同测试程序之间系统地变化而增加.

            However, in general, the results show that increasing the number
            of threads reliably increased the number of CPUs employed.

            但是，总的来说，结果表明，增加线程数量确实增加了所采用的CPU数量.

            Speedups are reported as Timen / Time1. The best overall
            speedup was seen for the integration program (speedup of 28.2
            for 30 threads). The worst was for the LU decomposition
            program (speedup of 15.35 for 30 threads).

            加速报告为Timen/Time1. 对于集成程序，整体速度最高（30个线程的速度最高为28.2）. 最糟糕的是LU分解程序（30个线程加速15.35）.

            Another way of measuring scalability is in terms of task rates,
            the average time taken to execute a single task (which may be
            either a recursive or a leaf step). The following figure shows
            data from a single instrumented run capturing task rates. Ideally,
            the numbers of tasks processed per unit time per thread should
            be constant. The fact that they generally slightly decrease with
            numbers of threads indicates some scalability limitations. Note
            the fairly wide difference in task rates, reflecting differences in
            task granularities. The smallest tasks sizes are seen in Fib,
            which even with a threshold setting of 13, generated and
            executed a total of 2.8 million tasks per second when run with
            30 threads.

            衡量可伸缩性的另一种方法是根据任务速率，即执行单个任务所需的平均时间（可以是递归步骤或叶子步骤）. 下图显示了来自单个仪器运行的数据，捕获了任务率.
            理想情况下，每个线程每单位时间处理的任务数应该是恒定的. 它们通常随线程数而略有减少的事实表明了一些可伸缩性限制. 请注意，任务速率的差异相当大，反映了任务粒度的差异.
            在Fib中可以看到最小的任务大小，即使将阈值设置为13，当使用30个线程运行时，每秒仍可生成和执行总计280万个任务.

            Four factors appear to account for tail−offs in speedups in those
            programs that did not scale linearly. Three of them are common
            to any parallel framework, but we’ll start with one unique to
            FJTask (as opposed to Cilk etc), GC effects.

            在那些没有线性扩展的程序中，有四个因素似乎可以解释加速中的拖尾现象. 它们中的三个对于任何并行框架都是通用的，但我们将从FJTask特有的GC效果开始.

        4.2 Garbage Collection

            In many ways, modern GC facilities are perfect matches to
            fork/join frameworks: These programs can generate enormous
            numbers of tasks, nearly all of which quickly turn into garbage
            after they are executed. At any given time, deterministic
            fork/join programs require only at most p (where p is the
            number of threads) times the bookkeeping space that would be
            required for sequential versions of these programs[10].
            Generational semispace copying collectors (including the one in
            the JVM used for these measurements − see [1]) cope with this
            well because they only traverse and copy non−garbage cells. By
            doing so, they evade one of the trickiest problems in manual
            parallel memory management, tracing memory that is allocated
            by one thread but then used by another. The garbage collector is
            oblivious to the origin of memory, so need not deal with such
            issues.

            在许多方面，现代GC设施与fork/join框架完美匹配：这些程序可以生成大量任务，几乎所有任务在执行后都会很快变成垃圾. 在任何给定时间，确定性fork/join程序最多只需要簿记空间的p倍，这是这些程序的顺序版本所需要的.
            分代半空间复制收集器（包括JVM中用于这些测量的收集器-参见[1]）可以很好地解决这一问题，因为它们仅遍历并复制非垃圾单元. 这样，它们可以避免手动并行内存管理中最棘手的问题之一，可以跟踪由一个线程分配但又由另一个线程使用的内存.
            垃圾收集器不了解内存的来源，因此无需处理此类问题

            As a simple indicator of the general superiority of generational
            copying collection, a four−thread run of Fib that executes in 5.1
            seconds when using the memory settings used in the main
            experiments reported here runs in 9.1 seconds if the generational
            copying phase is disabled on this JVM (in which case this JVM
            relies entirely on mark−sweep).

            However, these GC mechanisms turn into scaling problems
            when memory is allocated at such a high rate that threads must
            be stopped nearly continuously to perform collections. The
            following figure shows the difference in speedups across three
            memory settings (this JVM supports optional arguments to set
            memory parameters): with the default 4 megabyte semispaces,
            with 64 megabyte semispaces, and with the amount of memory
            scaled to the number of threads (2 + 2p megabytes). With
            smaller semispaces, the overhead of stopping threads and
            collecting garbage starts to impact scaling as the garbage−
            generation rate climbs due to additional threads.

            In light of these results, 64m semispaces were used for all other
            test runs. A better policy would have been to scale the amount
            of memory to the number of threads in each test. (As seen in the
            figure, this would have caused all speedups to appear slightly
            more linear.). Alternatively, or in addition, program−specific
            task granularity thresholds could be increased proportionally
            with numbers of threads.

        4.3 Memory Locality and Bandwidth

            Four of the test programs create and operate on very large
            shared arrays or matrices: Sorting numbers, and multiplying,
            decomposing or performing relaxation on matrices. Of these,
            sorting is probably the most sensitive to the consequences of
            having to move data around processors, and thus to the
            aggregate memory bandwidth of the system as a whole. To help
            determine the nature of these effects, the Sort program was
            recast into four versions, that sorted bytes, shorts, ints, and
            longs respectively. Each version sorted data only in the range of
            0...255, to ensure that they were otherwise equivalent. The wider
            the data elements, the more memory traffic.

            其中四个测试程序可在非常大的共享数组或矩阵上创建并运行：对数字进行排序，并对矩阵进行乘法，分解或松弛.
            其中，排序可能对必须在处理器之间移动数据的后果，从而对整个系统的总内存带宽最敏感. 为了帮助确定这些效果的性质，将Sort程序重铸为四个版本，分别对字节，short，int和long进行了排序.
            每个版本仅对0 ... 255范围内的数据进行排序，以确保它们在其他方面等效。 数据元素越宽，内存流量越大.

            The results show that increased memory traffic leads to poorer
            speedups, although they do not provide definitive evidence that
            this is the only cause of tail−offs.

            结果表明，增加的内存流量导致较慢的加速，尽管它们没有提供确定的证据表明这是拖尾的唯一原因.

            Element width also impacts absolute performance. For example,
            using only one thread, sorting bytes took 122.5 seconds, while
            sorting longs took 242.4 seconds.

            元素宽度也会影响绝对性能. 例如，仅使用一个线程，对字节的排序花费了122.5秒，而对long的排序花费了242.4秒.
        
        4.4 Task Synchronization

            As discussed in section 3.2, work−stealing frameworks
            sometimes encounter problems dealing with frequent global
            synchronization of tasks. The worker threads continue to poll for
            more work even though there isn’t any, thus generating
            contention, and, in FJTask, sometimes even forcing threads
            into idle sleeps. 

            如3.2节所述，工作窃取框架有时会遇到与任务的频繁全局同步有关的问题. 即使没有工作线程，工作线程也会继续轮询以进行更多工作，从而产生争用，并且在FJTask中，有时甚至迫使线程进入空闲睡眠状态.

            The Jacobi program illustrates some of the consequent issues.
            This program performs 100 steps, where in each step, all cells
            are updated according to a simple nearest neighbor averaging
            formula. A global (tree−based) barrier separates each step. To
            determine the magnitude of synchronization effects, a version of
            this program was written to synchronize only after every 10
            steps. The scaling differences show the impact of the current
            policy, and indicate the need to include additional methods in
            future versions of this framework to allow programmers to
            override default parameters and policies. (Note however that
            this graph probably slightly exaggerates pure synchronization
            effects, since the 10−step version is also likely to maintain
            somewhat greater task locality.)

            Jacobi程序说明了一些随之而来的问题. 该程序执行100个步骤，其中在每个步骤中，所有单元均根据简单的最近邻平均公式进行更新. 
            全局（基于树）屏障将每个步骤分隔开. 为了确定同步效果的大小，编写了该程序的一个版本，以便仅在每10个步骤后进行一次同步.
            缩放比例差异显示了当前策略的影响，并表明需要在此框架的未来版本中包括其他方法，以允许程序员覆盖默认参数和策略.
            （但是请注意，此图可能会稍微夸大纯同步效果，因为10步版本也可能会保留较大的任务局部性）

        4.5 Task Locality

            FJTask, like other fork/join frameworks, is optimized for the
            case where worker threads locally consume the vast majority of
            the tasks they create. When this does not occur, performance can
            suffer, for two reasons:

            与其他fork/join框架一样，FJTask已针对工作线程在本地消耗其创建的绝大部分任务的情况进行了优化. 如果不发生这种情况，则性能可能会受到影响，原因有两个：

                • Stealing tasks encounters much more overhead than does
                popping tasks.

                窃取任务比弹出任务遇到更多的开销.

                • In most programs in which tasks operate on shared data,
                running your own subdivided task is likely to maintain better
                locality of data access. 

                在大多数任务对共享数据进行操作的程序中，运行自己的细分任务可能会保持更好的数据访问位置.

            As seen in the figure, in most programs, the relative number of
            stolen tasks is at most a few percent. However, the LU and MM
            programs generate larger imbalances in workloads (and thus
            relatively more stealing) as the number of threads increase. It is
            possible that some algorithmic tuning of these programs could
            reduce this effect, and in turn lead to better speedups.

            如图所示，在大多数程序中，被盗任务的相对数量最多为百分之几. 但是，随着线程数量的增加，LU和MM程序会在工作负载上产生更大的不平衡（并因此导致更多的窃取）.
            对这些程序进行某种算法调整可能会降低这种影响，从而导致更好的加速.

        4.6 Comparisons to Other Frameworks

            It is impossible to perform definitive or even very meaningful
            measurements comparing programs across languages and
            frameworks. However, relative measurements can at least show
            some of the basic advantages and limitations of the FJTask
            framework versus similar frameworks written in other languages
            (here, C and C++). The following chart compares performance
            of programs that were either based on or are similar to those
            supplied with the Cilk, Hood, Stackthreads, and/or Filaments
            distributions. All of these were run with 4 threads on a 4−CPU
            Enterprise 450. To avoid reconfiguring other frameworks or
            their test programs, all tests were run with smaller problems sets
            than used above. All results represent the best of three runs,
            using the compiler and run−time settings that appeared to
            provide the fastest times. Fib was run without any granularity
            threshold; i.e., an implicit threshold of 1. (The "prune" setting in
            Filaments Fib was set to 1024, which causes it to behave more
            similarly to the other versions.)

            比较跨语言和跨框架的程序，不可能进行确定甚至非常有意义的测量. 但是，相对的测量至少可以显示FJTaskframework与用其他语言编写的类似框架相比的一些基本优点和局限性.
            下表比较了基于或类似于Cilk，Hood，Stackthreads和/或Filaments发行版提供的程序的性能. 所有这些都在4 CPU Enterprise 450上以4个线程运行.
            为了避免重新配置其他框架或它们的测试程序，所有测试都以比上面使用的更小的问题集运行. 所有结果代表了运行最好的三个编译器和运行时设置，这是运行最快的时间.
            Fib运行时没有任何粒度阈值； 即隐式阈值1.

            Speedups going from one to four threads were very similar
            (between 3.0 and 4.0) across the different frameworks and test
            programs, so the accompanying chart focuses on differences in
            absolute performance. However, because the multithreading
            aspects of all these frameworks are all fast, most of these
            differences reflect differences in application−specific code
            quality stemming from different compilers, optimization
            switches, and configuration parameters. In fact, it is likely that
            different choices among these than used here could produce
            nearly any relative performance ranking among these
            frameworks in many high−performance applications.

            在不同的框架和测试程序中，从一个线程到四个线程的加速非常相似（在3.0和4.0之间），因此所附图表着重于绝对性能的差异. 
            但是，由于所有这些框架的多线程方面都非常快，因此这些差异中的大多数反映了由于不同的编译器，优化开关和配置参数而导致的特定于应用程序的代码质量的差异.
            实际上，在许多高性能应用程序中，与此处使用的不同，很可能在这些框架之间产生几乎任何相对的性能排名.

            FJTask generally performs worse (on this JVM) on programs
            that mainly do floating point computations on arrays and
            matrices. Even though JVMs are improving, they are still not
            always competitive with powerful back−end optimizers used in
            C and C++ programs. Although not shown in this chart,
            FJTask versions of all programs ran faster than versions of
            programs in these other frameworks when they were compiled
            without optimization enabled, and some informal tests suggest
            that most of the remaining differences are due to array bounds
            checks and related runtime obligations. These are, of course,
            issues and problems that are attracting much attention and effort
            by JVM and compiler developers. The observed differences in
            code quality for computationally intensive programs are likely
            to diminish.

            FJTask通常在主要在数组和矩阵上进行浮点计算的程序上（在此JVM上）表现较差. 即使JVM有所改进，但它们仍不总是与C和C ++程序中使用的强大的后端优化器竞争.
            尽管未在此图表中显示，但在未启用优化的情况下进行编译时，所有程序的FJTask版本运行速度都比其他框架中的程序版本快，并且一些非正式测试表明，其余大多数差异是由于数组边界检查和相关的运行时所致义务.
            当然，这些是吸引JVM和编译器开发人员关注和努力的问题. 观察到的计算密集型程序的代码质量差异可能会减少.

        5. CONCLUSIONS     

            This paper has demonstrated that it is possible to support
            portable, efficient, scalable parallel processing in pure Java, and
            to provide a convenient API for programmers who can exploit
            the framework by following only a few simple design rules and
            design patterns (as presented and discussed in [7]). The
            observed performance characteristics of the sample programs
            measured here both provide further guidance for users of the
            framework, and suggest some areas of potential improvement to
            the framework itself.

            本文证明了有可能在纯Java中支持可移植，高效，可伸缩的并行处理，并为程序员提供了便捷的API，他们只需遵循一些简单的设计规则和设计模式即可利用该框架. 
            在这里测量的示例程序的观察到的性能特征既为框架的用户提供了进一步的指导，也为框架本身提出了一些潜在的改进领域.

            Even though scalability results were shown only for a single
            JVM, some of the main empirical findings should hold more
            generally:

                即使仅针对单个JVM显示了可伸缩性结果，但一些主要的经验发现应该更普遍.

                • While generational GC generally meshes well with
                parallelism, it can hinder scalability when garbage
                generation rates force very frequent collections. On this
                JVM, the underlying cause appears to be that stopping
                threads for collection takes time approximately proportional
                to the number of running threads. Because more running
                threads generate more garbage per unit time, overhead can
                climb approximately quadratically with numbers of threads.
                Even so, this significantly impacts performance only when
                the GC rate is relatively high to begin with. However, the
                resulting issues invite further research and development of
                concurrent and parallel GC algorithms. The results presented
                here additionally demonstrate the desirability of providing
                tuning options and adaptive mechanisms on multiprocessor
                JVMs to scale memory to the number of active processors.

                尽管世代GC通常与并行性很好地结合，但是当垃圾生成率迫使非常频繁的收集时，它可能会妨碍可伸缩性. 在此JVM上，根本原因似乎是停止收集线程所需的时间大约与正在运行的线程数成比例.
                由于更多的正在运行的线程每单位时间产生更多的垃圾，因此开销可以随着线程数的增加而增加大约两倍. 即使这样，仅当GC速率开始时相对较高时，这才会显着影响性能。 但是，由此产生的问题需要进一步研究并发和并行GC算法.
                此处呈现的结果还证明了在多处理器JVM上提供调整选项和自适应机制以将内存扩展到活动处理器数量的可取性.

                • Most scalability issues reveal themselves only when
                programs are run using more CPUs than are even available
                on most stock multiprocessors. FJTask (as well as other
                fork/join frameworks) appear to provide nearly ideal
                speedups for nearly any fork/join program on commonly
                available 2−way, 4−way, and 8−way SMP machines. The
                present paper appears to be the first reporting systematic
                results for any fork/join framework designed for stock
                multiprocessors run on more than 16 CPUs. Further
                measurements are needed to see if the patterns of results
                seen here hold in other frameworks as well.

                大多数可伸缩性问题仅在程序运行时使用的CPU数量超过大多数现有多处理器所能提供的数量时才会显示出来. FJTask（以及其他fork / join框架）似乎为常用的2向，4向和8向SMP机器上的几乎所有fork/join程序提供了几乎理想的加速比.
                本文似乎是第一个报告针对为运行在16个以上CPU上的常规多处理器设计的任何fork/join框架的系统结果的报告. 
                需要进一步测量，以查看此处看到的结果模式是否也适用于其他框架.

                • Characteristics of application programs (including memory
                locality, task locality, use of global synchronization) often
                have more bearing on both scalability and absolute
                performance than do characteristics of the framework, JVM
                or underlying OS. For example, informal tests showed that
                the careful avoidance of synchronization in deques
                (discussed in section 3.1) has essentially no impact on
                programs with relatively low task generation rates such as
                LU. However, the focus on keeping task management
                overhead to a minimum widens the range of applicability
                and utility of the framework and the associated design and
                programming techniques.

                与框架，JVM或底层OS的特性相比，应用程序的特性（包括内存局部性，任务局部性，全局同步的使用）通常对可伸缩性和绝对性能有更大的影响. 
                例如，非正式测试表明，谨慎地避免双端队列同步不会对任务生成率相对较低的程序（例如LU）造成影响. 但是，将任务管理开销保持在最低限度的重点扩大了框架以及相关设计和编程技术的适用范围和实用性.

            In addition to incremental improvements, future work on this
            framework may include construction of useful applications (as
            opposed to demos and tests), subsequent evaluations under
            production program loads, measurements on different JVMs,
            and development of extensions geared for use with clusters of
            multiprocessors. 

            除了逐步改进之外，此框架上的未来工作可能包括构建有用的应用程序，在生产程序负载下进行后续评估，在不同的JVM上进行测量以及开发适用于多处理器集群的扩展.

        6. ACKNOWLEDGMENTS

            This work was supported in part by a collaborative research
            grant from Sun Labs. Thanks to Ole Agesen, Dave Detlefs,
            Christine Flood, Alex Garthwaite, and Steve Heller of the Sun
            Labs Java Topics Group for advice, help, and comments. David
            Holmes, Ole Agesen, Keith Randall, Kenjiro Taura, and the
            anonymous referees provided useful comments on drafts of this
            paper. Bill Pugh pointed out the read−after−write limitations of
            JVMs discussed in section 3.1. Very special thanks to Dave Dice
            for reserving time and performing test runs on the 30−way
            Enterprise. 
    */

```


### 翻译ForkJoinPool注释，实际上该类的注释并没有提到很关键的信息

```java

   /**
    * An {@link ExecutorService} for running {@link ForkJoinTask}s.
    * A {@code ForkJoinPool} provides the entry point for submissions
    * from non-{@code ForkJoinTask} clients, as well as management and
    * monitoring operations.

    运行ForkJoinTask的ExecutorService. ForkJoinPool提供了来自非ForkJoinTaskk客户端提交以计管理和监视操作的入口点.

    * <p>A {@code ForkJoinPool} differs from other kinds of {@link
    * ExecutorService} mainly by virtue of employing
    * <em>work-stealing</em>: all threads in the pool attempt to find and
    * execute tasks submitted to the pool and/or created by other active
    * tasks (eventually blocking waiting for work if none exist). This
    * enables efficient processing when most tasks spawn other subtasks
    * (as do most {@code ForkJoinTask}s), as well as when many small
    * tasks are submitted to the pool from external clients.  Especially
    * when setting <em>asyncMode</em> to true in constructors, {@code
    * ForkJoinPool}s may also be appropriate for use with event-style
    * tasks that are never joined.

    ForkJoinPool与其他类型的ExecutorService的不同之处主要在于使用工作窃取：池中的所有线程尝试查找并执行提交到池中和/或由其他活动任务创建的任务（子任务）（如果不存在，最终阻塞等待工作）.
    当大多数任务产生其他子任务时，以及从外部客户端向池中提交许多小任务时，这均可实现高效处理. 特别是在构造函数中将asyncMode设置为true时，ForkJoinPool也可能适用于从未加入的事件样式任务.

    * <p>A static {@link #commonPool()} is available and appropriate for
    * most applications. The common pool is used by any ForkJoinTask that
    * is not explicitly submitted to a specified pool. Using the common
    * pool normally reduces resource usage (its threads are slowly
    * reclaimed during periods of non-use, and reinstated upon subsequent
    * use).

    静态commonPool方法是可用的，并且适用于大多数应用. 公用池由未显式提交到指定池的任何ForkJoinTask使用. 使用公共池通常可以减少资源使用（在不使用期间缓慢回收其线程，并在以后使用时恢复）

    * <p>For applications that require separate or custom pools, a {@code
    * ForkJoinPool} may be constructed with a given target parallelism
    * level; by default, equal to the number of available processors.
    * The pool attempts to maintain enough active (or available) threads
    * by dynamically adding, suspending, or resuming internal worker
    * threads, even if some tasks are stalled waiting to join others.
    * However, no such adjustments are guaranteed in the face of blocked
    * I/O or other unmanaged synchronization. The nested {@link
    * ManagedBlocker} interface enables extension of the kinds of
    * synchronization accommodated.
    
    对于需要单独或自定义池的应用程序，可以使用指定的目标并行级别构建ForkJoinPool；默认情况下，等于可用处理器的数量. 
    池尝试通过动态添加，挂起或恢复内部工作线程来维护足够的活动（或可用）线程，即使某些任务已停滞等待加入其他任务.
    但是，面对阻塞的I/O或其他不受管理的同步，无法保证此类调整.
    嵌套的ManagedBlocker接口可扩展所容纳的同步类型.

    * <p>In addition to execution and lifecycle control methods, this
    * class provides status check methods (for example
    * {@link #getStealCount}) that are intended to aid in developing,
    * tuning, and monitoring fork/join applications. Also, method
    * {@link #toString} returns indications of pool state in a
    * convenient form for informal monitoring.
    
    除了执行和生命周期控制方法外，此类还提供状态检查方法（例如getStealCount），意在帮助开发，调整和监视fork/join应用程序.
    同样，toString方法以方便的形式返回池状态的指示，以进行非正式监视.

    * <p>As is the case with other ExecutorServices, there are three
    * main task execution methods summarized in the following table.
    * These are designed to be used primarily by clients not already
    * engaged in fork/join computations in the current pool.  The main
    * forms of these methods accept instances of {@code ForkJoinTask},
    * but overloaded forms also allow mixed execution of plain {@code
    * Runnable}- or {@code Callable}- based activities as well.  However,
    * tasks that are already executing in a pool should normally instead
    * use the within-computation forms listed in the table unless using
    * async event-style tasks that are not usually joined, in which case
    * there is little difference among choice of methods.

    与其他ExecutorService一样，下表总结了三种主要的任务执行方法. 这些被设计为主要供尚未在当前池中进行fork/join计算的客户端使用.
    这些方法的主要形式接受ForkJoinTask的实例，但是重载形式还允许混合执行基于普通Runnable或Callable的活动.
    但是，已经在池中执行的任务通常应改用表中列出的内部计算形式，除非使用通常不联接的异步事件样式任务，在这种情况下，方法选择之间几乎没有区别.

    * <p>The common pool is by default constructed with default
    * parameters, but these may be controlled by setting three
    * {@linkplain System#getProperty system properties}:

    默认情况下，公共池是使用默认参数构造的，但是可以通过设置三个参数来控制：

    * <ul>
    * <li>{@code java.util.concurrent.ForkJoinPool.common.parallelism}
    * - the parallelism level, a non-negative integer

    并行级别，非负数

    * <li>{@code java.util.concurrent.ForkJoinPool.common.threadFactory}
    * - the class name of a {@link ForkJoinWorkerThreadFactory}

    ForkJoinWorkerThreadFactory的类名

    * <li>{@code java.util.concurrent.ForkJoinPool.common.exceptionHandler}
    * - the class name of a {@link UncaughtExceptionHandler}

    UncaughtExceptionHandler的类名

    * </ul>

    * If a {@link SecurityManager} is present and no factory is
    * specified, then the default pool uses a factory supplying
    * threads that have no {@link Permissions} enabled.
    * The system class loader is used to load these classes.
    * Upon any error in establishing these settings, default parameters
    * are used. It is possible to disable or limit the use of threads in
    * the common pool by setting the parallelism property to zero, and/or
    * using a factory that may return {@code null}. However doing so may
    * cause unjoined tasks to never be executed.
    
    如果存在SecurityManager且未指定工厂，则默认池将使用工厂提供未启用Permissions的线程. 系统类加载器用于加载这些类.
    在建立这些设置时出现任何错误时，将使用默认参数. 通过将parallelism属性设置为0和/或使用可能返回null的工厂，可以禁用或限制公共池中线程的使用. 但是，这样做可能导致未执行的任务永远不会执行.

    * <p><b>Implementation notes</b>: This implementation restricts the
    * maximum number of running threads to 32767. Attempts to create
    * pools with greater than the maximum number result in
    * {@code IllegalArgumentException}.
    
    实现说明：此实现将正在运行的最大线程数限制为32767. 尝试创建大于最大线程数的池会导致异常.

    * <p>This implementation rejects submitted tasks (that is, by throwing
    * {@link RejectedExecutionException}) only when the pool is shut down
    * or internal resources have been exhausted.

    仅当关闭池或内部资源耗尽时才拒绝提交的任务.

    */

```

### 翻译ForkJoinPool内部注释，这里才提供了关键的信息

```java

   /*
    * Implementation Overview
    
    实现概要

    * This class and its nested classes provide the main
    * functionality and control for a set of worker threads:
    * Submissions from non-FJ threads enter into submission queues.
    * Workers take these tasks and typically split them into subtasks
    * that may be stolen by other workers.  Preference rules give
    * first priority to processing tasks from their own queues (LIFO
    * or FIFO, depending on mode), then to randomized FIFO steals of
    * tasks in other queues.  This framework began as vehicle for
    * supporting tree-structured parallelism using work-stealing.
    * Over time, its scalability advantages led to extensions and
    * changes to better support more diverse usage contexts.  Because
    * most internal methods and nested classes are interrelated,
    * their main rationale and descriptions are presented here;
    * individual methods and nested classes contain only brief
    * comments about details.
    
    此类及其嵌套的类为一组工作线程提供主要功能和控制：来自非FJ线程的提交进入提交队列. 工作人员执行这些任务，通常将其拆分为其他工作人员可能窃取的子任务.
    优先规则优先处理来自其自身队列（LIFO或FIFO，取决于模式）的任务，然后优先处理其他队列中的随机FIFO窃取任务. 该框架最初是使用工作窃取来支持树状并行性的工具.
    随着时间的流逝，其可伸缩性优势导致了扩展和更改，以更好地支持更多不同的使用环境. 
    由于大多数内部方法和嵌套类是相互关联的，因此此处介绍了它们的主要原理和描述。 单个方法和嵌套类仅包含有关详细信息的简短注释.

    * WorkQueues
    * ==========
    
    工作队列

    * Most operations occur within work-stealing queues (in nested
    * class WorkQueue).  These are special forms of Deques that
    * support only three of the four possible end-operations -- push,
    * pop, and poll (aka steal), under the further constraints that
    * push and pop are called only from the owning thread (or, as
    * extended here, under a lock), while poll may be called from
    * other threads.  (If you are unfamiliar with them, you probably
    * want to read Herlihy and Shavit's book "The Art of
    * Multiprocessor programming", chapter 16 describing these in
    * more detail before proceeding.)  The main work-stealing queue
    * design is roughly similar to those in the papers "Dynamic
    * Circular Work-Stealing Deque" by Chase and Lev, SPAA 2005
    * (http://research.sun.com/scalable/pubs/index.html) and
    * "Idempotent work stealing" by Michael, Saraswat, and Vechev,
    * PPoPP 2009 (http://portal.acm.org/citation.cfm?id=1504186).
    * The main differences ultimately stem from GC requirements that
    * we null out taken slots as soon as we can, to maintain as small
    * a footprint as possible even in programs generating huge
    * numbers of tasks. To accomplish this, we shift the CAS
    * arbitrating pop vs poll (steal) from being on the indices
    * ("base" and "top") to the slots themselves.
    
    大多数操作发生在工作窃取队列中. 这些是Deques的特殊形式，仅支持四种可能的最终操作中的三种 -- push、pop、poll，在进一步的约束下，push和pop仅从拥有的线程中调用，poll可能被其他线程调用.
    最终的主要区别在于GC的要求，即使在生成大量任务的程序中，我们也要尽快清空已占用的插槽，以尽可能减小占用空间. 为了实现这一点，我们利用CAS控制pop与poll（窃取）从位于索引（基础和顶部）上移至插槽.

    * Adding tasks then takes the form of a classic array push(task):
    *    q.array[q.top] = task; ++q.top;
    
    然后添加任务采用经典数组push（task）的形式：q.arr[q.top] = task; ++q.top;

    * (The actual code needs to null-check and size-check the array,
    * properly fence the accesses, and possibly signal waiting
    * workers to start scanning -- see below.)  Both a successful pop
    * and poll mainly entail a CAS of a slot from non-null to null.
    
    成功的C和B都需要从非空到空的CAS.

    * The pop operation (always performed by owner) is:
    *   if ((base != top) and
    *        (the task at top slot is not null) and
    *        (CAS slot to null))
    *           decrement top and return task;
    *
    * And the poll operation (usually by a stealer) is
    *    if ((base != top) and
    *        (the task at base slot is not null) and
    *        (base has not changed) and
    *        (CAS slot to null))
    *           increment base and return task;
    *
    * Because we rely on CASes of references, we do not need tag bits
    * on base or top.  They are simple ints as used in any circular
    * array-based queue (see for example ArrayDeque).  Updates to the
    * indices guarantee that top == base means the queue is empty,
    * but otherwise may err on the side of possibly making the queue
    * appear nonempty when a push, pop, or poll have not fully
    * committed. (Method isEmpty() checks the case of a partially
    * completed removal of the last element.)  Because of this, the
    * poll operation, considered individually, is not wait-free. One
    * thief cannot successfully continue until another in-progress
    * one (or, if previously empty, a push) completes.  However, in
    * the aggregate, we ensure at least probabilistic
    * non-blockingness.  If an attempted steal fails, a thief always
    * chooses a different random victim target to try next. So, in
    * order for one thief to progress, it suffices for any
    * in-progress poll or new push on any empty queue to
    * complete. (This is why we normally use method pollAt and its
    * variants that try once at the apparent base index, else
    * consider alternative actions, rather than method poll, which
    * retries.)
    
    因为我们依赖于引用的CASes，所以我们不需要在基础或顶部使用标记位. 它们是任何基于循环数组中使用的简单整数. 更新索引可确保top == base表示队列为空, 但是在未完全落实push，pop或poll的情况下，可能会使队列显得非空，这可能会出错.
    因此，单独考虑的poll操作并非没有等待时间. 一个小偷不能成功地继续下去，直到另一个正在进行的小偷（或者，如果以前是空的，一次推送）完成. 但是，总的来说，我们确保至少概率无阻塞. 
    如果尝试的窃取失败，则小偷总是选择另一个随机的受害者目标来尝试. 因此，为了使一个小偷前进，它足以进行任何正在进行的轮询或对任何空队列进行新推送来完成. 

    * This approach also enables support of a user mode in which
    * local task processing is in FIFO, not LIFO order, simply by
    * using poll rather than pop.  This can be useful in
    * message-passing frameworks in which tasks are never joined.
    * However neither mode considers affinities, loads, cache
    * localities, etc, so rarely provide the best possible
    * performance on a given machine, but portably provide good
    * throughput by averaging over these factors.  Further, even if
    * we did try to use such information, we do not usually have a
    * basis for exploiting it.  For example, some sets of tasks
    * profit from cache affinities, but others are harmed by cache
    * pollution effects. Additionally, even though it requires
    * scanning, long-term throughput is often best using random
    * selection rather than directed selection policies, so cheap
    * randomization of sufficient quality is used whenever
    * applicable.  Various Marsaglia XorShifts (some with different
    * shift constants) are inlined at use points.
    
    这种方法还支持用户模式，在该模式下，仅通过使用poll而不是pop操作，就可以以FIFO（而不是LIFO）的顺序执行本地任务处理. 这在永不加入任务的消息传递框架中很有用. 
    但是，这两种模式都没有考虑亲和力，负载，缓存位置等因素，因此很少在给定的计算机上提供最佳性能，而是通过对这些因素进行平均来可移植地提供良好的吞吐量. 
    此外，即使我们确实尝试使用此类信息，我们通常也没有利用它的基础. 
    例如，某些任务集从高速缓存亲和力中获利，而另一些任务则受到高速缓存污染效应的损害. 此外，即使需要扫描，长期吞吐量通常最好使用随机选择而不是有向选择策略，因此在适用时会使用便宜且质量足够的随机化.
    在使用点内联了各种Marsaglia XorShift（某些具有不同的移位常数）

    * WorkQueues are also used in a similar way for tasks submitted
    * to the pool. We cannot mix these tasks in the same queues used
    * by workers. Instead, we randomly associate submission queues
    * with submitting threads, using a form of hashing.  The
    * ThreadLocalRandom probe value serves as a hash code for
    * choosing existing queues, and may be randomly repositioned upon
    * contention with other submitters.  In essence, submitters act
    * like workers except that they are restricted to executing local
    * tasks that they submitted (or in the case of CountedCompleters,
    * others with the same root task).  Insertion of tasks in shared
    * mode requires a lock (mainly to protect in the case of
    * resizing) but we use only a simple spinlock (using field
    * qlock), because submitters encountering a busy queue move on to
    * try or create other queues -- they block only when creating and
    * registering new queues. Additionally, "qlock" saturates to an
    * unlockable value (-1) at shutdown. Unlocking still can be and
    * is performed by cheaper ordered writes of "qlock" in successful
    * cases, but uses CAS in unsuccessful cases.
    
    WorkQueue也以类似的方式用于提交给池的任务. 我们不能将这些任务混合在工人使用的相同队列中. 相反，我们使用散列的形式将提交队列与提交线程随机关联. 
    ThreadLocalRandom探针值用作选择现有队列的哈希码，并且可以在与其他提交者竞争时随机重新定位. 从本质上讲，提交者的行为类似于工作人员，只是他们只能执行提交的本地任务（或者在CountedCompleters的情况下，其他任务具有相同的根任务）.
    在共享模式下插入任务需要一个锁（主要是在调整大小时进行保护），但我们仅使用简单的自旋锁（使用字段qlock），因为遇到繁忙队列的提交者会继续尝试或创建其他队列-他们阻止了仅在创建和注册新队列时. 
    此外，qlock在关闭时会饱和到可解锁的值（-1）. 在成功的情况下，仍然可以通过廉价的有序写入qlock来执行解锁，但是在不成功的情况下使用CAS.

    * Management
    * ==========
    
    管理

    * The main throughput advantages of work-stealing stem from
    * decentralized control -- workers mostly take tasks from
    * themselves or each other, at rates that can exceed a billion
    * per second.  The pool itself creates, activates (enables
    * scanning for and running tasks), deactivates, blocks, and
    * terminates threads, all with minimal central information.
    * There are only a few properties that we can globally track or
    * maintain, so we pack them into a small number of variables,
    * often maintaining atomicity without blocking or locking.
    * Nearly all essentially atomic control state is held in two
    * volatile variables that are by far most often read (not
    * written) as status and consistency checks. (Also, field
    * "config" holds unchanging configuration state.)
    
    工作窃取的主要吞吐量优势来自分散的控制-工人通常以每秒超过十亿的速度从自己或彼此之间接管任务.
    池本身可以创建，激活（启用扫描和运行任务），停用，阻止和终止线程，而这些操作只需很少的中央信息即可.
    我们只能全局跟踪或维护一些属性，因此我们将它们打包到少量变量中，通常保持原子性而不阻塞或锁定. 几乎所有基本的原子控制状态都保存在两个volatile变量中，这些变量迄今为止最常被读取（而不是写入）作为状态和一致性检查.
    （此外，字段config保留不变的配置状态）

    * Field "ctl" contains 64 bits holding information needed to
    * atomically decide to add, inactivate, enqueue (on an event
    * queue), dequeue, and/or re-activate workers.  To enable this
    * packing, we restrict maximum parallelism to (1<<15)-1 (which is
    * far in excess of normal operating range) to allow ids, counts,
    * and their negations (used for thresholding) to fit into 16bit
    * subfields.
    
    字段ctl包含64位信息，这些信息用于原子地决定添加，禁用，排队（在事件队列上），出队和/或重新激活工作程序所需的信息.
    为了实现这种打包，我们将最大并行度限制为（1 << 15）-1（远远超出正常工作范围），以允许id，计数及其取反（用于阈值化）适合16bit子字段.

    * Field "runState" holds lockable state bits (STARTED, STOP, etc)
    * also protecting updates to the workQueues array.  When used as
    * a lock, it is normally held only for a few instructions (the
    * only exceptions are one-time array initialization and uncommon
    * resizing), so is nearly always available after at most a brief
    * spin. But to be extra-cautious, after spinning, method
    * awaitRunStateLock (called only if an initial CAS fails), uses a
    * wait/notify mechanics on a builtin monitor to block when
    * (rarely) needed. This would be a terrible idea for a highly
    * contended lock, but most pools run without the lock ever
    * contending after the spin limit, so this works fine as a more
    * conservative alternative. Because we don't otherwise have an
    * internal Object to use as a monitor, the "stealCounter" (an
    * AtomicLong) is used when available (it too must be lazily
    * initialized; see externalSubmit).
    
    字段runState保存可锁定状态位（STARTED，STOP等），还保护对workQueues数组的更新. 当用作锁时，它通常仅保留几条指令（唯一的例外是一次性数组初始化和不常见的调整大小），因此几乎总是在经过短暂的旋转之后才可用.
    但为谨慎起见，旋转后，方法awaitRunStateLock（仅在初始CAS失败时才调用）方法在内置监视器上使用等待/通知机制来阻止（很少）需要时.
    对于高度竞争的锁，这将是一个糟糕的主意，但是大多数池在旋转限制之后都没有竞争时运行锁，因此，作为更保守的选择，它可以很好地工作. 
    因为否则我们没有内部对象可用作监视器，因此在可用时使用stealCounter（AtomicLong）（它也必须延迟初始化；请参阅externalSubmit）.

    * Usages of "runState" vs "ctl" interact in only one case:
    * deciding to add a worker thread (see tryAddWorker), in which
    * case the ctl CAS is performed while the lock is held.
    
    runState和ctl的用法仅在一种情况下交互：决定添加一个工作线程（请参阅tryAddWorker），在这种情况下，ctl CAS是在持有锁的情况下执行的.

    * Recording WorkQueues.  WorkQueues are recorded in the
    * "workQueues" array. The array is created upon first use (see
    * externalSubmit) and expanded if necessary.  Updates to the
    * array while recording new workers and unrecording terminated
    * ones are protected from each other by the runState lock, but
    * the array is otherwise concurrently readable, and accessed
    * directly. We also ensure that reads of the array reference
    * itself never become too stale. To simplify index-based
    * operations, the array size is always a power of two, and all
    * readers must tolerate null slots. Worker queues are at odd
    * indices. Shared (submission) queues are at even indices, up to
    * a maximum of 64 slots, to limit growth even if array needs to
    * expand to add more workers. Grouping them together in this way
    * simplifies and speeds up task scanning.
    
    记录工作队列. 工作队列记录在工作队列数组中. 该数组是在首次使用时创建的（请参阅externalSubmit），并在必要时进行扩展. 
    在记录新工作线程和取消记录终止工作线程时对数组的更新受到runState锁的保护，但彼此并发可读，并且可以直接访问该数组. 我们还确保对数组引用本身的读取永远不会过时. 
    为了简化基于索引的操作，数组大小始终是2的幂，并且所有读取器都必须允许空插槽. 工作人员队列处在奇数索引处. 共享（提交）队列的索引为偶数，最多64个插槽，以限制增长，即使阵列需要扩展以添加更多工作程序也是如此.
    以这种方式将它们分组在一起可以简化并加速任务扫描.

    * All worker thread creation is on-demand, triggered by task
    * submissions, replacement of terminated workers, and/or
    * compensation for blocked workers. However, all other support
    * code is set up to work with other policies.  To ensure that we
    * do not hold on to worker references that would prevent GC, All
    * accesses to workQueues are via indices into the workQueues
    * array (which is one source of some of the messy code
    * constructions here). In essence, the workQueues array serves as
    * a weak reference mechanism. Thus for example the stack top
    * subfield of ctl stores indices, not references.
    
    所有工作线程的创建都是按需的，由任务提交，替换终止的工作人员和/或补偿被阻止的工作人员触发. 但是，所有其他支持代码均已设置为可与其他策略一起使用. 
    为了确保不保留会阻止GC的工作程序引用，对workQueue的所有访问都是通过workQueues数组中的索引进行的（该数组是此处某些混乱代码构造的来源之一）.
    本质上，workQueues数组用作弱引用机制. 因此，例如ctl的stack top子字段存储索引，而不是引用.

    * Queuing Idle Workers. Unlike HPC work-stealing frameworks, we
    * cannot let workers spin indefinitely scanning for tasks when
    * none can be found immediately, and we cannot start/resume
    * workers unless there appear to be tasks available.  On the
    * other hand, we must quickly prod them into action when new
    * tasks are submitted or generated. In many usages, ramp-up time
    * to activate workers is the main limiting factor in overall
    * performance, which is compounded at program start-up by JIT
    * compilation and allocation. So we streamline this as much as
    * possible.
    
    排队空闲工人. 与HPC工作窃取框架不同，我们不能让工作人员无限期地旋转以立即查找到没有任务的任务，并且除非有任务可用，否则我们不能启动/继续工作.
    另一方面，在提交或生成新任务时，我们必须迅速使它们生效. 在许多情况下，激活工作人员的加速时间是整体性能的主要限制因素，在程序启动时，通过JIT编译和分配会更加复杂.
    因此，我们尽可能简化了这一过程.

    * The "ctl" field atomically maintains active and total worker
    * counts as well as a queue to place waiting threads so they can
    * be located for signalling. Active counts also play the role of
    * quiescence indicators, so are decremented when workers believe
    * that there are no more tasks to execute. The "queue" is
    * actually a form of Treiber stack.  A stack is ideal for
    * activating threads in most-recently used order. This improves
    * performance and locality, outweighing the disadvantages of
    * being prone to contention and inability to release a worker
    * unless it is topmost on stack.  We park/unpark workers after
    * pushing on the idle worker stack (represented by the lower
    * 32bit subfield of ctl) when they cannot find work.  The top
    * stack state holds the value of the "scanState" field of the
    * worker: its index and status, plus a version counter that, in
    * addition to the count subfields (also serving as version
    * stamps) provide protection against Treiber stack ABA effects.
    
    ctl字段原子地维护活动和总工作人员计数，以及用于放置等待线程的队列，以便可以定位它们以发出信号.
    主动计数也起到静态指标的作用，因此当工作人员认为没有更多的任务可以执行时，主动计数就会减少. 
    队列实际上是Treiber堆栈的一种形式. 堆栈是按最近使用的顺序激活线程的理想选择. 这改善了性能和局部性，克服了易于争用和无法释放工作程序（除非其在堆栈上位于顶部）的缺点. 
    当工人找不到工作时，他们在闲置的工人堆栈（由ctl的较低32位子字段表示）上推入后，就将其停放. 
    顶层堆栈状态保存工作程序的scanState字段的值：其索引和状态，以及一个版本计数器，该计数器除了count子字段（还用作版本标记）之外，还可以防止Treiber堆栈ABA的影响.

    * Field scanState is used by both workers and the pool to manage
    * and track whether a worker is INACTIVE (possibly blocked
    * waiting for a signal), or SCANNING for tasks (when neither hold
    * it is busy running tasks).  When a worker is inactivated, its
    * scanState field is set, and is prevented from executing tasks,
    * even though it must scan once for them to avoid queuing
    * races. Note that scanState updates lag queue CAS releases so
    * usage requires care. When queued, the lower 16 bits of
    * scanState must hold its pool index. So we place the index there
    * upon initialization (see registerWorker) and otherwise keep it
    * there or restore it when necessary.
    
    工作人员和池都使用字段scanState来管理和跟踪工作人员是不活动的（可能是阻塞的等待信号），还是对任务进行扫描（当两个都不持有它正在忙于运行任务时）.
    取消激活工作程序后，将设置其scanState字段，并阻止其执行任务，即使它必须对其进行扫描一次也可以避免排队.
    请注意，scanState更新延迟队列CAS释放，因此使用时需要注意. 排队时，scanState的低16位必须保留其池索引. 因此，我们在初始化时将索引放置在此处（请参见registerWorker），否则将其保留在该索引处或在必要时将其还原.

    * Memory ordering.  See "Correct and Efficient Work-Stealing for
    * Weak Memory Models" by Le, Pop, Cohen, and Nardelli, PPoPP 2013
    * (http://www.di.ens.fr/~zappa/readings/ppopp13.pdf) for an
    * analysis of memory ordering requirements in work-stealing
    * algorithms similar to the one used here.  We usually need
    * stronger than minimal ordering because we must sometimes signal
    * workers, requiring Dekker-like full-fences to avoid lost
    * signals.  Arranging for enough ordering without expensive
    * over-fencing requires tradeoffs among the supported means of
    * expressing access constraints. The most central operations,
    * taking from queues and updating ctl state, require full-fence
    * CAS.  Array slots are read using the emulation of volatiles
    * provided by Unsafe.  Access from other threads to WorkQueue
    * base, top, and array requires a volatile load of the first of
    * any of these read.  We use the convention of declaring the
    * "base" index volatile, and always read it before other fields.
    * The owner thread must ensure ordered updates, so writes use
    * ordered intrinsics unless they can piggyback on those for other
    * writes.  Similar conventions and rationales hold for other
    * WorkQueue fields (such as "currentSteal") that are only written
    * by owners but observed by others.
    
    内存排序. 有关分析，请参见Le，Pop，Cohen和Nardelli撰写的PPoPP 2013的正确有效地处理弱内存模型（http://www.di.ens.fr/~zappa/readings/ppopp13.pdf）.
    工作窃取算法中与此处使用的算法类似的内存排序要求. 我们通常需要比最小顺序更强的命令，因为有时我们必须向工人发出信号，要求像Dekker一样的全栅栏以避免信号丢失. 
    要安排足够的排序而又不花费过多的费用，则需要在表示访问限制的受支持方法之间进行权衡. 最重要的操作是从队列中获取并更新ctl状态，因此需要完整的CAS. 
    使用Unsafe提供的易失性仿真读取阵列插槽. 从其他线程访问WorkQueue的基数，顶部和数组需要对这些读取中的任何一个进行易变负载. 
    我们使用声明基本索引为volatile的约定，并始终在其他字段之前读取它. 
    所有者线程必须确保有序的更新，因此写操作将使用有序的内部函数，除非它们可以背负其他写操作的内容. 其他WorkQueue字段（例如currentSteal）也具有类似的约定和原理，这些字段仅由所有者编写但被其他人观察.

    * Creating workers. To create a worker, we pre-increment total
    * count (serving as a reservation), and attempt to construct a
    * ForkJoinWorkerThread via its factory. Upon construction, the
    * new thread invokes registerWorker, where it constructs a
    * WorkQueue and is assigned an index in the workQueues array
    * (expanding the array if necessary). The thread is then
    * started. Upon any exception across these steps, or null return
    * from factory, deregisterWorker adjusts counts and records
    * accordingly.  If a null return, the pool continues running with
    * fewer than the target number workers. If exceptional, the
    * exception is propagated, generally to some external caller.
    * Worker index assignment avoids the bias in scanning that would
    * occur if entries were sequentially packed starting at the front
    * of the workQueues array. We treat the array as a simple
    * power-of-two hash table, expanding as needed. The seedIndex
    * increment ensures no collisions until a resize is needed or a
    * worker is deregistered and replaced, and thereafter keeps
    * probability of collision low. We cannot use
    * ThreadLocalRandom.getProbe() for similar purposes here because
    * the thread has not started yet, but do so for creating
    * submission queues for existing external threads.
    
    创造工人. 要创建一个工作程序，我们要预增加总数（用作保留），并尝试通过其工厂构造一个ForkJoinWorkerThread. 构造后，新线程将调用registerWorker，在此构造一个WorkQueue并在workQueues数组中分配一个索引（必要时扩展该数组）.
    然后启动线程. 如果这些步骤有任何异常，或者工厂返回空值，则deregisterWorker会调整计数并进行相应记录. 如果返回空值，则池将继续以少于目标数目的worker的状态运行。如果例外，则通常将异常传播到某些外部调用方.
    辅助索引的分配避免了从工作队列数组的开头开始顺序填充条目时发生的扫描偏差. 我们将数组视为简单的二乘幂哈希表，并根据需要进行扩展. 
    seedIndex增量可确保在需要调整大小或注销并替换工作人员之前不会发生冲突，此后将发生冲突的可能性保持在较低水平. 
    我们不能在这里将ThreadLocalRandom.getProbe用于类似目的，因为该线程尚未启动，但是这样做是为了为现有外部线程创建提交队列.

    * Deactivation and waiting. Queuing encounters several intrinsic
    * races; most notably that a task-producing thread can miss
    * seeing (and signalling) another thread that gave up looking for
    * work but has not yet entered the wait queue.  When a worker
    * cannot find a task to steal, it deactivates and enqueues. Very
    * often, the lack of tasks is transient due to GC or OS
    * scheduling. To reduce false-alarm deactivation, scanners
    * compute checksums of queue states during sweeps.  (The
    * stability checks used here and elsewhere are probabilistic
    * variants of snapshot techniques -- see Herlihy & Shavit.)
    * Workers give up and try to deactivate only after the sum is
    * stable across scans. Further, to avoid missed signals, they
    * repeat this scanning process after successful enqueuing until
    * again stable.  In this state, the worker cannot take/run a task
    * it sees until it is released from the queue, so the worker
    * itself eventually tries to release itself or any successor (see
    * tryRelease).  Otherwise, upon an empty scan, a deactivated
    * worker uses an adaptive local spin construction (see awaitWork)
    * before blocking (via park). Note the unusual conventions about
    * Thread.interrupts surrounding parking and other blocking:
    * Because interrupts are used solely to alert threads to check
    * termination, which is checked anyway upon blocking, we clear
    * status (using Thread.interrupted) before any call to park, so
    * that park does not immediately return due to status being set
    * via some other unrelated call to interrupt in user code.
    
    停用并等待. 排队遇到了一些固有种族. 最值得注意的是，产生任务的线程可能会错过看到（并发信号）另一个放弃寻找工作但尚未进入等待队列的线程.
    当工人找不到要偷的任务时，它会停用并排队. 通常，由于GC或OS调度，缺少任务是暂时的. 为了减少错误警报的停用，扫描程序会在扫描期间计算队列状态的校验和.（这里和其他地方使用的稳定性检查是快照技术的概率变体，请参阅Herlihy＆Shavit）
    工作者放弃并尝试仅在跨扫描的总和稳定之后才停用它. 此外，为避免丢失信号，它们在成功入队后重复此扫描过程，直到再次稳定。在这种状态下，工作程序无法执行/运行它看到的任务，直到将其从队列中释放为止，因此工作程序本身最终会尝试释放其自身或任何后续任务（请参见tryRelease）. 否则，在进行空扫描时，停用的工作人员会在阻塞（通过停车）之前使用自适应本地自旋构造（请参阅awaitWork）.
    请注意有关Thread.interrupts围绕停放和其他阻塞的不寻常约定：由于中断仅用于提醒线程检查终止，而无论是否在阻塞时都会检查终止，因此我们在调用任何Park之前清除状态（使用Thread.interrupted），以便由于通过其他一些不相关的调用来设置状态以中断用户代码，因此Park不会立即返回.

    * Signalling and activation.  Workers are created or activated
    * only when there appears to be at least one task they might be
    * able to find and execute.  Upon push (either by a worker or an
    * external submission) to a previously (possibly) empty queue,
    * workers are signalled if idle, or created if fewer exist than
    * the given parallelism level.  These primary signals are
    * buttressed by others whenever other threads remove a task from
    * a queue and notice that there are other tasks there as well.
    * On most platforms, signalling (unpark) overhead time is
    * noticeably long, and the time between signalling a thread and
    * it actually making progress can be very noticeably long, so it
    * is worth offloading these delays from critical paths as much as
    * possible. Also, because inactive workers are often rescanning
    * or spinning rather than blocking, we set and clear the "parker"
    * field of WorkQueues to reduce unnecessary calls to unpark.
    * (This requires a secondary recheck to avoid missed signals.)
    
    信号和激活. 仅当似乎至少可以找到并执行一个任务时，才创建或激活工作程序. 在（由工作人员或外部提交者）将其推送到先前（可能是）空队列时，会在空闲时发出信号，如果存在的工作量少于给定的并行度，则会发出工作信号.
    每当其他线程从队列中删除任务并注意到那里还有其他任务时，这些主要信号就会被其他人支持. 在大多数平台上，发信号（释放）的开销时间非常长，发信号的线程与线程实际取得进展之间的时间可能非常长，因此值得从关键路径上分担这些延迟.
    另外，由于不活动的工作人员通常是重新扫描或旋转而不是阻塞，因此我们设置并清除了WorkQueues的parker字段，以减少不必要的取消停车的呼叫.（这需要进行二次重新检查，以避免信号遗漏）

    * Trimming workers. To release resources after periods of lack of
    * use, a worker starting to wait when the pool is quiescent will
    * time out and terminate (see awaitWork) if the pool has remained
    * quiescent for period IDLE_TIMEOUT, increasing the period as the
    * number of threads decreases, eventually removing all workers.
    * Also, when more than two spare threads exist, excess threads
    * are immediately terminated at the next quiescent point.
    * (Padding by two avoids hysteresis.)
    
    修剪工人. 要在不使用一段时间后释放资源，如果池在IDLE_TIMEOUT期间保持静态，则在池处于静止状态时开始等待的工作程序将超时并终止（请参阅awaitWork），随着线程数的减少，该时间段将增加，最终遣散所有工人.
    同样，当存在两个以上的备用线程时，多余的线程会在下一个静态点立即终止（两个填充可避免滞后）.

    * Shutdown and Termination. A call to shutdownNow invokes
    * tryTerminate to atomically set a runState bit. The calling
    * thread, as well as every other worker thereafter terminating,
    * helps terminate others by setting their (qlock) status,
    * cancelling their unprocessed tasks, and waking them up, doing
    * so repeatedly until stable (but with a loop bounded by the
    * number of workers).  Calls to non-abrupt shutdown() preface
    * this by checking whether termination should commence. This
    * relies primarily on the active count bits of "ctl" maintaining
    * consensus -- tryTerminate is called from awaitWork whenever
    * quiescent. However, external submitters do not take part in
    * this consensus.  So, tryTerminate sweeps through queues (until
    * stable) to ensure lack of in-flight submissions and workers
    * about to process them before triggering the "STOP" phase of
    * termination. (Note: there is an intrinsic conflict if
    * helpQuiescePool is called when shutdown is enabled. Both wait
    * for quiescence, but tryTerminate is biased to not trigger until
    * helpQuiescePool completes.)
    
    关机和终止. 调用shutdownNow会调用tryTerminate以原子方式设置runState位. 
    调用线程以及此后终止的所有其他工作线程，通过设置其（qlock）状态，取消其未处理的任务并唤醒它们，反复执行直到稳定为止，以帮助终止其他线程（但循环受工作线程数量限制） ）.
    调用non-abrupt shutdown可以通过检查是否应该终止终止来开始. 这主要取决于维持共识的“ctl的活动计数位-每当静止时，trywaiter从awaitWork调用.
    但是，外部提交者不参与该共识. 因此，tryTerminate扫描队列（直到稳定），以确保缺少正在进行中的提交，并且工作人员将在触发终止的“停止”阶段之前对其进行处理.
    （注意：启用关闭功能后，如果调用helpQuiescePool会发生内在冲突。两者都等待静态，但是tryTerminate偏向于在helpQuiescePool完成之前不会触发）
    
    * Joining Tasks
    * =============
    
    加入任务

    * Any of several actions may be taken when one worker is waiting
    * to join a task stolen (or always held) by another.  Because we
    * are multiplexing many tasks on to a pool of workers, we can't
    * just let them block (as in Thread.join).  We also cannot just
    * reassign the joiner's run-time stack with another and replace
    * it later, which would be a form of "continuation", that even if
    * possible is not necessarily a good idea since we may need both
    * an unblocked task and its continuation to progress.  Instead we
    * combine two tactics:
    
    当一个工人正在等待参加另一人偷来的（或总是被抓住的）任务时，可以采取多种措施中的任何一种. 
    因为我们将许多任务复用到一个工作池上，所以我们不能只让它们阻塞（如Thread.join中一样）.
    我们也不能只是将joiner的运行时堆栈重新分配给另一个，然后在以后替换它，这将是continuation的一种形式，即使可能，也不一定是一个好主意，因为我们既需要无阻塞的任务，又需要它的继续进行进展.
    相反，我们结合了两种策略：

    *   Helping: Arranging for the joiner to execute some task that it
    *      would be running if the steal had not occurred.

        帮助：安排加入者执行一些如果没有发生盗窃将要运行的任务.

    *   Compensating: Unless there are already enough live threads,
    *      method tryCompensate() may create or re-activate a spare
    *      thread to compensate for blocked joiners until they unblock.
    
        补偿：除非已经有足够的活动线程，否则方法tryCompensate可能会创建或重新激活备用线程以补偿阻塞的连接器，直到它们解除阻塞为止.

    * A third form (implemented in tryRemoveAndExec) amounts to
    * helping a hypothetical compensator: If we can readily tell that
    * a possible action of a compensator is to steal and execute the
    * task being joined, the joining thread can do so directly,
    * without the need for a compensation thread (although at the
    * expense of larger run-time stacks, but the tradeoff is
    * typically worthwhile).

    
    第三种形式（在tryRemoveAndExec中实现）相当于帮助一个假设的补偿器：如果我们可以很容易地看出补偿器的可能动作是窃取并执行要加入的任务，则加入线程可以直接这样做，而无需执行任何操作.
    补偿线程（尽管以较大的运行时堆栈为代价，但通常值得进行权衡）
    
    * The ManagedBlocker extension API can't use helping so relies
    * only on compensation in method awaitBlocker.
    
    ManagedBlocker扩展API不能使用帮助，因此仅依赖于方法awaitBlocker中的补偿.

    * The algorithm in helpStealer entails a form of "linear
    * helping".  Each worker records (in field currentSteal) the most
    * recent task it stole from some other worker (or a submission).
    * It also records (in field currentJoin) the task it is currently
    * actively joining. Method helpStealer uses these markers to try
    * to find a worker to help (i.e., steal back a task from and
    * execute it) that could hasten completion of the actively joined
    * task.  Thus, the joiner executes a task that would be on its
    * own local deque had the to-be-joined task not been stolen. This
    * is a conservative variant of the approach described in Wagner &
    * Calder "Leapfrogging: a portable technique for implementing
    * efficient futures" SIGPLAN Notices, 1993
    * (http://portal.acm.org/citation.cfm?id=155354). It differs in
    * that: (1) We only maintain dependency links across workers upon
    * steals, rather than use per-task bookkeeping.  This sometimes
    * requires a linear scan of workQueues array to locate stealers,
    * but often doesn't because stealers leave hints (that may become
    * stale/wrong) of where to locate them.  It is only a hint
    * because a worker might have had multiple steals and the hint
    * records only one of them (usually the most current).  Hinting
    * isolates cost to when it is needed, rather than adding to
    * per-task overhead.  (2) It is "shallow", ignoring nesting and
    * potentially cyclic mutual steals.  (3) It is intentionally
    * racy: field currentJoin is updated only while actively joining,
    * which means that we miss links in the chain during long-lived
    * tasks, GC stalls etc (which is OK since blocking in such cases
    * is usually a good idea).  (4) We bound the number of attempts
    * to find work using checksums and fall back to suspending the
    * worker and if necessary replacing it with another.
    
    helpStealer中的算法需要一种“线性帮助”形式. 每个工作人员（在currentSteal字段中）记录它从其他工作人员（或提交）中偷走的最新任务. 它还记录（在currentJoin字段中）当前正在主动加入的任务.
    helpStealer方法使用这些标记来尝试寻找可以帮助完成主动加入的任务的工作人员（即从中偷回任务并执行该任务）。因此，如果要加入的任务没有被窃取，则连接器执行的任务将由其自己的本地双端队列执行.
    这是Wagner＆Calder“跨越式：实现有效期货的便携式技术中描述的方法的保守变体.
    它的不同之处在于：（1）我们仅在窃取时在工人之间维护依赖关系链接，而不使用按任务记帐.
    有时这需要对workQueues数组进行线性扫描以定位盗窃者，但通常不需要这样做，因为盗窃者会留下提示（可能会陈旧/错误）如何定位盗窃者.
    这只是一个提示，因为一个工人可能有多次偷窃，而提示仅记录了其中一个（通常是最新的）。提示将成本隔离在需要的时间，而不是增加每个任务的开销.
    （2）它是“浅”的，忽略了嵌套和潜在的周期性相互窃取.
    （3）这是故意的：字段currentJoin仅在主动加入时进行更新，这意味着我们在长期任务，GC停顿等过程中会错过链中的链接（这很正常，因为在这种情况下进行阻塞通常是个好主意） .
    （4）我们使用校验和来限制找到工作的尝试的次数，然后回退到暂停该工作程序，并在必要时将其替换为另一个.

    * Helping actions for CountedCompleters do not require tracking
    * currentJoins: Method helpComplete takes and executes any task
    * with the same root as the task being waited on (preferring
    * local pops to non-local polls). However, this still entails
    * some traversal of completer chains, so is less efficient than
    * using CountedCompleters without explicit joins.
    
    CountedCompleters的帮助操作不需要跟踪currentJoins：方法helpComplete可以执行和执行与正在等待的任务具有相同根目录的任何任务（优先于本地弹出而不是非本地轮询）.
    但是，这仍然需要对完成程序链进行一些遍历，因此效率不如使用没有显式连接的CountedCompleters.

    * Compensation does not aim to keep exactly the target
    * parallelism number of unblocked threads running at any given
    * time. Some previous versions of this class employed immediate
    * compensations for any blocked join. However, in practice, the
    * vast majority of blockages are transient byproducts of GC and
    * other JVM or OS activities that are made worse by replacement.
    * Currently, compensation is attempted only after validating that
    * all purportedly active threads are processing tasks by checking
    * field WorkQueue.scanState, which eliminates most false
    * positives.  Also, compensation is bypassed (tolerating fewer
    * threads) in the most common case in which it is rarely
    * beneficial: when a worker with an empty queue (thus no
    * continuation tasks) blocks on a join and there still remain
    * enough threads to ensure liveness.
    
    补偿的目的并不是要确保在任何给定时间都运行无阻塞线程的目标并行度. 此类的某些先前版本对任何阻塞的连接采用立即补偿. 但是，实际上，绝大多数阻塞是GC和其他JVM或OS活动的暂时性副产品，这些副产品由于更换而变得更糟.
    当前，仅在通过检查字段WorkQueue.scanState确认所有据称活动的线程正在处理任务之后才尝试进行补偿，从而消除了大多数误报。此外，在最不常见的情况下，绕过补偿（允许更少的线程）是很少有好处的：当队列为空的工人（因此没有继续任务）在连接上阻塞时，仍然有足够的线程来确保活动.

    * The compensation mechanism may be bounded.  Bounds for the
    * commonPool (see commonMaxSpares) better enable JVMs to cope
    * with programming errors and abuse before running out of
    * resources to do so. In other cases, users may supply factories
    * that limit thread construction. The effects of bounding in this
    * pool (like all others) is imprecise.  Total worker counts are
    * decremented when threads deregister, not when they exit and
    * resources are reclaimed by the JVM and OS. So the number of
    * simultaneously live threads may transiently exceed bounds.
    
    补偿机制可能是有界的.
    使用commonPool的界限（请参阅commonMaxSpares）可以更好地使JVM在耗尽资源之前解决编程错误和滥用.
    在其他情况下，用户可能会提供限制线程构造的工厂.
    与其他所有池一样，此池中的边界影响不精确. 注销线程时，工作人员总数会减少，而不是在退出线程时，JVM和OS会回收资源时，总数会减少. 因此，同时处于活动状态的线程数可能会暂时超出限制.

    * Common Pool
    * ===========
    
    公共池

    * The static common pool always exists after static
    * initialization.  Since it (or any other created pool) need
    * never be used, we minimize initial construction overhead and
    * footprint to the setup of about a dozen fields, with no nested
    * allocation. Most bootstrapping occurs within method
    * externalSubmit during the first submission to the pool.
    
    静态公共池在静态初始化之后始终存在. 由于不需要使用它（或任何其他创建的池），因此我们将初始构造开销和占用空间最小化到大约十二个字段的设置，而没有嵌套分配.
    在第一次提交到池期间，大多数引导发生在方法externalSubmit中.

    * When external threads submit to the common pool, they can
    * perform subtask processing (see externalHelpComplete and
    * related methods) upon joins.  This caller-helps policy makes it
    * sensible to set common pool parallelism level to one (or more)
    * less than the total number of available cores, or even zero for
    * pure caller-runs.  We do not need to record whether external
    * submissions are to the common pool -- if not, external help
    * methods return quickly. These submitters would otherwise be
    * blocked waiting for completion, so the extra effort (with
    * liberally sprinkled task status checks) in inapplicable cases
    * amounts to an odd form of limited spin-wait before blocking in
    * ForkJoinTask.join.
    
    当外部线程提交到公共池时，它们可以在联接时执行子任务处理（请参阅externalHelpComplete和相关方法）.
    通过此呼叫者帮助策略，可以明智地将公共池并行度级别设置为比可用核心总数少一个（或多个），对于纯呼叫者运行，甚至可以设置为零.
    我们不需要记录是否将外部提交提交到公共池中-否则，外部帮助方法会迅速返回.
    否则，这些提交者将被阻止等待完成，因此在不适用的情况下，额外的工作量（大量分散的任务状态检查）构成了在ForkJoinTask.join中阻止之前有限的旋转等待的一种奇怪形式.

    * As a more appropriate default in managed environments, unless
    * overridden by system properties, we use workers of subclass
    * InnocuousForkJoinWorkerThread when there is a SecurityManager
    * present. These workers have no permissions set, do not belong
    * to any user-defined ThreadGroup, and erase all ThreadLocals
    * after executing any top-level task (see WorkQueue.runTask).
    * The associated mechanics (mainly in ForkJoinWorkerThread) may
    * be JVM-dependent and must access particular Thread class fields
    * to achieve this effect.
    
    作为托管环境中更合适的默认值，除非存在系统属性覆盖，否则当存在SecurityManager时，我们将使用子类InnocuousForkJoinWorkerThread的工作程序.
    这些工作程序没有权限设置，不属于任何用户定义的ThreadGroup，并且在执行任何顶级任务后请擦除所有ThreadLocals（请参阅WorkQueue.runTask）.
    关联的机制（主要在ForkJoinWorkerThread中）可能取决于JVM，并且必须访问特定的Thread类字段才能实现此效果

    * Style notes
    * ===========
    
    风格笔记

    * Memory ordering relies mainly on Unsafe intrinsics that carry
    * the further responsibility of explicitly performing null- and
    * bounds- checks otherwise carried out implicitly by JVMs.  This
    * can be awkward and ugly, but also reflects the need to control
    * outcomes across the unusual cases that arise in very racy code
    * with very few invariants. So these explicit checks would exist
    * in some form anyway.  All fields are read into locals before
    * use, and null-checked if they are references.  This is usually
    * done in a "C"-like style of listing declarations at the heads
    * of methods or blocks, and using inline assignments on first
    * encounter.  Array bounds-checks are usually performed by
    * masking with array.length-1, which relies on the invariant that
    * these arrays are created with positive lengths, which is itself
    * paranoically checked. Nearly all explicit checks lead to
    * bypass/return, not exception throws, because they may
    * legitimately arise due to cancellation/revocation during
    * shutdown.
    
    内存处理主要依赖于Unsafe内部函数，这些内部函数承担显式执行null和bounds检查的其他责任，否则将由JVM隐式执行.
    这可能很尴尬和丑陋，但也反映出需要控制非常活泼的代码中具有很少变量的异常情况下的结果.
    因此，这些显式检查无论如何都会以某种形式存在.
    使用之前，所有字段都被读入本地，如果它们是引用，则进行空检查.
    这通常以类似于C的样式在方法或块的开头列出声明，并在首次遇到时使用内联分配来完成.
    数组边界检查通常是通过用array.length-1进行掩码来执行的，array.length-1依赖于不变的条件，即这些数组是用正长度创建的，而其长度却是被随机检查的.
    几乎所有显式检查都会导致绕过/返回，而不是异常引发，因为它们可能由于关闭期间的取消/撤销而合法地出现.

    * There is a lot of representation-level coupling among classes
    * ForkJoinPool, ForkJoinWorkerThread, and ForkJoinTask.  The
    * fields of WorkQueue maintain data structures managed by
    * ForkJoinPool, so are directly accessed.  There is little point
    * trying to reduce this, since any associated future changes in
    * representations will need to be accompanied by algorithmic
    * changes anyway. Several methods intrinsically sprawl because
    * they must accumulate sets of consistent reads of fields held in
    * local variables.  There are also other coding oddities
    * (including several unnecessary-looking hoisted null checks)
    * that help some methods perform reasonably even when interpreted
    * (not compiled).
    
    在类ForkJoinPool，ForkJoinWorkerThread和ForkJoinTask之间，有很多表示层级的耦合.
    WorkQueue的字段维护由ForkJoinPool管理的数据结构，因此可以直接访问.
    减少这种情况毫无意义，因为无论如何将来表示中任何相关的更改都需要伴随算法的更改.
    几种方法本质上无处不在，因为它们必须累积对局部变量中保存的字段的一致读取集.
    还有其他编码异常（包括一些看起来不必要的悬挂式空检查），即使在解释（未编译）时也可以帮助某些方法合理执行.

    * The order of declarations in this file is (with a few exceptions):

    该文件中的声明顺序为（有一些例外）：

    * (1) Static utility functions
    * (2) Nested (static) classes
    * (3) Static fields
    * (4) Fields, along with constants used when unpacking some of them
    * (5) Internal control methods
    * (6) Callbacks and other support for ForkJoinTask methods
    * (7) Exported methods
    * (8) Static block initializing statics in minimally dependent order





    */

```