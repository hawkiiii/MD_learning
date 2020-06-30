![image-20200512184931114](./面试img/image-20200512184931114.png)

# rocketMQ命令记录

```shell
systemctl start vsftpd
cd /opt/hawkii
mkdir /usr/local/server
mkdir /usr/local/server/mq
mv rocketmq /usr/local/server/mq
cd /usr/local/server/mq/rocketmq
mkdir logs
mkdir store
cd store
mkdir commitlog
mkdir consumequeue
mkdir index
cd ../
cd conf
cd 2m-2s-async
#修改
vim broker-a.properties
```

```shell
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

#所属集群名字
brokerClusterName=rocket-cluster
#broker名字，注意此外不同的配置文件填写的不一样
brokerName=broker-a
#0 表示Master, >0 表示slave
brokerId=0
#nameServer 地址，分号分割
namesrvAddr=rocketmq-nameserver1:9876
#在发送消息时，自动创建服务器不存在的Topic，默认创建的队列数
defaultTopicQueueNums=4
#是否允许Broker 自动创建Topic，建议线下开启，线上关闭
autoCreateTopicEnable=true
#是否允许Broker 自动创建订阅组，建议线下开启，线上关闭
autoCreateSubscriptionGroup=true
#Broker 对外服务的监听端口
listenPort=10911
#删除文件时间点，默认是凌晨4点
deleteWhen=04
#文件保留时间，默认48小时
fileReservedTime=120
#commitLog每个文件的大小默认1G
mapedFileSizeCommitLog=1073741824
#ConsumeQueue每个文件默认存30W条，根据业务情况调整
mapedFileSizeConsumeQueue=300000
# destroyMapedFileIntervalForcibly=12000
# redeleteHangedFileInterval=12000
# 检测物理文件磁盘空间
diskMaxUsedSpaceRatio=88
# 存储路径
storePathRootDir=/usr/local/server/mq/rocketmq/store
# commitLog存储路径
storePathCommitLog=/usr/local/server/mq/rocketmq/store/commitlog
# 消息队列储存路径
storePathConsumeQueue=/usr/local/server/mq/rocketmq/store/consumequeue
# 消息索引粗存路径
storePathIndex=/usr/local/server/mq/rocketmq/store/index
# checkpoint 文件储存路径
storeCheckpoint=/usr/local/server/mq/rocketmq/store/checkpoint
# abort 文件存储路径
abortFile=/usr/local/server/mq/rocketmq/store/abort
# 限制的消息大小
maxMessageSize=65536
# flushCommitLogLeastPages=4
# flushConsumeQueueLeastPages=2
# flushCommitLogThoroughInterval=10000
# flushConsumeQueueThoroughInterval=60000
# Broker的角色
# -ASYNC_MASTER 异步复制Master
# -SYNC_MASTER 同步双写Master
# -SLAVE
brokerRole=ASYNC_MASTER
# 刷盘方式
# - ASYNC_FLUSH 异步刷盘
# - SYNC_FLUSH 同步刷盘
flushDiskType=ASYNC_FLUSH
# checkTransactionMessageEnable=false
# 发消息线程池数量
# sendMessageTreadPoolNums=128
# 拉消息线程池数量
# pullMessageTreadPoolNums=128
```

```shell
vim /etc/hosts
rocketmq-nameserver1 [虚拟机IP地址]
```



```shell
cd /usr/local/server/mq/rocketmq/conf
sed -i 's#${user.home}#/usr/local/server/mq/rocketmq#g' *.xml
cd /usr/local/server/mq/rocketmq/bin
```
**-Xms1g -Xmx1g -Xmn1g**

```shell
vim runbroker.sh
vim runserver.sh
#打开虚拟内存

#启动进程
nohup sh /usr/local/server/mq/rocketmq/bin/mqnamesrv &
nohup sh /usr/local/server/mq/rocketmq/bin/mqbroker -c /usr/local/server/mq/rocketmq/conf/2m-2s-async/broker-a.properties > /dev/null 2>&1 &
# yum install java-1.8.0-openjdk-devel.x86_64
#jps
jps
```

```shell
#拉取控制台
https://github.com/apache/rocketmq
#设置rocketmq服务器地址
rocketmq.config.namesrvAddr=172.16.143.49:9876
#要先启动rocketmq-console服务器，再启动虚拟机服务器
```

```shell
#关闭nameserver和broker
cd /usr/local/server/mq/rocketmq/bin
sh mqshutdown namesrv
sh mqshutdown broker
```

```shell
#修改 /etc/hosts
cd /etc
vim /hosts
###添加如下代码
172.16.143.49 rocketmq-nameserver1
172.16.143.49 rocketmq-master1
172.16.143.50 rocketmq-nameserver2
172.16.143.50 rocketmq-master1-slave1
###
```

```shell
#进入主节点配置文件夹
cd /usr/local/server/mq/rocketmq/conf
#删除broker-a-s.properties，复制broker-a.properties替换之
cp broker-a.properties ./broker-a-s.properties
vim broker-a-s.properties
###修改如下两行
namesrvAddr=rocketmq-nameserver1:9876;rocketmq-nameserver2:9876
brokerId = 1 #大于0即可
brokerRole = Slave
###

```

```shell
#分别启动主从namesrv
nohup sh /usr/local/server/mq/rocketmq/bin/mqnamesrv &
nohup sh /usr/local/server/mq/rocketmq/bin/mqnamesrv &

#分别启动broker
nohup sh /usr/local/server/mq/rocketmq/bin/mqbroker -c /usr/local/server/mq/rocketmq/conf/2m-2s-async/broker-a.properties > /dev/null 2>&1 &
nohup sh /usr/local/server/mq/rocketmq/bin/mqbroker -c /usr/local/server/mq/rocketmq/conf/2m-2s-async/broker-a-s.properties > /dev/null 2>&1 &
```

```shell
#访问
http://localhost:8080/#/cluster
```

# rocketMQ工程配置

## 生产者流程

1. 创建生产者，组名
2. Namesrv地址
3. 开启DefaultMQProducer
4. 创建Message，写Topic、Tag、消息体
5. 发送消息
6. 关闭DefaultMQProducer

## 消费者流程

1. 创建消费者，组名
2. Namesrv地址
3. 订阅Topic、Tag
4. 设置回调函数，处理消息，setMessageListener
5. 启动消费者

# rocketMQ知识概念

## 工作模式

基础、序列、广播、单次、事务（half message以及线程池）等等

## 不同集群方式优劣点

1. 单M：风险大，Broker重启宕机，服务不可用
2. 多M：
   优点：简单，用RAID模式消息安全无影响；
   缺点：Broker宕机，消息不可订
3. 多M多S-异步复制：每M配S，主备消息延迟，缺点：M宕，少量消息损失
4. 多M多S-同步双写：每M配S，主备都写入成功，才返回成功
   优点：服务数据无单点，都高可用，宕机无延迟
   缺点：性能略低于异步复制，Slave无法切为Master

M-S主节点挂了，生产不能写/消费还能订阅。

## 消息包括

Topic	Tag 	msg（可set属性）