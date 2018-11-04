## 1. 分布式系统介绍
>  曾宪杰  2007-2013年，近6年时间在淘宝和支撑整个网站应用的java中间件打交道 -- 从设计实现消息中间件到参与数据访问层的设计，再到负责整个java中间件团队； 与2013年11月 杭州 完成；
1. 分布式系统定义：  组件分布在网络计算机上，组件之间仅仅通过消息传递来通信并协调行动； 用户面对的是一个服务器，实际上背后的众多服务器组成的
分布式系统来提供服务，分布式系统看起来就像一个超级计算机一样
冯。诺依曼计算机5个组成部分： CPU 内存 外存 输入设备 输出设备；
2. 分布式系统意义：  单机有一些限制如性能瓶颈； 传统编程中，随着单核CPU性能提升，程序变快，而随着升级多核CPU，程序的并发和并行很重要，多线程
给我们带来的好处显而易见；
3. 多线程几种模式：  互不通信的多线程模式，各自执行自己的指令； 基于共享容器协同的多线程模式，如生产者消费者模型中多个线程会共享一个容器或
数据对象；如果数据在多线程中读写比例很高，则一般会采用读写锁而非简单的互斥锁。 java.util.concurrent包有许多支持并发的容器类； 通过事件协同
的多线程模式；
4. 网络通信基础知识  在分布式系统中，组件分布在网络上的多个节点中，通过消息的传递来通信并且进行动作的协调； 我们在实践中接触比较多的网络模型
主要是以太网及TCP/IP协议栈，当我们使用socket套接字进行网络通信开发时，经常使用3种方式 BIO,NIO,AIO；
 * BIO方式： blocking IO，采用阻塞的方式实现，也就是一个socket套接字需要使用一个线程来进行处理，发生建立连接，读数据，写数据的操作时，都可能
 发生阻塞。这种方式最大的好处是简单，这样做带来的问题是使用一个线程只处理一个socket，如果是server端，在支持并发的连接时，就需要更多的线程完
 成这个工作。
 * NIO方式： Nonblocking IO, 基于事件驱动思想，采用的是reactor模式； 这是java实现的服务端系统中也是采用比较多的一种方式，相对于BIO，NIO的一个明显的好处是不需要为
 每个socket套接字分配一个线程。而是可以在一个线程中处理多个socket套接字相关的工作。reactor模式会管理所有的handler,并且把所有出现的事件交给handler处理。
 在NIO方式下，不是用单个线程去应对单个socket套接字，而是统一通过reactor对所有客户端的socket套接字的事件做处理，这样就解决了BIO中为支撑更多的
 socket套接字而需要打开更多的线程的问题。  reactor(反应堆)
 * AIO方式： 在java中，java7之后才支持aio, 以前一般使用nio，实际上aio有更好的体验；
 aio即asynchronousIO,就是异步IO， aio就是在读写操作后，调用completionHandler; 
 aio是java7中引入的，在java领域，服务端代码目前基本都是基于nIO的，而aio,nio的一个最大区别是，NIO在有通知时可以进行相关操作，例如读或者写；
 AIO在有通知时表示相关操作已经完成。
 
 在实践中，使用tcp的几率多一些，udp比较少；

 ## 2. 大型网站及其架构演进过程
 ### 2.2. 大型网站架构演进 
 `拆分思想，负载过大，拆分负载；例如数据库读写数据过大，读写分离，数据量太大，按业务分库（用户，商户，订单分库），再水平拆分(用户按某种规则分为几个库)；
 应用太重，拆分负载，按业务分为几个模块的应用，由此衍生微服务的概念`
 >> 另一个思想增大负载，如单机负载不够，来一个集群，增大负载能力；
   * 用java技术和单机构建网站
   * 单机负载告警，数据库与应用分离；
   * 应用服务器负载告警，应用服务器走向集群；（待解决负载均衡<软硬件方法>，集群后session的问题（Session Sticky和Seesion数据集中存储是首选方案；））
