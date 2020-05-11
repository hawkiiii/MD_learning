```shell
docker run -di --name=tensquare_mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin centos/mysql-57-centos7
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
sudo systemctl daemon-reload #守护进程重启
systemctl restart  docker #重启docker服务
sudo service docker restart #重启docker服务
service docker stop #关闭docker
systemctl stop docker #关闭docker
```

