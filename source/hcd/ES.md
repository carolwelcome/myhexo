### ES分布式架构设计
ElesticSearch 的设计理念是分布式搜索引擎，基于Lucene,核心思想是在多台机器上启动多个es进程实例，组成一个es集群。
ES中存储数据的基本单位是索引，比如说你现在要在 es 中存储一些订单数据，你就应该在 es 中创建一个索引 order_idx，所有的订单数据就都写到这个索引里面去，一个索引差不多就是相当于是 mysql 里的一张表。
```bash
index -> type -> mapping -> document -> field
```
index 相当于 mysql 里的一张表。而 type 没法跟 mysql 里去对比，一个 index 里可以有多个 type，每个 type 的字段都是差不多的，但是有一些略微的差别。假设有一个 index，是订单 index，里面专门是放订单数据的。就好比说你在 mysql 中建表，有些订单是实物商品的订单，比如一件衣服、一双鞋子；有些订单是虚拟商品的订单，比如游戏点卡，话费充值。就两种订单大部分字段是一样的，但是少部分字段可能有略微的一些差别，所以就会在订单 index 里，建两个type，一个是实物商品订单type，一个是虚拟商品订单type，这两个type大部分字段是一样的，少部分字段是不一样的。

很多情况下，一个 index 里可能就一个 type，但是确实如果说是一个 index 里有多个 type 的情况（注意，mapping types 这个概念在 ElasticSearch 7.X 已被完全移除，详细说明可以参考官方文档），你可以认为 index 是一个类别的表，具体的每个 type 代表了 mysql 中的一个表。每个 type 有一个 mapping，如果你认为一个 type 是具体的一个表，index 就代表多个 type 同属于的一个类型，而 mapping 就是这个 type 的表结构定义，你在 mysql 中创建一个表，肯定是要定义表结构的，里面有哪些字段，每个字段是什么类型。实际上你往 index 里的一个type里面写的一条数据，叫做一条document，一条document就代表了mysql中某个表里的一行，每个document有多个field，每个field就代表了这个document中的一个字段的值。

`自己的理解：`在es中，一个index表示一个索引，每个索引可以对应多个类似的type，这时这个type可以认为是一张数据库表，mapping是表结构，document是表记录，field是表字段。

一个索引，这个索引可以拆分成多个 shard，每个 shard 存储部分数据。拆分多个 shard 是有好处的，一是支持横向扩展，比如你数据量是 3T，3 个 shard，每个 shard 就 1T 的数据，若现在数据量增加到 4T，怎么扩展，很简单，重新建一个有 4 个 shard 的索引，将数据导进去；二是提高性能，数据分布在多个 shard，即多台服务器上，所有的操作，都会在多台机器上并行分布式执行，提高了吞吐量和性能。

 shard 的数据实际是有多个备份，就是说每个 shard 都有一个 primary shard，负责写入数据，但是还有几个 replica shard。primary shard 写入数据之后，会将数据同步到其他几个 replica shard 上去。通过replica方案，各shard都有备份，实现高可用。
 
 es 集群多个节点，会自动选举一个节点为 master 节点，这个 master 节点其实就是干一些管理的工作的，比如维护索引元数据、负责切换 primary shard 和 replica shard 身份等。要是 master 节点宕机了，那么会重新选举一个节点为 master 节点
 
 如果是非 master节点宕机了，那么会由 master 节点，让那个宕机节点上的 primary shard 的身份转移到其他机器上的 replica shard。接着你要是修复了那个宕机机器，重启了之后，master 节点会控制将缺失的 replica shard 分配过去，同步后续修改的数据之类的，让集群恢复正常。
 
 说得更简单一点，就是说如果某个非 master 节点宕机了。那么此节点上的 primary shard 不就没了。那好，master 会让 primary shard 对应的 replica shard（在其他机器上）切换为 primary shard。如果宕机的机器修复了，修复后的节点也不再是 primary shard，而是 replica shard。
 `自己的理解：`一个索引可以存储为多个shard，可以分布在不同的服务器上，各shard通过replica相互备份。由master节点负责维护primary /replica shard,相当于是一个leader
 
 ###  es 写入数据的工作原理是什么啊？
客户端选择一个 node 发送请求过去，这个 node 就是 coordinating node（协调节点）。
coordinating node 对 document 进行路由，将请求转发给对应的 node（有 primary shard）。
实际的 node 上的 primary shard 处理请求，然后将数据同步到 replica node。
coordinating node 如果发现 primary node 和所有 replica node 都搞定之后，就返回响应结果给客户端。

 ###  es 查询数据的工作原理是什么啊？
