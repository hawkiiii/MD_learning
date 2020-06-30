

# 启动redis

```shell
#定位到本机redis
cd /usr/local/redis
#运行server
redis-server redis.conf
#redis登陆服务器
redis-cli
```

# Nosql概述

非关系型数据库Not only sql

Mysql关系型，行和列，超过三百万就要加索引，1kw就要分库分表。

Map<String, Object>

## Redis

Redis 一秒写八万次，读十一万次，NoSQL的缓存记录级别是细粒度缓存，性能大幅提高。

NoSQL特点：

1. 不仅仅是数据
2. 没有固定查询语言
3. 键值对存储，列存储，文档存储，图形数据存储
4. 最终一致性
5. CAP定理和BASE
6. 高性能 高可用 高可扩

队列 操作 执行

## 事务

```
multi

exec(discard)
```

单条命令是原子性的，但事务不保证原子性

一致性，顺序性，排他性

## 配置

重写RedisConfig，替代掉以下默认RedisConfig文件

```java
/org/springframework/boot/autoconfigure/data/redis/RedisAutoConfiguration.java
```

有注解

```java
@ConditionalOnMissingBean(
    name = {"redisTemplate"}
)
```

自己写RedisConfig替换掉以上类，实现自动序列化，将<Object, Object>替换成<String, Object>

## SpringBoot

springboot2.x后，原来用jedis修改为lettuce

jedis：采用直连，多个线程操作，不安全，如果避免不安全用jedisPool连接池，像BIO。
lettuce：采用netty，实例可以在多个线程中共享，不存在不安全情况，减少线程数据，更像NIO。

## 主从配置（集群搭建）

修改多个redis.conf文件

1. 端口
2. pid名字
3. log文件名字
4. dump.rdb名字

默认情况下，每台都是主机
命令行：输入slaveof ip 端口，命令操作暂时
conf文件：取消注释 replication，命令操作永久

全量复制 数据库文件，直传
增量复制 修改命令依次传给slave，完成同步

命令行：slaveof no one
其他从机命令行：选楼上为主机

哨兵模式：一般都是6个节点，哨兵集群3，redis服务3个。
哨兵监控到主机宕机，**主观下线**，哨兵数量足够多，监测、投票，故障转移，切换成功发布订阅模式，将各哨兵监控切换到新的主机，**客观下线**。

sentinel.conf

## redis缓存穿透、击穿和雪崩

**缓存穿透** 得到空的请求，频繁访问redis和mysql。
解决方案：
1 布隆过滤器：讲查询hash化，校验不符合丢弃
2 缓存空对象：对无意义查询，存一个空对象返回，涉及到占用资源，定时时间不一致。

**缓存击穿**
一个热点key，当这个key在过期瞬间，持续大并发击破缓存，而访问数据库，像在屏障上凿开一个洞

1. 设置热点数据永不过期
2. 加分布式锁 setnx，只有一个线程访问持久层mysql

**缓存雪崩**
缓存集中过期失效，或者停电

1. redis高可用

多几台redis

2. 限流降级

缓存失效后，加锁加队列，比如某个key只能一个线程访问

3. 数据预热，让失效时间均匀分布