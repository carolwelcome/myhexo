---
title: ARTS-week02
date: 2019-04-02 23:48:06
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - Link
description: 
    Part Algorithm:leetcode 86- Partition List<br/>
    Part Review:Bitcoin :A Peer to Peer Electronic Cash System<br/>
    Part Tips:Oracle 12 Rac 集群启停用<br/>
    Part Share:Bitcoin<br/>
---

1、Algorithm
-------

### Algorithm-86. Partition List
``` java


/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode part=null;//保存头指针
        ListNode tail=null;
        ListNode part1= null;
        ListNode tail1=null;
        while(head!=null){
            ListNode dataNode=new ListNode(head.val);
            if(head.val<x){
                if(part==null)
                    part=tail=dataNode;
                else{
                    tail.next=dataNode;//将新节点连接到链表的尾部
                    tail=dataNode;////tail永远存储最后一个节点的地址
                }
            }else{
                
                if(part1==null)
                    part1=tail1=dataNode;
                else{
                    tail1.next=dataNode;//将新节点连接到链表的尾部
                    tail1=dataNode;////tail永远存储最后一个节点的地址
                }
            }
            head=head.next;
        }
        if(tail==null) return part1;        
        tail.next=part1;
        return part;
    }
}

//执行用时 : 1 ms, 在Partition List的Java提交中击败了84.18% 的用户
//内存消耗 : 34.8 MB, 在Partition List的Java提交中击败了0.94% 的用户
```


2、Review
-------
Bitcoin :A Peer to Peer Electronic Cash System
比特币：点对点的电子现金系统
Abstract:A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to annother without going through a financial institution .Digital Signatures provide part of the solution,but the main benefits are lost if a trusted third party is still required to prevent double-spending.We propose a solution to the double-spending problem using a peer-to-peer network.The network timestamps transactions by hashing them into an ongoing chain
of hash-based proof of work , forming a record that cannot be changed without redoing the proof-of-work.The longest chain not only serves as proof of the sequence of events witnessed ,but proof that it came from the largest pool of CPU power.As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network , they'll generate the longest chain and outpace attackers.The network itself requires minimal structure.Messages are broadcast on the best effort basis, and nodes can leave and rejoin the network at will,accepting the longest proof-of-work chain as proof of what happened while they were gone.
组词：纯粹 点对点 版本 电子现金 允许 直接发送在线支付 从一方到另一方 无需通过金融机构
造句：1、一个纯粹的点对点电子现金系统是允许不通过金融机构，来完成从一方到另一方的直接在线支付。
组词：数字签名 提供 部分解决方案 但是 最大的收益 丢掉 是否需要一个可信的第三方 阻止“双花”
造句：2、数据签名为其提供了部分的解决方案，但是最大的收益是一个可信的、能阻止“双花”的第三方可以丢掉了
组词：我们的目的 解决方案 双花问题  通过点对点的网络
造句：3、我们的目标就是通过这样一个点对点的网络来解决“双花”问题

