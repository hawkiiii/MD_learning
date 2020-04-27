监听器快速入门：

创建一个监听器步骤

1 创建一个类，实现指定监听器接口
2 重写接口中的方法
3 在web.xml文件中对监听器进行注册，配置标签

```xml
<listener>
	<listener-class>com.itheima.listener.
    MyServletContextListener</listener-class>
</listener>
```

监听域对象的创建与销毁

监听ServletContext 创建与销毁 ServletContextListener
监听HttpSession 创建与销毁 HttpSessionListener
监听HttpServletRequest 创建与销毁 ServletRequestListener

监听域对象的属性变化

监听ServletContext 属性变化 ServletContextAttributeListener
监听HttpSession 属性变化 HttpSessionAttributeListener
监听HttpServletRequest 属性变化 ServletRequestAttributeListener
属性：添加、移除和替换