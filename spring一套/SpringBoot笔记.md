##   IDEA无法下载jar包、无法联网

​	解决：`pom.xml`文件里加入阿里云搭建的国内镜像http://maven.aliyun.com，跑起来速度很快，可以进行配置
​    

```xml
<repositories>
        <repository>
            <id>nexus-aliyun</id>
            <name>nexus-aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
</repositories>
```

## SpringBoot  404启动失败

- 文件映射路径不对
- 注解没加 `@RequestMapping("/路径")  或 @Responsebody`	
- 文件必须处在启动类所在包的子包下

## 想同时存在两个项目，IDEA默认一个窗口一个项目

​	解决：在已有项目的前提下，如果还想新增其他项目

​		通过New->Module新建

## IDEA文件夹包名不能断开

小齿轮设置->Flatten Packages，即可。

## SpringBoot配置文件

**application.properyies：**

```properties
server.port = 8080   更改Tomcat端口
```

application.yml

```yml
server:
	port: 8080
```

若设置冲突，优先级为：**properyies > yml** 

## SpringBoot引导注释处理器未配置

使用注解@ConfigurationProperties(prefix = “girl”)之后报错

解决办法：POM文件加入依赖

```xml
<dependency>
   <groupId> org.springframework.boot </groupId>
   <artifactId> spring-boot-configuration-processor </artifactId>
   <optional> true </optional>
</dependency>
```

## 静态资源

默认静态资源加载路径：

- classpath:/META-INF/resources/    
- classpath:/resources/
- classpath:/static/   
- classpath:/public/  

注意：这是SpringBoot2.0之前，2.0之后有所变动,暂时未知如何配置

**默认欢迎页**：index.html 

**默认网页logo**：favicon.ico

在任意静态路径中使用即可，但名称要一样。

**自定义静态资源**：配置文件中声明：`spring.resources.static-locations = classpath:/static`

如果有多个，用逗号分割

**设置服务项目名**：`server.servlet.context-path = /demo`



## @ConfigurationProperties和@Value的区别

|                | @ConfigurationProperties | @Value     | 实例               |
| -------------- | ------------------------ | ---------- | ------------------ |
| 实现功能       | 批量注入值               | 单一注入值 |                    |
| 松散语法       | 支持                     | 不支持     | last-name=lastName |
| SpEL           | 不支持                   | 支持       | #{10*2}            |
| 复杂类型封装   | 支持                     | 不支持     | emp.map            |
| JSR303数据校验 | 支持                     | 不支持     |                    |

**二者优先级**：@ConfigurationProperties > @Value

**@Value获取配置文件的值**：例如：@Value（"${emp.age}"）

## 数据校验（@Value不支持）

实体类加注解`@Validated`（开启JSR303数据校验）

属性加校验条件：例如验证邮箱：`@Email`

## 加载局部配置文件（出现问题，暂未解决！！！）

类路径下新建配置文件，例如：`emp.properties`

设置好对应属性的值

实体类加注解：`@PropertySource(value = {"classpath:emp.properties"}, encoding = "utf-8")`



## 导入xml配置文件

有些时候，我们不得不使用xml配置文件，使用方法：

1. 类路径下新建xml配置文件`springConf.xml`，右键New-> XML Configuration File -> Spring Config ,进行自己想要的配置
2. 启动类加注解：`@ImportResource(Locations={"classpath:springConf.xml"})`

```java
 @Autowired
    ApplicationContext context;

    @Test
    public void testConf(){
        EmpService empservice = (EmpService) context.getBean("empService");
        empservice.test();
        System.out.println();
    }
```

## 配置类替换xml配置文件（推荐）

SpringBoot官方推荐使用**配置类**形式注入，使用方法

新建配置类，类加注解`@Configuration`，方法加注解`@Bean`，如下：

```java
@Configuration
public class EmpConfig {

    @Bean
    public EmpService empservice2(){
        return new EmpService(); // 配置类里方法的返回值必须是要注入的组件对象
    }
}
```

要注入的Service方法：

```java
public class EmpService {

    public void testClass(){
        System.out.println("注入service组件成功！！！！");

    }
}
```

测试配置类注入：

```java
 @Test
    public  void testConfigClass(){
        EmpService empservice = (EmpService) context.getBean("empservice2");
       // getBean里的id值是配置类里的方法名
        empservice.testClass();
    }
```

