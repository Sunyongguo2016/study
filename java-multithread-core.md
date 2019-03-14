### java 多线程编程核心技术 高洪岩
#### java 多线程技能
#### 对象及变量的并发访问
#### 线程间通信
#### Lock的使用
> lock 类重点使用2个类， ReentrantLock类和ReentrantReadWriteLock类； reentrantlock类中 lock 可以替代synchronized关键字，保证了线程的安全性，即原子性
lock.newCondition() 获得condition类，condition类可以保证线程通信的功能，condition类await(),signal()代替 wait(),notify(); 而且lock可以new多个condition类，通过condition可以控制
精准通知某个线程被唤醒，比wait,notify更灵活；   ReentrantReadWriteLock类可以针对读写功能区分对待，比ReentrantLock可以更细致的控制对读和写操作进行加锁，
优点是读读操作的线程间，使用读锁不互斥。增大资源利用效率 ； 另外ReentrantLock默认是非公平锁，ReentrantLock(true) 构建公平锁，可以顺序调用线程(不绝对)
#### 定时器Timer
