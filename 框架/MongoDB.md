# 启动mongoDB

```shell
su root
cd /usr/local/mongodb/bin
#如有必要打开服务器
./mongod
#进入>模式
./mongo
#关闭服务器
./mongo
use  admin
db.shutdownServer()
```

采用MongoDB来实现文章评论功能

# 文章评论数据分析

1. 数据量大
2. 写入操作频繁
3. 价值较低（可丢，相对于订单数据）

# 什么是MongoDB

MongoDB是非关系性数据库，分布式文件存储。介于关系型数据库和非关系型数据库产品。非关中功能最丰富。支持数据结构很松散，类似于json格式。

高性能，易部署，易使用，存储方便。

# MongoDB体系结构

主要由：文档，集合，数据库构成

| MongoDB         | MySQL           |
| --------------- | --------------- |
| 数据库 DataBase | 数据库 DataBase |
| 集合 collection | 表 Table        |
| 文档 document   | 行 Row          |

# MongoDB支持数据类型

| 数据类型           | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| String             | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
| Integer            | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。 |
| Boolean            | 布尔值。用于存储布尔值（真/假）。                            |
| Double             | 双精度浮点值。用于存储浮点值。                               |
| Array              | 用于将数组或列表或多个值存储为一个键。                       |
| Timestamp          | 时间戳。记录文档修改或添加的具体时间。                       |
| Object             | 用于内嵌文档。                                               |
| Null               | 用于创建空值。                                               |
| Date               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
| Object ID          | 对象 ID。用于创建文档的 ID。**主键默认是该类型**             |
| Binary Data        | 二进制数据。用于存储二进制数据。                             |
| Code               | 代码类型。用于在文档中存储 JavaScript 代码。                 |
| Regular expression | 正则表达式类型。用于存储正则表达式。                         |