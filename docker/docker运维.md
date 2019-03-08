# docker运维
---
## 命令
````
#!/bin/bash
#进入容器
sudo docker exec -it 容器ID /bin/bash

#镜像制作
docker build -t registry.cn-hangzhou.aliyuncs.com/zul/jenkins:v3 .
docker push registry.cn-hangzhou.aliyuncs.com/zul/jenkins:v3

#删除镜像
docker rmi $(docker images|grep "关键字" | awk "{print $3}") //删除指定关键字的镜像
docker rmi $(docker images -q) //删除所有镜像

#删除容器
docker stop $(docker ps -a -q) //停止所有容器
docker rm $(docker ps -a -q) //删除所有容器

#查看容器在宿主机上映射后的进程信息
docker top 容器ID
````
## 修改Docker的默认存储位置(centos7)
````
//停掉Docker
systemctl stop docker 

//迁移Docker数据
cp -r /var/lib/docker/* /data/docker/

//修改Docker Root Dir
vi /usr/lib/systemd/system/docker.service // ExecStart=/usr/bin/dockerd --graph=/data/docker

//启动docker
systemctl daemon-reload && systemctl start docker

//查看修改后的信息
docker info

//卸载挂载点
umount /var/lib/docker/*

//删除原始数据
rm -rf /var/lib/docker/*
````
