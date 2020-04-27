```java
java.io
java.nio
```

java.io最为核心的概念是流（iostream），面向流的编程。java中要么是输入流，要么是输出流，无法同时做输入输出。io的非同步阻塞，是每有一个socket时候，new一个线程。来实现io通信。单个看阻塞，整体非阻塞。线程池可以节省线程的资源。

[BIO/NIO的讲解](https://www.bilibili.com/video/av23594034)

java.nio中拥有三个核心概念：Selector、Channel和Buffer。在java.nio中，我们是面向块（Block）或是缓冲区（Buffer）编程的，Buffer本身就是一块内存，底层实现是数组，数据的读写都是Buffer实现的，buffer再进入程序，不能从Channel到Buffer。

除了数组之外，Buffer还提供了对于数据结构化的访问方式，并且可以追踪到系统的读写过程。

Java中八种原生数据类型都有对应的Buffer类型，如IntBuffer、LongBuffer、ByteBuffer和CharBuffer。

Channel指的是可以向其写入数据或是从中读取数据的对象，它类似以java.io的Stream。

所有数据的读写都是通过Buffer进行的，永远不会出现直接想Channel写入数据的情况，或是直接从Channel读取数据的情况。

与Stream不同的是，Channel是双向的，一旦打开就可以是InputStream或OutputStream，可进行读取、写入和读写。

由于Channel是双向的，它能更好的反映出底层操作系统的真实情况，在Linux中底层操作系统的通道是双向的。

**模式**：银行排队取钱 -> 拿号 -> 排队 -> 柜台取钱
几个server就通过几个channel链接selector，通过channel赋值给buffer，选择不同线程。

**Channel只能和Buffer通信。**

![image-20200229191606718](/Users/hawkii/Library/Application Support/typora-user-images/image-20200229191606718.png)

#### Netty框架了解一下

1. netty是通讯框架、异步、非阻塞、高可用（NIO也是）
2. RPC远程调用框架dubbo底层还是netty（更底层是nio）
3. 为什么选netty，解决nio代码复杂问题+容错机制

服务器端

1. 创建服务对象

2. 创两个线程池，第一个监听端口号，nio监听

3. 线程池放入工程

4. 设置管道工程

   设置管道 / 传输数据为String / 事件监听类