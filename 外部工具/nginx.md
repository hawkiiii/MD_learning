nginx广泛的网页服务器

反向代理：把client请求分发到不同的server

https://www.jianshu.com/p/ae76c223c6ef

正向代理：VPN，客户端 -> 代理服务器 -> 谷歌

反向代理：nginx典型应用，客户端无感知，请求发送到反向代理服务器(nginx)，再指向目标服务器(tomcat)，只暴露代理服务器，隐藏真实目标服务器，将两者整体看做一个服务器。

负载均衡：多请求，分发到多个目标服务器。

动静分离：nginx代理服务器存储html/js/css/img等数据，tomcat放servlet/jsp等动态数据。

```powershell
/usr/local/nginx/sbin	#进入nginx目录
./nginx -v						#查看nginx版本
./nginx								#启动
./nginx -s stop
./nginx -s reload			#/usr/local/nginx/conf/nginx.conf 重新装载这个
```

1. 全局块
   worker_process越大处理并发越多

```powershell
worker_process 1;
```

2. events块
   worker_connections最大连接数

```powershell
events{
		worker_connections 1024;
}
```

3. http块
   nginx.conf中配置最频繁部分

   http全局块：文件引入，mime-types，日志自定义，连接超时时间等

   server块：和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立硬件主机完全一样。每个http块可以包含多个server块，每个server相当于一个虚拟主机

   location块：配置路径，或者资源

```powershell

```



---

b站链接
[CentOS+Nginx搭建1](https://www.bilibili.com/video/av49606102)
[CentOS+Nginx搭建2](https://www.bilibili.com/video/BV1sb411V7hu)

如果第1、2、3步已经存在，跳过直接运行第4步。
1. 安装make
```powershell
yum -y install gcc automake autoconf libtool make
yum install gcc gcc-c++
```
2. 安装pcre
```powershell
cd /usr/local/src
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
tar -zxvf pcre-8.39.tar.gz
cd pcre-8.39
./configure
make && make install
```

3. 安装zlib
```powershell
cd /usr/local/src
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make && make install
```

4. 安装openssl
```powershell
cd /usr/local/src
wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
tar -zxvf openssl-1.0.1t.tar.gz
```

5. 安装nginx
```powershell
#实际需要的依赖包
yum install -y zlib*

cd /usr/local/src
wget http://nginx.org/download/nginx-1.1.10.tar.gz
tar -zxvf nginx-1.1.10.tar.gz
cd nginx-1.1.10
./configure
make && make install
```

nginx启动命令:
**ifconfig ip地址验证nginx是否打开成功**
```powershell
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

**注：**
端口和防火墙开闭:
```powershell
firewall-cmd --zone=public --add-port=80/tcp --permanent #开放80端口

netstat -ntlp
kill xxxx

systemctl restart firewalld.service
systemctl stop firewalld.service#停止防火墙
systemctl start firewalld.service#启动防火墙
```

```powershell
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
-bash: wget: 未找到命令
#安装openssl时报错了
yum -y install #wget安装一下即可
```

6. http转https

一定需要SSL证书，可以映射到域名（ip和域名映射在一起）

- 访问ip地址映射到 /usr/local/nginx/html/index.html
- 用xftp上传ssl签名文件， 创建文件夹/etc/nginx/key/ 存放两证书文件crt+key
- 修改文件 /usr/local/nginx/conf/nginx.conf，server -> ssl_certificate，添加两证书文件crt+key映射信息。

映射（ln：link）两个文件夹地址同一个链接：
```powershell
ln -s /usr/local/lib/libpcre.so.1 /lib
```

```powershell
systemctl restart firewalld.service
systemctl stop firewalld.service#停止防火墙
systemctl start firewalld.service#启动防火墙
```