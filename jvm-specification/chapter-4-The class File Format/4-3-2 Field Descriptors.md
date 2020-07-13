### 翻译Java虚拟机规范第四章第三节-第二节(JSR-337/Java8)

> `由于第四章篇幅过长，所以将筛选重点内容进行翻译`

主要内容：`字段描述符对应的类型`


```java

    /**

    4.3.2 Field Descriptors
    
    字段描述符
   
    字段类型术语     类型            解释

    B               byte           signed byte
    C               char           Unicode character code point in the Basic Multilingual Plane, encoded with UTF-16
    D               double         double-precision floating-point value
    F               float          single-precision floating-point value
    I               int            integer
    J               long           long integer
    L               ClassName ;    reference an instance of class ClassName
    S               short          signed short
    Z               boolean        true or false
    [               reference      one array dimension

    */



```