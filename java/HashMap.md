**1、特点**
* HashMap 是一个散列桶（数组和链表）
* HashMap 是线程不安全的
* HashMap 可以接受 null 键和值，而 Hashtable 则不能

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

红黑树属于平衡二叉树，为了保持“平衡”是需要付出代价的，如果链表长度很短的话，根本不需要引入红黑树，引入反而会慢。

**6、什么是红黑树？**

[RB-Tree](https://github.com/c-agam/notes/blob/master/algorithm/RB-Tree.md)

**7、附录**

[HashMap](http://www.importnew.com/31278.html)