数据库方面改进  
   * 数据库读写分离，加缓存，加页面缓存；
   * 弥补关系型数据库的不足，引入分布式存储系统；
   * 数据库中数据内容太大，导致io承受能够依然不够； 数据库垂直拆分，水平拆分；即->分库；
数据库问题解决后，应对新的挑战，应用方面改进
   * 拆分应用，走服务化的路
   * 认识消息中间件：主要好处，异步和解耦， 消息中间件是在分布式系统中完后消息的发送和接受的基础软件；
   
 ## 3. 构建java中间件
 ### 3.1 java中间件的定义
 >> 中间件首先是一中软件，是一个组件；位于应用于操作系统之间；
 >> In its most general sense, middleware is computer software that provides services to software applications beyond those available from the operating system.
 引用维基百科 后个人总结大意为，中间件为软件应用提供了操作系统所提供的服务之外的服务，中间件不是操作系统不是应用软件，位于2者中间的位置，能够让
 软件开发者方便的处理通信，输入和输出，能够专注在他们自己应用的部分。
 中间件理解，是出于中间位置的组件，是应用与应用中间的桥梁，也是应用于服务的桥梁，特定中间件是解决特定场景问题的组价，他能够让软件开发人员专注于
 自己的开发；
  * 远程过程调用和对象访问中间件
  * 消息中间件
  * 消息访问中间件
 本文着重介绍这三个中间件，因为它们与大型网站的发展演进有非常密切的关系；
 
 ### 3.2 构建java中间件的基础知识
 #### 3.2.1 跨平台的java运行环境-JVM
 JVM是java中间件运行的基础平台； 具体的java虚拟机的产品有很多种， java诞生时的口号是" write once, run anywhere";不同平台有不同java虚拟机，但是不同java虚拟机所
 识别的是统一的中间代码，也就是我们常说的java Byte Code(java 字节码)；
 JVM是java Byte codes的一个运行环境， JVM的调优及运行时问题的定位处理也非常重要的内容，本书不对这2部分展开介绍； 提醒一句，没有一个万能的调优和问题定位解决方式。
 我们只能了解一些基本的原理和方式，然后根据每个系统的不同特点来进行不同的处理。
 83/336
#### 3.2.2 垃圾回收与内存堆布局
 使用java虚拟机不得不说就是垃圾回收，java虚拟机通过垃圾回收机制内存回收，与c++显式释放不同，因此设置不同的垃圾回收方式及参数都会影响垃圾回收的效果，而这对网站
 产生的影响就在于系统的稳定性及单机的支撑能力方面，这也是我们特别关注的部分；
 对我们来说使用较多的还是ORacle Hotspot JVM,建议读者多花时间去了解针对hotspot的垃圾回收策略，设置和调优；
 #### 3.2.3 java并发编程的类，接口和方法；
 现在是多核的时代，因此多核编程非常重要，因此基于java的并发核多线程开发非常重要；
 ##### 3.2.3.1 线程池
 线程池可以降低创建线程的开销，也就是线程执行结束后进行的是回收操作，而不是真正的线程销毁工作； 节省了创建线程和销毁线程的开销；
 在java中，我们主要使用的线程池就是threadPoolExecutor,此外还有定时的线程池ScheduledThreadPoolExecutor, 需要注意的是Executors.newCachedThreadPool()方法返回的线程池的使用，因为该
 方法返回的线程池是没有线程上限的，没法控制线程总数，假设每个线程消耗内存1m,这可能导致很多内存被占用，因此java阿里巴巴开发规约，禁止使用Executors的方法创建线程；
