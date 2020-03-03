### 翻译CompletionStage注释

```java

   /**
    * A stage of a possibly asynchronous computation, that performs an
    * action or computes a value when another CompletionStage completes.
    * A stage completes upon termination of its computation, but this may
    * in turn trigger other dependent stages.  The functionality defined
    * in this interface takes only a few basic forms, which expand out to
    * a larger set of methods to capture a range of usage styles: <ul>
    
    异步计算的阶段，当另一个CompletionStage完成时，执行操作或计算值. 一个阶段在其计算终止时完成，但是这又可能触发其他相关阶段.
    该接口定义的功能仅采用几种基本形式，这些形式扩展为更大的方法集，以捕获各种使用样式：

    * <li>The computation performed by a stage may be expressed as a
    * Function, Consumer, or Runnable (using methods with names including
    * <em>apply</em>, <em>accept</em>, or <em>run</em>, respectively)
    * depending on whether it requires arguments and/or produces results.
    * For example, {@code stage.thenApply(x -> square(x)).thenAccept(x ->
    * System.out.print(x)).thenRun(() -> System.out.println())}. An
    * additional form (<em>compose</em>) applies functions of stages
    * themselves, rather than their results. </li>

    阶段执行的计算可以表示为Function、Consumer、Runnable，取决于它是否要求入参或生产结果值.
    例如，stage.thenApply().thenAccept().thenRun().
    另一种形式compose应用阶段本身的功能，而不是其结果.

    * <li> One stage's execution may be triggered by completion of a
    * single stage, or both of two stages, or either of two stages.
    * Dependencies on a single stage are arranged using methods with
    * prefix <em>then</em>. Those triggered by completion of
    * <em>both</em> of two stages may <em>combine</em> their results or
    * effects, using correspondingly named methods. Those triggered by
    * <em>either</em> of two stages make no guarantees about which of the
    * results or effects are used for the dependent stage's
    * computation.</li>
    
    一个阶段的执行可以由一个阶段的完成或两个阶段的全部或两个阶段中的一个所触发. 使用带有前缀then的方法来安排对单个阶段的依赖关系. 
    通过两个阶段都完成而触发的结果可以使用相应命名的方法组合其结果或效果. 由两个阶段之一触发的那些并不能保证哪个结果或效果用于从属阶段的计算.

    * <li> Dependencies among stages control the triggering of
    * computations, but do not otherwise guarantee any particular
    * ordering. Additionally, execution of a new stage's computations may
    * be arranged in any of three ways: default execution, default
    * asynchronous execution (using methods with suffix <em>async</em>
    * that employ the stage's default asynchronous execution facility),
    * or custom (via a supplied {@link Executor}).  The execution
    * properties of default and async modes are specified by
    * CompletionStage implementations, not this interface. Methods with
    * explicit Executor arguments may have arbitrary execution
    * properties, and might not even support concurrent execution, but
    * are arranged for processing in a way that accommodates asynchrony.
    
    阶段之间的依赖关系控制着计算的触发，但是不能保证任何特定的顺序. 另外，可以以三种方式中的任何一种来安排执行新阶段的计算：
    默认执行、默认异步执行（使用带有后缀async的方法，该方法采用阶段的默认异步执行工具）、自定义（通过应用Executor）.
    默认和异步模式的执行属性由CompletionStage实现指定，而不是由此接口指定.
    具有显式Executor自变量的方法可能具有任意执行属性，甚至可能不支持并发执行，但被安排为以适应异步的方式进行处理.

    * <li> Two method forms support processing whether the triggering
    * stage completed normally or exceptionally: Method {@link
    * #whenComplete whenComplete} allows injection of an action
    * regardless of outcome, otherwise preserving the outcome in its
    * completion. Method {@link #handle handle} additionally allows the
    * stage to compute a replacement result that may enable further
    * processing by other dependent stages.  In all other cases, if a
    * stage's computation terminates abruptly with an (unchecked)
    * exception or error, then all dependent stages requiring its
    * completion complete exceptionally as well, with a {@link
    * CompletionException} holding the exception as its cause.  If a
    * stage is dependent on <em>both</em> of two stages, and both
    * complete exceptionally, then the CompletionException may correspond
    * to either one of these exceptions.  If a stage is dependent on
    * <em>either</em> of two others, and only one of them completes
    * exceptionally, no guarantees are made about whether the dependent
    * stage completes normally or exceptionally. In the case of method
    * {@code whenComplete}, when the supplied action itself encounters an
    * exception, then the stage exceptionally completes with this
    * exception if not already completed exceptionally.</li>
    
    两种方法形式都支持处理，无论触发阶段是正常还是异常完成：
    方法whenComplete允许不考虑结果而注入动作，否则在完成时保留结果. 方法handle允许该阶段计算替换结果，该结果可能使其他相关阶段能够进行进一步处理.
    在所有其他情况下，如果阶段的计算由于（未经检查的）异常或错误而突然终止，则所有需要完成该阶段的从属阶段也会异常完成，CompletionException将异常作为其原因.
    如果一个阶段依赖于两个阶段，两个阶段又都异常完成，则CompletionException可能对应于这些异常之一.
    如果某个阶段依赖于其他两个阶段中的任何一个，并且其中只有一个异常完成，则不能保证从属阶段是正常或异常完成.
    在方法whenComplete的情况下，如果提供的操作本身遇到异常，则该阶段将以该异常异常完成.

    * </ul>
    *
    * <p>All methods adhere to the above triggering, execution, and
    * exceptional completion specifications (which are not repeated in
    * individual method specifications). Additionally, while arguments
    * used to pass a completion result (that is, for parameters of type
    * {@code T}) for methods accepting them may be null, passing a null
    * value for any other parameter will result in a {@link
    * NullPointerException} being thrown.
    
    所有方法都遵循上述触发，执行和异常完成规范（在各个方法规范中不再重复）
    此外，用于传递接受结果的方法的参数可能为null，为任何其他参数传递null值将导致抛出NullPointerException.

    * <p>This interface does not define methods for initially creating,
    * forcibly completing normally or exceptionally, probing completion
    * status or results, or awaiting completion of a stage.
    * Implementations of CompletionStage may provide means of achieving
    * such effects, as appropriate.  Method {@link #toCompletableFuture}
    * enables interoperability among different implementations of this
    * interface by providing a common conversion type.
    
    此接口未定义用于初始创建，强制正常或异常完成，探测完成状态或结果或等待阶段完成的方法. 
    CompletionStage的实现可以酌情提供实现这种效果的方法.
    方法toCompletableFuture通过提供常见的转换类型来实现此接口的不同实现之间的互操作性

    /

```