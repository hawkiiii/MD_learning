zookeeper：
一个分布式协调框架，统一配置管理、统一命名服务、分布式锁、集群管理

核心：
1 文件系统的数据结构（目录数据结构存数据）
2 事件监听机制，对任意文件节点监听

四种节点：持久化节点 、临时节点、持久化顺序节点、临时顺序节点

架构：CS架构，基于session

集群部署环境：配置文件，配置中心可以写到zookeeper中

-s：排序
-e：临时节点

防治多个节点竞争一个锁，采用 -s 排序

选举机制：（默认投大数）
5个服务器集群，一号服务器投自己，没有大于等于3结束；
一二号都投二号，没有大于等于3结束；
一二三号都投三号，大于等于3结束。

监听器原理：
1 首先要有main线程
2 main线程创建一个zookeeper客户端，同时创建两个线程，一个connect，一个listener
3 通过connect线程将注册的监听事件发送给zookeeper
4 在zookeeper注册监听器列表中将注册监听事件加到列表中
5 zookeeper监听到数据或路径变化，会将消息发送给listener线程
6 listener线程内部调用process方法

![image-20200307180659244](/Users/hawkii/Library/Application Support/typora-user-images/image-20200307180659244.png)

常见监听：
1 监听节点数据变化
2 监听子节点数量，（路径变化）

zookeeper的部署方式有哪几种？
单机模式，集群模式

角色：
follower和leader

常用命令：
ls create get delete set …