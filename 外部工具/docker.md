0 docker安装

[docker安装教程](https://www.jianshu.com/p/9b1fddbf0dad)

[国内镜像](https://blog.csdn.net/whatday/article/details/86770609)

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

```
vi /etc/systemd/system/docker.service
ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375
```



```sh
systemctl daemon-reload
systemctl restart docker
docker start registry
```

1 先在mac运行docker 下的centos7环境

```
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

docker：统一开发环境、测试环境、运维环境

最火的概念：集群、分布式、微服务

docker集装箱至于物流

| 特性       | 容器               | 虚拟机     |
| ---------- | ------------------ | ---------- |
| 启动       | 秒级               | 分钟级     |
| 硬盘使用   | 一般为MB           | 一般为GB   |
| 性能       | 接近原生           | 弱于       |
| 系统支持量 | 单机支持上千个容器 | 一般几十个 |

