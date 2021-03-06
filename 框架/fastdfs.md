### FastDFS

FastDFS 是一个开源的轻量级分布式文件系统，它解决了大数据量存储和负载均衡等问题，中小文件在线存储服务。

FastDFS 不是通用的文件系统，只能通过专有 API 访问，为互联网应用量身定做，解决了大容量文件存储问题，追求高性能和高扩展性，FastDFS 可以看做是基于文件的 Key Value Pair 存储系统，称作分布式文件存储服务会更合适。

#### FastDFS 特性

- 文件不分块存储，上传的文件和 OS 文件系统中的文件一一对应
- 支持相同内容的文件只保存一份，节约磁盘空间
- 下载文件支持 HTTP 协议，可以使用内置 Web Server，也可以和其他 Web Server 配合使用
- 支持在线扩容
- 支持主从文件
- 存储服务器上可以保存文件属性（meta-data）V2.0 网络通信采用 libevent，支持大并发访问，整体性能更好

#### FastDFS 相关概念

FastDFS 服务端有三个角色：跟踪服务器（Tracker Server）、存储服务器（Storage Server）和客户端（Client）。

- **Tracker Server**：主要做调度工作，起负载均衡的作用，是客户端和数据服务器交互的枢纽。
- **Storage Server**：存储服务器（又称存储节点或数据服务器），文件和文件属性（Meta Data）都保存到存储服务器上。
- **Client**：客户端，作为业务请求的发起方，通过专有接口，使用 TCP/IP 协议与跟踪器服务器或存储节点进行数据交互。

通过一张图来看一下 FastDFS 的运行机制：



![图片](https://pic1.zhimg.com/v2-d3ec1b813b2de207a7e4d57faed6084a.png)图片

Tracker 相当于 FastDFS 的大脑，不论是上传还是下载都是通过 Tracker 来分配资源；客户端一般可以使用 Ngnix 等静态服务器来调用或者做一部分的缓存；存储服务器内部分为卷（或者叫做组），卷与卷之间是平行的关系，可以根据资源的使用情况随时增加，卷内服务器文件相互同步备份，以达到容灾的目的。

#### 上传机制

首先客户端请求 Tracker 服务获取到存储服务器的 IP 地址和端口，然后客户端根据返回的 IP 地址和端口号请求上传文件，存储服务器接收到请求后生产文件，并且将文件内容写入磁盘并返回给客户端 file_id、路径信息、文件名等信息，客户端保存相关信息上传完毕。



![图片](https://pic4.zhimg.com/v2-a2646824e9f69801e818a8a011ae31fb.png)图片

#### 下载机制

客户端带上文件名信息请求 Tracker 服务获取到存储服务器的 IP 地址和端口，然后客户端根据返回的 IP 地址和端口号请求下载文件，存储服务器接收到请求后返回文件给客户端。



![图片](https://pic1.zhimg.com/v2-f1595ae483938d1809a5872ae3b44c8a.png)

文件的信息：客户端file_id、路径信息、文件名等信息