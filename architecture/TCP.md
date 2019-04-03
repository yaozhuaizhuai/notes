## 三次握手
![](https://github.com/c-agam/notes/blob/master/images/3%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

**第一次握手**

客户端向服务器发出连接请求报文，客户端进程进入了 SYN-SENT状态。

**第二次握手**

TCP服务器收到请求报文后，如果同意连接，则发出确认报文，服务器进程进入了SYN-RCVD状态。

**第三次握手**

客户进程收到确认后，还要向服务器给出确认。
