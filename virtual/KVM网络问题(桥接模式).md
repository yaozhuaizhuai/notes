# 一.问题描述
---
## 宿主机
1. 可以ping通网关
2. 可以ping通虚拟机
3. 局域网内其他机器可以ping通宿主机
## 虚拟机
1. 可以ping通宿主机
2. ping不通网关
3. 局域网内其他机器可以ping不通虚拟机
# 二.排查过程
## 重启节点或网络
实践证明，重启宿主机或者虚拟机来解决问题的可能性不大（偶然的一次问题修复来至于重启）
## 检查网络配置
1. 确认网络配置文件中没有错别字或者书写错误
2. 确认宿主机上网络连接(DNS设置重点查看，一次问题的修复就是重新设置了DNS)
## 检查路由的配置
在宿主机或者虚拟机中检查路由的信息，配置都是正常的
## 检查网络状态
检查网桥的状态（命令参考brctl show）
如果虚拟机启动之后生成的那些虚拟机网卡设备vnetX没有附在网桥上，使用命令brctl addif 网桥(br0) vnetX(虚拟机设备) 将目前启动的2个虚拟机的网卡附着在网桥上.
## 检查其他
上述排查如果均无结果，就要考虑是不是安装了其他软件，导致网络上面的冲突。
### 案例一：安装docker带来的网络不通问题
````
解析：安装完docker之后，会默认生成网桥docker0，而docker0的网段和局域网网段有时候会出现冲突。
解决：
// 删除原有配置
[root@localhost /]# service docker stop  
[root@localhost /]# ip link set dev docker0 down
[root@localhost /]# brctl delbr docker0
[root@localhost /]# iptables -t nat -F POSTROUTING
// 创建新网桥
[root@localhost /]# brctl addbr docker0
[root@localhost /]# ip addr add 192.168.2.1/24 dev docker0
[root@localhost /]# ip link set dev docker0 up
// 配置docker文件
[root@localhost /]# vi /etc/docker/daemon.json
{
  "bip": "192.168.2.1/24"
}
[root@localhost /]# systemctl restart docker
````
# 参考
````
brctl show 查看网桥
````
