# conf
````
应用在使用JMX向外界暴露监控信息时，需要设置以下相关参数：
1.-Dcom.sun.management.jmxremote //是否支持远程JMX访问，默认true
2.-Dcom.sun.management.jmxremote.port //监听端口号
3.-Dcom.sun.management.jmxremote.authenticate //是否需要开启用户认证,默认开启
4.-Dcom.sun.management.jmxremote.ssl //是否对连接开启SSL加密，默认开启
5.-Dcom.sun.management.jmxremote.access.file //对访问用户的权限授权的文件的路径，默认路径JRE_HOME/lib/management/jmxremote.access
6.-Dcom.sun.management.jmxremote. password.file //设置访问用户的用户名和密码，默认路径JRE_HOME/lib/management/ jmxremote.password
````
# MBean
## standard MBean
````
这种类型的MBean最简单，它能管理的资源（包括属性，方法，时间）必须定义在接口中，然后MBean必须实现这个接口。它的命名也必须遵循一定的规范，例如我们的MBean为Hello，则接口必须为HelloMBean
````
## dynamic MBean
````
必须实现javax.management.DynamicMBean接口，所有的属性，方法都在运行时定义
````
## open MBean
````
此MBean的规范还不完善，正在改进中
````
## model MBean
````
略 
Tip: 实践中比较少见
````