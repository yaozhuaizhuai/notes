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
[root@linux200 ~]# cp /etc/named.conf{,.bak}

[root@linux200 ~]# vim /etc/named.conf

