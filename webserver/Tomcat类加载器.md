# Tomcat 类加载器
---
在Tomcat目录结构中，有四组目录：

* 放置在/common目录中：类库可被Tomcat和所有web应用共同使用
* 放置在/server目录中：类库可被Tomcat使用，对所有web应用都不可见
* 放置在/shared目录中：类库可被所有web应用可见，对Tomcat不可见
* 放置在/webapp/WEB-INF目录中：类库仅仅可以被此web应用使用，对Tomcat和其他web应用不可见

为了支持这套目录结构，并对目录里面的类进行加载和隔离，Tomcat自定义了多个类加载器，这些类加载器按照经典的双亲委派模型来实现，其关系如下:

![](https://github.com/c-agam/notes/blob/master/images/Tomcat%E7%B1%BB%E5%8A%A0%E8%BD%BD%E5%99%A8.png)


 上述关系中，要特别注意的是：WebAppClassLoader这个类加载器，它的加载流程是有条件执行双亲委派模型。换句话说：有的时候按照双亲委派模型来执行，有的时候不按照双亲委派模型来执行。实现代码片段如下：
![](https://github.com/c-agam/notes/blob/master/images/WebAppClassLoader-0.png)
![](https://github.com/c-agam/notes/blob/master/images/WebAppClassLoaderBase-1.png)

在Tomcat6.x以及之后的版本中，只有指定了tomcat/conf/catalina.properties配置文件的server.loader和share.loader项后才会真正建立CatalinaClassLoader和ShareClassLoader的实例。
