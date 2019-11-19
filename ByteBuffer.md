### 翻译ByteBuffer注释

```java

   /**
    * A byte buffer.
    
    一个字节缓冲区

    * <p> This class defines six categories of operations upon
    * byte buffers:
    
    此类定义了在字节缓冲区上六种操作类型.

    * <ul>
    *
    *   <li><p> Absolute and relative {@link #get() <i>get</i>} and
    *   {@link #put(byte) <i>put</i>} methods that read and write
    *   single bytes; </p></li>
    
        put、get方法用来写入和获取单个字节.

    *   <li><p> Relative {@link #get(byte[]) <i>bulk get</i>}
    *   methods that transfer contiguous sequences of bytes from this buffer
    *   into an array; </p></li>

        get(byte[])：将连续的字节序列从缓冲区传递到数组中.

    *   <li><p> Relative {@link #put(byte[]) <i>bulk put</i>}
    *   methods that transfer contiguous sequences of bytes from a
    *   byte array or some other byte
    *   buffer into this buffer; </p></li>

        put(byte[])：将连续的字节序列从字节数组或其他字节缓冲区传递到该缓冲区上.

    *   <li><p> Absolute and relative {@link #getChar() <i>get</i>}
    *   and {@link #putChar(char) <i>put</i>} methods that read and
    *   write values of other primitive types, translating them to and from
    *   sequences of bytes in a particular byte order; </p></li>
    
        putChar(putDouble)、getChar(getDouble)方法用来写入和获取其他基本类型的值，它们以特定字节顺序在字节序列之间来回转换.

    *   <li><p> Methods for creating <i><a href="#views">view buffers</a></i>,
    *   which allow a byte buffer to be viewed as a buffer containing values of
    *   some other primitive type; and </p></li>

        创建视图缓冲区的方法，允许将字节缓冲区视为包含其他基本类型值的缓冲区.   ByteBuffer bb = ByteBuffer.allocate(1024); CharBuffer cb = bb.asCharBuffer(); 实际上操作的还是该字节缓冲区

    *   <li><p> Methods for {@link #compact compacting}, {@link
    *   #duplicate duplicating}, and {@link #slice slicing}
    *   a byte buffer.  </p></li>
    
        提供compact、duplicate、slice方法.

    * </ul>
    *
    * <p> Byte buffers can be created either by {@link #allocate
    * <i>allocation</i>}, which allocates space for the buffer's
    * content, or by {@link #wrap(byte[]) <i>wrapping</i>} an
    * existing byte array  into a buffer.

    通过计算缓冲区中数据的空间来创建-> ByteBuffer.allocate
    通过包裹一个存在的字节数组到缓冲区来创建 -> ByteBuffer.wrap(byte[])

    * <a name="direct"></a>
    * <h2> Direct <i>vs.</i> non-direct buffers </h2>
    
    直接缓冲区 VS 间接缓冲区

    * <p> A byte buffer is either <i>direct</i> or <i>non-direct</i>.  Given a
    * direct byte buffer, the Java virtual machine will make a best effort to
    * perform native I/O operations directly upon it.  That is, it will attempt to
    * avoid copying the buffer's content to (or from) an intermediate buffer
    * before (or after) each invocation of one of the underlying operating
    * system's native I/O operations.
    
    直接缓冲区或间接缓冲区. 对于直接缓冲区，JVM将尽最大努力直接执行原生IO操作（应该是直接执行操作系统的IO）
    也就是说，它将尝试避免在每次调用底层操作系统的IO操作之前（之后）复制缓冲区的内容到中间缓冲区（或从中间缓冲区复制）

    * <p> A direct byte buffer may be created by invoking the {@link
    * #allocateDirect(int) allocateDirect} factory method of this class.  The
    * buffers returned by this method typically have somewhat higher allocation
    * and deallocation costs than non-direct buffers.  The contents of direct
    * buffers may reside outside of the normal garbage-collected heap, and so
    * their impact upon the memory footprint of an application might not be
    * obvious.  It is therefore recommended that direct buffers be allocated
    * primarily for large, long-lived buffers that are subject to the underlying
    * system's native I/O operations.  In general it is best to allocate direct
    * buffers only when they yield a measureable gain in program performance.
    
    ByteBuffer.allocateDirect创建直接字节缓冲区. 直接缓冲区通常比间接缓冲区具有更高的分配和释放成本.
    直接缓冲区的内容可能驻留在堆外内存，因此它们对应用程序内存占用的影响可能并不明显.
    因此，建议对于大型缓冲区来说直接分配直接缓冲区，这些缓冲区需要底层系统的IO操作.
    通常，最好仅在直接缓冲区产生可衡量的程序性能提升时才分配它们.

    * <p> A direct byte buffer may also be created by {@link
    * java.nio.channels.FileChannel#map mapping} a region of a file
    * directly into memory.  An implementation of the Java platform may optionally
    * support the creation of direct byte buffers from native code via JNI.  If an
    * instance of one of these kinds of buffers refers to an inaccessible region
    * of memory then an attempt to access that region will not change the buffer's
    * content and will cause an unspecified exception to be thrown either at the
    * time of the access or at some later time.
    
    FileChannel#map通过将文件区域直接存入内存来创建直接缓冲区. Java平台的实现可以选择支持通过JNI从原生代码创建直接字节缓冲区.
    如果这些缓冲区类型的其中一个实例引用了内存的不可访问区域，则访问该区域的尝试将不会改变缓冲区的内容，并且将导致在访问时或稍后出现未指定的异常.

    * <p> Whether a byte buffer is direct or non-direct may be determined by
    * invoking its {@link #isDirect isDirect} method.  This method is provided so
    * that explicit buffer management can be done in performance-critical code.
    
    通过调用isDirect方法来决定是直接缓冲区还是间接缓冲区. 提供该方法是为了可以在关键性能代码中进行明确性的缓冲区管理.

    * <a name="bin"></a>
    * <h2> Access to binary data </h2>
    
    访问二进制数据

    * <p> This class defines methods for reading and writing values of all other
    * primitive types, except <tt>boolean</tt>.  Primitive values are translated
    * to (or from) sequences of bytes according to the buffer's current byte
    * order, which may be retrieved and modified via the {@link #order order}
    * methods.  Specific byte orders are represented by instances of the {@link
    * ByteOrder} class.  The initial order of a byte buffer is always {@link
    * ByteOrder#BIG_ENDIAN BIG_ENDIAN}.
    
    该类定义了用于读写所有其他基本类型值的方法, 除了boolean. 按照缓冲区的当前字节顺序将基本类型值转换成字节序列, 通过order方法可以获取和修改字节顺序
    特定的字节顺序由ByteOrder类的实例表示. 字节缓冲区的初始顺序总是ByteOrder#BIG_ENDIAN.

    * <p> For access to heterogeneous binary data, that is, sequences of values of
    * different types, this class defines a family of absolute and relative
    * <i>get</i> and <i>put</i> methods for each type.  For 32-bit floating-point
    * values, for example, this class defines:
    
    为了访问异构二进制数据，即不同类型的值的序列，该类定义了每种类型的get、put. 例如，对于32位浮点值，此类定义：

    * <blockquote><pre>
    * float  {@link #getFloat()}
    * float  {@link #getFloat(int) getFloat(int index)}
    *  void  {@link #putFloat(float) putFloat(float f)}
    *  void  {@link #putFloat(int,float) putFloat(int index, float f)}</pre></blockquote>
    
    ...

    * <p> Corresponding methods are defined for the types <tt>char</tt>,
    * <tt>short</tt>, <tt>int</tt>, <tt>long</tt>, and <tt>double</tt>.  The index
    * parameters of the absolute <i>get</i> and <i>put</i> methods are in terms of
    * bytes rather than of the type being read or written.
    
    为char、short、int、long、double类型定义了相应的方法. 绝对get和put方法的索引参数以字节为单位，而不是读取或写入的类型.

    * <a name="views"></a>
    
    视图

    * <p> For access to homogeneous binary data, that is, sequences of values of
    * the same type, this class defines methods that can create <i>views</i> of a
    * given byte buffer.  A <i>view buffer</i> is simply another buffer whose
    * content is backed by the byte buffer.  Changes to the byte buffer's content
    * will be visible in the view buffer, and vice versa; the two buffers'
    * position, limit, and mark values are independent.  The {@link
    * #asFloatBuffer() asFloatBuffer} method, for example, creates an instance of
    * the {@link FloatBuffer} class that is backed by the byte buffer upon which
    * the method is invoked.  Corresponding view-creation methods are defined for
    * the types <tt>char</tt>, <tt>short</tt>, <tt>int</tt>, <tt>long</tt>, and
    * <tt>double</tt>.
    
    为了访问异构二进制数据，即同一类型的值的序列，该类定义了可以创建指定字节缓冲区的视图的方法. 视图缓冲区只是另外一个缓冲区，其内容由字节缓冲区支持.
    对字节缓冲区内容的更改将在视图缓冲区中可见，反之亦然； 两个缓冲区的position、limit、mark值是相互独立.
    例如，创建FloatBuffer类的实例，该实例由调用方法的字节缓冲区支持. 针对char、short、int、long、double类型定义了相应的视图创建方法.

    * <p> View buffers have three important advantages over the families of
    * type-specific <i>get</i> and <i>put</i> methods described above:
    
    与上述类型特定的get和put方法系列相比，视图缓冲区具有三个重要优势.

    * <ul>
    *
    *   <li><p> A view buffer is indexed not in terms of bytes but rather in terms
    *   of the type-specific size of its values;  </p></li>

        视图缓冲区的索引不是以字节为单位，而是根据其值的特定于类型的大小.        

    *   <li><p> A view buffer provides relative bulk <i>get</i> and <i>put</i>
    *   methods that can transfer contiguous sequences of values between a buffer
    *   and an array or some other buffer of the same type; and  </p></li>
    
        视图缓冲区提供相对的批量获取和放置方法，这些方法可以在缓冲区与数组或相同类型的其他缓冲区之间传输值的连续序列.

    *   <li><p> A view buffer is potentially much more efficient because it will
    *   be direct if, and only if, its backing byte buffer is direct.  </p></li>

        视图缓冲区可能会更有效，因为只有当其缓冲区是直接缓冲区时，视图缓冲区才是直接的.

    * </ul>
    *
    * <p> The byte order of a view buffer is fixed to be that of its byte buffer
    * at the time that the view is created.  </p>

    视图缓冲区的字节顺序固定为创建视图时其字节缓冲区的字节顺序.

    * <h2> Invocation chaining </h2>

    调用链

    * <p> Methods in this class that do not otherwise have a value to return are
    * specified to return the buffer upon which they are invoked.  This allows
    * method invocations to be chained.
    
    此类中没有其他要返回值的方法被指定为返回在其上调用它们的缓冲区. 这允许方法调用被链接

    * The sequence of statements
    *
    * <blockquote><pre>
    * bb.putInt(0xCAFEBABE);
    * bb.putShort(3);
    * bb.putShort(45);</pre></blockquote>
    *
    * can, for example, be replaced by the single statement
    *
    * <blockquote><pre>
    * bb.putInt(0xCAFEBABE).putShort(3).putShort(45);</pre></blockquote>

    ...

```