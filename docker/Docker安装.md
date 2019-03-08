# Docker安装
---
```
#!/bin/bash
#卸载旧版本
yum remove docker docker-common docker-selinux
#安装依赖
yum install -y yum-utils device-mapper-persistent-data lvm2
#配置稳定仓库
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#安装docker-ce-selinux
yum install -y https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-selinux-17.03.0.ce-1.el7.centos.noarch.rpm
#安装docker
yum install docker-ce-17.03.0.ce-1.el7.centos.x86_64
#启动服务
systemctl enable docker
systemctl start docker
```
