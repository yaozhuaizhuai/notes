k8s搭建
===========================
## 1.集群环境
|hostname|IP|remark|os|
|---|---|---|---|
|k8s-master|172.19.100.210|docker、kubectl、kubelet、kubeadm、flannel|centos7.*|
|k8s-node1|172.19.100.211|docker、kubectl、kubelet、kubeadm|centos7.*|
|k8s-node2|172.19.100.212|docker、kubectl、kubelet、kubeadm|centos7.*|
## 2.软件版本
```
1. k8s=v1.12.3
2. flannel=v0.10.0-amd64 
3. istio
```
## 3.环境准备
### 3.1设置主机名称
```
hostnamectl set-hostname k8s-master
hostnamectl set-hostname k8s-node1
hostnamectl set-hostname k8s-node2
```
### 3.2配置主机映射
```
172.19.100.210 k8s-master
172.19.100.211 k8s-node1
172.19.100.212 k8s-node2
```
### 3.3关闭防火墙
```
systemctl stop firewalld
systemctl disable firewalld
```
### 3.4关闭selinux
```
setenforce  0
sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/sysconfig/selinux
sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
sed -i "s/^SELINUX=permissive/SELINUX=disabled/g" /etc/sysconfig/selinux 
sed -i "s/^SELINUX=permissive/SELINUX=disabled/g" /etc/selinux/config
```
### 3.5关闭Swap
```
swapoff -a //零时关闭，重启后失效
sed -i 's/.*swap.*/#&/' /etc/fstab  //永久关闭，重启后生效
```
### 3.6同步时间
```
yum install ntpdate -y
ntpdate  ntp.api.bz
timedatectl set-timezone Asia/Shanghai
```
### 3.7添加阿里云YUM软件源
```
cat << EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```
### 3.8设置内核(可选)
```
echo "* soft nofile 65536" >> /etc/security/limits.conf
echo "* hard nofile 65536" >> /etc/security/limits.conf
echo "* soft nproc 65536"  >> /etc/security/limits.conf
echo "* hard nproc 65536"  >> /etc/security/limits.conf
echo "* soft  memlock  unlimited"  >> /etc/security/limits.conf
echo "* hard memlock  unlimited"  >> /etc/security/limits.conf
```
### 3.9配置转发参数
```
//转发配置
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
//系统生效
sysctl --system
```
## 4.docker安装
```
yum remove docker docker-common docker-selinux //卸载旧版本
yum install -y yum-utils device-mapper-persistent-data lvm2 //安装依赖
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo //配置稳定仓库
yum install -y https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-selinux-17.03.0.ce-1.el7.centos.noarch.rpm
yum install docker-ce-17.03.0.ce-1.el7.centos.x86_64
systemctl enable docker
systemctl start docker
Tip: docker-ce-selinux的版本要与docker的版本一致，否则会有问题
```
## 5.安装kubeadm，kubelet，kubectl
```
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
systemctl enable kubelet
systemctl start kubelet
```
## 6.使用kubeadm创建单个Master集群
```
1.kubeadm init --kubernetes-version=1.12.3 --pod-network-cidr=172.20.0.0/16 --apiserver-advertise-address=172.19.100.210
2.mkdir -p $HOME/.kube
3.cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
4.chown $(id -u):$(id -g) $HOME/.kube/config
Tip: 如果1步骤执行，出现拉取镜像超时,原因是默认下载镜像地址在国外,无法访问所以需要根据提示准备镜像
K8S_VERSION=v1.12.3
ETCD_VERSION=3.2.24
DASHBOARD_VERSION=v1.8.3
FLANNEL_VERSION=v0.10.0-amd64
DNS_VERSION=1.2.2
PAUSE_VERSION=3.1
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:$K8S_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager-amd64:$K8S_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler-amd64:$K8S_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy-amd64:$K8S_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd-amd64:$ETCD_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$DNS_VERSION
docker pull quay.io/coreos/flannel:$FLANNEL_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:$K8S_VERSION k8s.gcr.io/kube-apiserver:$K8S_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager-amd64:$K8S_VERSION k8s.gcr.io/kube-controller-manager:$K8S_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler-amd64:$K8S_VERSION k8s.gcr.io/kube-scheduler:$K8S_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy-amd64:$K8S_VERSION k8s.gcr.io/kube-proxy:$K8S_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd-amd64:$ETCD_VERSION k8s.gcr.io/etcd:$ETCD_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION k8s.gcr.io/pause:$PAUSE_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$DNS_VERSION k8s.gcr.io/coredns:$DNS_VERSION
```
## 7.加入Node
```
在Node节点切换到root账号
kubeadm join 172.19.100.210:6443 --token maqmd6.m0nib6xccbfmfuqa --discovery-token-ca-cert-hash sha256:be9ee64c2ebd0b2abd8c14cac6cfa622ff965ac77e7d84005ce24a488d5501e4
Tip: 
1.token获取：需在master节点执行kubeadm token create获取。有过期效应，如果过期重新执行该命令获取
2.ca证书sha256编码hash值：openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```
### 8.安装Pod网络插件flannel
```
wget https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
kubectl apply -f kube-flannel.yml
Tip：文件中修改下面配置项
"Network": "172.20.0.0/16"
```
### 9.运维
```
//重启kubelet
[root@k8s-master /]# systemctl enable kubelet && systemctl start kubelet 

//重新初始化
[root@k8s-master /]# kubeadm reset 
[root@k8s-master /]# kubeadm init ....

//修改NodePort端口范围
[root@k8s-master /]# vi /etc/kubernetes/manifests/kube-apiserver.yaml
...
spec:
  containers:
  - command:
  - --service-node-port-range=1-65535
...
[root@k8s-master /]# systemctl enable kubelet && systemctl start kubelet

//k8s创建docker-registry类型的Secret认证私有镜像库
//删除
[root@k8s-master /]# kubectl delete secret registry-secrets -n default
//创建
[root@k8s-master /]# kubectl create secret docker-registry registry-secrets -n default --docker-server=... --docker-username=... --docker-password=...

```
### 10.附录
```
参考：kubeadm部署K8s.pdf
问题：https://blog.csdn.net/qq_34857250/article/details/82562514
```
