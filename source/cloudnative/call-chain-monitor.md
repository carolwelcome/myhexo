---
title: 云原生-调用链监管技术选型
date: 2019-08-28 13:26:51
categories:  #分类
    - 云原生
tags:   #标签
    - 云原生
    - 调用链
description: 
    云原生技术体系
---
### 为什么需要调用链监控
也是生产就绪的重要环节,微服务系统由许多服务组成，没有有效的监控手段出现问题就很难快速排查。tracing监控是比较有较的手段，

### 比较流行的调用链中间件
CAT、Zipkin、Skywalking是比较主流的开源调用链监控产品

1.CAL：由eBay于2002年研发,未开源
2.Dapper：论文由Google于2010年发表，现行很多调用链产品都是基于这篇论文来实现的
3.CAT：由大众点评于2011年开源，在设计和实现上借鉴了CAL
4.Zipkin：由twitter于2012年开源，参考Dapper论文思路实现
5.Pinpiont：于2012年由韩国人开源，使用字节码注入这种无侵入的实现
6.Skywalking：由apache背书的顶级项目于2015年开源，借鉴了Pinpiont
7.Jaeger：于2016年由Uber开源，借鉴Zipkin思路研发了GoLang版，是推荐的云原生推荐产品


### CAT、Zipkin、Skywalking比较
 compare| CAT | Zipkin | Skywalking 
- | :-: | :-: | :-: 
调用链可视化 | 有 | 有 | 有 | 
聚合报表 | 非常丰富| 少 | 较丰富 |
服务依赖图 | 简单| 简单 | 好 |
埋点方式 | 侵入| 侵入 | 非侵入，运行期字节码增强 |
VM指标监控 | 好| 无 | 有 |
告警支持 | 有| 无 | 有 |
多语言支持 | Java/.Net| 丰富 | Java/.Net/NodeJS/PHP自动 Go手动 |
存储机制 | Mysql(报表),本地文件，HDFS调用链| 可选in memory,Mysql,ES(生产) ,Cassandra(生产) | H2,ES(生产) |
社区支持 | 主要在国内，点评/美团| 文档丰富，国外主流 | apache支持，国内社区好 |
国内案例 | 点评、携程、陆金所| 京东、阿里定制不开源 | 华为、小米、当当 |
APM | yes| no | yes |
祖先源头 | eBay CAL| Dapper | Dapper |
同类产品 | 暂无 | Jaeger\Sleuth | Pinpoint |
GitHub | 9.6k| 11.2k | 9.2k |
亮点 | 企业生产级，报表丰富| 社区生态好 | 非侵入，Apache背书  |
不足 | 用户体验一般，社区一般| APM报表能力弱 | 时间不长，文档一般，仅限于中文社区  |