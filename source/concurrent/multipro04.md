---
title: 并发编程-04
date: 2020-03-15 08:47:42
donate: false
decription: 
	一年了才开始学习，这次要认真学习完。
---

### Lock的基本使用及原理分析

#### JUC简介
java.unit.concurrent;并发工具包



#### Lock的基本使用

synchronized锁

Lock可以灵活处理

Lock是一个接口，juc里面有不同的实现，ReentrantLock唯一一个实现lock接口的类。

#### ReentrantLock重入（互斥）锁
可以重新进入的锁。
synchronized和ReentrantLock都支持重入
ReentrantLock是一个重入互斥锁，避免死锁的问题
```java
//基本使用：	
public class ReentrantLockDemo {
	
    static ReentrantLock lock=new ReentrantLock();//默认是false，即非公平锁，new ReentrantLock(true)时为公平锁

    public static void main(String[] args) {
        new Thread(()->test(),"t1").start();
        new Thread(()->test(),"t2").start();
        new Thread(()->test(),"t3").start();
        new Thread(()->test(),"t4").start();
    }

    public static void test(){
        for (int i = 0; i <2 ; i++) {
        try{

                lock.lock();
                System.out.println(Thread.currentThread().getName()+",获得了锁");
                TimeUnit.SECONDS.sleep(2);

        }catch (InterruptedException e){
           e.printStackTrace();
        }  finally{
            lock.unlock();
        }
        }
    }
}

==============
非公平锁返回：
t1,获得了锁
t1,获得了锁
t2,获得了锁
t2,获得了锁
t4,获得了锁
t4,获得了锁
t3,获得了锁
t3,获得了锁

公平锁返回：
t1,获得了锁
t2,获得了锁
t3,获得了锁
t4,获得了锁

t1,获得了锁
t2,获得了锁
t3,获得了锁
t4,获得了锁
//公平锁：不允许插队，谁等待时间长就先执行谁，非公平锁随机排序，谁运气好算谁的。
```

ps:
* 响应中断就是一个线程获取不到锁，不会傻傻的一直等下去，会给予一个中断回应
* 通过我们的tryLock方法来实现，可以选择传入时间参数，表示等待指定的时间，无参则表示立即返回锁申请的结果：true表示获取锁成功，false表示获取锁失败。我们可以将这种方法用来解决死锁问题

```java
public class ReentrantLockDemo1 {

    static Lock lock1=new ReentrantLock();
    static Lock lock2=new ReentrantLock();

    public static void main(String[] args) {
        Thread t1=new Thread(new ThreadDemo(lock1,lock2));
        Thread t2=new Thread(new ThreadDemo(lock2,lock1));
        t1.start();
        t2.start();

    }

    static class ThreadDemo implements Runnable{
        Lock first;
        Lock second;

        public ThreadDemo(Lock lock1,Lock lock2){
            this.first=lock1;
            this.second=lock2;
        }
        @Override
        public void run() {
            try {
                while(!lock1.tryLock()){
                    TimeUnit.MILLISECONDS.sleep(10);
                }
                while(!lock2.tryLock()){
                    lock1.unlock();
                    TimeUnit.MILLISECONDS.sleep(10);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                first.unlock();
                second.unlock();
                System.out.println(Thread.currentThread().getName()+"正常结束!");
            }
        }
    }
}
```

Condition的使用
使用synchronized结合Object上的wait和notify方法可以实现线程间的等待通知机制。ReentrantLock结合Condition接口同样可以实现这个功能

```java
public class ConditionTest {
    static ReentrantLock lock = new ReentrantLock();
    static Condition condition = lock.newCondition();
    public static void main(String[] args) throws InterruptedException {

        lock.lock();
        new Thread(new SignalThread()).start();
        System.out.println("主线程等待通知");
        try {
            condition.await();
        } finally {
            lock.unlock();
        }
        System.out.println("主线程恢复运行");
    }
    static class SignalThread implements Runnable {

        @Override
        public void run() {
            lock.lock();
            try {
                condition.signal();
                System.out.println("子线程通知");
            } finally {
                lock.unlock();
            }
        }
    }
}
```

基于ReentrantLock实现阻塞队列 

```java
public class MyBlockingQueue<E> {

    int size;//阻塞队列最大容量

    ReentrantLock lock = new ReentrantLock();

    LinkedList<E> list=new LinkedList<>();//队列底层实现

    Condition notFull = lock.newCondition();//队列满时的等待条件
    Condition notEmpty = lock.newCondition();//队列空时的等待条件

    public MyBlockingQueue(int size) {
        this.size = size;
    }

    public void enqueue(E e) throws InterruptedException {
        lock.lock();
        try {
            while (list.size() ==size)//队列已满,在notFull条件上等待
                notFull.await();
            list.add(e);//入队:加入链表末尾
            System.out.println("入队：" +e);
            notEmpty.signal(); //通知在notEmpty条件上等待的线程
        } finally {
            lock.unlock();
        }
    }

    public E dequeue() throws InterruptedException {
        E e;
        lock.lock();
        try {
            while (list.size() == 0)//队列为空,在notEmpty条件上等待
                notEmpty.await();
            e = list.removeFirst();//出队:移除链表首元素
            System.out.println("出队："+e);
            notFull.signal();//通知在notFull条件上等待的线程
            return e;
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {

        MyBlockingQueue<Integer> queue = new MyBlockingQueue<>(2);
        for (int i = 0; i < 10; i++) {
            int data = i;
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        queue.enqueue(data);
                    } catch (InterruptedException e) {

                    }
                }
            }).start();

        }
        for(int i=0;i<10;i++){
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        Integer data = queue.dequeue();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }

    }
}
```

总结：ReentrantLock是可重入的独占锁。比起synchronized功能更加丰富，支持公平锁实现，支持中断响应以及限时等待等等。可以配合一个或多个Condition条件方便的实现等待通知机制
#### AQS基本实现

同步器

双向链表，
锁的基本要素
1. 一个共享的数据来记录锁的状态


