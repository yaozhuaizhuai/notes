**1、特点**
* HashMap 是一个散列桶（数组和链表）
* HashMap 是线程不安全的
* HashMap 可以接受 null 键和值，而 Hashtable 则不能

**2、HashMap中hash函数怎么是实现的？**
```
//高低16位异或
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

//获取下标
(n-1) & hash
```
