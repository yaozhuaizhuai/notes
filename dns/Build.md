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

[root@linux200 ~]# vi /etc/named.conf
```
options {
    listen-on port 53 { 172.19.100.230; };   
         ....
    allow-query     { localhost;any; };
         ...
}
```
[root@linux200 ~]# named-checkconf /etc/named.conf //检查配置

[root@linux200 ~]# systemctl start named //启动服务

[root@linux200 ~]# dig baidu.com @172.19.100.230 //测试DNS服务器
```
; <<>> DiG 9.9.4-RedHat-9.9.4-73.el7_6 <<>> baidu.com @172.19.100.230
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21705
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 5, ADDITIONAL: 6

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;baidu.com.			IN	A

;; ANSWER SECTION:
baidu.com.		600	IN	A	220.181.57.216
baidu.com.		600	IN	A	123.125.115.110

;; AUTHORITY SECTION:
baidu.com.		172799	IN	NS	ns7.baidu.com.
baidu.com.		172799	IN	NS	ns3.baidu.com.
baidu.com.		172799	IN	NS	ns1.baidu.com.
baidu.com.		172799	IN	NS	ns2.baidu.com.
baidu.com.		172799	IN	NS	ns4.baidu.com.

;; ADDITIONAL SECTION:
ns2.baidu.com.		172799	IN	A	220.181.37.10
ns3.baidu.com.		172799	IN	A	112.80.248.64
ns4.baidu.com.		172799	IN	A	14.215.178.80
ns1.baidu.com.		172799	IN	A	202.108.22.220
ns7.baidu.com.		172799	IN	A	180.76.76.92

;; Query time: 1272 msec
;; SERVER: 172.19.100.230#53(172.19.100.230)
;; WHEN: Wed Mar 13 17:53:47 CST 2019
;; MSG SIZE  rcvd: 240
```
