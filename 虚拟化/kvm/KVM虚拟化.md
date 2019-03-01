KVM虚拟化
=====================

## 环境
```
硬件：Dell-710 8C 16G
系统：centos-7.5
```
## 准备
### 1.yum设置
```
#备份配置
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
#替换墙内yum源
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
#生成缓存
yum clean all
yum makecache
```
### 2.安装kvm环境
```
#安装kvm管理工具
yum -y install qemu-kvm libvirt virt-install libvirt-python virt-manager libguestfs-tools  python-virtinst bridge-utils
#关闭虚拟网卡（可选）
[root@localhost ~]# virsh net-list
[root@localhost ~]# virsh net-destroy default
[root@localhost ~]# virsh net-undefine default
[root@localhost ~]# systemctl restart libvirtd
Tip:安装libvirt完之后，会自动创建一个虚拟网卡以及一个virbr0网桥
```
### 3.配置网桥
```
#备份ifcfg-em1
    cp ifcfg-em1 ifcfg-em1.bak
#修改ifcfg-em1
    BRIDGE=br0
    #IPADDR=172.19.100.210
    #NETMASK=255.255.255.0
    #GATEWAY=172.19.100.1
    #DNS1=218.2.135.1
    #DNS2=8.8.8.8
#创建网桥
  brctl addbr br0
  brctl addif br0 em1
#创建ifcfg-br0
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
#重启网络服务
    systemctl restart network.service
```
### 4.检查KVM环境
```
#重启宿主机，以便加载 kvm 模块
reboot
#查看KVM模块是否被正确加载
lsmod | grep kvm
```
### 5.关闭selinux
```
sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/sysconfig/selinux
sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
```
### 6.启动libvert,并设置开机启动
```
systemctl start libvirtd
systemctl enable libvirtd
```
### 7.上传centos7.5并创建虚拟机文件存放目录
```
#上传centos后，如下所示：
/data/kvm/CentOS-7-x86_64-DVD-1804.iso
#创建虚拟机文件存放目录
mkdir -p /data/kvm/images
```
### 8.创建虚拟机
```
#kvm-1
virt-install 
--name kvm-1  //虚拟机名称
--ram 4096 //内存大小
--vcpus=2 //CPU个数
--disk path=/data/kvm/images/kvm-1.img,size=300 //系统存放位置和硬盘大小（GB）
--network bridge=br0  //桥接网卡信息
--cdrom /data/kvm/CentOS-7-x86_64-DVD-1804.iso //iso文件位置
--vnclisten=172.19.100.210 //vnc连接地址
--vncport=6900 //vnc连接端口
--vnc //连接方式

#kvm-2
virt-install 
--name kvm-2  //虚拟机名称
--ram 4096 //内存大小
--vcpus=3 //CPU个数
--disk path=/data/kvm/images/kvm-2.img,size=300 //系统存放位置和硬盘大小（GB）
--network bridge=br0  //桥接网卡信息
--cdrom /data/kvm/CentOS-7-x86_64-DVD-1804.iso //iso文件位置
--vnclisten=172.19.100.210 //vnc连接地址
--vncport=6901 //vnc连接端口
--vnc //连接方式

安装过程中如果出现：
Allocating 'kvm-1.img'   | 300 GB  00:00:00     
Domain installation still in progress. Waiting for installation to complete.
就需要打开VNC客户端软件连接虚拟机,接着安装系统
```
### 9.维护
```
virsh list //查看正在运行 
virsh list --all //查看所有 
virsh start kvm-1 //启动 
virsh shutdown kvm-1 //关机 
virsh destroy kvm-1 //强制关机 
virsh autostart kvm-1 //随机宿主机启动 
virsh suspend kvm-1 //挂起 
virsh resume kvm-1 //恢复 
virsh undefine kvm-1 //删除
virsh edit kvm-1 //编辑

#简述彻底删除流程
virsh shutdown kvm-1
virsh destroy kvm-1
virsh undefine kvm-1
rm /data/kvm/images/kvm-1.img

#磁盘扩容
qemu-img resize kvm-1.img +100G
virsh shutdown kvm-1
virsh start kvm-1
```
### 10.附录
```
vncviewer下载地址https://www.realvnc.com/en/connect/download/viewer/
如果远程连接不上,极有可能是主机开启了防火墙，可以指定开启某一端口：
iptables -I INPUT -p tcp --dport 6900 -j ACCEPT
iptables -I INPUT -p tcp --dport 6901 -j ACCEPT
```