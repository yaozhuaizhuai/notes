# DNS搭建
---
## 1.环境
OS：CentOS Linux release 7.5.1804 (Core)

## 2.准备
**关闭防火墙**

[root@linux200 ~]# systemctl stop firewalld

**关闭Selinux**

[root@linux200 ~]# setenforce 0

## 3.安装bind
[root@linux200 ~]# yum install  bind*  -y

[root@linux200 ~]# rpm -ql bind

## 4.配置
[root@linux200 ~]# cp /etc/named.conf{,.bak} // 备份文件

[root@linux200 ~]# vi /etc/named.conf //修改配置
```
options {
    listen-on port 53 { 172.19.100.230; };   
         ....
    allow-query     { localhost;any; };   //允许DNS查询客户端
         ...
}
```
[root@dns-server ~]# named-checkconf /etc/named.conf //检查配置

[root@dns-server ~]# systemctl start named 