可以通过 doc id 来查询，会根据 doc id 进行 hash，判断出来当时把 doc id 分配到了哪个 shard 上面去，从那个 shard 去查询。


`写请求是写入 primary shard，然后同步给所有的 replica shard；读请求可以从 primary shard 或 replica shard 读取，采用的是随机轮询算法。`

客户端发送请求到任意一个 node，成为 coordinate node。
coordinate node 对 doc id 进行哈希路由，将请求转发到对应的 node，此时会使用 round-robin 随机轮询算法，在 primary shard 以及其所有 replica 中随机选择一个，让读请求负载均衡。
接收请求的 node 返回 document 给 coordinate node。
coordinate node 返回 document 给客户端。
 ###  底层的 lucene 介绍一下呗？
lucene 就是一个 jar 包，里面包含了封装好的各种建立倒排索引的算法代码。我们用 Java 开发的时候，引入 lucene jar，然后基于 lucene 的 api 去开发就可以了。

通过 lucene，我们可以将已有的数据建立索引，lucene 会在本地磁盘上面，给我们组织索引的数据结构。 
 ### 写数据的底层原理
数据先写入内存 buffer，然后每隔 1s，将数据 refresh 到 os cache，到了 os cache 数据就能被搜索到（所以我们才说 es 从写入到能被搜索到，中间有 1s 的延迟）。每隔 5s，将数据写入 translog 文件（这样如果机器宕机，内存数据全没，最多会有 5s 的数据丢失），translog 大到一定程度，或者默认每隔 30mins，会触发 commit 操作，将缓冲区的数据都 flush 到 segment file 磁盘文件中。

 ### 倒排索引了解吗？
es 无非就是写入数据，搜索数据;
在搜索引擎中，每个文档都有一个对应的文档 ID，文档内容被表示为一系列关键词的集合。例如，文档 1 经过分词，提取了 20 个关键词，每个关键词都会记录它在文档中出现的次数和出现位置。

那么，倒排索引就是关键词到文档 ID 的映射，每个关键词都对应着一系列的文件，这些文件中都出现了关键词。
 
 
 ### es 在数据量很大的情况下（数十亿级别）如何提高查询效率啊？
#### 方案一
filesystem cache,往es里面写的数据都存储在磁盘上，查询的时候操作系统会将磁盘的数据自动缓存到filesystem cache中。所以说es搜索引擎严重依赖底层的filesystem cache,如果给filesystem cache更多的内存，尽量让内存可以容纳所有的idx segment file索引数据文件，那么搜索都是走内存的，性能会非常高。所以说如果想让es性能良好，最佳情况下，你机器的内存至少可以容纳数据总量的一半。

假如一个表有30几个字段，检索字段只有3~5个，在写入到es中的时候如果把全部字段都写入,那么无疑会给filesystem cache带来额外的负担，也会很影响检索的性能。但是如果我们在写入到es中的数据只包括少数的检索字段，其他的字段信息都放到mysql/hbase中（建议架构 es+hbase)。具体作法如下：在写入的时候，先把海量数据写入到hbase中，由hbase提供简单查询操作。查询的时候可以这么做：从es中根据检索字段进行查询，然后根据查询结果的doc id,再去hbase中查询完整的数据，拼装完成后返回给前端 。
`hbase的特点是适用海量数据的在线存储`
#### 方案二
数据预热：专门搞一个缓存预热子系统，对热数据每隔一段时间，提前访问一下。让数据进入到filesystem cache中去。
#### 方案三
冷热分离：将冷数据写入一个索引中，然后热数据写入另外一个索引中，这样就可以确保热数据在预热之后，尽量都让他们留在filesystem cache里，被让冷数据给冲刷掉。
#### 方案四
document模型设计，在es里面复杂的关联查询尽量别用，一旦用了对性能一般都不太好。具体做法应该是先在java系统里完成关联，将关联好的数据直接写入es中，搜索的时候就不需要利用es的搜索语法来完成join之类的关联搜索了。
#### 方案五
分页性能优化：不允许深度分页
### es 生产集群的部署架构是什么？每个索引的数据量大概有多少？每个索引大概有多少个分片？
es 生产集群我们部署了 5 台机器，每台机器是 6 核 64G 的，集群总内存是 320G。
我们 es 集群的日增量数据大概是 2000 万条，每天日增量数据大概是 500MB，每月增量数据大概是 6 亿，15G。目前系统已经运行了几个月，现在 es 集群里数据总量大概是 100G 左右。
目前线上有 5 个索引（这个结合你们自己业务来，看看自己有哪些数据可以放 es 的），每个索引的数据量大概是 20G，所以这个数据量之内，我们每个索引分配的是 8 个 shard，比默认的 5 个 shard 多了 3 个 shard。
