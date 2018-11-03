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
 所谓原子类说简单点就是具有原子/原子操作特征的类； 一个操作一旦开始，就不会被其他线程干扰；
 并发包 java.util.concurrent 的原子类都存放在java.util.concurrent.atomic下；
#### 4.2 JUC 包中的原子类是哪4类?
 * 基本类型  如 AtomicInteger
 * 数组类型  如 AtomicIntegerArray
 * 引用类型  如 AtomicReference; 
 * 对象的属性修改类型 如 AtomicIntegerFieldUpdater;
#### 4.3 讲讲 AtomicInteger 的使用
AtomicInteger 类的使用示例
使用 AtomicInteger 之后，不用对 increment() 方法加锁也可以保证线程安全。
`class AtomicIntegerTest {
        private AtomicInteger count = new AtomicInteger();
      //使用AtomicInteger之后，不需要对该方法加锁，也可以实现线程安全。
        public void increment() {
                  count.incrementAndGet();
        }

       public int getCount() {
                return count.get();
        }
}`

#### 4.4 能不能给我简单介绍一下 AtomicInteger 类的原理
AtomicInteger 线程安全原理简单分析
AtomicInteger 类主要利用 CAS (compare and swap) + volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。
`CAS的原理是拿期望的值和原本的一个值作比较，如果相同则更新成新的值。UnSafe 类的 objectFieldOffset() 方法是一个本地方法，
这个方法是用来拿到“原来的值”的内存地址，返回值是 valueOffset。另外 value 是一个volatile变量，在内存中可见，因此 JVM 可以保证任何时刻任何线程总能拿到该变量的最新值。`

### 五 AQS
#### 5.1 AQS 介绍
AQS的全称为（AbstractQueuedSynchronizer），这个类在java.util.concurrent.locks包下面
AQS是一个用来构建锁和同步器的框架，使用AQS能简单且高效地构造出应用广泛的大量的同步器，比如我们提到的ReentrantLock，Semaphore，其他的诸如ReentrantReadWriteLock，SynchronousQueue，
FutureTask等等皆是基于AQS的。当然，我们自己也能利用AQS非常轻松容易地构造出符合我们自己需求的同步器
#### 5.2 AQS 原理分析
##### 5.2.1 AQS 原理概览
AQS核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，
那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中;CLH(Craig,Landin,and Hagersten)队列是一个虚拟的双向队列)

AQS使用一个int成员变量来表示同步状态，通过内置的FIFO队列来完成获取资源线程的排队工作。AQS使用CAS对该同步状态进行原子操作实现对其值的修改。
private volatile int state;//共享变量，使用volatile修饰保证线程可见性
##### 5.2.2 AQS 对资源的共享方式
AQS定义两种资源共享方式:
* Exclusive独占
只有一个线程能执行，如ReentrantLock; 又分为公平锁和非公平锁，公平锁按队列顺序获得资源，非公平锁抢资源
* Share共享
多个线程可同时执行
##### 5.2.3 AQS底层使用了模板方法模式
同步器的设计是基于模板方法模式的，如果需要自定义同步器一般的方式是这样（模板方法模式很经典的一个应用）：

使用者继承AbstractQueuedSynchronizer并重写指定的方法。（这些重写方法很简单，无非是对于共享资源state的获取和释放）

将AQS组合在自定义同步组件的实现中，并调用其模板方法，而这些模板方法会调用使用者重写的方法

#### 5.3 AQS 组件总结
