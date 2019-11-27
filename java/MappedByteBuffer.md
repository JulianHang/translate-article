### 翻译MappedByteBuffer注释

```java

   /**
    * A direct byte buffer whose content is a memory-mapped region of a file.

    直接字节缓冲区，其内容是文件的内存映射区域

    * <p> Mapped byte buffers are created via the {@link
    * java.nio.channels.FileChannel#map FileChannel.map} method.  This class
    * extends the {@link ByteBuffer} class with operations that are specific to
    * memory-mapped file regions.
    
     通过FileChannel#map闯将MappedByteBuffer对象. 此类通过特定于内存映射文件区域的操作扩展了ByteBuffer类.

    * <p> A mapped byte buffer and the file mapping that it represents remain
    * valid until the buffer itself is garbage-collected.
    
    映射的字节缓冲区及其表示的文件映射将保持有效，直到缓冲区本身被垃圾回收为止.

    * <p> The content of a mapped byte buffer can change at any time, for example
    * if the content of the corresponding region of the mapped file is changed by
    * this program or another.  Whether or not such changes occur, and when they
    * occur, is operating-system dependent and therefore unspecified.
    
    映射的字节缓冲区的内容可以随时更改，例如，如果此程序或其他程序更改了映射文件的相应区域的内容。 此类更改是否发生以及何时发生，取决于操作系统，因此未指定.

    * <a name="inaccess"></a><p> All or part of a mapped byte buffer may become
    * inaccessible at any time, for example if the mapped file is truncated.  An
    * attempt to access an inaccessible region of a mapped byte buffer will not
    * change the buffer's content and will cause an unspecified exception to be
    * thrown either at the time of the access or at some later time.  It is
    * therefore strongly recommended that appropriate precautions be taken to
    * avoid the manipulation of a mapped file by this program, or by a
    * concurrently running program, except to read or write the file's content.
    
    映射字节缓冲区的全部或部分可能随时无法访问，例如，如果映射文件被截断.
    尝试访问映射的字节缓冲区的不可访问区域将不会更改缓冲区的内容，并且将导致在访问时或稍后发生未指定的异常.
    因此，强烈建议采取适当的预防措施，以防止该程序或并发运行的程序对映射文件的操作，除非要读取或写入文件的内容.

    * <p> Mapped byte buffers otherwise behave no differently than ordinary direct
    * byte buffers. </p>

    映射的字节缓冲区在其他方面的行为与普通的直接字节缓冲区没有什么不同

```