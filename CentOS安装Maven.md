# CentOS安装Maven
---
## 1.将下载的压缩文件解压
```
[root@k8s-master ~]# cd /data/package
[root@k8s-master ~]# wget http://mirrors.shu.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
[root@k8s-master ~]# tar zxf apache-maven-3.6.0-bin.tar.gz
[root@k8s-master ~]# mv apache-maven-3.6.0 /usr/local/maven3
```
## 2.配置环境变量
```
[root@k8s-master ~]# vi /etc/profile
export M2_HOME=/usr/local/maven3
export PATH=$PATH:$JAVA_HOME/bin:$M2_HOME/bin
```
## 3.重启服务
```
[root@k8s-master ~]# source /etc/profile
```
## 4.附录
```
//下载指定依赖
mvn dependency:get -DremoteRepositories=http://repo1.maven.org/maven2/ -DgroupId=junit -DartifactId=junit -Dversion=4.8.2
```
