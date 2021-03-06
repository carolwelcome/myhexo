---
title: Http协议为什么是无状态的
date: 2019-05-16 18:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - tips 
description: 
    http协议
---

### http协议与html有什么关系？
http协议是浏览器与服务器之间数据传送的协议，是基于TCP/IP来传送数据的应用层协议，但是该协议并不涉及数据包的传输，主要规定了客户端与服务器之间的通信格式。
html是服务器端响应的结果。也就是说http协议跟其他应用层协议一样，本质上是一种通信格式，html才是通信的目标，这就好比信封跟信内容的关系。

### 如何保持请求之间的关系？
cookie:cookie本质是一份存储在用户本地的文件，里面包含了每次请求都需要传递的信息
session：session是在服务器端为用户开僻出来的一块存储空间，是cookie的升级。
这两种都是可以解决保持请求之间有关系。

### 为什么不把http协议改成有状态的？
无状态是指协议对于事务处理没有记忆功能，对同一个url请求没有上下文关系，每次的请求都是独立的，服务器中没有保存客户端的状态。HTTP协议长连接、短连接实质上是TCP协议的长连接、短连接。长连接省去了较多的TCP建立、关闭操作，减少了浪费，节约时间；短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。具体的应用场景采用具体的策略，没有十全十美的选择，只有合适的选择。
那为什么HTTP协议会被设计成无状态的呢？http最初设计成无状态的是因为只是用来浏览静态文件的，无状态协议已经足够，也没什么其他的负担。随着web的发展，它需要变得有状态，但是不是就要修改http协议使之有状态呢？是不需要的。因为我们经常长时间逗留在某一个网页，然后才进入到另一个网页，如果在这两个页面之间维持状态，代价是很高的。其次，历史让http无状态，但是现在对http提出了新的要求，按照软件领域的通常做法是，保留历史经验，在http协议上再加上一层实现我们的目的。所以引入了cookie、session等机制来实现这种有状态的连接。