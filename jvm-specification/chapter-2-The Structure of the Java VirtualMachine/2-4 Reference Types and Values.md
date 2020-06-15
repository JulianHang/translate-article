### 翻译Java虚拟机规范第二章第四节-(JSR-337/Java8)

主要内容：`介绍引用类型的概念与类别`

```java

    /**

    2.4 Reference Types and Values

    引用类型和值

    There are three kinds of reference types: class types, array types, and interface
    types. Their values are references to dynamically created class instances, arrays, or
    class instances or arrays that implement interfaces, respectively.

    引用类型有三种：类类型、数组类型和接口类型. 它们的值分别引用动态创建的类实例，数组或实现接口的类实例或数组。

    An array type consists of a component type with a single dimension (whose length
    is not given by the type). The component type of an array type may itself be an array
    type. If, starting from any array type, one considers its component type, and then
    (if that is also an array type) the component type of that type, and so on, eventually
    one must reach a component type that is not an array type; this is called the element
    type of the array type. The element type of an array type is necessarily either a
    primitive type, or a class type, or an interface type.

    数组类型由具有单个维度的组件类型组成（其长度不是由类型指定）. 数组类型的组件类型本身可以是数组类型. 
    如果从任何数组类型开始，先考虑其组件类型，然后再考虑（如果也是数组类型）该类型的组件类型，依此类推，则最终必须达到不是数组类型的组件类型. 这称为数组类型的元素类型.
    数组类型的元素类型必须是基本类型，类类型或接口类型.

    A reference value may also be the special null reference, a reference to no object,
    which will be denoted here by null. The null reference initially has no run-time
    type, but may be cast to any type. The default value of a reference type is null.
    This specification does not mandate a concrete value encoding null.

    引用值也可以是特殊的空引用，即无对象的引用，此处将以Null表示. 空引用最初没有运行时类型，但可以强制转换为任何类型. 本规范不要求编码为null的具体值.

    */

```