## Profile的多环境切换

> Profile是Spring用来针对不同的环境要求，提供不同的配置支持

### application.properties多环境切换

命名规则：`application-环境名.properties`

例如：

- `application.properties（默认环境）`
- `application-dev.properties（测试环境）`
- `application-prod.properties（生产环境）`

**如何确定使用哪个properties环境？**

在`application.properties（默认环境）`中声明：

```properties
spring.profiles.active=dev   # 声明使用dev环境
```

### application.yml多环境切换

```yaml
server:
  port: 8080    # 默认环境
spring:
  profiles:
    active: dev  # 显式声明使用dev环境，端口号为：6464
---  # 使用三个“-”来分割环境  
server:
  port: 6464
spring:
  profiles: dev   # 测试环境

---
server:
  port: 5252
spring:
  profiles: prod  #开发环境
```

### 运行参数切换环境

上方下拉三角框 -> Edit Configurations -> Environment -> Program arguments ，写入

```yaml
--spring.profiles.active=prod  # 切换prod环境
```

**注意：**这种方式优先级要大于properties和yml配置文件

### 虚拟机参数切换环境

同样上面的路径，只不过在**VM options**输入框中填入

```
-Dspring.properties.active=dev
```

**优先级：**VM options < Program arguments 

### 打成JAR包情况下切换环境

1. 利用Maven将程序打包
2. cmd命令行进入包的文件夹执行命令

```xml
java -jar jar包全名 --spring.profiles.active=prod  # 使用prod环境
```

**优先级：**此类方法优先级最高。

## 配置文件加载位置

- SpringBoot启动时，会扫描以下位置的`application.properties`和`application.yml`

| 配置文件位置         | 说明                               |
| -------------------- | ---------------------------------- |
| `root目录/config/`   | 根目录下的config目录下（级别最高） |
| `root目录`           | 跟目录                             |
| `classpath:/config/` | 类路径的config目录下               |
| `classpath:`         | 类路径下（级别最低）               |

- 配置文件加载顺序：**由下往上**，**逐层覆盖**，这样一来`root目录/config/`级别就是最高的。

- **注意：**==如果使用IDEA创建的项目是 Module （如果是 Project 则忽略）,当前项目的根目录不是你这个项目所有目录（是Project所在目录） ，这样使用 file: 存放配置文件时会找不到配置==

