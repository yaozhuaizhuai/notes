# 网络配置
---
## 环境
```
硬件：Dell-710
系统：CentOS-7.5 
tip：系统安装前需设置好RAID (ctrl + R 进入Raid设置区)
```
## 配置
```
/etc/systconf/network-scripts路径下如下文件：
ifcfg-em1
ifcfg-em2
ifcfg-em3
ifcfg-em4
ifcfg-lo
只需改动ifcfg-em1即可，修改之前通过cp命令备份。
-----------------------------------------------
原始内容：
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=em1
UUID=0b962a89-c6b8-4dc9-a0cc-1f8e9ca5b746
DEVICE=em1
ONBOOT=no
-------------------------------------------------
修改：
BOOTPROTO=static
ONBOOT=yes
----------------
增加:
NM_CONTROLLED=no
IPADDR=172.19.100.210
NETMASK=255.255.255.0
GATEWAY=172.19.100.1
DNS1=218.2.135.1
DNS2=8.8.8.8
--------------------------
重启网络服务：
systemctl restart network
--------------------------
```
