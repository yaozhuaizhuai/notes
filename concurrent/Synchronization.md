# Synchronization
---
The Java programming language provides multiple mechanisms for communicating between threads.The most basic of these methods is synchronization, which is implemented using monitors.Each object in Java is associated with a monitor, which a thread can lock or unlock. Only one thread at a time may hold a lock on a monitor. Any other threads attempting to lock that monitor are blocked until they can obtain a lock on that monitor. A thread t may lock a particular monitor multiple times; each unlock reverses the effect of one lock operation.
## A synchronized method
```
public synchronized void syncTask(){
  System.out.println("A synchronized method");
}
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/877cdc7177346fbdbe5ca658d69fe70063a.jpg)
```
```
public static synchronized void syncTask(){
  System.out.println("测试");
}
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/6797438e9d996a810f314d4fc1d8983d090.jpg)
```
## The synchronized statement
```
public void syncTask(){
 synchronized (this){
   System.out.println("The synchronized statement");
 }
}
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/e4e6f5949498d2206ad3064656190c2174a.jpg)
```
