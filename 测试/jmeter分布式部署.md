# 1.环境
|服务器地址|CPU|Memory|最大连接数|网络上传带宽|网络下载带宽|角色|
|---|---|---|---|---|---|---|
|172.19.100.210|2C|8G|1024|10M/s|5M/s|Agent|
|172.19.100.211|4C|8G|1024|10M/s|5M/s|Agent|
|172.19.100.212|4C|8G|1024|10M/s|5M/s|Contorller|
# 2.配置Agent
```
[root@agent-1 ~]# vi jmeter-server
RMI_HOST_DEF=-Djava.rmi.server.hostname=172.19.100.210
[root@agent-2 ~]# vi jmeter-server
RMI_HOST_DEF=-Djava.rmi.server.hostname=172.19.100.211
```
# 3.配置Contorller
```
[root@Contorller ~]# vi jmeter.properties
remote_hosts=172.19.100.210:1099，172.19.100.211:1099
```
# 4.预启动Agent
## window
```
jmeter-server.bat
```
## Linux
```
jmeter-server
```
# 5.启动Contorller
## 图形界面
```
Window: jmeter.bat
Linux: jmeter.sh
Run-->Remote start...
```
## 命令行
```
Window: jmeter.bat -n -t onlineTest.jmx -R 172.19.100.210，172.19.100.211 -l line_1.jtl
Linux: jmeter.sh -n -t onlineTest.jmx -R 172.19.100.210，172.19.100.211 -l line_1.jtl
```
# 6.附录
```
如果1099端口不通，请检查防火墙
```

