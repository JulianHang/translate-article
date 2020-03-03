### 翻译CompletableFuture注释

```java

   /**
    * A {@link Future} that may be explicitly completed (setting its
    * value and status), and may be used as a {@link CompletionStage},
    * supporting dependent functions and actions that trigger upon its
    * completion.
    
    可以显式（设置其值与状态）完成的Future，并且可以用作CompletionStage，支持在完成时触发的相关功能和操作.

    * <p>When two or more threads attempt to
    * {@link #complete complete},
    * {@link #completeExceptionally completeExceptionally}, or
    * {@link #cancel cancel}
    * a CompletableFuture, only one of them succeeds.
    
    当两个或多个线程complete、completeExceptionally或cancel一个CompletableFuture，只有其中一个线程会成功.

    * <p>In addition to these and related methods for directly
    * manipulating status and results, CompletableFuture implements
    * interface {@link CompletionStage} with the following policies: <ul>
    
    除了直接操作状态和结果的这些方法和相关方法之外，CompletableFuture还通过以下策略实现接口CompletionStage.

    * <li>Actions supplied for dependent completions of
    * <em>non-async</em> methods may be performed by the thread that
    * completes the current CompletableFuture, or by any other caller of
    * a completion method.</li>
    
    为非异步方法的相关完成提供的动作可以由完成当前CompletableFuture的线程执行，也可以由完成方法的任何其他调用者执行.

    * <li>All <em>async</em> methods without an explicit Executor
    * argument are performed using the {@link ForkJoinPool#commonPool()}
    * (unless it does not support a parallelism level of at least two, in
    * which case, a new Thread is created to run each task).  To simplify
    * monitoring, debugging, and tracking, all generated asynchronous
    * tasks are instances of the marker interface {@link
    * AsynchronousCompletionTask}. </li>
    
    所有没有显式Executor参数的async方法都是使用ForkJoinPool＃commonPool执行.

    * <li>All CompletionStage methods are implemented independently of
    * other public methods, so the behavior of one method is not impacted
    * by overrides of others in subclasses.  </li> </ul>
    
    所有CompletionStage方法均独立于其他公共方法实现，因此一个方法的行为不受子类中其他方法的覆盖影响.

    * <p>CompletableFuture also implements {@link Future} with the following
    * policies: <ul>
    
    CompletableFuture还通过以下策略实现了Future：

    * <li>Since (unlike {@link FutureTask}) this class has no direct
    * control over the computation that causes it to be completed,
    * cancellation is treated as just another form of exceptional
    * completion.  Method {@link #cancel cancel} has the same effect as
    * {@code completeExceptionally(new CancellationException())}. Method
    * {@link #isCompletedExceptionally} can be used to determine if a
    * CompletableFuture completed in any exceptional fashion.</li>
    
    由于（与FutureTask不同）此类无法直接控制导致其完成的计算，因此取消被视为异常完成的另一种形式. 方法cancel与completeExceptionally（new CancellationException（））具有相同的作用.
    方法isCompletedExceptionally可用于确定CompletableFuture是否以任何异常方式完成.

    * <li>In case of exceptional completion with a CompletionException,
    * methods {@link #get()} and {@link #get(long, TimeUnit)} throw an
    * {@link ExecutionException} with the same cause as held in the
    * corresponding CompletionException.  To simplify usage in most
    * contexts, this class also defines methods {@link #join()} and
    * {@link #getNow} that instead throw the CompletionException directly
    * in these cases.</li> </ul>
    *

    如果CompletionException异常完成，则方法get和get（long，TimeUnit）}抛出ExecutionException，其原因与相应的CompletionException中保存的原因相同.
    为了简化大多数情况下的使用，此类还定义了方法join和getNow，在这些情况下，它们直接抛出CompletionException.

    /

```

### 翻译CompletableFuture内部注释

