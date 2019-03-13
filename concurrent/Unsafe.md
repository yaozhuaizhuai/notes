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

用bootclasspath选项授信代码