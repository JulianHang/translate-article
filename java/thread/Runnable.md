### 翻译Runable类注释

```java

   /**
    * The <code>Runnable</code> interface should be implemented by any
    * class whose instances are intended to be executed by a thread. The
    * class must define a method of no arguments called <code>run</code>.
    * <p>

    Runnable接口应该由其实例打算由线程执行的任何类实现. 该类必须定义没有参数的run方法.

    * This interface is designed to provide a common protocol for objects that
    * wish to execute code while they are active. For example,
    * <code>Runnable</code> is implemented by class <code>Thread</code>.
    * Being active simply means that a thread has been started and has not
    * yet been stopped.
    
    此接口旨在为希望在活动状态下执行代码的对象提供通用协议. 例如，Thread实现了Runnable.
    处于活动状态仅表示线程已启动但尚未停止.

    * In addition, <code>Runnable</code> provides the means for a class to be
    * active while not subclassing <code>Thread</code>. A class that implements
    * <code>Runnable</code> can run without subclassing <code>Thread</code>
    * by instantiating a <code>Thread</code> instance and passing itself in
    * as the target.  In most cases, the <code>Runnable</code> interface should
    * be used if you are only planning to override the <code>run()</code>
    * method and no other <code>Thread</code> methods.
    * This is important because classes should not be subclassed
    * unless the programmer intends on modifying or enhancing the fundamental
    * behavior of the class.

    此外，Runnable提供了一种在不继承Thread的情况下使类处于活动状态的方法. 
    实现Runnable的类可以通过实例化Thread实例并将其自身作为目标传递而无需继承Thread的子类来运行.  -> Thread thread = new Trehad(new Test())
    在大多数情况下，如果您仅打算覆盖run方法而没有其他Thread方法，则应使用Runnable接口.
    这很重要，因为除非程序员打算修改或增强类的基本行为，否则不应将类归为子类.

```