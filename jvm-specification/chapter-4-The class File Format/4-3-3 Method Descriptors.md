### 翻译Java虚拟机规范第四章第三节-第三节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`简单提及方法描述符`


```java

    /**

    4.3.3 Method Descriptors
    
    方法描述符

    比如有一个方法是如下格式：
        Object m(int i, double d, Thread t) {...}

    最终将编译成：
        (IDLjava/lang/Thread;)Ljava/lang/Object;
    
    其中：
       （）表示方法的入参，IDL正好对应到int double Thread
        L表示返回类型，正好对应Object


    A method descriptor is the same whether the method it describes is a class method
    or an instance method. Although an instance method is passed this, a reference
    to the object on which the method is being invoked, in addition to its intended
    arguments, that fact is not reflected in the method descriptor. The reference to this
    is passed implicitly by the Java Virtual Machine instructions which invoke instance
    methods (§2.6.1, §4.11).

    无论方法描述的是类方法还是实例方法，方法描述符都是相同的. 
    尽管为对象方法传递了this，调用引用对象的方法除了预期的参数外，该事实并未反映在方法描述符中.
    Java虚拟机指令调用对象方法来隐式传递this.


    */



```