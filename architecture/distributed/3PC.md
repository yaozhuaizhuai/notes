# 3PC
---
## 一.前言
3PC是2PC的改进版，其将2PC中的准备阶段一分为二，形成了cancommit，precommit，do commit三个阶段。与2PC相比，3PC有两点改动：
1. 引入超时机制（超时提交策略，当第三阶段参与者等待协调者超时后会提交事务，解决参与者同步阻塞问题，同时能在发生单点故障时，继续达成一致）
2. 在第一阶段和第二阶段中插入一个准备阶段（为了减少同步阻塞的发生范围）
## 二.协议简介
### 阶段一：CanCommit
1. 事务询问

    协调者向参与者发送CanCommit请求，询问是否可以执行事务操作，并开始等待各参与者的响应

2. 响应反馈
   
   参与者接到CanCommit请求之后，正常情况下，如果其自身认为可以顺利执行事务，则返回Yes响应，否则反馈No
### 阶段二：precommit
**（1）执行事务预提交**

如果所有参与者的反馈都是 Yes 响应，那么

 1. 发送预提交请求

    协调者向参与者发送PreCommit请求，并进入Prepared阶段

 2. 事务预提交

    参与者接收到PreCommit请求后，会执行事务操作，并将undo和redo信息记录到事务日志中

 3. 响应反馈

    如果参与者成功的执行了事务操作，则返回ACK响应，同时开始等待最终指令
     
**（2）中断事务**

任何一个参与者反馈了 No 响应，或者在等待超时之后，协调者尚无法接收到所有参与者的反馈响应，那么就会中断事务。

1. 发送中断请求

   协调者向所有参与者发送abort请求

2. 中断事务

   参与者收到来自协调者的abort请求之后，执行事务的中断
### 阶段三：doCommit
**（1）执行提交**

如果阶段二进入等待最终指令，那么

1. 发送提交请求

   协调接收到参与者发送的ACK响应，那么他将从预提交状态进入到提交状态。并向所有参与者发送doCommit请求。

2. 事务提交 

   参与者接收到doCommit请求之后，执行正式的事务提交。并在完成事务提交之后释放所有事务资源。
   
3. 响应反馈 

   事务提交完之后，向协调者发送ACK响应。

4. 完成事务 

   协调者接收到所有参与者的ack响应之后，完成事务。
   
**（1）中断事务**

如果阶段二进入中断，那么

1. 发送中断请求

   协调者向所有参与者发送abort请求

2. 事务回滚 
   
   参与者接收到abort请求之后，利用其在阶段二记录的undo信息来执行事务的回滚操作，并在完成回滚之后释放所有的事务资源。

3. 反馈结果 

   参与者完成事务回滚之后，向协调者发送ACK消息

4. 中断事务 

   协调者接收到参与者反馈的ACK消息之后，执行事务的中断。
   
## 三.总结
相对于2PC，3PC主要解决的单点故障问题，并减少阻塞，因为一旦参与者无法及时收到来自协调者的信息之后，它会默认执行commit，这样新的协调者产生之后，就可以确定事务的状态，自然单点问题就得到了解决。但是这种机制也会导致数据一致性问题，因为，由于网络原因，协调者发送的abort响应没有及时被参与者接收到，那么参与者在等待超时之后执行了commit操作。这样就和其他接到abort命令并执行回滚的参与者之间存在数据不一致的情况。

