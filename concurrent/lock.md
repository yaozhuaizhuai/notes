# 锁
* 重入性：可以被单个线程重复获取

* 独占性：同一个时间点只能被一个线程持有

* 公平锁：线程依次排队获取锁

* 非公平锁：在锁是可获取状态时，不管是不是在队列的开头都会获取锁
---
## ReentrantLock

（1）重入性

（2）独占性

（3）支持公平和非公平获取锁

## ReentrantReadWriteLock
（1）重入性

（2）允许从写锁降级为读锁，但不允许从读锁升级到写锁

（3）读取锁和写入锁都支持锁获取期间的中断

（4）支持公平和非公平的获取锁

（5）Condition支持（仅写锁提供了Conditon实现而读取锁不支持Conditon）