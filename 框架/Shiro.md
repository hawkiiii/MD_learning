构建一个pojo，包含 id/name 和 password。

包括用户(密码正误)、角色（拥有多个权限）和权限，有两个方法：

```java
private static boolean hasRole(User user,String role)
private static boolean isPermitted(User user,String permit)
```

两个特别方法

```java
Subject 		//对本类对象构建方法
private static Subject getSubject(User user)	
//获得配置文件对象，返回subject
private static boolean login(User user)
//判断是否登录
```

需要事先往sql写入表数据。增加shiro数据库支持，POJO增加三个属性。

DatabaseRealm.java 重写了两个方法，被shiro调用，用户若干个java文件不会调用。

Dao.java增加createUser和getUser的两个方法。

listRoles和listPermissions分别获得角色(3表)和权限(5表)信息

加密方法，md5加密，加盐，加次数。

```java
String password = "123";
String salt = new SecureRandomNumberGenerator().nextBytes().toString();
int times = 2;
String algorithmName = "md5";
String encodedPassword = new SimpleHash(algorithmName,password,salt,times).toString();
```

