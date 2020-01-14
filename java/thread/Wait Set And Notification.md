### 翻译Wait、notify、notifyAll关键词，引入Java Specification（Java说明书），可自行Google下载针对版本

```java

    /**

     每个对象除了具有关联的监视器（对象锁）外，还具有关联的等待集. 等待集是一组线程（等待中的线程会被添加到该等待集中）.

     首次创建对象时，其等待集为空. 向等待集中添加线程或从等待集中删除线程的基本操作是原子的（原子性）. 等待集仅通过Object#wait、Object#notify、Object#notify方法进行操作.

     等待集操作也可能受线程的中断状态以及线程类处理中断的方法的影响. 此外，线程类用于sleep和join其他线程的方法具有从等待和通知操作的属性派生而来的属性.

     等待（Wait）

     调用wait方法或定时wait方法时发生等待动作.

        参数为0的wait(long millisecs)方法或者两个参数都为0的wait(long millisecs, int nanosecs)是等价于wait方法的调用.

     如果线程返回而没有引入异常，则线程通常从等待中返回. 
     
     设线程t为在对象m上执行wait方法的线程, 设n为t在m上的未与解锁动作匹配的锁定动作的数量（应该是对锁的次数进行了计数）. 发生以下操作之一:

     - 如果n = 0（线程t尚未拥有目标m的锁），则抛出IllegalMonitorStateException异常（说明wait必须在同步控制方法或同步代码块中才能使用）

     - 如果这是定时wait方法，并且nanosecs参数不在0-999999范围内（纳秒与毫秒的关系应该是6个零），或者millisecs参数为负，则抛出IllegalArgumentException

     - 如果线程t被中断，则抛出InterruptedException异常，并将t的中断状态设置为false

     - 否则，将发生以下顺序：
      
       1 线程t被添加到对象m的等待集中，并在m上执行n次解锁操作（意思是这个时候它已经释放锁了！！！！而不管是多个锁的数量）

       2 线程t在从m的等待集中删除之前不会执行任何其他指令. 由于以下任何一种操作，该线程可能会从等待集中删除，并且稍后会恢复执行：

         - 在m上执行notify操作，从对象m的等待集中删除t

         - 在m上执行notifyAll操作

         - 在t上执行interrupt操作

         - 如果这是定时wait方法，则从此等待动作开始至少经过毫秒 + 纳秒后发生的内部动作即为从对象m的等待集中删除t

         - 由内部操作实现. 允许但不鼓励执行"虚假唤醒"的实现，即从等待集中删除线程，从而无需明确的指示即可进行恢复

           - 请注意，此规定需要Java编码实践，即仅在某些线程正在等待的逻辑条件成立时才终止的循环内使用wait
    

        每个线程必须确定可能导致其从等待集中删除的事件的顺序. 该顺序不必与其他顺序一致，但是线程必须表现得好像这些事件以该顺序发生.

        例如，如果线程t在等待m的集合中，然后同时发生t的中断和m的通知，则这些事件必须有顺序. 如果认为该中断首先发生，则t将通过抛出InterruptedException最终从等待中返回，并且等待集中m的其他线程（如果在通知时存在）将接收该通知. 如果认为该通知首先发生，则t将最终从等待状态正常返回，而中断仍在处理中.

        3 线程t对m执行n次锁定操作（获取CPU时间片后重新上多个锁）

        4 如果由于中断而在步骤2中将线程t从m的等待集中移除，则t的中断状态设置为false，并且wait方法抛出InterruptedException

        ！！！！即使线程从等待集中删除，也必须要等待获取到锁后才能继续往下走
     

     通知（Notification）

     调用方法notify和notifyAll时发生通知动作. 设线程t为在对象m上执行这两种方法之一的线程，设n为t在m上的未与解锁动作匹配的锁定动作的数量. 发生以下操作之一：

     - 如果n为零，则抛出IllegalMonitorStateException异常（说明notify、notifyAll必须在同步控制方法或同步代码块中才能使用）, 这是线程t尚未拥有目标m的锁的情况.

     - 如果n大于零且这是一个notify操作，则如果m的等待集不为空，则选择m的当前等待集成员的线程u并将其从等待集中删除.

     无法保证在等待集中选择了哪个线程. 从等待集中删除可以使u在等待操作中恢复。但是请注意，直到t完全解锁m后一段时间（锁的次数到达0），u的恢复锁定操作才能成功.

     - 如果n大于零，并且这是一个notifyAll动作，则所有线程都从m的等待集中删除，从而恢复.

     但是请注意，在恢复等待期间，所有线程中的其中一个才会获取锁.




     中断（Interruptions）

     调用Thread#interrupt以及为依次调用它而定义的方法（例如ThreadGroup.interrupt）时，将发生中断操作.

     设t为调用u.interrupt的线程，对于某些线程u，其中t和u可能相同. 此操作（Thread#interrupt）会使u的中断状态设置为true（在t线程上查看u线程的中断状态isInterrupted为true）.

     另外，如果存在某个对象m，其等待集中包含u，则u从m的等待集中删除. 这使u可以在等待操作中恢复，在这种情况下，此等待将在m重新上锁之后引发InterruptedException异常（同时中断状态被设置为false）.

     Thread#isInterrupted的调用可以确定线程的中断状态. 线程可以调用静态方法Thread#interrupted来观察和清除其自身的中断状态




     等待，通知和中断的相互作用（Interactions of Waits, Notification, and Interruption）

     以上规范使我们能够确定与wait，notify和interrupt的交互作用有关的几个属性.

     如果线程在等待时既被通知又被中断，则可能是：

     - 从等待状态正常返回，同时仍具有挂起的中断（换句话说，对Thread.interrupted的调用将返回true）

     - 通过抛出InterruptedException异常来从等待中返回

     线程可能不会重置其中断状态，并从等待调用中正常返回

     同样，通知也不会由于中断而丢失. 假设一组线程s在对象m的等待集中，而另一个线程对m执行通知. 然后：

     - s中至少有一个线程必须从等待中正常返回

     - s中的所有线程必须通过抛出InterruptedException退出等待

     请注意，如果一个线程同时被中断和通知（notify），并且该线程通过抛出InterruptedException从等待返回，则必须通知等待集中的其他线程.

    /

```