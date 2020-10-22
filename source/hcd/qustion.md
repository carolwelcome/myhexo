### kafka 高可用架构原理

### es 分布式架构原理
1、分布式搜索引擎，底层基于lucene实现，核心思想是在多台机器上启动多个ES进程实例，组成一个ES集群
2、关键点：index、type、mapping、document、field
3、索引又可以分成多个shard,shard负责存储数据。shard分为primary shard和replica shard，并通过这种replica方案实现高可用
### redis 线程模型原理
单线程模型，采用 IO 多路复用机制同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理
### Dubbo 工作原理

### zookeeper 使用场景
1、分布式协调
2、分布式锁
3、元数据/配置信息管理
4、HA高可用性