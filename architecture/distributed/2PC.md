# 2PC
---
## 一.协议简介
### 阶段一：准备阶段
1. 事务询问

   协调者向所有的参与者发送事务内容，询问是否可以执行事务操作，并开始等待各参与者的响应
   
2. 执行事务

   各参与者节点执行事务操作，并将 Undo 和 Redo 信息写入事务日志中
   
3. 响应反馈

   如果参与者成功执行了事务操作，那么就反馈给协调者 Yes 响应，表示事务可以执行；如果参与者没有成功执行事务，那么就反馈给协调者 No 响应，表示事务不可以执行。
### 阶段二：提交阶段
**（1） 执行事务提交**

如果所有参与者的反馈都是 Yes 响应，那么

1. 发送提交请求

   协调者向所有参与者节点发出Commit请求

2. 事务提交

   参与者接收到Commit请求后，会正式执行事务提交操作，并在完成提交之后释放在整个事务执行期间占用的事务资源

3. 反馈事务提交结果

   参与者在完成事务提交之后，向协调者发送ACK信息
   
4. 完成事务

   协调者接收到所有参与者反馈的ACK消息后，完成事务
   
**（2）中断事务**

任何一个参与者反馈了 No 响应，或者在等待超时之后，协调者尚无法接收到所有参与者的反馈响应，那么就会中断事务。

1. 发送回滚请求

   协调者向所有参与者节点发出Rollback请求

2. 事务回滚

   参与者接收到rollback请求后，会利用其在阶段一中记录的Undo信息来执行事务回滚操作，并在完成回滚之后释放整个事务执行期间占用的资源

3. 反馈事务回滚结果

   参与者在完成事务回滚之后，向协调者发送ACK信息

4. 中断事务

   协调者接收到所有参与者反馈的ACK信息后，完成事务中断
### 二.优缺点
优点：原理简单、实现方便

缺点：同步阻塞、单点问题、数据不一致、太过保守

（1）同步阻塞

执行过程中，所有参与节点都是事务阻塞型的。当参与者占有公共资源时，其他第三方节点访问公共资源不得不处于阻塞状态，各个参与者在等待协调者发出提交或中断请求时，会一直阻塞，而协调者的发出时间要依赖于所有参与者的响应时间，如果协调者宕机了（单点），那么他就一直阻塞在这，而且无法达成一致（3PC引入了超时提交解决）

（2）单点问题

由于协调者的重要性，一旦协调者发生故障，参与者会一直阻塞下去。尤其在第二阶段，协调者发生故障，那么所有的参与者还都处于锁定事务资源的状态中，而无法继续完成事务操作

（3）数据不一致

在阶段二，当协调者向所有参与者发送commit请求之后，发生了局部网络异常或协调者在尚未发完commit请求之前自身发生了崩溃，导致最终只有部分参与者接收到了commit请求，于是这部分参与者执行事务提交，而没收到commit请求的参与者则无法进行事务提交，于是整个分布式系统出现了数据不一致性现象。

（4）太过保守

如果参与者在与协调者通信期间出现故障，协调者只能靠超时机制来判断是否需要中断事务，这个策略比较保守，需要更为完善的容错机制，任意一个节点的失败都会导致整个事务的失败。

（5）2PC无法解决的问题

协调者发出commit消息之后宕机，而唯一接收到这条消息的参与者同时也宕机了。那么即使协调者通过选举协议产生了新的协调者，这条事务的状态也是不确定的，没人知道事务是否被已经提交。


