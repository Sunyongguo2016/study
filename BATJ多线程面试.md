BATJ 都爱问的多线程面试题；from--SnailClimb
link: https://mp.weixin.qq.com/s?__biz=MzU4NDQ4MzU5OA==&mid=2247484564&idx=1&sn=d8467fdc5c1b3883e9b99485f7b0fb9a&chksm=fd9852f5caefdbe364d1c438865cff84acd8f40c1c9e2f9f5c8fef673b30f905b4c5f5255368&mpshare=1&scene=2&srcid=1102ioZVRdd9tFSx5ACirwwq&from=timeline#rd
### 1. 面试中关于synchronied关键字的5连击
#### 1.1 说说自己对synchronized关键字的了解
synchronized 关键字解决的是多个线程之间访问资源的同步性，synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。
早期的synchronized属于重量级锁，jdk1.6对锁的实现进行较大优化，效率优化有很大提升
#### 1.2 说说你是怎么使用synchronized的，项目中遇到了吗
 * 修饰实例方法，作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁
 * 修饰静态方法，作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁
 * 修饰代码块，指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。 
 尽量不要使用 synchronized(String a) 因为JVM中，字符串常量池具有缓冲功能； 面试到这一步 ，面试官通常会说，写一个单例模式，解释一下双重检验
 锁方式实现单例模式的原理。
#### 1.3 讲一下 synchronized 关键字的底层原理
synchronized 关键字底层原理属于 JVM 层面;
synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置
synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法，JVM 通过该 ACC_SYNCHRONIZED 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。
#### 1.4 说说 JDK1.6 之后的synchronized 关键字底层做了哪些优化，可以详细介绍一下这些优化吗
#### 1.5 谈谈 synchronized和ReenTrantLock 的区别
 * 两者都是可重入锁
 * synchronized以来jvm, 而reenTrantLock依赖于API，reenTrantLock有一些更高级的功能，一般工作使用synchronized就可以了
 * 性能已不是选择指标
### 二 面试中关于线程池的 2 连击
#### 2.1 讲一下Java内存模型
#### 2.2 说说 synchronized 关键字和 volatile 关键字的区别
 * volatile关键字是线程同步的轻量级实现，所以volatile性能肯定比synchronized关键字要好。但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块
 * volatile关键字能保证数据的可见性，但不能保证数据的原子性。synchronized关键字两者都能保证
 * volatile关键字主要用于解决变量在多个线程之间的可见性，而 synchronized关键字解决的是多个线程之间访问资源的同步性。
### 三 面试中关于 线程池的 4 连击
#### 3.1 为什么要用线程池？
 * 《java并发编程的艺术》， 降低资源消耗，提升相应速度，便于管理线程的状况（单独零散的线程不方便统一监控调优）
#### 3.2 实现Runnable接口和Callable接口的区别
 Runnable接口或Callable接口实现类都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行。两者的区别在于 Runnable 接口不会返回结果但是 Callable 接口可以返回结果
#### 3.3 执行execute()方法和submit()方法的区别是什么呢？
 * execute() 方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
 * submit()方法用于提交需要返回值的任务。线程池会返回一个future类型的对象，通过这个future对象可以判断任务是否执行成功；future.get()阻塞当前线程知道任务完成，future.get(long timeout,TimeUnit unit)阻塞当前线程一段时间后立即返回，此时可能任务没有执行完
#### 3.4 如何创建线程池
《阿里巴巴Java开发手册》中强制线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式
 * 1.通过构造方法创建
### 四 面试中关于 Atomic 原子类的 4 连击
#### 4.1 介绍一下Atomic 原子类
#### 4.2 JUC 包中的原子类是哪4类?
#### 4.3 讲讲 AtomicInteger 的使用
#### 4.4 能不能给我简单介绍一下 AtomicInteger 类的原理
### 五 AQS
#### 5.1 AQS 介绍
#### 5.2 AQS 原理分析
##### 5.2.1 AQS 原理概览
##### 5.2.2 AQS 对资源的共享方式
##### 5.2.3 AQS底层使用了模板方法模式
#### 5.3 AQS 组件总结
