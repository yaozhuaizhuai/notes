# Java对象模型
---
Java是一种面向对象的语言，而对象在JVM中的存储也是有一定的结构的，而这个关于对象自身的存储模型称之为对象模型。
HotSpot虚拟机中，设计了一个OOP-Klass  Model，OOP（Ordinary Object Pointer）指的是普通对象指针，而Klass用来描述对象实例的具体类型。
![](https://github.com/c-agam/notes/blob/master/images/Java%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B.png)
每一个Java类，在被JVM加载的时候，JVM会给这个类创建一个instanceKlass，保存在方法区，用来在JVM层面表示该Java类。当我们在Java代码中，创建一个对象时，JVM会创建一个instanceOopDesc对象，这个对象包含了两部分信息，方法头以及元数据。对象头中有一些运行时数据，其中就包括和多线程相关的锁信息。元数据其实维护的是指针，指向的是对象所属的类的instanceKlass。
