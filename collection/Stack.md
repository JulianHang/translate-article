### 翻译Vector注释

```java

   /**
    * The <code>Stack</code> class represents a last-in-first-out
    * (LIFO) stack of objects. It extends class <tt>Vector</tt> with five
    * operations that allow a vector to be treated as a stack. The usual
    * <tt>push</tt> and <tt>pop</tt> operations are provided, as well as a
    * method to <tt>peek</tt> at the top item on the stack, a method to test
    * for whether the stack is <tt>empty</tt>, and a method to <tt>search</tt>
    * the stack for an item and discover how far it is from the top.
    * <p>
    * When a stack is first created, it contains no items.

    Stack类表示后进先出（LIFO）的对象栈.它在Vector基础上扩展了5个方法，这些方法使Vector看作成栈.
    提供了push、pop、peek查看栈的顶层元素，empty测试栈是否为空，search在栈中查找元素离顶层元素多远（索引的差值）
    stack首次被创建时，它不包含任何元素。

    * <p>A more complete and consistent set of LIFO stack operations is
    * provided by the {@link Deque} interface and its implementations, which
    * should be used in preference to this class.  For example:
    * <pre>   {@code
    *   Deque<Integer> stack = new ArrayDeque<Integer>();}</pre>

    Deque接口和它的实现类提供了一组更完善、更一致的后进先出栈操作，应优先使用Deque.
    例如：Deque<Integer> stack = new ArrayDeque<Integer>()

```