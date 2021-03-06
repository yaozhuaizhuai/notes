**一、AQS？**

AQS是JDK1.5提供的一个基于FIFO等待队列实现的一个用于实现同步器的基础框架，这个基础框架的重要性可以这么说，JCU包里面几乎所有的有关锁、多线程并发以及线程同步器等重要组件的实现都是基于AQS这个框架。AQS的核心思想是基于volatile int state这样的一个属性同时配合Unsafe工具对其原子性的操作来实现对当前锁的状态进行修改。当state的值为0的时候，标识该Lock不被任何线程所占有。

**二、AQS队列**

作为AQS的核心实现的一部分，举个例子来描述一下这个队列长什么样子：
* 假设目前只有一个线程去竞争锁，此时队列为空，即head与tail均为NIL。
* 假设只有线程A获取到了锁并且一直没有释放，接着B与C依次去竞争锁，这样B与C线程会进入队列，如下所示：
![](https://github.com/c-agam/notes/blob/master/images/AQS-Queue.png)

AQS的等待队列基于一个双向链表实现的，head结点不关联线程，后面两个节点分别关联B和C，他们将会按照先后顺序被串联在这个队列上。这个时候如果后面再有线程进来的话将会被当做队列的tail。

**Tip：为了简化分析，下面章节所述都建立在“只有线程A获取到了锁并且一直没有释放，接着B与C依次去竞争锁”这个基础上**

**三、入队列**

从代码层面分析下入队列的过程
```
// Acquires lock in exclusive mode,ignoring interrupts.
public final void acquire(int arg) {
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}

//Creates and enqueues node for current thread and given mode.
private Node addWaiter(Node mode) {
    Node node = new Node(Thread.currentThread(), mode);
    // Try the fast path of enq; backup to full enq on failure
    Node pred = tail;
    if (pred != null) {
        node.prev = pred;
        if (compareAndSetTail(pred, node)) {
            pred.next = node;
            return node;
        }
    }
    enq(node);
    return node;
}

//Inserts node into queue, initializing if necessary.
private Node enq(final Node node) {
    for (;;) {
        Node t = tail;
        if (t == null) { // Must initialize
            if (compareAndSetHead(new Node()))
                tail = head;
        } else {
            node.prev = t;
            if (compareAndSetTail(t, node)) {
                t.next = node;
                return t;
            }
        }
    }
}
```
线程A一直没有释放锁的情况，B线程调用了上述acquire方法，继而会进入到addWaiter方法中，新建一个B-Node。初始情况下tail为Nil，所以线程B会直接进入enq方法中执行相关逻辑。enq内部是一层for的死循环（这里我们给它一个好听点的名字：自旋），第一次自旋会初始化队列，新建一个Node，此时head和tail都会指向Node结点。第二次自旋，程序进入else分支执行，会进行指针互换，继而构建出Head-Node到Tail-B-Node的队列。

**四、挂起等待**

上面只是讲述了addWaiter的实现部分，那么结点入队列之后会继续发生什么呢？那就要看看acquireQueued是怎么实现的了，代码如下：
```
final boolean acquireQueued(final Node node, int arg) {
    boolean failed = true;
    try {
        boolean interrupted = false;
        for (;;) {
            final Node p = node.predecessor();
            if (p == head && tryAcquire(arg)) {
                setHead(node);
                p.next = null; // help GC
                failed = false;
                return interrupted;
            }
            if (shouldParkAfterFailedAcquire(p, node) &&
                parkAndCheckInterrupt())
                interrupted = true;
        }
    } finally {
        if (failed)
            cancelAcquire(node);
    }
}
```
对于B来说，它的prev指向head-node，因此会首先再尝试获取锁一次，如果失败，则会将head-node的waitStatus值为SIGNAL，下次循环的时候再去尝试获取锁，如果还是失败，且这个时候prev节点的waitStatus已经是SIGNAL，则这个时候线程会被通过LockSupport挂起。

**五、唤醒恢复**
```
//Releases in exclusive mode
public final boolean release(int arg) {
    if (tryRelease(arg)) {
        Node h = head;
        if (h != null && h.waitStatus != 0)
            unparkSuccessor(h);
        return true;
    }
    return false;
}
```
Head-node的waitStatus为Node.SIGNAL，因此这个时候要通知队列中的等待线程，可以过来拿锁了，这也是unparkSuccessor做的事情：
* 将队头的waitStatus设置为0
* 找到离队头最近的没有被cancelled的那个结点，并设置它为head
* 将找到的这个结点唤醒

**六、羊群效应**

这里说一下羊群效应，当有多个线程去竞争同一个锁的时候，假设锁被某个线程占用，那么如果有成千上万个线程在等待锁，有一种做法是同时唤醒这成千上万个线程去去竞争锁，这个时候就发生了羊群效应，海量的竞争必然造成资源的剧增和浪费，因此终究只能有一个线程竞争成功，其他线程还是要老老实实的回去等待。**AQS的FIFO的等待队列给解决在锁竞争方面的羊群效应问题提供了一个思路：保持一个FIFO队列，队列每个节点只关心其前一个节点的状态，线程唤醒也只唤醒队头等待线程。**
