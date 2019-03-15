### java 多线程编程核心技术 高洪岩
#### java 多线程技能
#### 对象及变量的并发访问
#### 线程间通信
> 线程并非孤立的，本章主要介绍wait() notify() notifyall() 的使用，使线程间相互通信合作完成任务。 以及ThreadLocal类的使用 ，方法join使用，生产者消费者模式的实现。

> 等待通知机制，举个例子，厨师和服务员之间的交互在‘菜品传递台’上，服务员取菜时间取决于厨师，所以wait() ;厨师将菜品放到传递台，是一种notify(); 在此之前线程可以通过读取相同变量也实现通信，但是这不是‘等待/通知’机制，线程完全是主动读取变量，而且不一定是想要的结果，这里介绍的线程通信是wair/notify 通知等待机制，可以看做是一种被动式的通信机制。

#### Lock的使用
> lock 类重点使用2个类， ReentrantLock类和ReentrantReadWriteLock类； reentrantlock类中 lock 可以替代synchronized关键字，保证了线程的安全性，即原子性,另外ReentrantLock默认是非公平锁，ReentrantLock(true) 构建公平锁，可以顺序调用线程(不绝对)

> lock.newCondition() 获得condition类，condition类可以保证线程通信的功能，condition类await(),signal()代替 wait(),notify(); 而且lock可以new多个condition类，通过condition可以控制
精准通知某个线程被唤醒，比wait,notify更灵活；  

> ReentrantReadWriteLock类可以针对读写功能区分对待，比ReentrantLock可以更细致的控制对读和写操作进行加锁，
优点是读读操作的线程间，使用读锁不互斥。增大资源利用效率 ； 
#### 定时器Timer
