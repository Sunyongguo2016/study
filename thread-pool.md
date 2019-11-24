### 线程池学习笔记

参考资料：

博客园，作者：Matrix海子， 
> 我们就来详细讲解一下Java的线程池，1.首先我们从最核心的ThreadPoolExecutor类中的方法讲起，2.然后再讲述它的实现原理，
3.接着给出了它的使用示例，4.最后讨论了一下如何合理配置线程池的大小,详细，可靠
http://www.cnblogs.com/dolphin0520/p/3932921.html

简书：
Java常用四大线程池用法以及ThreadPoolExecutor详解
> 在Java中，线程池的概念是Executor这个接口，具体实现为ThreadPoolExecutor类，学习Java中的线程池，1.就可以直接学习他了对线程池的配置，
就是对ThreadPoolExecutor构造函数的参数的配置, 篇幅较短，介绍了ThreadPoolExecutor 构造参数的含义；讲解了2.常用的四个线程池以及他们的实现原理
https://www.jianshu.com/p/ae67972d1156

博客：
线程池监控


java 线程池监控

　一、线程池监控参数

　二、线程池监控类

　三、注意事项

1. 线程池监控参数

getActiveCount()	线程池中正在执行任务的线程数量
getCompletedTaskCount()	线程池已完成的任务数量，该值小于等于taskCount
getCorePoolSize()	线程池的核心线程数量
getLargestPoolSize()	线程池曾经创建过的最大线程数量。通过这个数据可以知道线程池是否满过，也就是达到了maximumPoolSize
getMaximumPoolSize()	线程池的最大线程数量
getPoolSize()	线程池当前的线程数量
getTaskCount()	
线程池已经执行的和未执行的任务总数

通过这些方法，可以对线程池进行监控，在 ThreadPoolExecutor 类中提供了几个空方法，如 beforeExecute 方法， afterExecute 方法和 terminated 方法，可以扩展这些方法在执行前或执行后增加一些新的操作，例如统计线程池的执行任务的时间等，可以继承自 ThreadPoolExecutor 来进行扩展。

2. 线程池监控类 （原文link）

https://www.cnblogs.com/warehouse/p/10732965.html

借鉴：
1.  private static final Logger LOGGER = LoggerFactory.getLogger(ThreadPoolMonitor.class);
2. Date startDate = startTimes.remove(String.valueOf(r.hashCode())); // 在after里获取执行的时间长度
3.  poolName 参数， 设置符合业务的名称

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.Date;
import java.util.List;
import java.util.Objects;
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * 继承ThreadPoolExecutor类，覆盖了shutdown(), shutdownNow(), beforeExecute() 和 afterExecute()
 * 方法来统计线程池的执行情况
 * <p>
 * Created by on 2019/4/19.
 */
public class ThreadPoolMonitor extends ThreadPoolExecutor {

    private static final Logger LOGGER = LoggerFactory.getLogger(ThreadPoolMonitor.class);

    /**
     * 保存任务开始执行的时间，当任务结束时，用任务结束时间减去开始时间计算任务执行时间
     */
    private ConcurrentHashMap<String, Date> startTimes;

    /**
     * 线程池名称，一般以业务名称命名，方便区分
     */
    private String poolName;

