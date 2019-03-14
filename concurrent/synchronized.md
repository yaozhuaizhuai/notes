# synchronized
---
## 简述
```
synchronized：
- 可以保证变量的原子性，可见性和顺序性
- 可以保证方法或者代码块在运行时只有一个方法可以进入临界区获取资源
- 可以保证内存变量的内存可见性
```
## 原理
### 修饰方法
```
public synchronized void syncTask(){
  System.out.println("测试");
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/877cdc7177346fbdbe5ca658d69fe70063a.jpg)
```
由字节码可以看出，JVM是通过编译之后的标志位ACC_SYNCHRONIZED来识别是否使用了synchronized
```
### 修饰静态方法
```
public static synchronized void syncTask(){
  System.out.println("测试");
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/6797438e9d996a810f314d4fc1d8983d090.jpg)
```
你会发现字节码中除了多了一个标识ACC_STATIC，其余都与上一案例一致，当然结论也就是一致的了
```
### 修饰代码块
```
public void syncTask(){
 synchronized (this){
   System.out.println("测试");
 }
}
```
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/e4e6f5949498d2206ad3064656190c2174a.jpg)
```
从字节码中可以获知，这种情况，synchronized的实现是基于进入和退出Monitor对象实现（monitorenter与monitorexit），其中monitorenter指令作用于同步代码块的开始位置，monitorexit指令作用于同步代码块结束的位置。任何一个对象都有一个Monitor与之相关联，当一个线程持有Minitor后，它将处于锁定状态。
```
