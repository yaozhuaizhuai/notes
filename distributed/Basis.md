# Basis
---
## CAP
CAP定理是由加州大学伯克利分校Eric Brewer教授提出来的，他指出WEB服务无法同时满足以下3个属性:
* 一致性(Consistency):客户端知道一系列的操作都会同时发生(生效)
* 可用性(Availability):每个操作都必须以可预期的响应结束
* 分区容错性(Partition tolerance):即使出现单个组件无法可用,操作依然可以完成
## BASE
在分布式系统中，我们往往追求的是可用性，它的重要程序比一致性要高，那么如何实现高可用性呢？ 前人已经给我们提出来了另外一个理论，就是BASE理论，它是用来对CAP定理进行进一步扩充的。BASE理论指的是：
* Basically Available （基本可用）
* Soft state （软状态）
* Eventually consistent （最终一致性）

BASE理论是对CAP中的一致性和可用性进行一个权衡的结果，理论的核心思想就是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。