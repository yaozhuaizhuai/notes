**1、特点**
* HashMap 是一个散列桶（数组和链表）
* HashMap 是线程不安全的
* HashMap 可以接受null键和值，而Hashtable则不能

**2、hash函数是怎么实现的？**
```
//高低16位异或
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

//获取下标
(n-1) & hash
```
**3、如何解决Hash碰撞**

HashMap使用链接法（拉链法）来解决Hash冲突问题，使用红黑树来解决链表过深问题。

**4、为什么不用二叉查找树代替而选择红黑树？**

二叉查找树在特殊情况下会变成一条线性结构，这就跟原来使用链表结构一样了，造成层次很深的问题。

**5、为什么不一直使用红黑树？**

红黑树属于平衡二叉树，为了保持“平衡”是需要付出代价的，如果链表长度很短的话，根本不需要引入红黑树，引入反而会变慢。

**6、什么是红黑树？**
* 每个节点非红即黑
* 根节点是黑色
* 每个叶子节点（Nil）是黑色
* 如果一个节点是红色，则它的子节点必须是黑色
* 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑色节点

**7、HashMap与HashTable区别？**
* 默认容量不同，扩容不同
* 线程安全性：HashTable 安全
* 效率不同：HashTable 要慢，因为加锁

**8、附录**

[参考](http://www.importnew.com/31278.html)
