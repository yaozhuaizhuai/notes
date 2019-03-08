# NFS服务安装
---
## 服务端部署
### 1.检查NFS服务nfs-utils和rpcbind是否已经安装
```
[root@k8s-master init.d]# rpm -qa nfs-utils rpcbind
nfs-utils-1.3.0-0.54.el7.x86_64
rpcbind-0.2.0-44.el7.x86_64
```
### 2.安装NFS服务nfs-untils和rpcbind
```
[root@k8s-master init.d]# yum install nfs-utils rpcbind -y
```
### 3.启动rpcbind服务(一定要先启动rpcbind服务再启动NFS服务)
```
[root@k8s-master init.d]# systemctl start rpcbind.service //启动rpcbind服务
[root@k8s-master init.d]# systemctl status rpcbind.service //查看rpcbind服务状态
[root@k8s-master init.d]# systemctl enable rpcbind.service //开机自启动
```
### 4.启动NFS服务
```
[root@k8s-master init.d]# systemctl start nfs.service  //启动NFS服务
[root@k8s-master init.d]# systemctl status nfs.service //查看NFS服务状态
[root@k8s-master init.d]# systemctl enable nfs.service //开机自启动
```
### 5.NFS服务器配置
```
[root@k8s-master conf]# vi /etc/exports
#[NFS共享目录] [NFS客户端地址1(参数1,参数2,参数3……)] [客户端地址2(参数1,参数2,参数3……)]
/data/jenkins 172.19.100.211(rw,sync,no_root_squash) 172.19.100.212(rw,sync,no_root_squash)
/data/maven 172.19.100.211(rw,sync,no_root_squash) 172.19.100.212(rw,sync,no_root_squash)
[root@k8s-master conf]# exportfs -rv //重新加载NFS配置
```
## 客户端部署
### 1.检查NFS服务nfs-utils和rpcbind是否已经安装
```
[root@k8s-master init.d]# rpm -qa nfs-utils rpcbind
nfs-utils-1.3.0-0.54.el7.x86_64
rpcbind-0.2.0-44.el7.x86_64
```
### 2.安装NFS服务nfs-untils和rpcbind
```
[root@k8s-master init.d]# yum install nfs-utils rpcbind -y
```
### 3.启动rpcbind服务(一定要先启动rpcbind服务再启动NFS服务)
```
[root@k8s-master init.d]# systemctl start rpcbind.service //启动rpcbind服务
[root@k8s-master init.d]# systemctl status rpcbind.service //查看rpcbind服务状态
[root@k8s-master init.d]# systemctl enable rpcbind.service //开机自启动
```
### 4.启动NFS服务
```
[root@k8s-master init.d]# systemctl start nfs.service  //启动NFS服务
[root@k8s-master init.d]# systemctl status nfs.service //查看NFS服务状态
[root@k8s-master init.d]# systemctl enable nfs.service //开机自启动
```
