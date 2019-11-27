### 翻译Buffer注释

```java


   /**
    * A container for data of a specific primitive type.
    
    特定基本类型数据的容器.

    * <p> A buffer is a linear, finite sequence of elements of a specific
    * primitive type.  Aside from its content, the essential properties of a
    * buffer are its capacity, limit, and position: </p>

    缓冲区是特定基本类型的元素的线性有限序列. 除了其内容之外，缓冲区的基本属性还包括其capacity、limit、position.

    * <blockquote>
    *
    *   <p> A buffer's <i>capacity</i> is the number of elements it contains.  The
    *   capacity of a buffer is never negative and never changes.  </p>

        缓冲区的容量是它包含元素的数量. 缓冲区的容量不会为负不会改变.

    *   <p> A buffer's <i>limit</i> is the index of the first element that should
    *   not be read or written.  A buffer's limit is never negative and is never
    *   greater than its capacity.  </p>
    
        缓冲区的界限是不应该读取或写入的第一个元素的索引. 缓冲区的界限不会为负不会超过它的容量. limit <= capacity

    *   <p> A buffer's <i>position</i> is the index of the next element to be
    *   read or written.  A buffer's position is never negative and is never
    *   greater than its limit.  </p>

        缓冲区的位置是下一个读取或写入的元素的索引. 缓冲区的位置不会为负不会超过它的界限. position <= limit

    * </blockquote>
    *
    * <p> There is one subclass of this class for each non-boolean primitive type.
    
        每种非布尔基本类型都有一个此类的子类

    * <h2> Transferring data </h2>
    
        传递数据

    * <p> Each subclass of this class defines two categories of <i>get</i> and
    * <i>put</i> operations: </p>
    
        该类的每个子类定义了get、put操作

    * <blockquote>
    *
    *   <p> <i>Relative</i> operations read or write one or more elements starting
    *   at the current position and then increment the position by the number of
    *   elements transferred.  If the requested transfer exceeds the limit then a
    *   relative <i>get</i> operation throws a {@link BufferUnderflowException}
    *   and a relative <i>put</i> operation throws a {@link
    *   BufferOverflowException}; in either case, no data is transferred.  </p>
    
        相对操作从当前位置开始读取或写入一个或多个元素，然后将位置(position)增加传递的元素数量.
        如果请求的传递超出限制，则相对get操作抛出异常和相对put操作抛出异常;无论哪种情况，都不会传递数据.

    *   <p> <i>Absolute</i> operations take an explicit element index and do not
    *   affect the position.  Absolute <i>get</i> and <i>put</i> operations throw
    *   an {@link IndexOutOfBoundsException} if the index argument exceeds the
    *   limit.  </p>

        绝对操作会使用显式元素索引，并且不会影响位置(position). 如果索引参数超过限制则绝对get和put操作抛出异常. 

    * </blockquote>
    *
    * <p> Data may also, of course, be transferred in to or out of a buffer by the
    * I/O operations of an appropriate channel, which are always relative to the
    * current position.
    
    当然，数据也可以通过始终相对于当前位置的适当通道的I/O操作传入或传出缓冲区.
    
    * <h2> Marking and resetting </h2>
    
    标记与重置

    * <p> A buffer's <i>mark</i> is the index to which its position will be reset
    * when the {@link #reset reset} method is invoked.  The mark is not always
    * defined, but when it is defined it is never negative and is never greater
    * than the position.  If the mark is defined then it is discarded when the
    * position or the limit is adjusted to a value smaller than the mark.  If the
    * mark is not defined then invoking the {@link #reset reset} method causes an
    * {@link InvalidMarkException} to be thrown.
    
    缓冲区的标记(mark)是调用reset方法时将其位置重置到的索引. mark并非总是定义的，但是定义时，它不会为负不会大于位置. mark <= position
    如果定义了mark，则在将位置或限制调整为小于mark的值时将其丢弃. 如果mark并未定义而调用reset方法将抛出异常.
    
    * <h2> Invariants </h2>
    
    不变式

    * <p> The following invariant holds for the mark, position, limit, and
    * capacity values:
    
    对于标记、位置、限制和容量值，以下不变式成立.
    
    * <blockquote>
    *     <tt>0</tt> <tt>&lt;=</tt>
    *     <i>mark</i> <tt>&lt;=</tt>
    *     <i>position</i> <tt>&lt;=</tt>
    *     <i>limit</i> <tt>&lt;=</tt>
    *     <i>capacity</i>
    * </blockquote>

    0 <= mark <= position <= limit <= capacity
    
    * <p> A newly-created buffer always has a position of zero and a mark that is
    * undefined.  The initial limit may be zero, or it may be some other value
    * that depends upon the type of the buffer and the manner in which it is
    * constructed.  Each element of a newly-allocated buffer is initialized
    * to zero.
    
    新创建的缓冲区的位置始终为0，并且标记未定义. 初始限制可以为0，也可以是其他一些值，具体取决于缓冲区的类型和构造方式.
    新分配的缓冲区的每个元素都初始化为0.
    
    * <h2> Clearing, flipping, and rewinding </h2>
    
    clearing、fliping、rewinding

    * <p> In addition to methods for accessing the position, limit, and capacity
    * values and for marking and resetting, this class also defines the following
    * operations upon buffers:
    
    除了访问位置、限制和容量值以及标记和重置的方法之外，此类还定义了以下对缓冲区的操作.

    * <ul>

    *   <li><p> {@link #clear} makes a buffer ready for a new sequence of
    *   channel-read or relative <i>put</i> operations: It sets the limit to the
    *   capacity and the position to zero.  </p></li>
    
        clear使缓冲区准备好进行新的通道读取或相对put操作的序列：设置限制到容量大小及位置为0.

    *   <li><p> {@link #flip} makes a buffer ready for a new sequence of
    *   channel-write or relative <i>get</i> operations: It sets the limit to the
    *   current position and then sets the position to zero.  </p></li>
    
        flip使缓冲区准备好进行新的通道读取或相对get操作的序列：设置限制到当前位置大小及位置为0.
    
    *   <li><p> {@link #rewind} makes a buffer ready for re-reading the data that
    *   it already contains: It leaves the limit unchanged and sets the position
    *   to zero.  </p></li>

        使缓冲区准备好重新读取它已经包含的数据：它将限制保持不变并将位置设置为0.
    
    * </ul>
    *
    *
    * <h2> Read-only buffers </h2>
    
        只读缓冲区

    * <p> Every buffer is readable, but not every buffer is writable.  The
    * mutation methods of each buffer class are specified as <i>optional
    * operations</i> that will throw a {@link ReadOnlyBufferException} when
    * invoked upon a read-only buffer.  A read-only buffer does not allow its
    * content to be changed, but its mark, position, and limit values are mutable.
    * Whether or not a buffer is read-only may be determined by invoking its
    * {@link #isReadOnly isReadOnly} method.
    
    每个缓冲区都是可读的，但不是每个缓冲区是可写的. 每个缓冲区类的可变方法都被指定为可选操作，当在只读缓冲区调用这些方法时，它们将抛出异常.
    只读缓冲区不允许它的内容被修改，但是它的标记、位置和限制值是可变的. 缓冲区是否为只读可以通过调用其isReadOnly方法来确定.

    * <h2> Thread safety </h2>
    
        线程安全

    * <p> Buffers are not safe for use by multiple concurrent threads.  If a
    * buffer is to be used by more than one thread then access to the buffer
    * should be controlled by appropriate synchronization.
    
    缓冲区不能安全用于多个并发线程. 如果一个缓冲区将由多个线程使用，则应通过适当的同步来控制对该缓冲区的访问.

    * <h2> Invocation chaining </h2>
    
    调用链

    * <p> Methods in this class that do not otherwise have a value to return are
    * specified to return the buffer upon which they are invoked.  This allows
    * method invocations to be chained; for example, the sequence of statements
    
    此类中没有其他要返回值的方法被指定为返回在其上调用它们的缓冲区. 这允许方法调用被链接

    * <blockquote><pre>
    * b.flip();
    * b.position(23);
    * b.limit(42);</pre></blockquote>
    *
    * can be replaced by the single, more compact statement
    *
    * <blockquote><pre>
    * b.flip().position(23).limit(42);</pre></blockquote>

    ...

```