**配置文件常用属性：**[SpringBoot官网链接](https://docs.spring.io/spring-boot/docs/2.1.1.RELEASE/reference/htmlsingle/#common-application-properties)

## SpringBoot日志配置

- Spring Boot 采用了 `slf4j+logback`的组合形式，Spring Boot也提供对`JUL`、`log4j2`、`Logback`提供了默认配置
- 由于默认配置好了，只要启动 Spring Boot 项目就会在控制台输出日志信息。

### 设置日志等级

==日志等级划分：trace < debug < info < warn < error==

```properties
#设置某个包的级别
logging.level.包名=trace
#设置root级别（SpringBoot自带包的级别）
logging.level.root=debug
```

### 输出日志到文件

- 在`application.properties`配置文件中修改配置文件

```properties
# 输出到当前项目根目录下的springboot.log文件中
logging.file = springboot.log

# 输出到E盘下的springboot.log文件中
logging.file = E:/springboot.log

# 输出到当前项目所在磁盘根目录下的springboot文件夹中的springboot.log
logging.path = /springboot/springboot.log
```

- 推荐使用`logging.file`来定义日志输出文件，它可以自由定义日志输出位置以及文件名称。
- 都是以追加方式输出到文件

### 修改日志输出格式

- 在`application.properties`配置文件中修改配置文件

```properties
# 修改控制台输出的日志格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n

# 修改文件中输出的日志格式
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss.SSS} >>> [%thread] >>> %-5level >>> %logger{50} >>> %msg%n
```

### 利用webjars引入依赖

- `pom.xml`添加`jquery`依赖

```xml
 <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>3.3.1</version>
 </dependency>
```

- 前台引入方式

```html
<script th:src="@{webjars/jquery/3.3.1/jquery.js}"></script>
```

应用会自动去webjars下的目录里寻找对应的文件。

## SpringBoot热部署设置

- 在开发中，我们修改一个文件后，要重启应用。会浪费很多时间，我们希望在不重启的情况下，程序可以自动热部署，不需要重启。

添加SpringBoot热部署依赖

```xml
<!--热部署-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
</dependency>
```

配置文件禁用模板缓存

```properties
#开发环境下关闭thymeleaf模板缓
spring.thymeleaf.cache=false
```

手动Ctrl+F9（其实IDEA有热部署，但效果不明显，所以还是手动按键进行Build）

- 此时可以直接刷新浏览器看到效果。

## 配置资源路径映射类

- 我希望自定义默认的访问，也就是`localhost:8080`可以访问到模板文件`main/login.html`

```java
@Configuration
public class MySpringMvcConfigurer implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("main/login");
    }
   /* 另一种方法，不需要实现接口，利用匿名类方式返回WebMvcConfigurer对象
   @Bean   
    public WebMvcConfigurer webMvcConfigurer(){
        return  new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("main/login");
            }
        };
    }*/
}
```

## 语言国际化设置

- 在`resources`目录下创建`i18n`文件夹
- 利用IDEA方式创建对应的国际化文件。zh_CN：中文         en_US：英文（美国）

自定义区域解析器

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Arrays;
import java.util.Locale;

/**
 * @author 张帅
 * @date 2020/1/4 15:39
 * @description
 */
public class MyLocaleResolver implements LocaleResolver {
    /**
     * 自定义区域解析器
     * @param httpServletRequest
     * @return
     */
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String language = httpServletRequest.getParameter("l");
        // 先将默认的请求参数赋值给locale，防止l为空。
        Locale locale = Locale.getDefault();
        // 如果locale不为空，则进行获取自定义信息
        if (!StringUtils.isEmpty(language)){
            String[] split = language.split("_");
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }
    
    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

将区域解析器在配置类里注入spring容器

```java
 @Bean
 public LocaleResolver localeResolver() {
      return new MyLocaleResolver();
}
```

前台发送链接

```html
<div style="margin-left: 100px;">
   <a href="#" th:href="@{/index.html(l='zh_CN')}">中文</a>
   <a href="#" th:href="@{/index.html(l='en_US')}">English</a>
</div>
```

## SpringBoot整合MyBatis

1.在`pom.xml`文件里确保添加相关依赖

```xml
<!-- MyBatis依赖-->
<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
	<artifactId>mybatis-spring-boot-starter</artifactId>
	<version>1.2.0</version>
</dependency>
<!-- MySQL依赖-->
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```

2.建立mapper接口文件

```java
@Repository
public interface LoginMapper {
    List<User> findAllUser();
}
```

- 这里不需要加`@mapper`注解，因为我们一会会在主配置类上加扫描
- 加`@Repository`注解是因为在控制层注入实例时会有警告

3.建立mapper映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
				<!--这里放mapper接口文件的位置-->
<mapper namespace="com.zs.ots.mapper.LoginMapper">
		<!--mapper接口文件对应的方法名-->
    <select id="findAllUser" resultType="com.zs.ots.entity.User">
        select * from user
    </select>

</mapper>
```

- `namespace`：这里放mapper接口文件的位置
- `id`：存放接口文件里对应的方法名
- `resultType`：可以把查询结果封装到pojo类型中，但必须pojo类的属性名和查询到的数据库表的字段名一致。 
-  `resultMap`：如果sql查询到的字段与pojo的属性名不一致，则需要使用**resultMap**将字段名和属性名对应起来，进行手动配置封装，将结果映射到pojo中

4.`application.properties`文件加入配置

```properties
mybatis.mapper-locations = classpath*:mybatis/mapper/*.xml
```

- 指定 mapper 文件的位置。如果在项目中你的 mapper 文件是按目录来放置，

5.`启动类`添加注解

```java
@MapperScan("com.zs.ots.mapper") // 定义了在哪里扫描mapper文件
@SpringBootApplication
public class OtsApplication {
	public static void main(String[] args) {
		SpringApplication.run(OtsApplication.class, args);
	}
}

```

- 我们在这里添加了`@MapperScan`注解，就不需要在mapper接口文件里添加`@mapper`注解，因为那样比较麻烦，必须每一个文件都要加上注解。

6.控制层调用

```java
@RequestMapping
@RestController
public class LoginController {
    @Autowired
    LoginMapper loginMapper;

    @GetMapping("/user")
    public List<User> findAllUser(){
        return loginMapper.findAllUser();

    }
}
```

