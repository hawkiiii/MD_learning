Application.java是否重写了configure(SpringApplicationBuilder application)这个配置方法。

Application.java是否继承了SpringBootServletInitializer，是否加上了@ServletComponentScan注解？



application.yml是用户级的资源配置项

bootstrap.yml是系统级的

bootstrap.yml > application.yml > application-dev.yml



很多错误都是maven dependency的问题，不同版本注解包不同



layui
thmeleaf 一个前端静态模板
html里面th标签

---

1 eurekaserver+configserver+各种微服务;

2 微服务(下简称A)的相关配置
	bootstrap.yml——配置服务名字
	A-dev.yml——配置端口号，数据库，mybatis等等

3 	创建A的实体类 		entity/A.java
   	创建A的AHandler 	controller/AHandler.java
	   创建A的ARepository	repository/ARepository.java
		创建A的运行类			AApplication.java

4	1）用postman测试微服务的各种方法是否正常
	  2）用浏览器测试微服务下findAll,findById等方法
	  3）用浏览器测试feign的微服务下findAll,findById等方法

5	两种不同注解区别

```
@PathVariable 	接的是斜杠地址
@RequestParam 	接的是？ xxx = yyy

```

6	@RestController注解相当于@ResponseBody ＋ @Controller合在一起的作用
1) 如果只是使用@RestController注解Controller，则Controller中的方法无法返回jsp页面，或者html，配置的视图解析器 InternalResourceViewResolver不起作用，返回的内容就是Return 里的内容。
2) 如果需要返回到指定页面，则需要用 @Controller配合视图解析器InternalResourceViewResolver才行。
如果需要返回JSON，XML或自定义mediaType内容到页面，则需要在对应的方法上加上@ResponseBody注解。

7 主外键的约束，你去删一个菜但是别的订单里面有，就会影响删除，确保在别的表里面外键没有
考虑使用**级联删除**

8 如果说jar部署运行有问题，需要添加如下依赖

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>1.5.4.RELEASE</version>
            </plugin>
        </plugins>
    </build>
```