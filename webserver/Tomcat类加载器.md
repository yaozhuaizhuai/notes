# Tomcat 类加载器
---
在Tomcat目录结构中，有四组目录：

* 放置在/common目录中：类库可被Tomcat和所有web应用共同使用
* 放置在/server目录中：类库可被Tomcat使用，对所有web应用都不可见
* 放置在/shared目录中：类库可被所有web应用可见，对Tomcat不可见
* 放置在/webapp/WEB-INF目录中：类库仅仅可以被此web应用使用，对Tomcat和其他web应用不可见

为了支持这套目录结构，并对目录里面的类进行加载和隔离，Tomcat自定义了多个类加载器，这些类加载器按照经典的双亲委派模型来实现，其关系如下：