组词：网络时间戳 交易 做哈希 基于哈希工作量证明r 不间断的链  形成不可篡改的记录
造句：4、对网络时间戳交易做哈希，并把他们链接到基于哈希工作量证明的不间断链上，以此来形成不可篡改的记录。
组词：最长的链 不仅 作用于 事件的顺序证明，而且证明了 CPU算力的最大资源池。
造句：5、最长的链不仅可以证明交易事件的先后顺序，还可以证明它是来自于最大CPU算力的资源池
组词：只要 主要的 CPU算力 被控制 节点（不合作 去攻击网络），他们会形成最长的链并超过攻击者
造句：6、只要大部分的CPU算力被这些攻击网络的节点所控制，他们就会形成最长的链并成为超级攻击者
组词：网络本身 需要 最小结构
造句：7、网络本身需要最小的结构
组词：消息 广播 尽力而为 节点 离开 重新加入 网络 随意 接收 最长工作量证明链 证据 在他们离开期间发生了什么
造句：8、消息被全网尽最大可能的传播，网络中的节点也可以随意的离开和重新加入，但这些节点得下载他们离开期间所产生的最长的链
Introduction:
Commerce on the Internet has come to rely almost exclusively on financial institutions serving as
trusted third parties to process electronic payments. 
While the system works well enough for
most transactions, it still suffers from the inherent weaknesses of the trust based model.
Completely non-reversible transactions are not really possible, since financial institutions cannot
avoid mediating disputes. 
The cost of mediation increases transaction costs, limiting the
minimum practical transaction size and cutting off the possibility for small casual transactions,
and there is a broader cost in the loss of ability to make non-reversible payments for non-reversible services. 
With the possibility of reversal, the need for trust spreads.
Merchants must be wary of their customers, hassling them for more information than they would otherwise need.
A certain percentage of fraud is accepted as unavoidable. 
These costs and payment uncertainties
can be avoided in person by using physical currency, but no mechanism exists to make payments
over a communications channel without a trusted party.
What is needed is an electronic payment system based on cryptographic proof instead of trust,
allowing any two willing parties to transact directly with each other without the need for a trusted
third party. Transactions that are computationally impractical to reverse would protect sellers
from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. In
this paper, we propose a solution to the double-spending problem using a peer-to-peer distributed
timestamp server to generate computational proof of the chronological order of transactions. The
system is secure as long as honest nodes collectively control more CPU power than any
cooperating group of attacker nodes.


组词：商业 互联网（电商？） 依赖 几乎完全 金融机构  服务 可信任的第三方 进行电子支付
造句：1、电商几乎完全依赖于可信的第三方金融机构服务来完成电子支付。
组词：虽然 系统 正常工作 对大部分交易来说就足够了 但仍然 受制于 基础信用模型固有缺陷
造句：2、只要系统能够正常运转，对于大部分交易来说足够了，但这种模式仍然受制于基础信用模型的固有缺陷
组词：完全不可逆的交易是不可能出现的 金融机构不可能避免调节纠纷
造句：3、完全不可逆的交易是真的不可能，就像金融机构不可能避免需要调节纠纷一样。
组词：调节的花费 增长 交易花费 限制 最小实现交易大小 消失小型临时交易的可能性 较大的花费 制造不可逆支付能力的丢失 不可逆服务
造句：4、调节纠纷的费用甚至超过了交易本身的花费，限制实际交易的最小大小，并切断小型临时交易的可能性。还有一个花费较大的点就是在实现对不可逆服务的不可逆支付能力的丢失上。
组词：可逆的可能性 对于信任的需要 传播开来
造句：5、对于可逆的可能性，对于信任的需要被广泛传播开来
组词：商人必须警惕他们的客户，跟他们为需要更多信息而争论
造句：6、商人必须警惕他们的客户，并跟他们为需要更多信息而进行争论

3、Tips
-------
```Bash
crsctl stop res -all
``` 
oracle 集群的停机顺序为，先停监听->实例->服务->cluster 软件或者直接用上面的命令，可以用-t 查看是否全是offline 

在linux 环境下 su user、su - user、su -user 有什么区别呢？  
使用su userx 切换到userx用户之后，不可以再使用service命令了，使用su - userx就可以使用service命令。这两者的区别是
su只是切换了用户的身份，但shell环境没有改变（还是su之前的用户），而su -则连同用户以及shell环境一起切换成了新用户的环境，只有切换了shell环境才会出现path变量错误，报command not found 可以能过pwd命令来区别。

周末部里要断电，被要求关机。一大堆的东西该怎么关呢？
1、先把业务系统给停止，再停止中间件及其他应用软件，停止数据库（12c 做的rac，就是上面那个命令的，不过需要grid用户，在使用该用户的时候发生了su user 与su - user的区别问题），所有服务都停止了之后就是关机断电了，最后一部关存储了。存储这块用的是杭州xx，竟然没有控制端，还需要用网线直连才能使用，最终竟然还需要让我把本机上的jdk都卸载了再去装他们那个7.0，完全都处理好了最后还是不行，被告知是因为机器问题。what the fuck,停是停了，不知道断电之后再重启是个什么球样子，希望周一一切都ok,验证过了的确是Ok

4、Share
-------
[bitcoin](/file/bitcoin.pdf)