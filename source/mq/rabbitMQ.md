---
title: RabbitMQ
date: 2020-03-29 23:13:51
type: mq
comments: true
categories:  #分类
    - MQ
tags:   #标签
    - MQ
description: 
    选择适合自己的消息队列，让业务体验飞起来的感觉
---

### RabbitMQ

#### 典型应用场景

1. 跨系统的异步通信
2. 应用内的同步变异步
3. 基于Pub/Sub模型实现的事件驱动
4. 利用RabbitMQ实现事务的最终一致性

#### 基本介绍

AMQP协议：Advanced Message Queuing Protocol,一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准为面向消息的中间件设计。`基于此协议的客户端与消息中间件可传递消息` ，并不受客户端/中间件产品，不同的开发语言等条件的限制。
AMQP的实现 有：RabbitMQ、OpenAMQ、等

#### RabbitMQ的特性

* 可靠性
* 灵活路由
* 消息集群
* 高可用
* 多种协议 
* 多种语言客户端
* 管理界面
* 插件机制

#### 工作模型

Producer

Broker
	Virtual Host
		Exchange
			Queue
			Queue
	Virtual Host
		Exchange
			Queue
			Queue
			Queue
Connection
	Channel
	
Consumer


#### 三种交换机模型

Headers模式的交换机
Direct Exchange直接交换机：按key进行绑定（路由关键字==绑定关键字）
```java
basicPublish("Direct_EXCHANGE","Key3","消息内容")
```
Topic Exchange主题交换机：
*代表一个单词，#代表0个或多个单词.分隔
```java
basicPublish("TOPIC_EXCHANGE","bj.#","消息内容")
```
Fanout Exchange广播交换机：不需要绑定

#### 进阶知识 
##### x-message-ttl 消息存活时间

##### DLQ dead letter Queue死信队列
DLX Dead Letter Exchange 
哪些情况下消息会变成死信
1. 消息过期
2. 消息被拒绝
3. 队列达到最长度，先入队的消息会被删除

变成死信之后会去哪？
DLX_EXCHANGE 死信队列

##### 优先级队列

可以设置消息的优先级

##### 延迟队列

延迟队列插件
1、过期时间TTL+死信队列DLX 实现延迟队列

#### RPC实现
使用两个队列来实现的，关键点是correlationId,来保证server接收到消息之后顺利返回给客户端

#### 可靠性投递分析
1. 确保省发送到RabbitMQ服务器
2. 确保路由到正确的队列
3. 确保在队列正确的存储
4. 确保消息 从队列正确的投递到消费者
5. 其他

##### 确保发送到RabbitMQ服务器

* Transaction模式:不建议在生产环境使用效率比较低,100%保证消息提供者发送消息到broker,是阻塞的方式

```java

channel.txSelect();//将channel设置成事务模式 
channel.txCommit();//提交事务
channel.txRollback();//回滚事务

```

* 确认模式Confirm模式，可批量

```Java
channel.confirmSelect();//将channel设置成confirm协议
channel.waitForConfirms();

```

##### 路由保证

1. ReturnListener

2. 备份交换机

##### 消息存储

1. 队列持久化
durable=true
2. 交换机持久化
durable=true
3. 消息持久化
设置deliveryMode=2


##### 消费者确认

channel.basicAck();//手工应答
channel.basicReject();//单条拒绝
channel.basicNack();//批量拒绝

##### 消息什么时候从队列删除？

1.  在ack=true的情况下是发送给消费者之后
2.  在ack=false的情况下可以等待业务消费完之后

#### 其他

1. 消费者回调：
2. 补偿机制
3. 消息幂等性
4. 消息顺序性

#### 高开用架构

集群:支持局域网部署
镜像队列
HA方案

##### 实践经验总结

1. 配置文件和命名规范（元数据放到资源文件中）
2. 调用封装
3. 信息落库（消息的可追溯、可重发）+定时任务（效率降低、占用磁盘空间）
4. 如何减少连接数
5. 生产者先发送消息还是先登记业务表？
6. 谁来创建对象（交换机、队列、绑定关系）----消费者
7. 运维监控
8. 其他插件


##### 面试题
1. 消息队列的作用与使用场景
* 思路--》流程：异步（批量数据异步处理如批量上传文件）、削峰（高负载任务负载均衡，电商秒杀）、解耦（串行任务并行化：退货流程解耦）、广播（基于发布/订阅模型实现一对多通信）
2. 创建队列和交换机的方法

3. 多个消费者监听一个生产者时，消息如何分发
* 轮询（round-robin)
* 公平分发（fair dispatch）
4. 无法被路由的消息去了哪里
* 没有任何配置，这条消息肯定是丢了解决方案如下 ：
* 使用mandatory=true配合ReturnListener 实现消息回发
* 声明交换机时，指定备份交换机
5. 消息在什么时候变成死信
* 队列消息到达死信队列时才被称为死信
* 过期
* 拒绝
* 队列达到上限
6. 如何实现延迟队列
* 过期时间TTL+死信队列
* 插件
7. 如何保证消息的可靠性投递

* 生产者发送消息到broker(事务，confirm)
* 保证消息从交换机路由到正确的队列见(4)
* 消息正确的存储(对交换机队列和消息做持久化)
* 消息从队列发送到消费者（开启手工应答，补偿机制）

8. 消息幂等性
消息唯一ID(为每条消息产一个唯一的ID)
9. 如何在服务端和消费端做限流
* 服务端（内存和磁盘）40% 1G
* 质量服务保证Qos，设置prefetchCount
* 网关接入层限制
10. 如何保证消息的顺序性
一个队列一个消费者可以保证顺序性
全局ID,messageID parentMessageID
11. 集群模式和集群节点类型
* 普通模式(消息不同步)
* 镜像模式

DISC-磁盘
RAM-内存