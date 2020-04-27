servlet公司可能不用，但是帮助我们理解其他框架

所谓servlet，即用于动态显示网页网站的一套组建规范。目前最常用的框架spring其底层就是一个比较复杂的servlet。 而单独的servlet本身是没有工作能力的，可以简单理解为servlet没有main方法，所以没办法运行。需要将其配置到相应的容器中，才能发挥对应servlet的作用。

1、需要写一个servlet容器，也就是需要编写一套能够处理浏览器的请求信息的程序。浏览器属于客户端，因此我们需要编写一套服务端的程序能够让客户端做相应的跳转。所以第一步，必须先有一个服务器的类，即WebServer类；

2、有了服务器类，显然由于会有大量线程访问网站，因此我们肯定需要有线程去处理。所以我们需要一个能够处理客户端请求的线程，即ClientHandler类；

3、客户端发过来的请求，往往不会是简单的页面跳转，可能包括注册，登录，下载等一系列复杂的指令要求。因此我们需要一个类，该类的每个对象用来表示一个客户端的请求，即HttpRequest类；

4、同样，服务端根据客户端的请求，需要给出相应的回应。因此需要一个回应类：HttpResponse类。

比较常见servlet容器是tomcat

![](/Users/hawkii/自学MD/servlet0.png)

mac下修改地址
<Context path**=**"" docBase**=**"/Users/hawkii/eclipse-workspace/j2ee/web/" reloadable**=**"true" caseSensitive**=**"false" debug**=**"0"></Context>



Servlet_生命周期：首先加载servlet的class，实例化servlet，然后初始化servlet调用init()的方法，接着调用服务的service的方法处理doGet和doPost方法，最后是我的还有容器关闭时候调用destroy 销毁方法。

1.被创建：执行init方法，只执行一次

　　1.1Servlet什么时候被创建？

　　--默认情况下，第一次被访问时，Servlet被创建，然后执行init方法；

　　--可以配置执行Servlet的创建时机；

2.提供服务：执行service方法，执行多次

3.被销毁：当Servlet服务器正常关闭时，执行destroy方法，只执行一次

---

---

.project

```xml
<projectDescription>
	<name>springmvc</name>
```

此处是配置tomcat的地址，如果配置错在此处修改