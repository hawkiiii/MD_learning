## Tomcat

1 Tomcat中四大servlet容器
2 Tomcat执行请求流程解析
3 Tomcat与socket、tcp协议和http协议间的关系
4 Tomcat中的io模型解析

ps：servlet是一个类，服务器连接器 server applet

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp){}
```

Tomcat -> 对象servletDemo.doGet()
Tomcat -> RequestFacade实现 -> 接口HttpServletRequest

Facade门面模式，多个代理

### 下载链接TomCat

https://tomcat.apache.org/download-90.cgi

### 搭建tomcat环境

https://www.jianshu.com/p/69496fb3495e

```shell
cd /Library/Tomcat/bin
sudo chmod 755 *.sh
sudo sh ./startup.sh
```

```shell
sh ./shutdown.sh
```



### 4.验证Tomcat是否启动

打开你的safari,然后在网址输入框输入http://localhost:8080/回车

### 部署 j2ee项目

https://blog.csdn.net/mercury0916/article/details/86612038

rdr pass on lo0 inet proto tcp from any to 127.0.0.1 port 80 -> 127.0.0.1 port 8080
rdr pass on lo0 inet proto tcp from any to 127.0.0.1 port 443 -> 127.0.0.1 port 8443

rdr-anchor "forwarding"
load anchor "forwarding"from "/etc/pf.anchors/eclipse.tomcat.forwarding"



xml是html和java文件之间桥梁

- 导入新的html，先在eclipse外面创建好，然后编辑完，new - > file - > advanced link to，添加完自动关联至工程

- tomcat的server.xml是启动一个工程必要条件，需要把地址配对，eclipse自动部署的tomcat不用配置xml

- mysql 需要ip 端口 账号密码 库名字

- Tomcat⁩ /⁨logs⁩/catalina.out 可以监视提交数据 json

- 怎么修改web content dir，工程properties->project facets->Dynamic Web Module->further configuration->web content directory

#### - [Tomcat] server.xml配置appBase与docBase的用法

https://blog.csdn.net/chenxiaodan_danny/article/details/45397765

- 关闭指定端口
https://www.jianshu.com/p/c879c27ed110