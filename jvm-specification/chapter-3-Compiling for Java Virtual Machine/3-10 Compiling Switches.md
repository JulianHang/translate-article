### 翻译Java虚拟机规范第三章第十节(JSR-337/Java8)

主要内容：`通过示例说明Java虚拟机对switch语法所采用的指令`

```java

    /**

    3.10 Compiling Switches

    编译switch

    Compilation of switch statements uses the tableswitch and lookupswitch
    instructions. The tableswitch instruction is used when the cases of the switch can
    be efficiently represented as indices into a table of target offsets. The default
    target of the switch is used if the value of the expression of the switch falls outside
    the range of valid indices. For instance:

    switch语句的编译使用tableswitch和lookupswitch指令. 针对switch的情况使用tableswitch指令能有效地索引到目标偏移量表.
    如果switch的表达式的值超出有效索引的范围，则使用switch的默认目标. 例如：

        int chooseNear(int i) {
            switch (i) {
            case 0: return 0;
            case 1: return 1;
            case 2: return 2;
            default: return -1;
            }
        }

    compiles to:

        Method int chooseNear(int)
        0 iload_1 // Push local variable 1 (argument i)
        1 tableswitch 0 to 2: // Valid indices are 0 through 2
        0: 28 // If i is 0, continue at 28
        1: 30 // If i is 1, continue at 30
        2: 32 // If i is 2, continue at 32
        default:34 // Otherwise, continue at 34
        28 iconst_0 // i was 0; push int constant 0...
        29 ireturn // ...and return it
        30 iconst_1 // i was 1; push int constant 1...
        31 ireturn // ...and return it
        32 iconst_2 // i was 2; push int constant 2...
        33 ireturn // ...and return it
        34 iconst_m1 // otherwise push int constant -1...
        35 ireturn // ...and return it

    The Java Virtual Machine's tableswitch and lookupswitch instructions operate only
    on int data. Because operations on byte, char, or short values are internally
    promoted to int, a switch whose expression evaluates to one of those types is
    compiled as though it evaluated to type int. If the chooseNear method had been
    written using type short, the same Java Virtual Machine instructions would have
    been generated as when using type int. Other numeric types must be narrowed to
    type int for use in a switch.

    Java虚拟机的tableswitch和lookupswitch指令仅对int数据起作用. 
    因为在内部将对byte、char或short值的操作提升为int，所以将对表达式的计算结果为这些类型之一的switch进行编译，就好像其对int类型的计算一样.
    如果chooseNear方法是使用short类型编写的，则将生成与使用int类型相同的Java虚拟机指令.
    必须将其他数字类型的类型缩小为int才能在switch中使用.

    Where the cases of the switch are sparse, the table representation of the tableswitch
    instruction becomes inefficient in terms of space. The lookupswitch instruction may
    be used instead. The lookupswitch instruction pairs int keys (the values of the case
    labels) with target offsets in a table. When a lookupswitch instruction is executed,
    the value of the expression of the switch is compared against the keys in the table.
    If one of the keys matches the value of the expression, execution continues at the
    associated target offset. If no key matches, execution continues at the default
    target. For instance, the compiled code for:

    如果switch的情况很少，则tableswitch指令的表示形式在空间方面变得效率低下. 可以改用lookupswitch指令.
    lookupswitch指令将int键（大小写标签的值）与表中的目标偏移量配对.
    当执行lookupswitch指令时，会将switch表达式的值与表中的键进行比较.
    如果其中一个键与表达式的值匹配，则在关联的目标偏移量处继续执行.
    如果没有键匹配，则在默认目标处继续执行.
    例如，以下代码的编译后代码：

        int chooseFar(int i) {
            switch (i) {
                case -100: return -1;
                case 0: return 0;
                case 100: return 1;
                default: return -1;
            }
        }

    looks just like the code for chooseNear, except for the lookupswitch instruction:

        Method int chooseFar(int)
        0 iload_1
        1 lookupswitch 3:
            -100: 36
            0: 38
            100: 40
            default: 42
        36 iconst_m1
        37 ireturn
        38 iconst_0
        39 ireturn
        40 iconst_1
        41 ireturn
        42 iconst_m1
        43 ireturn

    The Java Virtual Machine specifies that the table of the lookupswitch instruction
    must be sorted by key so that implementations may use searches more efficient than
    a linear scan. Even so, the lookupswitch instruction must search its keys for a match
    rather than simply perform a bounds check and index into a table like tableswitch.
    Thus, a tableswitch instruction is probably more efficient than a lookupswitch
    where space considerations permit a choice.

    Java虚拟机指定必须按键对lookupswitch指令的表进行排序，以便实现比线性扫描更有效地使用搜索.
    即使这样，lookupswitch指令也必须在其键中搜索匹配项，而不是简单地执行边界检查和索引到tableswitch之类的表中.
    因此，在空间考虑允许选择的情况下，tableswitch指令可能比lookupswitch更有效.

    */



```