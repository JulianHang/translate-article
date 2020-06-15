### 翻译Java虚拟机规范第二章第七节(JSR-337/Java8)

主要内容：`介绍在内存中对象的表现形式`

```java

    /**

    2.7 Representation of Objects 
    
    对象的表现形式

    The Java Virtual Machine does not mandate any particular internal structure for
    objects.

    Java虚拟机不要求对象具有任何特定的内部结构.

    In some of Oracle’s implementations of the Java Virtual Machine, a reference to a class
    instance is a pointer to a handle that is itself a pair of pointers: one to a table containing
    the methods of the object and a pointer to the Class object that represents the type of the
    object, and the other to the memory allocated from the heap for the object data.

    在一些Oracle的Java虚拟机的实现中，对类实例的引用是指向句柄的指针，句柄本身就是一对指针.
    一个指向包含对象的方法的表，另一个指向代表该对象类型的Class对象的指针，另一个指向从堆为对象数据分配的内存.

    */


```