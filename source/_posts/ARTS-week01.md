---
title: ARTS-week01
date: 2019-04-02 11:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Part Algorithm:leetcode 27- Remove Element，206-Reverse Linked List<br/>
    Part Review:Dapper，a Large Scale Distributed Systems Tracing Infrastructure<br/>
    Part Tips:MySql Version<br/>
    Part Share:Dapper<br/>
---

1、Algorithm
-------

/**
Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.
还有这么奇葩的题？
正常的解法
执行用时 : 8 ms, 在Remove Element的Java提交中击败了71.39% 的用户
内存消耗 : 37.6 MB, 在Remove Element的Java提交中击败了0.97% 的用户

*/

### Algorithm-27. Remove Element
``` java


class Solution {
    public int removeElement(int[] nums, int val) {
        int k=0;
        for(int i=0;i<nums.length;i++){
            if(val!=nums[i]){
                nums[k]=nums[i];
                k++;
            }
        }
        return k;
    }
}
```

### Algorithm-206. Reverse Linked List
``` java

class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode reverse=null;//最终返回
        ListNode nodeData=null;//临时节点
        while(head!=null){
            nodeData=new ListNode(head.val);
            if(reverse==null){
                reverse=nodeData;
            }else{
                nodeData.next=reverse;
                reverse=nodeData;
            }
           head=head.next;
        }
        return reverse;
    }
    public ListNode reverseList01(ListNode head) {
        ListNode reverse=null;//最终返回
        ListNode nodeData=null;//临时节点
        while(head!=null){
            nodeData=head.next;
            head.next=reverse;
            reverse=head;
            head=nodeData;
        }
        return reverse;
    }
    //说实话递归这个还是没搞太懂，一周以后再看看
    public ListNode reverseList02(ListNode head) {
        if(head==null||head.next==null) return head;
        ListNode reverse=reverseList02(head.next);
        head.next.next=head;
        head.next=null;
        return reverse;
    }
}

```

2、Review
-------
From Dapper, a Large Scale Distributed Systems Tracing Infrastructure
* three  design goals of Dapper:
    a.low overhead:a sample of just one out of thousands of requests provides sufficient information for many common uses of the tracing data.
    b.Application_level transparency: Dapper is restricted to a low enough level in the software stack that even largescale distributed systems like Google web search could be traced without additional annotations.
    c.Scalability: handle the size of Google’s services and clusters for at least the next few years
* other similiar tracing systems:
    a.Magpie
    b.X-Trace
    c.Pinpoint	
* a tree of nested RPCs
    model traces using trees,spans annotation.
    span id 
    parent span id

3、Tips
-------
关于MySql的版本问题
* 查看Mysql当前的版本
	已登录:
	``` bash	
		a.select @@version
		b.select version()
	```
	未登录:
	``` bash	
		a.mysql -V(大写字母)
		b.mysql --version(两个横线)
	```
* 各自版本
	5.0:2005年添加了存储过程，服务端游标，触发器，查询优化以及分布式事务功能
	5.1:2008年发布，增加一个崩溃恢复功能的MyISAM,使用表级锁，可以做到读写不冲突
	5.5:2010年发布，默认存储引擎改为InnoDB,有多个回滚段
	5.6:2013年发布，InnoDB可以限制大体量表打开的时候占用内存过多的问题，InnoDB性能加强
	5.7:2015年发布，查询性能大幅提升，比5.6提升1倍降低了建立数据库连接的时间
	8.0:对Mysql源码进行了重构，对Mysql Optimizer优化器进行改进，支持隐藏索引，基本上和MyISAM说再见了
	其他：这个tips也太水了吧，就在挖个坑吧。这块儿内容我会以专栏的形式补齐。
* 补丁
	Oracle Mysql被扫描出来的安全漏洞也是花样繁多，主要原因还是不同的版本问题。今天就打算修复一下测试服务器上的Mysql漏洞。官网上找了半天还是没有找到对应的补丁在哪儿下载（这估计能成为我想学习英文的主要原因），不过笨人有笨的办法，那就找不用打该补丁的版本喽
	MySQL Client, versions 5.5.60 and prior, 5.6.40 and prior, 5.7.22 and prior, 8.0.11 and prior
	于是就重新下了一个5.5.62。
* 安装Mysql，Start Service未响应
	 这个问题真的是太有意思了，一开始还以为是服务器的问题，好几次重试之后发现，这就是已经卸载没有清理干净的问题了。以Windows为例，默认会在C盘下隐藏目录ProgramData中存在已经安装的版本，直接干掉他，再来一遍over!!

4、Share
-------
[dapper](/file/dapper.pdf)