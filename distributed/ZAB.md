**前言**

ZAB协议是为分布式协调服务 Zookeeper专门设计的一种支持崩溃恢复和原子广播的协议。它的全称是：Zookeeper Atomic Broadcast（Zookeeper原子广播协议）。

基于该协议，Zookeeper实现了一种主备模式的系统架构来保持集群中各个副本之间数据一致性。