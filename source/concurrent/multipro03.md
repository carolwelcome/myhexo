---
title: 并发编程-03
date: 2020-03-13 08:47:42
donate: false
decription: 
	一年了才开始学习，这次要认真学习完。
---

### volatile

保证可见性

#### 如何保证可见性
hsdis
多了一个lock的汇编指令

1. 硬件层面
2. JMM层面

### 高速缓存：
L1,L2,L3。

会产生缓存不一致问题。解决方案：总线锁、缓存锁（降低锁的粒度）

#### MESI模型

缓存一致性协议，MESI模型（缓存行的四种状态：M-Modify  E-Exclusive S-Share I-Invalid)

主内存与缓存数据一致：share，缓存发生修改，S->M,其它缓存由S->I



MESI基于CPU硬件层面的实现。会引起CPU层面上阻塞，由此引入storebuffer

CPU的乱序执行->重排序->可见性问题

CPU层面提供了指令->内存屏障

内存屏障解决可见性问题

CPU提供了三种屏障：写屏障（写屏障之前所有已经存在storebuffer中的指令要同步的主内存)、读屏障（失效队列,读屏障之后所有指令一定可以读到最新数据）、全屏障（写屏障+读屏障配合使用就是全屏障，确保屏障前的内存读写操作对于后续的内存操作是可见的）

store barrier---写屏障
load barrier----读屏障
full barrier----全屏障




### JMM
可见性的根本问题：高速缓存、重排序

JMM就是提供了禁用缓存和禁用重排序的方法，去解决可见性问题

最核心的价值就解决了可见性和有序性。（内存屏障）

语言级别抽象 内存模型
volatile synchronized final happens-before规则


### Happens-Before 规则
正确的理解 ：前面一个操作的结果对后续操作是可见的

1. 程序的顺序性规则：在一个线程中，按照程序顺序前面的操作Happens-Before于后续的任意操作
2. volatile变量规则：对一个volatile变量的修改，Happens-Before于后续任何对于这个变量的读操作
3. 传递性：如果AHappens-Before于B,BHappens-Before于C,那个AHappens-Before于C
4. 管程中的锁：对一个锁的解锁操作Happens-Before于后续所有对这个锁的加锁
5. 线程的start()规则 ：主线程A启动子线程B后，子线程B能够看到主线程在启动子线程B前的操作
6. 线程的join()规则：主线程A等待子线程B完成（主线程 A 通过调用子线程 B 的 join() 方法实现），当子线程 B 完成后（主线程 A 中 join() 方法返回），主线程能够看到子线程的操作。
7. 线程中断规则：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测到是否有中断发生。
8. 对象终结规则：一个对象的初始化完成(构造函数执行结束)先行发生于它的finalize()方法的开始。

ps：管程是一种通用的同步原语，在 Java 中指的就是 synchronized，synchronized 是 Java 里对管程的实现


```java

	public static void main(String[] args){
		Thread t1=new Thread(()->{
			System.out.println("t1");
		});
		Thread t2=new Thread(()->{
			System.out.println("t2");
		});
		Thread t3=new Thread(()->{
			System.out.println("t3");
		});
		
		t1.start();
		t2.start();
		t3.start();
		//三个线程的执行顺序不一致，如果想按顺序执行该怎么办呢？
		//加join();//join的实现是wait()
		//顺序执行的话，就失去了线程的意义，只是子线程的执行结果对主线程可见
	}


```

