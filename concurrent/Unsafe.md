# Unsafe
---
## 一.简述
Java无法直接访问底层操作系统，而是通过本地方法来访问的。不过尽管如此，JVM还是开了一个后门，JDK中有一个类Unsafe，它提供了硬件级别的原子操作。

这个类尽管里面的方法都是public的，但是并没有办法使用它们，JDK API文档也没有提供任何关于这个类的方法解释。总而言之，对于Unsafe类的使用都是受限制的，
只有授信的代码才能获得该类的实例，当然JDK库里面的类是可以随意使用的。

## 二.使用
从上述可知，正常情况下是无法使用Unsafe这个类。这里提供两种特殊方式来达到使用这个类：

**（1）打破规则**
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
**（2）授信代码**

从getUnsafe方法的使用限制条件出发，通过Java命令行命令-Xbootclasspath/a把调用Unsafe相关方法的类A所在jar包路径追加到默认的bootstrap路径中，使得A被引导类加载器加载，从而通过Unsafe.getUnsafe方法安全的获取Unsafe实例。
```
java -Xbootclasspath/a: ${path}   // 其中path为调用Unsafe相关方法的类所在jar包路径 
```
## 附录
http://mp.weixin.qq.com/s?__biz=MjM5NjQ5MTI5OA==&mid=2651750294&idx=3&sn=6d5c4fb07aad1809b05f0a02b5d56d66&chksm=bd12a6db8a652fcd1641462103ceb34f4aa607d61d2aaa7de63385d83a253ad28f22ffdd5fa9&mpshare=1&scene=23&srcid=03157BGpHg3OyQQAco3OSGJ0#rd