    /**

     * 调用父类的构造方法，并初始化HashMap和线程池名称
     *
     * @param corePoolSize    线程池核心线程数
     * @param maximumPoolSize 线程池最大线程数
     * @param keepAliveTime   线程的最大空闲时间
     * @param unit            空闲时间的单位
     * @param workQueue       保存被提交任务的队列
     * @param poolName        线程池名称
     */
    public ThreadPoolMonitor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                             TimeUnit unit, BlockingQueue<Runnable> workQueue, String poolName) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
                Executors.defaultThreadFactory(), poolName);
    }


    /**
bug:不管是那里调用这里了，在之前需要对入参做合法性校验，先给定默认参数，然后对可以动态配置的参数做校验，如果传入的参数不合法，依然使用默认参数；
这个地方入参不合法，导致启动失败，经验！！！！
     * 调用父类的构造方法，并初始化HashMap和线程池名称
     *
     * @param corePoolSize    线程池核心线程数
     * @param maximumPoolSize 线程池最大线程数
     * @param keepAliveTime   线程的最大空闲时间
     * @param unit            空闲时间的单位
     * @param workQueue       保存被提交任务的队列
     * @param threadFactory   线程工厂
     * @param poolName        线程池名称
     */
    public ThreadPoolMonitor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                             TimeUnit unit, BlockingQueue<Runnable> workQueue,
                             ThreadFactory threadFactory, String poolName) {
        super(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory);
        this.startTimes = new ConcurrentHashMap<>();
        this.poolName = poolName;
    }

    /**
     * 线程池延迟关闭时（等待线程池里的任务都执行完毕），统计线程池情况
     */
    @Override
    public void shutdown() {
        // 统计已执行任务、正在执行任务、未执行任务数量
        LOGGER.info("{} Going to shutdown. Executed tasks: {}, Running tasks: {}, Pending tasks: {}",
                this.poolName, this.getCompletedTaskCount(), this.getActiveCount(), this.getQueue().size());
        super.shutdown();
    }

    /**
     * 线程池立即关闭时，统计线程池情况
     */
    @Override
    public List<Runnable> shutdownNow() {
        // 统计已执行任务、正在执行任务、未执行任务数量
        LOGGER.info("{} Going to immediately shutdown. Executed tasks: {}, Running tasks: {}, Pending tasks: {}",
                this.poolName, this.getCompletedTaskCount(), this.getActiveCount(), this.getQueue().size());
        return super.shutdownNow();
    }

    /**
     * 任务执行之前，记录任务开始时间
     */
    @Override
    protected void beforeExecute(Thread t, Runnable r) {
        startTimes.put(String.valueOf(r.hashCode()), new Date());
    }

    /**
     * 任务执行之后，计算任务结束时间
     */
    @Override
    protected void afterExecute(Runnable r, Throwable t) {
        Date startDate = startTimes.remove(String.valueOf(r.hashCode()));
        Date finishDate = new Date();
        long diff = finishDate.getTime() - startDate.getTime();
        // 统计任务耗时、初始线程数、核心线程数、正在执行的任务数量、
        // 已完成任务数量、任务总数、队列里缓存的任务数量、池中存在的最大线程数、
        // 最大允许的线程数、线程空闲时间、线程池是否关闭、线程池是否终止
        LOGGER.info("{}-pool-monitor: " +
                        "Duration: {} ms, PoolSize: {}, CorePoolSize: {}, Active: {}, " +
                        "Completed: {}, Task: {}, Queue: {}, LargestPoolSize: {}, " +
                        "MaximumPoolSize: {},  KeepAliveTime: {}, isShutdown: {}, isTerminated: {}",
                this.poolName,
                diff, this.getPoolSize(), this.getCorePoolSize(), this.getActiveCount(),
                this.getCompletedTaskCount(), this.getTaskCount(), this.getQueue().size(), this.getLargestPoolSize(),
                this.getMaximumPoolSize(), this.getKeepAliveTime(TimeUnit.MILLISECONDS), this.isShutdown(), this.isTerminated());
    }

    /**
     * 创建固定线程池，代码源于Executors.newFixedThreadPool方法，这里增加了poolName
     *
     * @param nThreads 线程数量
     * @param poolName 线程池名称
     * @return ExecutorService对象
     */
    public static ExecutorService newFixedThreadPool(int nThreads, String poolName) {
        return new ThreadPoolMonitor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<>(), poolName);
    }

    /**
     * 创建缓存型线程池，代码源于Executors.newCachedThreadPool方法，这里增加了poolName
     *
     * @param poolName 线程池名称
     * @return ExecutorService对象
     */
    public static ExecutorService newCachedThreadPool(String poolName) {
        return new ThreadPoolMonitor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<>(), poolName);
    }

    /**
     * 生成线程池所用的线程，只是改写了线程池默认的线程工厂，传入线程池名称，便于问题追踪
     */
    static class EventThreadFactory implements ThreadFactory {
        private static final AtomicInteger poolNumber = new AtomicInteger(1);
        private final ThreadGroup group;
        private final AtomicInteger threadNumber = new AtomicInteger(1);
        private final String namePrefix;

        /**
         * 初始化线程工厂
         *
         * @param poolName 线程池名称
         */
        EventThreadFactory(String poolName) {
            SecurityManager s = System.getSecurityManager();
            group = Objects.nonNull(s) ? s.getThreadGroup() : Thread.currentThread().getThreadGroup();
            namePrefix = poolName + "-pool-" + poolNumber.getAndIncrement() + "-thread-";
        }

        @Override
        public Thread newThread(Runnable r) {
            Thread t = new Thread(group, r, namePrefix + threadNumber.getAndIncrement(), 0);
            if (t.isDaemon())
                t.setDaemon(false);
            if (t.getPriority() != Thread.NORM_PRIORITY)
                t.setPriority(Thread.NORM_PRIORITY);
            return t;
        }
    }
}
```
3. 注意事项

1. 在 afterExecute 方法中需要注意，需要调用 ConcurrentHashMap 的 remove 方法移除并返回任务的开始时间信息，而不是调用 get 方法，因为在高并发情况下，线程池里要执行的任务很多，如果只获取值不移除的话，会使 ConcurrentHashMap 越来越大，引发内存泄漏或溢出问题。该行代码如下：

Date startDate = startTimes.remove(String.valueOf(r.hashCode()));
2. 有了ThreadPoolMonitor类之后，我们可以通过 newFixedThreadPool(int nThreads, String poolName) 和 newCachedThreadPool(String poolName) 方法创建两个日常我们使用最多的线程池，跟默认的 Executors 里的方法不同的是，这里需要传入 poolName 参数，该参数主要是用来给线程池定义一个与业务相关并有具体意义的线程池名字，方便我们排查线上问题。

3. 在生产环境中，谨慎调用 shutdown() 和 shutdownNow() 方法，因为调用这两个方法之后，线程池会被关闭，不再接收新的任务，如果有新任务提交到一个被关闭的线程池，会抛出 java.util.concurrent.RejectedExecutionException 异常。其实在使用Spring等框架来管理类的生命周期的条件下，也没有必要调用这两个方法来关闭线程池，线程池的生命周期完全由该线程池所属的Spring管理的类决定。 