```java


   /*
    * Overview:
    *
    * A CompletableFuture may have dependent completion actions,
    * collected in a linked stack. It atomically completes by CASing
    * a result field, and then pops off and runs those actions. This
    * applies across normal vs exceptional outcomes, sync vs async
    * actions, binary triggers, and various forms of completions.
    
    CompletableFuture可能具有从属完成操作，这些操作收集在链接堆栈中. 它通过CASing结果字段来自动完成，然后弹出并运行这些操作. 
    这适用于正常与异常结果，同步与异步操作，二进制触发器以及各种形式的完成.

    * Non-nullness of field result (set via CAS) indicates done.  An
    * AltResult is used to box null as a result, as well as to hold
    * exceptions.  Using a single field makes completion simple to
    * detect and trigger.  Encoding and decoding is straightforward
    * but adds to the sprawl of trapping and associating exceptions
    * with targets.  Minor simplifications rely on (static) NIL (to
    * box null results) being the only AltResult with a null
    * exception field, so we don't usually need explicit comparisons.
    * Even though some of the generics casts are unchecked (see
    * SuppressWarnings annotations), they are placed to be
    * appropriate even if checked.
    
    非空字段结果（通过CAS设置）指示已完成. AltResult用于将结果装箱为空，并保存异常. 使用单个字段可使完成易于检测和触发. 编码和解码非常简单，但是却增加了将异常与目标联系起来的麻烦.
    较小的简化依赖于（静态）NIL（将空结果装箱）是唯一一个具有空异常字段的AltResult，因此我们通常不需要显式比较. 即使未选中某些泛型转换（请参见SuppressWarnings注释），即使已选中，也将它们放置为适当的位置.

    * Dependent actions are represented by Completion objects linked
    * as Treiber stacks headed by field "stack". There are Completion
    * classes for each kind of action, grouped into single-input
    * (UniCompletion), two-input (BiCompletion), projected
    * (BiCompletions using either (not both) of two inputs), shared
    * (CoCompletion, used by the second of two sources), zero-input
    * source actions, and Signallers that unblock waiters. Class
    * Completion extends ForkJoinTask to enable async execution
    * (adding no space overhead because we exploit its "tag" methods
    * to maintain claims). It is also declared as Runnable to allow
    * usage with arbitrary executors.
    
    依赖动作由链接为Treiber堆栈（以字段stack为头）的Completion对象表示. 每种操作都有完成类，这些类分为单输入，两输入，投影，共享，零输入源操作和解除阻塞服务程序的信号器.
    Completion类扩展了ForkJoinTask以启用异步执行（不增加空间开销，因为我们利用其"标记"方法来维护声明）.
    它也被声明为Runnable以允许与任意Executors一起使用

    * Support for each kind of CompletionStage relies on a separate
    * class, along with two CompletableFuture methods:
    
    对每种CompletionStage的支持都依赖于一个单独的类以及两个CompletableFuture方法：

    * * A Completion class with name X corresponding to function,
    *   prefaced with "Uni", "Bi", or "Or". Each class contains
    *   fields for source(s), actions, and dependent. They are
    *   boringly similar, differing from others only with respect to
    *   underlying functional forms. We do this so that users don't
    *   encounter layers of adaptors in common usages. We also
    *   include "Relay" classes/methods that don't correspond to user
    *   methods; they copy results from one stage to another.
    
    具有名称X的Completion类，对应于函数，其前缀为Uni，Bi或Or. 每个类都包含源，操作和从属. 它们无聊地相似，仅在基本功能形式上与众不同.
    我们这样做是为了使用户在常见用法中不会遇到适配器层. 我们还包括与用户方法不对应的"中继"类/方法. 他们将结果从一个阶段复制到另一个阶段.

    * * Boolean CompletableFuture method x(...) (for example
    *   uniApply) takes all of the arguments needed to check that an
    *   action is triggerable, and then either runs the action or
    *   arranges its async execution by executing its Completion
    *   argument, if present. The method returns true if known to be
    *   complete.
    
    布尔值CompletableFuture方法x（...）（例如uniApply）采用检查动作是否可触发所需的所有参数，然后运行该动作或通过执行其Completion参数（如果存在）来安排其异步执行. 如果已知完成，则该方法返回true.

    * * Completion method tryFire(int mode) invokes the associated x
    *   method with its held arguments, and on success cleans up.
    *   The mode argument allows tryFire to be called twice (SYNC,
    *   then ASYNC); the first to screen and trap exceptions while
    *   arranging to execute, and the second when called from a
    *   task. (A few classes are not used async so take slightly
    *   different forms.)  The claim() callback suppresses function
    *   invocation if already claimed by another thread.
    
    完成方法tryFire（int模式）使用其保留的参数调用关联的x方法，并在成功时清除. 模式参数允许tryFire被调用两次（SYNC，然后是ASYNC）; 第一个在安排执行时筛选和捕获异常，第二个从任务中调用. 
    （一些类未异步使用，因此形式略有不同.） 如果另一个线程已经声明了Claim回调，它将禁止函数调用.

    * * CompletableFuture method xStage(...) is called from a public
    *   stage method of CompletableFuture x. It screens user
    *   arguments and invokes and/or creates the stage object.  If
    *   not async and x is already complete, the action is run
    *   immediately.  Otherwise a Completion c is created, pushed to
    *   x's stack (unless done), and started or triggered via
    *   c.tryFire.  This also covers races possible if x completes
    *   while pushing.  Classes with two inputs (for example BiApply)
    *   deal with races across both while pushing actions.  The
    *   second completion is a CoCompletion pointing to the first,
    *   shared so that at most one performs the action.  The
    *   multiple-arity methods allOf and anyOf do this pairwise to
    *   form trees of completions.
    
    从CompletableFuture x的公共阶段方法调用CompletableFuture方法xStage（...）. 它筛选用户参数并调用和/或创建阶段对象. 如果不是异步的并且x已经完成，则操作将立即运行. 
    否则，将创建完成c，将其压入x的堆栈（除非完成），然后通过c.tryFire启动或触发. 如果x在推入时完成，这也涵盖了可能的比赛. 具有两个输入的类（例如BiApply）在推动动作时处理两者之间的竞争.
    第二个完成是指向第一个完成的CoCompletion，该共享将使最多一个执行该动作. 多个对数方法allOf和anyOf配对执行以形成补全树.

    * Note that the generic type parameters of methods vary according
    * to whether "this" is a source, dependent, or completion.
    
    请注意，方法的通用类型参数根据"this"是源，从属还是完成而有所不同.

    * Method postComplete is called upon completion unless the target
    * is guaranteed not to be observable (i.e., not yet returned or
    * linked). Multiple threads can call postComplete, which
    * atomically pops each dependent action, and tries to trigger it
    * via method tryFire, in NESTED mode.  Triggering can propagate
    * recursively, so NESTED mode returns its completed dependent (if
    * one exists) for further processing by its caller (see method
    * postFire).
    
    除非保证目标不可观察（即尚未返回或链接），否则将在完成时调用postComplete方法. 多个线程可以调用postComplete，该原子自动弹出每个依赖的动作，并尝试在NESTED模式下通过tryFire方法触发它.
    触发可以递归传播，因此NESTED模式返回其完成的依赖项（如果存在），以供其调用者进行进一步处理（请参见方法postFire）.

    * Blocking methods get() and join() rely on Signaller Completions
    * that wake up waiting threads.  The mechanics are similar to
    * Treiber stack wait-nodes used in FutureTask, Phaser, and
    * SynchronousQueue. See their internal documentation for
    * algorithmic details.
    
    阻塞方法get和join依赖于唤醒等待线程的Signaller Completions. 其机制类似于FutureTask，Phaser和SynchronousQueue中使用的Treiber堆栈等待节点. 请参阅其内部文档以获取算法详细信息.

    * Without precautions, CompletableFutures would be prone to
    * garbage accumulation as chains of Completions build up, each
    * pointing back to its sources. So we null out fields as soon as
    * possible (see especially method Completion.detach). The
    * screening checks needed anyway harmlessly ignore null arguments
    * that may have been obtained during races with threads nulling
    * out fields.  We also try to unlink fired Completions from
    * stacks that might never be popped (see method postFire).
    * Completion fields need not be declared as final or volatile
    * because they are only visible to other threads upon safe
    * publication.

    如果没有预防措施，随着完成链的建立，CompletableFutures将易于产生垃圾堆积，每个指向其来源. 因此，我们会尽快使字段无效（尤其是方法Completion.detach）.
    无论如何，需要进行的筛选检查无害地忽略了可能在比赛期间使用线程使字段无效的空参数. 我们还尝试从可能永远不会弹出的堆栈中取消已触发的完成的链接（请参见方法postFire）.
    完成字段不必声明为final或volatile，因为它们只有在安全发布后才对其他线程可见.

    */


```