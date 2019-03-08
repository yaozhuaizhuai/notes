# CentOS安装JDK
---
## 1.卸载CentOS原本自带的OpenJDK
```
[root@k8s-master ~]# rpm -qa | grep java
python-javapackages-3.4.1-11.el7.noarch
java-1.8.0-openjdk-headless-1.8.0.191.b12-0.el7_5.x86_64
java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64
tzdata-java-2018f-2.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
java-1.7.0-openjdk-1.7.0.191-2.6.15.4.el7_5.x86_64
java-1.7.0-openjdk-headless-1.7.0.191-2.6.15.4.el7_5.x86_64

[root@k8s-master ~]# rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.191.b12-0.el7_5.x86_64
[root@k8s-master ~]# rpm -e --nodeps java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64
[root@k8s-master ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.191-2.6.15.4.el7_5.x86_64
[root@k8s-master ~]# rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.191-2.6.15.4.el7_5.x86_64
```
## 2.将下载的压缩文件解压
```
[root@k8s-master ~]# cd /data/package
[root@k8s-master ~]# tar zxf jdk-8u191-linux-x64.tar.gz
[root@k8s-master ~]# mv jdk1.8.0_191 /usr/local/jdk1.8.0_191
```
## 3.修改环境变量
```
[root@k8s-master ~]# vim /etc/profile
export JAVA_HOME=/usr/local/jdk1.8.0_191
export CLASSPATH=.:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin
```
## 4.重启服务
```
[root@k8s-master ~]# source /etc/profile
```
