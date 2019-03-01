# 使用nmcli工具来设置
````
[root@k8s-master /]# nmcli connection show  //显示当前网络连接
NAME        UUID                                  TYPE      DEVICE  
Bridge br0  d2d68553-f97e-7549-7a26-b34a26f29318  bridge    br0     
cni0        21266f36-3861-4f7b-a175-a1ddebd5630b  bridge    cni0    
docker0     89d7ec4a-e7c9-4285-96bf-f056d6081da9  bridge    docker0 
virbr0      a86290f1-c733-4b92-b449-f20b7ba5a1e6  bridge    virbr0  
vnet0       4ca1be5a-5061-4868-948d-d713d5ef02e1  tun       vnet0   
vnet1       020d1e4c-c278-49bd-ae14-2fe740a8515b  tun       vnet1
[root@k8s-master /]# nmcli con mod vnet0 ipv4.dns "114.114.114.114 8.8.8.8" // 修改当前网络连接对应的DNS服务器，这里的网络连接可以用名称或者UUID来标识
[root@k8s-master /]# nmcli con up vnet0 // 将DNS配置生效
````
# 使用传统方法，手工修改/etc/resolv.conf
````
1.修改/etc/NetworkManager/NetworkManager.conf 文件，在main部分添加dns=none选项
2.systemctl restart NetworkManager.service
3.修改 /etc/resolv.conf,增加
  nameserver 114.114.114.114
  nameserver 8.8.8.8
````