### 翻译String注释

```java

   /**
    * The {@code String} class represents character strings. All
    * string literals in Java programs, such as {@code "abc"}, are
    * implemented as instances of this class.
    
    String类表示字符串. 在Java程序中的字符串文字，如"abc"被实现为此类的实例.
    
    * Strings are constant; their values cannot be changed after they
    * are created. String buffers support mutable strings.
    * Because String objects are immutable they can be shared. For example:
    * <blockquote><pre>
    *     String str = "abc";
    * </pre></blockquote><p>
    * is equivalent to:
    * <blockquote><pre>
    *     char data[] = {'a', 'b', 'c'};
    *     String str = new String(data);
    * </pre></blockquote><p>
    * Here are some more examples of how strings can be used:
    * <blockquote><pre>
    *     System.out.println("abc");
    *     String cde = "cde";
    *     System.out.println("abc" + cde);
    *     String c = "abc".substring(2,3);
    *     String d = cde.substring(1, 2);
    * </pre></blockquote>
    
    字符串是常量. 它们被创建后值不允许被改变. 字符串缓冲区支持多变的字符串.
    由于字符串对象是不可变的，因此它们可以被共享. 
    例如 String str = "abc" 是等价于 char[] data = {'a', 'b', 'c'}; String str = new String(data);
    以下是更多有关如何使用字符串的示例

    ...

    * The class {@code String} includes methods for examining
    * individual characters of the sequence, for comparing strings, for
    * searching strings, for extracting substrings, and for creating a
    * copy of a string with all characters translated to uppercase or to
    * lowercase. Case mapping is based on the Unicode Standard version
    * specified by the {@link java.lang.Character Character} class.
    
    String类包括检查序列中各个字符的方法, 比较字符串，查找字符串，提取子串， 创建字符串的副本，并将所有字符转换为大写或小写.
    大小写映射基于Character类指定的Unicode标准版本.

    * The Java language provides special support for the string
    * concatenation operator (&nbsp;+&nbsp;), and for conversion of
    * other objects to strings. String concatenation is implemented
    * through the {@code StringBuilder}(or {@code StringBuffer})
    * class and its {@code append} method.
    * String conversions are implemented through the method
    * {@code toString}, defined by {@code Object} and
    * inherited by all classes in Java. For additional information on
    * string concatenation and conversion, see Gosling, Joy, and Steele,
    * <i>The Java Language Specification</i>.
    
    Java语言为字符串连接运算符(+)、其他对象转换成字符串提供了特殊的支持. 字符串连接是通过StringBuilder或StringBuffer类及其方法实现的.
    字符串转换是通过toString实现的, 该方法由Object定义及被Java中的所有类所继承.
    有关字符串连接和转换的更多信息，...

    * <p> Unless otherwise noted, passing a <tt>null</tt> argument to a constructor
    * or method in this class will cause a {@link NullPointerException} to be
    * thrown.
    
    除非另有说明, 传递null参数给该类的构造器或方法将会抛出空指针.

    * <p>A {@code String} represents a string in the UTF-16 format
    * in which <em>supplementary characters</em> are represented by <em>surrogate
    * pairs</em> (see the section <a href="Character.html#unicode">Unicode
    * Character Representations</a> in the {@code Character} class for
    * more information).
    * Index values refer to {@code char} code units, so a supplementary
    * character uses two positions in a {@code String}.
    * <p>The {@code String} class provides methods for dealing with
    * Unicode code points (i.e., characters), in addition to those for
    * dealing with Unicode code units (i.e., {@code char} values).

    字符串表示UTF-16格式的字符串，其中补充字符由代理对表示（有关更多信息，请参见Character类中的Unicode字符表示部分.
    索引值是指字符代码单位，因此补充字符在字符串中使用两个位置.
    String类提供了用于处理Unicode码点的方法，以及用于处理Unicode代码单元的方法.

```