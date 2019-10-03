  1. 线程池的使用， 采用阿里巴巴java规范推荐的方式，
  2.  线程池参数怎么写； 
  3. 线程池 线程执行过程，继承了ThreadPoolExecutor类， 在beforeExecute() afterExecute() 可以怎么做； 做日志，收集数据；
  4. 下面是使用中遇到4篇好的文章


  [ThreadPoolExecutor-1-掘金](https://juejin.im/post/5b25b3d0f265da597d0a9803)
  
  [ThreadPoolExecutor-2-csdn](https://blog.csdn.net/qq_25806863/article/details/71126867)
  
  [ThreadPoolExecutor-3-简书](https://www.jianshu.com/p/b42cdf3f9af8)
  
  [ThreadPoolExecutor-4-cnblog](https://www.cnblogs.com/zedosu/p/6665306.html)
  
  ```
  //---继承 ThreadPoolExecutor class, 在beforeExecute afterExecute,中存入记录
   public class MyThreadPoolExecutor extends ThreadPoolExecutor {

    /**
     * @param corePoolSize 核心线程数 执行中中的线程数
     * @param maximumPoolSize 最大线程数 包括等待的，使用无界队列的话这个参数无效
     * @param keepAliveTime 线程销毁等待时间 空闲线程多长时间后销毁
     * @param unit 时间单位
     * @param workQueue 等待队列
     * @param threadFactory 线程构造工厂 通常用于设置线程名称 方便记录线程信息debug 和定位问题很有帮助
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
    public static MyThreadPoolExecutor executor;

    /**
     * 初始化方法
     * 启动时调用
     */
    public static void init() {
    //用于设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字，Debug和定位问题时非常又帮助
        ThreadFactory threadFactory = new ThreadFactoryBuilder().setNameFormat("task-syncSave-%d").build();

        int reportThreadsNum = 20;
        
        /**
         * 阻塞队列采用了LinkedBlockingQueue，没有设置大小，它是一个无界队列；
         * 设置大小后，超出核心线程的任务进入队列里，超出队列新建线程，不允许超出最大线程参数； 如果又超出最大线程数，抛出异常，丢弃任务；
         */
         
        executor = new VisThreadPoolExecutor(5, reportThreadsNum,
                0L, TimeUnit.MILLISECONDS,
                new LinkedBlockingDeque<Runnable>(reportThreadsNum), threadFactory, new ThreadPoolExecutor.AbortPolicy());
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
