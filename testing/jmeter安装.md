# 1.下载jmeter安装包
```
[root@agent-1 ~]# wget http://mirror.bit.edu.cn/apache//jmeter/binaries/apache-jmeter-5.0.tgz
```
# 2.解压jmeter安装包
```
[root@agent-1 ~]# tar -zxvf apache-jmeter-5.0.tgz
```
# 3.配置环境
```
[root@agent-1 ~]# vi /ect/profile
export JMETER_HOME=/data/apache-jmeter-5.0
export CLASSPATH=$JMETER_HOME/lib/ext/ApacheJMeter_core.jar:$JMETER_HOME/lib/jorphan.jar:$CLASSPATH
export PATH=$JMETER_HOME/bin:$PATH
```
# 4.激活配置
```
[root@agent-1 ~]# source /etc/profile
```