# Unsafe
---
## 1、简介

Java无法直接访问底层操作系统，而是通过本地方法来访问的。不过尽管如此，JVM还是开了一个后门，JDK中有一个类Unsafe，它提供了硬件级别的原子操作。

这个类尽管里面的方法都是public的，但是并没有办法使用它们，JDK API文档也没有提供任何关于这个类的方法解释。总而言之，对于Unsafe类的使用都是受限制的，只有授信的代码才能获得该类的实例，当然JDK库里面的类是可以随意使用的。

从上述可知，正常情况下是无法使用Unsafe这个类。这里提供两种特殊方式来达到使用这个类：

* 打破规则
```
public class UnsafeDemo {
  private static Unsafe unsafe;
  
  static {
    try {
      Field field = Unsafe.class.getDeclaredField("theUnsafe");
      field.setAccessible(true);
      unsafe = (Unsafe) field.get(null);
    } catch(Exception e) {
       e.printStackTrace();
    }
  }
}

```
* 授信代码：从getUnsafe方法的使用限制条件出发，通过Java命令行命令-Xbootclasspath/a把调用Unsafe相关方法的类A所在jar包路径追加到默认的bootstrap路径中，使得A被引导类加载器加载，从而通过Unsafe.getUnsafe方法安全的获取Unsafe实例。
```
java -Xbootclasspath/a: ${path}   // 其中path为调用Unsafe相关方法的类所在jar包路径 
```

## 2、功能介绍
### 内存操作
这部分主要包含堆外内存的分配、拷贝、释放、给定地址值操作等方法。
![](https://github.com/c-agam/notes/blob/master/images/unsafe-mp.png)
通常，我们在Java中创建的对象都处于堆内内存（heap）中，堆内内存是由JVM所管控的Java进程内存，并且它们遵循JVM的内存管理机制，JVM会采用垃圾回收机制统一管理堆内存。与之相对的是堆外内存，存在于JVM管控之外的内存区域，Java中对堆外内存的操作，依赖于Unsafe提供的操作堆外内存的native方法

**使用堆外内存的原因**

* 对垃圾回收停顿的改善。由于堆外内存是直接受操作系统管理而不是JVM，所以当我们使用堆外内存时，即可保持较小的堆内内存规模，从而在GC时减少回收停顿对于应用的影响。
* 提升程序I/O操作的性能。通常在I/O通信过程中，会存在堆内内存到堆外内存的数据拷贝操作，对于需要频繁进行内存间数据拷贝且生命周期较短的暂存数据，都建议存储到堆外内存。


## 三、附录
http://mp.weixin.qq.com/s?__biz=MjM5NjQ5MTI5OA==&mid=2651750294&idx=3&sn=6d5c4fb07aad1809b05f0a02b5d56d66&chksm=bd12a6db8a652fcd1641462103ceb34f4aa607d61d2aaa7de63385d83a253ad28f22ffdd5fa9&mpshare=1&scene=23&srcid=03157BGpHg3OyQQAco3OSGJ0#rd
