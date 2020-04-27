## Mybatis

Mybatis是一个实现了数据持久化的开源框架，简单理解就是对JDBC进行封装。

ORMapping：对象关系映射，即java到mysql的映射，开发者可以用面向对象思想管理数据库。

**优点**：
1 sql写在xml文件里，从代码中彻底分离，降低耦合度
2 xml标签可以编写动态sql语句
3 提供映射标签，支持对象与数据库的映射ORM

**缺点：**
1 sql语句编写工作量大，尤其是字段多、关联表多时；
2 sql依赖数据库，移植性差。

SqlSessionFactoryBuilder -> **build()** -> SqlSessionFactory  -> **openSession()**  -> SqlSession

![image-20200221154324143](/Users/hawkii/Library/Application Support/typora-user-images/image-20200221154324143.png)

### 如何使用

- 新建工程

```xml
<dependencies>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.5</version>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.11</version>
  </dependency>
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
  </dependency>
</dependencies>
```

  

- 新建数据表

  ```mysql
  use mybatis;
  create table t_account(
  	id int primary key auto_increment,
  	username varchar(11),
  	password varchar(11),
  	age int
  )
  ```
  
- 新建数据表对应的实体类Account

  ```java
  package com.southwind.entity;
  import lombok.Data;
  @Data
  public class Account {
      private long id;
      private String username;
      private String password;
      private int age;
  }
  ```

- 创建Mybatis配置文件config.xml，文件名可以自己取

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置Mybatis运行环境-->
    <!--每个environment对应一个数据库表,可以选择不同库-->
    <environments default="dev">
        <environment id="dev">
            <!--配置JDBC事务管理-->
            <transactionManager type="JDBC"></transactionManager>
            <!--POOLED配置JDBC数据源连接池-->
            <dataSource type="POOLED">
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=UTF-8"/>
            </dataSource>
        </environment>
        <environment id="bck">
            <transactionManager type=""></transactionManager>
            <dataSource type=""></dataSource>
        </environment>
    </environments>
</configuration>
```

> 使用原生接口

1. mybatis框架需要开发者自定义sql语句，写在Mapper.xml文件中，实际开发中，会为每个实体类创建创建对应的Mapper.xml，定义管理该对象数据的sql。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.mapper.AccountMapper">
	<insert id="save" parameterType="com.southwind.entity.Account">
		insert into t_account(username,password,age) values(#{username},#{password},#{age})
	</insert>
</mapper>
```

> namespace通常设置为文件所在包+文件名的形式
> insert标签表示执行添加操作
> delete标签表示执行删除操作
> select标签表示执行选择操作
> update标签表示执行修改操作
> id实际调用mybatis方法时要调用的参数
> parameterType是调用以上方法的实体类

2. 在全局配置文件config.xml中注册AccountMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置Mybatis运行环境-->
    <!--每个environment对应一个数据库表,可以选择不同库-->
    <environments default="dev">
        <environment id="dev">
            <!--配置JDBC事务管理-->
            <transactionManager type="JDBC"></transactionManager>
            <!--POOLED配置JDBC数据源连接池-->
            <dataSource type="POOLED">
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=UTF-8"/>
            </dataSource>
        </environment>
    </environments>
  	<!--注册AccountMapper.xml-->
  	<mappers>
      <mapper resource="com/southwind/mapper/AccountMapper.xml"></mapper>
  	</mappers>
</configuration>
```

3. 
   调用Mybatis原生接口执行添加操作

   ```java
   public class Test {
       public static void main(String[] args) {
           //加载MyBatis配置文件
           InputStream inputStream = Test.class.getClassLoader().getResourceAsStream("config.xml");
           SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
           SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
           SqlSession sqlSession = sqlSessionFactory.openSession();
           String statement = "com.southwind.mapper.AccoutMapper.save";
           Account account = new Account(1L,"张三","123123",22);
           sqlSession.insert(statement,account);
           sqlSession.commit();
           sqlSession.close();
       }
   }
   ```
> 通过 Mapper 代理理实现⾃定义接⼝

* 自定义接口，定义相关业务方法。

* 编写与方法相对应的 Mapper.xml。

1. 自定义接口

```java
package com.southwind.repository;
import com.southwind.entity.Account;
import java.util.List;
public interface AccountRepository {
    public int save(Account account);
    public int update(Account account);
    public int deleteById(long id);
    public Account findById(long id);
    public List<Account> findAll();
}
```

2、创建接⼝对应的 Mapper.xml，定义接⼝方法对应的 SQL 语句。

statement标签可根据SQL选择insert、delete、update和select。
MyBatis 框架会根据规则⾃动创建接口实现类的代理对象。

规则：

* Mapper.xml中namespace为接口的全类名；
* Mapper.xml中statement的id为接口对应方法名；
* Mapper.xml中statement的parameterType和接口中对应方法的参数类型一致；
* Mapper.xml中statement的resultType和接口中对应方法的返回值类型一致。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.southwind.repository.AccountRepository">
    <insert id="save" parameterType="com.southwind.entity.Account">
        insert into t_account(username,password,age) values(#{username},#
{password},#{age})
    </insert>
    <update id="update" parameterType="com.southwind.entity.Account">
        update t_account set username = #{username},password = #{password},age
= #{age} where id = #{id}
    </update>
    <delete id="deleteById" parameterType="long">
        delete from t_account where id = #{id}
    </delete>
    <select id="findAll" resultType="com.southwind.entity.Account">
        select * from t_account
</select>
    <select id="findById" parameterType="long"
resultType="com.southwind.entity.Account">
        select * from t_account where id = #{id}
    </select>
</mapper>
```

3、在 config.xml 中注册 AccountRepository.xml

```xml
<!-- 注册AccountMapper.xml --> 
<mappers>
  <mapper resource="com/southwind/mapper/AccountMapper.xml"></mapper>
  <mapper resource="com/southwind/repository/AccountRepository.xml"></mapper>
</mappers>
```

4、用接口实现一个运行实体类

```java
AccountRepository accountRepository = sqlSession.getMapper(AccountRepository.class);
```

```java
package com.southwind.test;

import com.southwind.repository.AccountRepository;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class Test2 {
    public static void main(String[] args) {
        InputStream inputStream = Test1.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //获取实现接口的代理对象
        AccountRepository accountRepository = sqlSession.getMapper(AccountRepository.class);
//        //添加对象
//        Account account = new Account(3L,"王五","111111",24);
//        int result = accountRepository.save(account);
//        sqlSession.commit();
//        //查询全部对象
//        List<Account> list = accountRepository.findAll();
//        for (Account _account:list){
//            System.out.println(_account);
//        }
//        sqlSession.close();
        //通过id查询对象
//        Account account = accountRepository.findById(3L);
//        System.out.println(account);
//        sqlSession.close();
        //修改对象
//        Account account = accountRepository.findById(2L);
//        account.setUsername("小明");
//        account.setPassword("001122");
//        account.setAge(18);
//        int result = accountRepository.update(account);
//        sqlSession.commit();
//        System.out.println(result);
//        sqlSession.close();
        //通过id删除对象
        int result = accountRepository.deleteById(2L);
        System.out.println(result);
        sqlSession.commit();
        sqlSession.close();
    }
}
```

