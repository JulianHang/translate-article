### 翻译Java虚拟机规范第一章第一节-(JSR-337/Java8)

主要内容：`简单地阐述了Java语言的历史`

```java

    /**

    1.1 A Bit of History

    一点历史

    The Java® programming language is a general-purpose, concurrent, object-oriented
    language. Its syntax is similar to C and C++, but it omits many of the features that
    make C and C++ complex, confusing, and unsafe. The Java platform was initially
    developed to address the problems of building software for networked consumer
    devices. It was designed to support multiple host architectures and to allow secure
    delivery of software components. To meet these requirements, compiled code had
    to survive transport across networks, operate on any client, and assure the client
    that it was safe to run.

    Java程序语言是一个通用、并发、面向对象的语言. 它的语法类似于C和C++，但是它省略了使C和C++复杂化、混乱和不安全的许多功能.
    最初开发Java平台是为了解决网络消费设备构建软件的问题. 它被设计成支持多种主机体系结构并允许软件组件的安全交付.
    为了满足这些要求，编译后的代码必须能够承受跨网络的传输，可以在任何客户端上运行，并确保客户端可以安全运行.

    The popularization of the World Wide Web made these attributes much more
    interesting. Web browsers enabled millions of people to surf the Net and access
    media-rich content in simple ways. At last there was a medium where what you
    saw and heard was essentially the same regardless of the machine you were using
    and whether it was connected to a fast network or a slow modem.

    万维网的普及使这些属性更加有趣. Web浏览器使数以百万计的人能够以简单的方式上网并访问媒体丰富的内容.
    最后有了一种媒介，不管你是使用什么机器，是否它是连接到一个快速网络还是慢速调制解调器，你看到的和听到的实际上是一致的.

    Web enthusiasts soon discovered that the content supported by the Web's HTML
    document format was too limited. HTML extensions, such as forms, only
    highlighted those limitations, while making it clear that no browser could include
    all the features users wanted. Extensibility was the answer.

    Web爱好者很快发现，Web的HTML文档格式所支持的内容太有限了. HTML扩展，例如表单，仅强调了这些限制，同时明确指出没有浏览器可以包含用户想要的所有功能. HTML扩展就是答案.

    The HotJava browser first showcased the interesting properties of the Java
    programming language and platform by making it possible to embed programs
    inside HTML pages. Programs are transparently downloaded into the browser
    along with the HTML pages in which they appear. Before being accepted by the
    browser, programs are carefully checked to make sure they are safe. Like HTML
    pages, compiled programs are network- and host-independent. The programs
    behave the same way regardless of where they come from or what kind of machine
    they are being loaded into and run on.

    HotJava浏览器首次通过将程序嵌入HTML页面来展示Java编程语言和平台的有趣特性. 随着HTML页面的出现，程序被透明地下载到浏览器中.
    在被浏览器接受之前，应仔细检查程序以确保其安全性. 与HTML页面一样，编译程序与网络和主机无关. 
    这些程序表现出相同的方式，无论它们来自何处或被加载到哪种计算机上并在其上运行.

    A Web browser incorporating the Java platform is no longer limited to a
    predetermined set of capabilities. Visitors to Web pages incorporating dynamic
    content can be assured that their machines cannot be damaged by that content.
    Programmers can write a program once, and it will run on any machine supplying
    a Java run-time environment.

    包含Java平台的Web浏览器不再局限于一组预定功能. 用户访问包含动态内容的Web页面可以确保其机器不会被该内容破坏.
    程序员可以编写一次程序，并且该程序可以在提供Java运行时环境的任何计算机上运行.

    */

```




