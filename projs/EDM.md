# 搭建环境

安装好centos7后

## 克隆镜像

克隆镜像，修改hostname、修改网卡配置文件中IP地址和mac地址

```shell
hostnamectl set-hostname 172.16.143.49
cd /etc/sysconfig/network-scripts/ifcfg-ens33
```

在虚拟机设置中，高级生成新的mac，不要与原体冲突。

## 安装MariaDB

分别安装 mysql和MariaDB

```shell
#最好用yum预先删除mariaDB和mysql两个包
yum remove ****
#######
yum install mysql
yum -y install mariadb mariadb-server

cp /usr/share/mysql/my-huge.cnf /etc/my.cnf
vi /etc/my.cnf
#####
在[mysqld]后面添加
lower_case_table_names=1
#####
```

## 启动MariaDB以及初始设置

```shell
#先运行起来
systemctl start mariadb
/usr/bin/mysql_secure_installation
#####
默认无密码，按enter
#####

```

## 配置主从节点

主节点不要动

```shell
vim /etc/my.cnf

#修改从机的id
server-id = 2

systemctl restart mariadb
```

```shell
#主节点登陆
 mysql -uroot -p808784
#配置从节点访问
 GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' IDENTIFIED BY 'slave';
show master status;
```

```shell
#从节点登陆
mysql -uroot -p808784
#配置访问主节点，注意逗号前后不要有空格
CHANGE MASTER TO MASTER_HOST='172.16.143.49',MASTER_USER='slave',MASTER_PASSWORD='slave',MASTER_LOG_FILE='mysql-bin.000005',MASTER_LOG_POS=387;

show slave status\G;

#如果搭建成功 两个YES
            Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
            
#如果失败了两个NO
stop slave; 
start slave; 
```

**从节点只访问不要修改！！！**

# EDM系统项目

[视频连接](https://www.bilibili.com/video/BV1F4411Q7rX?p=1)

[github连接](https://github.com/2301887641/email-support)

![image-20200507110241983](/Users/hawkii/MD_learning/面试img/image-20200507110241983.png)

![image-20200507110427202](/Users/hawkii/MD_learning/面试img/image-20200507110427202.png)