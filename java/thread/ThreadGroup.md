### 翻译ThreadGroup注释

```java

   /**
    * A thread group represents a set of threads. In addition, a thread
    * group can also include other thread groups. The thread groups form
    * a tree in which every thread group except the initial thread group
    * has a parent.
    
    ThreadGroup代表一组线程. 除此之外， 线程组能包含其他线程组. 线程组（之间）形成一棵树，其中除了初始线程组以外的每个线程组都有一个父亲（父线程组）.

    * A thread is allowed to access information about its own thread
    * group, but not to access information about its thread group's
    * parent thread group or any other thread groups.
    
    线程允许访问有关它所属线程组的信息，但不允许访问有关它所属线程组的父线程组或其他线程组的信息.

    */

```