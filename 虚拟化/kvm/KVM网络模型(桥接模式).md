# 概述

```
桥接网络： 虚拟机复用宿主机物理网卡，虚拟机与宿主机在网络中角色完全相同，支持外部访问

```

# 实验环境

```
硬件：Dell-710 8C 16G
系统：centos-7.5

```

# 实验准备

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup //备份配置
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo //替换墙内yum源

```

# 安装桥接工具

```
yum install bridge-utils

```

# 配置网桥

```
a.备份ifcfg-em1
	cp ifcfg-em1 ifcfg-em1.bak
b.修改ifcfg-em1
	BRIDGE=br0
	#IPADDR=172.19.100.210
	#NETMASK=255.255.255.0
	#GATEWAY=172.19.100.1
	#DNS1=218.2.135.1
	#DNS2=8.8.8.8
c.创建网桥
  brctl addbr br0
  brctl addif br0 em1
d.创建ifcfg-br0
	TYPE=Bridge
	BOOTPROTO=static
	DEFROUTE=yes
	DEVICE=br0
	ONBOOT=yes
	IPADDR=172.19.100.210
	NETMASK=255.255.255.0
	GATEWAY=172.19.100.1
	DNS1=218.2.135.1
	DNS2=8.8.8.8
e.重启网络服务
	systemctl restart network

```