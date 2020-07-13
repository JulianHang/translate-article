### 翻译Java虚拟机规范第四章第二节-第二节(JSR-337/Java8)

主要内容：`简单说明字段、方法、形参的名称不能随意编写`


```java

    /**

    4.2.2 Unqualified Named

    不合格的名称

    Names of methods, fields, local variables, and formal parameters are stored as
    unqualified names. An unqualified name must contain at least one Unicode code
    point and must not contain any of the ASCII characters . ; [ / (that is, period or
    semicolon or left square bracket or forward slash).

    方法、字段、局部变量和形式参数的名称存储为非限定名称. 
    不合格的名称必须至少包含一个Unicode代码点，并且不得包含任何ASCII字符.
    . ; [ /（即句号或分号或左方括号或正斜杠）.

    Method names are further constrained so that, with the exception of the special
    method names <init> and <clinit> (§2.9), they must not contain the ASCII
    characters < or > (that is, left angle bracket or right angle bracket).

    方法名称受到进一步限制，因此，除了特殊方法名称<init>和<clinit>（第2.9节）外，它们不得包含ASCII字符<或>（即，左尖括号或右尖括号）.

    Note that a field name or interface method name may be <init> or <clinit>, but
    no method invocation instruction may reference <clinit> and only the invokespecial
    instruction (§invokespecial) may reference <init>.

    请注意，字段名称或接口方法名称可以是<init>或<clinit>，但是没有任何方法调用指令可以引用<clinit>，并且只有invokespecial指令（§invokespecial）可以引用<init>.

    */



```