### 翻译Collections注释

```java

   /**
    * This class consists exclusively of static methods that operate on or return
    * collections.  It contains polymorphic algorithms that operate on
    * collections, "wrappers", which return a new collection backed by a
    * specified collection, and a few other odds and ends.
    
    此类仅包含对集合进行操作或返回集合的静态方法. 它包含对集合进行操作的多态算法，“包装器”，它们返回由指定集合支持的新集合，及其他的差别.

    * <p>The methods of this class all throw a <tt>NullPointerException</tt>
    * if the collections or class objects provided to them are null.
    
    如果集合或类是null，该类的方法会抛出空指针.

    * <p>The documentation for the polymorphic algorithms contained in this class
    * generally includes a brief description of the <i>implementation</i>.  Such
    * descriptions should be regarded as <i>implementation notes</i>, rather than
    * parts of the <i>specification</i>.  Implementors should feel free to
    * substitute other algorithms, so long as the specification itself is adhered
    * to.  (For example, the algorithm used by <tt>sort</tt> does not have to be
    * a mergesort, but it does have to be <i>stable</i>.)
    
    此类中包含的多态算法文档通常包括对实现的简短描述. 此类描述应被视为实现说明，而不是规范的一部分.
    只要遵守规范本身，实现者就可以随意替换其他算法.

    * <p>The "destructive" algorithms contained in this class, that is, the
    * algorithms that modify the collection on which they operate, are specified
    * to throw <tt>UnsupportedOperationException</tt> if the collection does not
    * support the appropriate mutation primitive(s), such as the <tt>set</tt>
    * method.  These algorithms may, but are not required to, throw this
    * exception if an invocation would have no effect on the collection.  For
    * example, invoking the <tt>sort</tt> method on an unmodifiable list that is
    * already sorted may or may not throw <tt>UnsupportedOperationException</tt>.
    
    略

    * <p>This class is a member of the
    * <a href="{@docRoot}/../technotes/guides/collections/index.html">
    * Java Collections Framework</a>.

    该类是Java Collections Framework的一员.


```