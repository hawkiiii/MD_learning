# 0 概念

| 特性       | 容器               | 虚拟机     |
| ---------- | ------------------ | ---------- |
| 启动       | 秒级               | 分钟级     |
| 硬盘使用   | 一般为MB           | 一般为GB   |
| 性能       | 接近原生           | 弱于       |
| 系统支持量 | 单机支持上千个容器 | 一般几十个 |

**容器**：在镜像层之上增加了可写层，出现Running和Exited状态。

**镜像**：linux内核0层，镜像在1层，在内核之上， 不能被修改或不能保存状态。镜像可以构建在另一个镜像之上

**仓库**：装镜像的



```shell
#https://x9k2w806.mirror.aliyuncs.com
#阿里的容器镜像服务
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://x9k2w806.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

```shell
systemctl daemon-reload
systemctl restart docker
docker start registry
```

# 1 docker安装

[docker安装教程](https://www.jianshu.com/p/9b1fddbf0dad)

[国内镜像](https://blog.csdn.net/whatday/article/details/86770609)

```shell
#容器端口映射到linux上
docker run -di --name=tensquare_mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin centos/mysql-57-centos7
```
```shell
docker -v
docker images
docker rm [容器ID]
docker rmi [镜像ID]
docker ps -a
docker ps
docker rm [容器ID]
docker 
```

```shell
#虚拟机重启需要强行关掉，mysql服务 否则docker容器启动不起来
netstat -tanlp
ps -ef | grep mysql
systemctl stop mysqld.service
```

```shell
systemctl start docker #启动
systemctl restart  docker #重启docker服务
sudo service docker restart #重启docker服务
service docker stop #关闭docker
systemctl stop docker #关闭docker
```

```sh
sudo systemctl start docker
docker pull registry
docker run -di
docker images
docker run -it --name=myjdk openjdk:11 /bin/bash
docker pull registry
vi /etc/docker/daemon.json
curl https://www.baidu.com
#列出所有容器端口
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
#删除命令
docker rm 容器id
docker rmi 镜像id
#切记添加端口允许，运营端
```

```shell
vi /etc/systemd/system/docker.service
#修改docker文件
ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375
```

```shell
docker run -it --name=myjdk openjdk:11 /bin/bash
```

```sh
#复制镜像和复制容器都是通过保存为新镜像而进行的。
#保存镜像
docker save ID > xxx.tar
docker load < xxx.tar
#保存容器
docker export ID >xxx.tar
docker import xxx.tar containr:v1
#然后再
docker run -it containr:v1 bash
```

**删除容器**

```shell
docker rm 容器id
docker rm -f $(docker ps -aq)
docker ps -a -q | xargs docker rm
```

![image-20200514220304660](../面试img/image-20200514220304660.png)

![image-20200514220539365](../面试img/image-20200514220539365.png)