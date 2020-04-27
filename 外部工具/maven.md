Maven 是专门用于构建和管理Java相关项目的工具。

Maven是意第绪语，依地语（犹太人使用的国际语），表示专家的意思

---

---

使用Maven管理的Java 项目都有着相同的项目结构
1. 有一个pom.xml 用于维护当前项目都用了哪些jar包

2. 所有的java代码都放在 src/main/java 下面

3. 所有的测试代码都放在src/test/java 下面

---

---

比如说有3个Java 项目，这些项目都不是maven风格。那么这3个项目，就会各自维护一套jar包。 而其中有些jar包是相同的。
而maven风格的项目，首先把所有的jar包都放在"[仓库](https://how2j.cn/k/maven/maven-repositories/1330.html)“ 里，然后哪个项目需要用到这个jar包，只需要给出jar包的名称和版本号就行了。 这样jar包就实现了共享

---

---

所谓的仓库就是用于存放项目需要的jar包的。
maven采用一个仓库，多个项目的方式，让多个项目共享一个仓库里的相同jar包。

---

---

maven配置详述
https://www.cnblogs.com/iceJava/p/10356309.html

---

---

https://www.jianshu.com/p/9d7892788a6a

---

---

下载
http://maven.apache.org/download.cgi

安装
https://blog.csdn.net/u010429286/article/details/77847676
https://www.jianshu.com/p/bf6a2cbf039d

mac的环境变量，注意path路径要修改，根据版本
要source .bash_file

---

---

运行时没有java application
https://blog.csdn.net/culven/article/details/86737332

用springboot1的pom.xml

---

---

maven异常：Updating Maven Project 的统一解决方案

https://blog.csdn.net/moneyshi/article/details/67637563

---

---

doesn't contain a type
https://blog.csdn.net/zhiyuan_ma/article/details/51649004

---

---

maven异常：Updating Maven Project 的统一解决方案
https://blog.51cto.com/13545923/2053427

---

---

Spring Boot下无法加载主类 org.apache.maven.wrapper.MavenWrapperMain问题解决

https://blog.csdn.net/jiangyu1013/article/details/79261084

---

---

填坑之路：SpringBoot导包坑之spring-boot-starter-parent

https://blog.csdn.net/weixin_42236404/article/details/84073969

---

---

.m2 文件在hawkii(home图标)文件夹下，ctrl+shift+. 显示隐藏文件夹

---

---

maven项目，import pom.xml 可以resolve所有依赖