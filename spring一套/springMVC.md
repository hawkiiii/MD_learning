前者有输入框，提交按钮
http://localhost:8080/springmvc/addProduct.jsp

后者木有
http://localhost:8080/springmvc/addProduct

jsp文件中

Form中的get和post方法，在数据传输过程中分别对应了[HTTP协议](https://www.baidu.com/s?wd=HTTP协议&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)中的GET和POST方法。二者主要区别如下：
1、Get是用来从服务器上获得数据，而Post是用来向服务器上传递数据。
2、Get将表单中数据的按照variable=value的形式，添加到action所指向的URL后面，并且两者使用“?”连接，而各个变量之间使用“&”连接；Post是将表单中的数据放在form的数据体中，按照变量和值相对应的方式，传递到action所指向URL。
3、Get是不安全的，因为在传输过程，数据被放在请求的URL中，而如今现有的很多服务器、代理服务器或者[用户代理](https://www.baidu.com/s?wd=用户代理&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)都会将请求URL记录到日志文件中，然后放在某个地方，这样就可能会有一些隐私的信息被第三方看到。另外，用户也可以在浏览器上直接看到提交的数据，一些系统内部消息将会一同显示在用户面前。Post的所有操作对用户来说都是不可见的。
4、Get传输的数据量小，这主要是因为受URL长度限制；而Post可以传输大量的数据，所以在上传文件只能使用Post（当然还有一个原因，将在后面的提到）。
5、Get限制Form表单的数据集的值必须为ASCII字符；而Post支持整个ISO10646字符集。
6、Get是Form的默认方法。
使用Post传输的数据，可以通过设置编码的方式正确转化中文；而Get传输的数据却没有变化。

- 上传jpg文件，只要添加servlet-mapping即可，不用servlet

```xml
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.jpg</url-pattern>
    </servlet-mapping>
```

- 开放对上传功能的支持

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
```

- handler：java中异步消息处理的

  



- SimpleUrlHandlerMapping的配置

https://www.cnblogs.com/lemon-now/p/5553510.html

- Interceptor的配置
https://www.cnblogs.com/rayallenbj/p/8484276.html
https://www.cnblogs.com/junzi2099/p/8260137.html