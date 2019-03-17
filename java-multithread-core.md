## java 多线程编程核心技术 高洪岩
### 1. java 
> 介绍线程，如何使用线程启动线程， currentThread()方法 isAlive(), sleep(), getId()方法等，停止线程推荐方法：异常法；  暂停线程 suspend()与resume()方法使用；

> yield（）停一小会线程， 线程的优先级学习，用户线程和守护线程了解。
### 2. 对象及变量的并发访问
> 主要介绍了synchronized关键字使用，为学习同步打下基础； synchronized修饰的代码块具有原子性； synchronized可以修饰3种数据，对象，类.class文件，以及String常量如"123"；
synchronized在分析代码的要点是分析是否要获得对象锁，然后就可以理解代码的执行顺序了。 synchronized锁是可重入的锁。 当出现异常时，锁会自动释放；

> 对对象加锁synchronized,则其他方法持有锁的方法都会同步，因为要获取同一个锁对象；（互斥），代码块也同理；

> 使用非this对象锁有独特的优势，由于对象实例锁的所有方法会互斥形成同步，互相影响，所以使用非this锁更灵活。

> 多线程等待一个不会释放的锁，就会形成死锁，一般这种情况会出现在多个线程互相等待对方释放自己想要的锁，然后形成死锁的。

> valitile修饰的变量具有可见性（不具有原子性），可以在多个线程间实现实时可见的特性； volatile的特性是强制从公共堆栈中取得变量的值，而不是从线程私有数据栈中取变量的值。
（这里要了解公共堆栈及线程的私有堆栈）。 volatile关键字强制变量直接写入公共堆栈(主内存)，直接读取主内存； 跳过线程的工作内存（私有堆栈）；

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
> 1.如何实现指定时间执行任务 2.如何实现指定周期执行任务

##### 5.1 定时器timer使用

1. Timer类的主要作用是设置计划任务， 封装任务的类是TimerTask类， TimerTask实现runnable接口，将执行任务的代码放到timerTask类run方法中；

2. timer.schedule(timertask task, date time); 任务延迟time执行

3. Timer timer = new Timer(true); //守护线程，随着主线程结束结束； 否则主线程结束后自己仍然存在；

4. 一个timer 可以执行多个timer.schedule()，需要注意的是，timer.schedule()执行的多个任务是以队列的形式执行的，所以有可能和设置好的延迟data时间不一致；

5. timer.schedule(TimerTask task,Date firstTime,long period); 设置任务task在指定的日期之后按照指定的时间间隔周期执行某一任务；

6. TimerTask的cancel()方法的作用是将自身从任务队列中进行清除。

7. timer.cancel() 方法的作用是将自己关联的所有任务task清除，并且销毁自身的进程； 有的时候不会执行成功，是因为和task竞争queue锁失败，然后让TimerTask类中的任务正常执行；

8. timer.schedule() 和 timer.scheduleAtFixedRate() 区别， 后者具有追赶特性； 即后者可以把过时的任务都执行一次； 而前者之后执行当前时间之后的任务，对于过时的任务不管；

### 6. 单例模式与多线程
### 7. 查漏补缺
