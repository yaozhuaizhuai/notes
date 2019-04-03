## 三次握手
![](https://github.com/c-agam/notes/blob/master/images/3%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

**第一次握手**

客户端向服务器发出连接请求报文，这时报文首部中的同部位SYN=1，同时随机生成初始序列号 seq=x。此时，TCP客户端进程进入了 SYN-SENT（同步已发送）状态。
