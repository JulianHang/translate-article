### 翻译Java虚拟机规范第三章第九节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机对创建数组所采用的指令`

```java

    /**

    3.9 Arrays

    数组

    Java Virtual Machine arrays are also objects. Arrays are created and manipulated
    using a distinct set of instructions. The newarray instruction is used to create an
    array of a numeric type. The code:

    Java虚拟机数组也是对象. 使用一组不同的指令来创建和操作数组. newarray指令用于创建数字类型的数组. 代码：

        void createBuffer() {
            int buffer[];
            int bufsz = 100;
            int value = 12;
            buffer = new int[bufsz];
            buffer[10] = value;
            value = buffer[11];
        }

    might be compiled to:

        Method void createBuffer()
        0 bipush 100 // Push int constant 100 (bufsz)
        2 istore_2 // Store bufsz in local variable 2
        3 bipush 12 // Push int constant 12 (value)
        5 istore_3 // Store value in local variable 3
        6 iload_2 // Push bufsz...
        7 newarray int // ...and create new int array of that length
        9 astore_1 // Store new array in buffer
        10 aload_1 // Push buffer
        11 bipush 10 // Push int constant 10
        13 iload_3 // Push value
        14 iastore // Store value at buffer[10]
        15 aload_1 // Push buffer
        16 bipush 11 // Push int constant 11
        18 iaload // Push value at buffer[11]...
        19 istore_3 // ...and store it in value
        20 return

    The anewarray instruction is used to create a one-dimensional array of object
    references, for example:

    anewarray指令用于创建对象引用的一维数组，例如：

        void createThreadArray() {
            Thread threads[];
            int count = 10;
            threads = new Thread[count];
            threads[0] = new Thread();
        }

    becomes:

        Method void createThreadArray()
        0 bipush 10 // Push int constant 10
        2 istore_2 // Initialize count to that
        3 iload_2 // Push count, used by anewarray
        4 anewarray class #1 // Create new array of class Thread
        7 astore_1 // Store new array in threads
        8 aload_1 // Push value of threads
        9 iconst_0 // Push int constant 0
        10 new #1 // Create instance of class Thread
        13 dup // Make duplicate reference...
        14 invokespecial #5 // ...for Thread's constructor
        // Method java.lang.Thread.<init>()V
        17 aastore // Store new Thread in array at 0
        18 return

    The anewarray instruction can also be used to create the first dimension of a
    multidimensional array. Alternatively, the multianewarray instruction can be used
    to create several dimensions at once. For example, the three-dimensional array:

    anewarray指令还可用于创建多维数组的第一维. 或者，可以使用multianewarray指令一次创建多个维度. 例如，三维数组：

        int[][][] create3DArray() {
            int grid[][][];
            grid = new int[10][5][];
            return grid;
        }

    is created by:

        Method int create3DArray()[][][]
        0 bipush 10 // Push int 10 (dimension one)
        2 iconst_5 // Push int 5 (dimension two)
        3 multianewarray #1 dim #2 // Class [[[I, a three-dimensional
        // int array; only create the
        // first two dimensions
        7 astore_1 // Store new array...
        8 aload_1 // ...then prepare to return it
        9 areturn

    The first operand of the multianewarray instruction is the run-time constant pool
    index to the array class type to be created. The second is the number of dimensions
    of that array type to actually create. The multianewarray instruction can be used to
    create all the dimensions of the type, as the code for create3DArray shows. Note
    that the multidimensional array is just an object and so is loaded and returned by
    an aload_1 and areturn instruction, respectively. For information about array class
    names, see §4.4.1.

    multianewarray指令的第一个操作数是要创建的数组类类型的运行时常量池索引. 第二个是实际创建的数组类型的维数.
    multianewarray指令可用于创建类型的所有维，如create3DArray的代码所示. 
    请注意，多维数组只是一个对象，因此分别由aload_1和areturn指令加载和返回.
    有关数组类名称的信息，请参见第4.4.1节.

    All arrays have associated lengths, which are accessed via the arraylength
    instruction.
    
    所有数组都有关联的长度，可通过arraylength指令进行访问.


    */



```