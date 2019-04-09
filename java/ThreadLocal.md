## 一、前言
ThreadLocal的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。但是不恰当的使
用，可能会导致内存泄漏。

## 二、实现原理
![](https://github.com/c-agam/notes/blob/master/images/ThreadLocal.png)

每个线程维护一个ThreadLocalMap映射表，这个映射表的key是ThreadLocal实例本身，value 是真正需要存储的数据。也就是是说ThreadLocal本身并不存储值，它只是
作为一个key来让线程从ThreadLocalMap获取value。值得注意的是图中的虚线，表示ThreadLocalMap 是使用ThreadLocal的弱引用作为Key的，弱引用的对象在GC时会被
回收。

## 三、内存泄漏
ThreadLocalMap使用ThreadLocal的弱引用作为key，如果一个ThreadLocal没有外部强引用来引用它，那么系统GC的时候，这个ThreadLocal势必会被回收，这样一来，
ThreadLocalMap中就会出现key为null的Entry，就没有办法访问这些key为null的Entry的value，如果当前线程再迟迟不结束的话，这些key为null的Entry的value就会
一直存在一条强引用链：Thread Ref -> Thread -> ThreaLocalMap -> Entry -> value永远无法回收，从而造成内存泄漏。
