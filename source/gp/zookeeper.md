#### zookeeper概述（分布式协调服务）

一、zookeeper集群安装
myid zoo.cfg

二、zookeeper数据模型（znode)

三、 节点特性（持久化、临时节点、有序节点、同级节点必须唯一、临时节点不存在子节点）

四、节点的stat信息

五、watcher机制

六、应用场景：
1. 注册中心（watcher 监听感知）
2. 配置中心
3. 负载均衡
4. 分布式锁

#### zookeeper实现原理

一、了解zookeeper的由来

zookeeper的设计猜想
1. 防止单点故障
	集群方案（leader follower)
	还可以分担请求
2. 每个节点的数据是一致的（必须有leader)
	leader master 
	redis-cluster(无中心)
3. leader挂了怎么办？数据如何恢复？选举机制
4. 如何保证数据一致性？分布式事务，2PC协议（二阶段提交）
	2PC，二阶段提交，事务协调者机制
	跨节点事务
	基于2PC来实现数据同步

结论：
1. 为什么要用zab来实现选举
2. 为什么要做集群
3. 为什么要用2pc做数据一致性

ZAB协议：
用于`崩溃恢复`的`原子广播`协议，基于paxos协议衍生算法。
依赖ZAB协议来实现分布式的`数据一致性`(zookeeper类似于主备的形式，通过ZAB协议保证在集群里面各数据节点的数据一致性)

原子广播：改进版的2PC,zxid64位自增ID
1. 对每个消息生成一个zxid
2. 带有zxid的消息作为一个事务分发给集群中的每一个foller节点
3. 把事务写入到磁盘，返回ack
4. leader收到合法数量请求的ack以后再发起commit请求

投票不需要observer的参与，但是必须要和leader节点保持数据同步

崩溃恢复（针对数据层来说）:leader挂掉之后 
1. leader失去过半的follower节点的联系
2. 当leader服务挂了

集群进入到崩溃恢复阶段，对于数据恢复来说，有两点需要考虑，`1.已经被处理的消息不能丢失`，（当leader收到合法数量的ack后,会向各follower发起commit请求同时也会向本地的leader发起commit,这是正常流程。但会出现这种情况，如果部分follower在收到commit命令之前，leader挂了，就导致部分收到，部分没收到，这个时候，`ZAB协议要保证已经处理的消息不能丢失`;`2.被丢弃的消息不能再次出现`(当leader节点在收到事务请求之后，在生成proposal之前，leader挂了，这个时候就要满足leader被重新选举出来以后，新选举出来的leader,能够让这条已经被丢失的消息被其它的节点都丢弃掉)，两个前提

ZAB的设计思想
1. 新选举出来的leader服务器， 是集群中zxid是最大事务（乐观锁）的话，那么新选举出来的leader一定具备已经提交的提案。这样就可以保证已经处理的消息被提交
2. Epoch概念，每产生一个新的leader选举，新的leader的epoch会+1，zxid是64位的数据，低32位表示 消息计数器，高32位存储epoch编号。这样可以保证老的leader恢复以后不会再成为leader,被丢弃的消息也不能再次出现。

理解ZAB协议
leader选举：fast leader做选举，选举指标：zxid最大(64位)【事务id越大表示数据越新】、myid(服务器id,sid)【myid越大，在leader选举中权重越大）
epoch（每一轮投票，epoch会递增）

选举状态：LOOKING、LEADING、FOLLOWING、OBSERVING

选举过程：
启动的时候初始化，每个server会发送（myid,zxid,epoch),发布给集群中的每一个节点，每一个节点收到这些信息之后，进行投票。投票过程中会比较对应的这些数据：检查zxid，如果比较大，会把zxid最大的节点投为leader，如果zxid相同，则去检查myid，myid比较大的投为leader，投票完之后会进行投票统计，根据结果处理。











集群：
角色：leader follower observer
1. 如果是读请求，可以在任意节点为去处理数据 
2. 如果是写请求，那么这个请求会转发给leader去处理
	leader在接收到事务请求（request)之后，会把这个事务(request)发送给集群中每一个节点(除observer之外)，follower节点在接收请求之后，会为返回一个ACK给leader。leader一旦发现过半数的节点是同意的，就会提交该事务（commit),并返回结果（response）给客户端
	Observer是一个观察者的角色，可以了解集群中的状态变化 ，并对这些状态进行同步。不参与投票。奇数节点（过半节点投票）
	


















