# Synchronization
---
The Java programming language provides multiple mechanisms for communicating between threads.The most basic of these methods is synchronization, which is implemented using monitors.Each object in Java is associated with a monitor, which a thread can lock or unlock. Only one thread at a time may hold a lock on a monitor. Any other threads attempting to lock that monitor are blocked until they can obtain a lock on that monitor. A thread t may lock a particular monitor multiple times; each unlock reverses the effect of one lock operation.
## A synchronized method
```
public synchronized void syncTask(){
  System.out.println("测试");
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/877cdc7177346fbdbe5ca658d69fe70063a.jpg)
```
可以看出，JVM是通过编译之后的标志位ACC_SYNCHRONIZED来识别是否使用了synchronized
```
```
public static synchronized void syncTask(){
  System.out.println("测试");
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/6797438e9d996a810f314d4fc1d8983d090.jpg)
```
可以发现除了多了一个标识ACC_STATIC，其余都与上一案例一致，当然结论也就是一致的了
```
## The synchronized statement
```
public void syncTask(){
 synchronized (this){
   System.out.println("测试");
 }
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/e4e6f5949498d2206ad3064656190c2174a.jpg)
```
可以获知，这种情况，synchronized的实现是基于进入和退出Monitor对象实现（monitorenter与monitorexit），其中monitorenter指令作用于同步代码块的开始位置，monitorexit指令作用于同步代码块结束的位置。任何一个对象都有一个Monitor与之相关联，当一个线程持有Minitor后，它将处于锁定状态。
```
