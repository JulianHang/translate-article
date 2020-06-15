### 翻译Java虚拟机规范第二章第三节-第一节-(JSR-337/Java8)

主要内容：`简单介绍整数类型的范围`

```java

    /**

    2.3.1 Integral Types and Values

    整数类型和值

    The values of the integral types of the Java Virtual Machine are:
    • For byte, from -128 to 127 (-2的7幂次方 to 2的7幂次方 - 1), inclusive
    • For short, from -32768 to 32767 (-2的15幂次方 to 2的15幂次方 - 1), inclusive
    • For int, from -2147483648 to 2147483647 (-2的31幂次方 to 2的31幂次方 - 1), inclusive
    • For long, from -9223372036854775808 to 9223372036854775807 (-2的63幂次方 to 2的63幂次方- 1), inclusive
    • For char, from 0 to 65535 inclusive

    Java虚拟机整数类型的值是：
    byte：-128 - 127
    short：-32768 - 32767
    int：-2147483648 - 2147483647
    long：-9223372036854775808 - 9223372036854775807
    char：0 - 65535

    */
```