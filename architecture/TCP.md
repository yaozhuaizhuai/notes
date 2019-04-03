## 三次握手
![](https://github.com/c-agam/notes/blob/master/images/3%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

**第一次握手**

客户端向服务器发出连接请求报文，这时报文首部中的同部位SYN=1，同时随机生成初始序列号 seq=x，此时，TCP客户端进程进入了 SYN-SENT（同步已发送状态）状态。TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。这个三次握手中的开始。表示客户端想要和服务端建立连接。
