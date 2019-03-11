# 源码解析
---
## 动态配置
````
命令行操作相关配置参数，DynamicConfigManager如何做到内存配置更新？也就是说命令行操作行为如何通知到DynamicConfigManager？
目前从源码上面没有通知，除非zNodeChangeHandlers，zNodeChildChangeHandlers，stateChangeHandlers由两个进程共享
````
## Endpoint
````
n * Endpoint ---> n * acceptor ---> n * (m * processors)
这个里面Endpoint，对应主机网卡个数
````
## 事务问题