```java
 ThreadPoolExecutor tp = new ThreadPoolExecutor(1,1,60,TimeUnit.SECONDS,new LinkedBlockingQueue<Runnable>(count));
 tp.execute(new Runnable(){
 @Override 
 public void run(){
     //do something;
 }
 });
```
 #### syncronized
 syncronized 使用方式：
  * syncronized 修饰类的static 方法，对类加锁，获得类的锁； 互斥class / static  synchronized 方法；   public synchronized static void foo1()();
  * synchronized 修饰类的成员方法， 对类的实例枷锁， 获得new的实例的锁， 互斥 需要实例锁的 任何方法； public synchronized void fon(){};
  * synchronized 修饰代码块（这种方式最灵活）， 可以对类枷锁，对实例枷锁， 对任意对象枷锁；  
  public void foo6(){ synchronized(this`获得当前实例的锁`){//代码块 code 对任意获得当前实例的成员方法都互斥； 锁的是当前实例}}
  public void foo5(){ synchronized(SynchronizedDemo.class`获得当前类的锁`){//代码块 code 对任意获得当前类的锁都互斥， 锁的是当前类}}
 #### ReentrantLock jdk5加入的
 reentrantLock是java.util.concurrent.locks中的一个类，是从jdk5开始加入的，不过需要显式的unlock；因此如果出现异常没有unlock就出问题； 这么比较 synchronized比较安全；
 那么ReentrantLock 区别、优势？
 * 提供tryLock 方法 返回true，false；是否被他人持有，如果没有返true并持有锁；
 * 构造ReentrantLock对象的时候，有一个构造函数可以接收一个boolean类型的参数，描述锁的公平与否； 非公平锁更有效率，但是可能有的等待任务等很久或一直拿不到资源；
 ReentrantLock也提供了reentrantReadWriteLock,是读写锁，主要用于读多写少并且读不需要互斥的场景；比直接使用全部互斥锁有很大的性能提升；
 ```java
 lock.lock();
 try{
     //do something;
 }
 finally{
     lock.unlock();
 }
 ```
 #### 3.2.3.4 volatile
 91/336
 之前提到了synchronized关键字，synchronized除了互斥的作用外，还有可见性的作用，可见性是指一个线程修改了变量的值以后，其他线程能够看到这个值；
 synchronized保证了代码块的变量可见性，而volatile保证了所修饰变量的可见性，而具体使用也很简单，在变量前面增加volatile关键字就行了；volatile
 只是保证了同一个变量在多线程中的可见性，所以它是更多用于修饰作为开关状态的变量。
 
 可见性与互斥性是2个事情，不要搞混了；
 >> 与synchronized及ReentrantLock等提供的互斥相比，volatile只提供了变量的可见性支持。同一个变量线程间的可见性与多个线程中操作互斥是两件事情，操作
 >> 互斥是提供操作整体的原子性，千万不要混淆了；  volatile解决了可见性的问题，但是不能控制并发，所以对于代码块，需要用synchronized控制并发问题；
 >> synchronized保证了代码块的串行执行；
 
 #### 3.2.3.5 Atomics
 jdk5 中增加了java.util.concurrent.atomic包； 包中是以Atomic开头的类，这些类主要提供一些相关的原子操作；
 多线程，多内核，每个线程各执行自己的实例，使用自己实例下的变量；
 理解代码：
 传统count
 ```
 public class Counter1{
	 private int counter =0;
	 public int increase(){
		synchronized(this){
			counter = counter+1;
			return counter;
		}
	 }
	 public int decrease(){
		synchronized(this){
			counter =counter-1;
			return counter;
		}
	 }
 }
 ```
 使用AtomicInterger后；
 ```
 public class Counter2{
	private AtomicInteger counter = new AtomicInteger();
	public int increase(){
		return counter.incrementAndGet();
	}
	public int decrease(){
		return counter.decrementAndGet();
	}
 }
 ```
 >> 采用AtomicInteger之后，代码变简洁了，而且性能有很大提升，性能提升的主要原因是在于AtomicInteger内部通过JNI的方式使用了硬件支持的CAS指令；
 java.util.concurrent.atomic包中，有很多实用的类，可以参考相关说明；
 
 #### 3.2.3.6 wait,notify,notifyAll
 都是java的Object对象上的三个方法，wait是进行等待，notify,notifyAll是进行通知的，可以把某个对象作为事件对象，通过这个对象的wait,notify,
 notifyALL完成线程间的状态通知。 notify会唤醒一个等待线程，notifyAll唤醒所有等待线程；
 对对象的wait,notify,notifyAll的调用都必须是在对象的synchronized块中。
 在实践中，对wait的使用一般是嵌在一个循环中，并且会判断相关的数据状态是否到达预期，如果没有则会继续等待，这么做是为了防止虚假唤醒。
 
 #### 3.2.3.7 CountDownLatch
 CountDownLatch是java.util.concurrent包中的一个类， 主要机制是当多个线程（都）达到了预期状态或完成工作时触发时间，然后其他线程可以等待
 这个事件来触发自己的后续工作。
 达到预期状态的线程会调用CountDownLatch的countDown方法，而等待的线程会调用CountDownLatch的await方法，之后执行await之后的代码。
 如果CountDownLatch初始化的count值为1，那么就退化为一个单一事件了，即是由一个线程来通知其他线程，效果等同于wait和notifyAll;count值大于1
 是常用的方式，目的是让多个线程到达各自的预期状态，变为一个事件进行通知，线程则继续自己的行为；
 
 #### 3.2.3.8 CyclicBarrier
 CyclicBarrier,字面理解就是指循环屏障。 CyclicBarrier可以协同多个线程，让多个线程在这个屏障前等待，知道所有线程都到达了这个屏障时，再一起
 执行后面的动作。
 
 >> CyclicBarrier和CountDownLatch 都是用于多个线程间的协调的，二者的很大一个差别是countDownLatch是在多个线程都进行了lath.countDown后才会触发
 事件，唤醒await在latch上的线程(latch的线程可以是多个)，而执行countDown的线程，执行完countDown后会继续自己线程的工作；CyclicBarrier是一个栅栏，
 用于同步所有调用await方法的线程，并且等待线程都到了await方法时，这些线程才一起返回继续各自的工作（CyclicBarrier的线程都会阻塞在await方法上，
 所以在线程池中使用CyclicBarrier时要小心，如果线程数很少，那么就会发生死锁了）；另外，CyclicBarrier可以循环使用；
 #### 3.2.3.9 Semaphore
 semaphore是用于管理信号量的，构造的时候传入可供管理的信号量的数值，信号量对象管理的信号就像令牌，构造时传入个数，总数就是控制并发的数量。
 如果总数为1，就退化为互斥锁。主要用于控制并发数；
 我们需要控制并发的代码，执行前先获取信号（通过acquire获取信号许可），执行后归还信号（通过release归还信号许可）。
 
 举例：如果我们要控制远程方法的并发量，超过并发量的方法就等待有其他方法执行返回后再执行，那么代码大概是这样的：
 ```java
 semaphore.acquire();
 try{
	//调用通信的方法；
 }
 finally{
	semaphore.release();
 }
 ```
 acquire和release是可以有参数的额，参数的含义就是获取/返还的信号量的个数；
 #### 3.2.3.10 Exchanger
 Exchanger,从名字上理解就是交换。Exchanger用于在2个线程间进行数据交换。
 线程会阻塞在Exchanger的exchange方法上，直到另一个线程也到了同一个Exchanger的exchange方法，然后两者数据交换。。具体使用看资料
 #### 3.2.3.11 Future和FutureTask
 Future是一个接口，FutureTask是一个具体的实现类。 
 我们在执行代码时，调用某一个函数，然后执行以后的代码，这样的方式有2中，一种方法时回调，另一种是Future； 在需要用的时候再具体执行代码；
 #### 3.2.3.12 并发容器
 在jdk中，有一些线程不安全的容器，也有一些线程安全的容器，并发容器是线程安全容器的一种，但是并发容器强调的是容器的并发性，不仅保证线程安全，
 还保证容器的并发性，提升在容器并发环境下的性能。
 互斥加锁的方式确实能够方便的完成线程安全，不过代价是降低了并发性，或者说是串行了。 在并发容器中，不仅保证容器安全，还保证并发性；
 有代表性的是以CopyOnWrite和Concurrent开头的几个容器。
 ### 3.2.4 动态代理
 103/336
