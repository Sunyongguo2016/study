  1. 线程池的使用， 采用阿里巴巴java规范推荐的方式，
  2.  线程池参数怎么写； 
  3. 线程池 线程执行过程，继承了ThreadPoolExecutor类， 在beforeExecute() afterExecute() 可以怎么做； 做日志，收集数据；
  4. 下面是使用中遇到4篇好的文章


  [ThreadPoolExecutor-1-掘金](https://juejin.im/post/5b25b3d0f265da597d0a9803)
  
  [ThreadPoolExecutor-2-csdn](https://blog.csdn.net/qq_25806863/article/details/71126867)
  
  [ThreadPoolExecutor-3-简书](https://www.jianshu.com/p/b42cdf3f9af8)
  
  [ThreadPoolExecutor-4-cnblog](https://www.cnblogs.com/zedosu/p/6665306.html)
  
  有基础后详读&加深记忆细节：
  [线程池详解-java团长-林冠宏](https://mp.weixin.qq.com/s?__biz=MzIwMTY0NDU3Nw==&mid=2651939964&idx=2&sn=65f86227d4aad75a96afa4111d9492dc&chksm=8d0f0b32ba788224f6b7a68279ad047993d41af8401e04f2bd70054d0b93a77ded5481fff83c&scene=0&xtrack=1&key=a43ea466d03028f9409b7ea08105d0f6527670ee4f04ce0abece88eef1c04f7fbe6adcada8cb282ade7e6fe29f29a1a7e8401159e6e58bef1fb632010c1bd6d9a13e37a0d583a9941b0f48b9ee86e973&ascene=14&uin=MjUwOTQxMzEzNw%3D%3D&devicetype=Windows+7&version=62060833&lang=zh_CN&pass_ticket=7mQZ6BjFmlHntAhelfuMIYlv%2B4C5LzpLk6TL4IfNggUpkVnNDN6mVUyuCJ1LYrey)
  
  
### 你知道如何安全正确的关闭线程池吗 (vx公众号 纯洁的微笑)
 线程池总共存在 5 种状态，分别为：

RUNNING：线程池创建之后的初始状态，这种状态下可以执行任务。

SHUTDOWN:该状态下线程池不再接受新任务，但是会将工作队列中的任务执行结束。

STOP: 该状态下线程池不再接受新任务，但是不会处理工作队列中的任务，并且将会中断线程。

TIDYING：该状态下所有任务都已终止，将会执行 terminated() 钩子方法。

TERMINATED：执行完 terminated() 钩子方法之后。

当我们执行 ThreadPoolExecutor#shutdown 方法将会使线程池状态从 RUNNING 转变为 SHUTDOWN。而调用 ThreadPoolExecutor#shutdownNow 之后线程池状态将会从 RUNNING 转变为 STOP。从上面的图上还可以看到，当线程池处于 SHUTDOWN，我们还是可以继续调用 ThreadPoolExecutor#shutdownNow 方法，将其状态转变为 STOP 。

  ```
  //---继承 ThreadPoolExecutor class, 在beforeExecute afterExecute,中存入记录
   public class MyThreadPoolExecutor extends ThreadPoolExecutor {

/**
任务队列大小有限时

当LinkedBlockingDeque(size)塞满时，新增的任务会直接创建新线程来执行，当创建的线程数量超过最大线程数量时会抛异常。
SynchronousQueue没有数量限制。因为他根本不保持这些任务，而是直接交给线程池去执行。当任务数量超过最大线程数时会直接抛异常。

*/
    /**
     * @param corePoolSize 核心线程数 执行中中的线程数, 由于新建core thread 后不自动停止，所以看任务类型，如果是每天跑一次这种不常用的，核心数设置为0，用完就移除； 如果任务执行频度比较高，设置一个合理的核心线程数，一直用来执行任务。
     * @param maximumPoolSize 最大线程数 包括等待的，使用无界队列的话这个参数无效
     * @param keepAliveTime 线程池中超过corePoolSize数目的空闲线程最大存活时间； corePoolSize线程创建了不销毁
     * 当设置allowCoreThreadTimeOut(true)时，线程池中corePoolSize线程空闲时间达到keepAliveTime也将关闭 
     * @param unit 时间单位
     * @param workQueue 等待队列
     * @param threadFactory 线程构造工厂 通常用于设置线程名称 方便记录线程信息debug 和定位问题很有帮助
     * 创建线程的工厂，一般都是采用Executors.defaultThreadFactory()方法返回的DefaultThreadFactory
     * @param handler 阻塞队列满了之后的线程池操作 通常使用{@link RejectedExecutionHandler} 例如AbortPolicy 直接抛出异常
     */
    public VisThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler) {
        super(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory, handler);
    }

    @Override
    protected void beforeExecute(Thread t, Runnable r) {
        // 收集数据，保存日志， 在单个线程执行run方法前执行这里
        // 记录 thread name / task code / user code / time

		    reportDataSyncLogDao.save(log);
        super.beforeExecute(t, r);
    }

    @Override
    protected void afterExecute(Runnable r, Throwable t) {
        // 异常处理， 日志处理，收集信息
       // r表示运行的线程, t表示运行中 线程抛出的异常， 这里一般是运行时异常
			 reportDataSyncLogDao.save(log);
       super.afterExecute(r, t);
    }
    }
    ```
    
    ```
    //====2. 下面这种创建线程池的方式 是阿里巴巴java开发规范建议的一种创建线程方式，需要google 的包；
    public class ReportDataSyncThreadFactory {
	 /**
     * 在系统启动时初始化
     */
    private MyThreadPoolExecutor executor;

    /**
     * 初始化方法
     * 启动时调用
     */
    public static void init() {
    //用于设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字，Debug和定位问题时非常又帮助
        ThreadFactory threadFactory = new ThreadFactoryBuilder().setNameFormat("task-syncSave-%d").build();
       // CustomThreadFactory;
       
        int reportThreadsNum = 20;
        
        /**
         * 阻塞队列采用了LinkedBlockingQueue，没有设置大小，它是一个无界队列；
         * 设置大小后，超出核心线程的任务进入队列里，超出队列新建线程，不允许超出最大线程参数； 如果又超出最大线程数，抛出异常，丢弃任务；
         */
         
        executor = new VisThreadPoolExecutor(5, reportThreadsNum,
                0L, TimeUnit.MILLISECONDS,
                new LinkedBlockingDeque<Runnable>(reportThreadsNum), threadFactory, new ThreadPoolExecutor.AbortPolicy());
    }

    public MyThreadPoolExecutor  getMyThreadPoolExecutor (){
         return this.executor;
    }
    
    /**
     * 销毁线程池
     */
    public static void destroy() {
        executor.shutdown();
    }
    
    // 重启线程池
    /**
     * 销毁线程池，并重现创建线程池
     *  改了 最大线程数和 线程里队列数
     * @throws InterruptedException 
     */
    public static void restart(){
    	destroy(); // shutdown会 先执行完线程 再依次关闭
    	try {
			while (executor.awaitTermination(5, TimeUnit.SECONDS)) {
		//如果线程池任务执行结束，awaitTermination 方法将会返回 true，否则当等待时间超过指定时间后将会返回 false。
                 //如果需要使用这种进制，建议在上面的基础上增加一定重试次数。这个真的很重要！！
			    System.out.println("======VisThreadPoolExecutor=A-thread-Pool==was closed====="
			    		+ "====will restart the thread-pool======");
				// 所有线程关闭后 重启线程池 （用于系统参数改变后，重启线程池）
			   //executor.isShutdown() 调用 shutdown() shutdownNow()  isShutDown() 返回true
			    //executor.isTerminated() 当所有的任务都已关闭后,才表示线程池关闭成功，调用isTerminated() 返回true
			      if(executor.getPoolSize()==0 &&  executor.isTerminated()){
			    	  // 重启线程，会去找系统参数，这样刚修改的系统参数就生效了
			    	  init();
			      }
			   }
		} catch (InterruptedException e) {
			System.out.println("=============异步保存数据线程池shutdown失败================");
			e.printStackTrace();
		}
    }
   
 
    
   }


public class CustomThreadPoolExecutor {

  /**
	 * 线程池初始化方法
	 * 
	 * corePoolSize 核心线程池大小----10
	 * maximumPoolSize 最大线程池大小----30
	 * keepAliveTime 线程池中超过corePoolSize数目的空闲线程最大存活时间----30+单位TimeUnit
	 * TimeUnit keepAliveTime时间单位----TimeUnit.MINUTES
	 * workQueue 阻塞队列----new ArrayBlockingQueue<Runnable>(10)====10容量的阻塞队列
	 * threadFactory 新建线程工厂----new CustomThreadFactory()====定制的线程工厂
	 * rejectedExecutionHandler 当提交任务数超过maxmumPoolSize+workQueue之和时,
	 * 							即当提交第41个任务时(前面线程都没有执行完,此测试方法中用sleep(100)),
	 * 						          任务会交给RejectedExecutionHandler来处理
	 */
	public void init() {
		pool = new ThreadPoolExecutor(
				10,
				30,
				30,
				TimeUnit.MINUTES,
				new ArrayBlockingQueue<Runnable>(10),
				new CustomThreadFactory(),
				new CustomRejectedExecutionHandler());
	}
	
      private class CustomThreadFactory implements ThreadFactory {

		private AtomicInteger count = new AtomicInteger(0);
		
		@Override
		public Thread newThread(Runnable r) {
			Thread t = new Thread(r);
			String threadName = CustomThreadPoolExecutor.class.getSimpleName() + count.addAndGet(1);
			System.out.println(threadName);
			t.setName(threadName);
			return t;
		}
	}
	
    private class CustomRejectedExecutionHandler implements RejectedExecutionHandler {

		@Override
		public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
			// 记录异常
			// 报警处理等
			System.out.println("error.............");
		}
	}
   }
    ```
    
    ```
    线程执行时，直接调用即可
    ReportDataSyncThreadFactory.executor.execute(xxxThread);
    ```
    
    ```
    线程池的关闭（新的理解）
    
   我们可以通过调用线程池的shutdown或shutdownNow方法来关闭线程池，但是它们的实现原理不同，shutdown的原理是只是将线程池的状态设置成SHUTDOWN状态，然后中断所有“没有正在执行”任务的线程。
   shutdownNow的原理是遍历线程池中的工作线程，然后逐个调用线程的interrupt方法来中断线程，所以无法响应中断的任务可能永远无法终止。shutdownNow会首先将线程池的状态设置成STOP，然后尝试停止所有的正在执行或暂停任务的线程，并返回等待执行任务的列表


   只要调用了这两个关闭方法的其中一个，isShutdown方法就会返回true。当所有的任务都已关闭后,才表示线程池关闭成功，这时调用isTerminated方法会返回true。

   至于我们应该调用哪一种方法来关闭线程池，应该由提交到线程池的任务特性决定，通常调用shutdown来关闭线程池，如果任务不一定要执行完，则可以调用shutdownNow。
    ```
    
  ```
  强烈建议使用有界队列，否则队列可能不断增加，影响其他任务； 比如和数据库交互的任务，刚好数据库出问题了， 结果任务无法执行，之后不断有和数据库交互的任务进入队列，如果此时队列无界，就会一直拓展。
 
 ```
线程池 建议把同类型的任务，放在一个线程，比如都是消耗CPU的类型，或者都是消耗IO的类型的任务； 方便根据特性，决定设置线程池参数。
  ```


###优雅关闭线程池 方法2

我们知道处于 SHUTDOWN 的状态下的线程池依旧可以调用 shutdownNow。所以我们可以结合 shutdown ， shutdownNow，awaitTermination ，更加优雅关闭线程池。

shutdown ->  SHUTDOWN:该状态下线程池不再接受新任务，但是会将工作队列中的任务执行结束。

shutdownnow -> STOP: 该状态下线程池不再接受新任务，但是不会处理工作队列中的任务，并且将会中断线程。

```
      threadPool.shutdown(); // Disable new tasks from being submitted
        // 设定最大重试次数
        try {
            // 等待 60 s
            if (!threadPool.awaitTermination(60, TimeUnit.SECONDS)) {
	    //使用awaitTermination，建议在上面的基础上增加一定重试次数。这个真的很重要！！
                // 调用 shutdownNow 取消正在执行的任务
                threadPool.shutdownNow();
                // 再次等待 60 s，如果还未结束，可以再次尝试，或者直接放弃
                if (!threadPool.awaitTermination(60, TimeUnit.SECONDS))
                    System.err.println("线程池任务未正常执行结束");
            }
        } catch (InterruptedException ie) {
            // 重新调用 shutdownNow
            threadPool.shutdownNow();
        }
```
