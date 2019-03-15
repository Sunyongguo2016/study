## java 多线程编程核心技术 高洪岩
### 1. java 多线程技能
### 2. 对象及变量的并发访问
### 3. 线程间通信
> 线程并非孤立的，本章主要介绍wait() notify() notifyall() 的使用，使线程间相互通信合作完成任务。 以及ThreadLocal类的使用 ，方法join使用，生产者消费者模式的实现。

> 等待通知机制，举个例子，厨师和服务员之间的交互在‘菜品传递台’上，服务员取菜时间取决于厨师，所以wait() ;厨师将菜品放到传递台，是一种notify(); 在此之前线程可以通过读取相同变量也实现通信，但是这不是‘等待/通知’机制，线程完全是主动读取变量，而且不一定是想要的结果，这里介绍的线程通信是wair/notify 通知等待机制，可以看做是一种被动式的通信机制。 wait() notify()重点在于理解‘通知等待机制’;

> wait 线程进入等待，并且释放锁，notify() 唤醒等待相同锁的等待线程，notify执行完 synchornized块代码才释放锁。notifyAll()唤醒所有等待线程。 wait,notify 都需要先获得锁， 相同锁的wait,notify才会被唤醒

> 生产者消费者模式，不容易理解

> join 使所属线程对象x正常执行run()方法中的任务，使当前线程z无限期的阻塞，等待线程x销毁后再执行线程z后面的代码； join具有使线程排队运行的作用，类似同步的效果。join与synchronized区别： join在内部使用wait()方法进行等待，synchronized关键字使用‘对象监听器’原理做同步。join(long)内部调用wait(long),wait(long)执行后当前线程会释放锁，Thread.sleep(long)不释放锁。

> threadlocal类使用，threadlocal类主要是每个线程绑定了自己的值， public static ThreadLocal t1; t1.set(); t1.get();

> 使用InheritableThreadLocal类可以在子线程中取得父线程继承下来的值

### 4. Lock的使用
> lock 类重点使用2个类， ReentrantLock类和ReentrantReadWriteLock类； reentrantlock类中 lock 可以替代synchronized关键字，保证了线程的安全性，即原子性,另外ReentrantLock默认是非公平锁，ReentrantLock(true) 构建公平锁，可以顺序调用线程(不绝对)

> lock.newCondition() 获得condition类，condition类可以保证线程通信的功能，condition类await(),signal()代替 wait(),notify(); 而且lock可以new多个condition类，通过condition可以控制
精准通知某个线程被唤醒，比wait,notify更灵活；  

> ReentrantReadWriteLock类可以针对读写功能区分对待，比ReentrantLock可以更细致的控制对读和写操作进行加锁，
优点是读读操作的线程间，使用读锁不互斥。增大资源利用效率 ； 
### 5. 定时器Timer
### 6. 单例模式与多线程
### 7. 查漏补缺
