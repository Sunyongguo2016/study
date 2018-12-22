# 深入浅出extJs

## [1.ext概述](#1)
    [1.1extgaishu11](#1.1)
     [1.1.1extgaishu111](#1.1.1)
## [2.ext框架基础](#2) 
## [3. 表格控件](#3)
## [4.表单与输入控件](#4)
## [5.树形结构](#5)
## [6.布局](#6)
## [7.弹出窗口](#7)
## [8.工作条与菜单](#8)
## [9.数据存储于传输](#9)
## [10.用户拓展与插件](#10)
## [11.拖放](#11)
## [12.实用工具](#12)
## [13.实例完整的ext应用](#13)
## [14.应用MVC](#14)

<h2 id='1'> 一级目录 </h2>
0.Java 中多线程同步是什么？ 

在多线程程序下，同步能控制对共享资源的访问。如果没有同步，当一个 Java 线程在修改一个共享变量时，另外一个线程正在使用或者更新同一个变量，这样容易导致程序出现错误的结果。 

1.解释实现多线程的几种方法? 

一 Java 线程可以实现 Runnable 接口或者继承 Thread 类来实现，当你打算多重继承时，优先选择实现 Runnable。 

2.Thread.start ()与 Thread.run ()有什么区别？ 

Thread.start ()方法(native)启动线程，使之进入就绪状态，当 cpu 分配时间该线程时，由 JVM 调度执行 run ()方法。 

3.为什么需要 run ()和 start ()方法，我们可以只用 run ()方法来完成任务吗？ 

我们需要 run ()&start ()这两个方法是因为 JVM 创建一个单独的线程不同于普通方法的调用，所以这项工作由线程的 start 方法来完成，start 由本地方法实现，需要显示地被调用，使用这俩个方法的另外一个好处是任何一个对象都可以作为线程运行，只要实现了 Runnable 接口，这就避免因继承了 Thread 类而造成的 Java 的多继承问题。 

4.什么是 ThreadLocal 类，怎么使用它？ 

ThreadLocal 是一个线程级别的局部变量，并非“本地线程”。ThreadLocal 为每个使用该变量的线程提供了一个独立的变量副本，每个线程修改副本时不影响其它线程对象的副本(译者注)。 

下面是线程局部变量(ThreadLocal variables)的关键点： 

一个线程局部变量(ThreadLocal variables)为每个线程方便地提供了一个单独的变量。 

ThreadLocal 实例通常作为静态的私有的(private static)字段出现在一个类中，这个类用来关联一个线程。 

当多个线程访问 ThreadLocal 实例时，每个线程维护 ThreadLocal 提供的独立的变量副本。 

常用的使用可在 DAO 模式中见到，当 DAO 类作为一个单例类时，数据库链接(connection)被每一个线程独立的维护，互不影响。(基于线程的单例) 

ThreadLocal 难于理解，下面这些引用连接有助于你更好的理解它。 

《Good article on ThreadLocal on IBM DeveloperWorks 》、《理解 ThreadLocal》、《Managing data : Good example》、《Refer Java API Docs》 

5.什么时候抛出 InvalidMonitorStateException 异常，为什么？ 

调用 wait ()/notify ()/notifyAll ()中的任何一个方法时，如果当前线程没有获得该对象的锁，那么就会抛出 IllegalMonitorStateException 的异常(也就是说程序在没有执行对象的任何同步块或者同步方法时，仍然尝试调用 wait ()/notify ()/notifyAll ()时)。由于该异常是 RuntimeExcpetion 的子类，所以该异常不一定要捕获(尽管你可以捕获只要你愿意).作为 RuntimeException，此类异常不会在 wait (),notify (),notifyAll ()的方法签名提及。 

6.Sleep ()、suspend ()和 wait ()之间有什么区别？ 

Thread.sleep ()使当前线程在指定的时间处于“非运行”（Not Runnable）状态。线程一直持有对象的监视器。比如一个线程当前在一个同步块或同步方法中，其它线程不能进入该块或方法中。如果另一线程调用了 interrupt ()方法，它将唤醒那个“睡眠的”线程。 

注意：sleep ()是一个静态方法。这意味着只对当前线程有效，一个常见的错误是调用t.sleep ()，（这里的t是一个不同于当前线程的线程）。即便是执行t.sleep ()，也是当前线程进入睡眠，而不是t线程。t.suspend ()是过时的方法，使用 suspend ()导致线程进入停滞状态，该线程会一直持有对象的监视器，suspend ()容易引起死锁问题。 

object.wait ()使当前线程出于“不可运行”状态，和 sleep ()不同的是 wait 是 object 的方法而不是 thread。调用 object.wait ()时，线程先要获取这个对象的对象锁，当前线程必须在锁对象保持同步，把当前线程添加到等待队列中，随后另一线程可以同步同一个对象锁来调用 object.notify ()，这样将唤醒原来等待中的线程，然后释放该锁。基本上 wait ()/notify ()与 sleep ()/interrupt ()类似，只是前者需要获取对象锁。 


<h4 id='2'> 二级目录 </h4>

0.Java 中多线程同步是什么？ 

在多线程程序下，同步能控制对共享资源的访问。如果没有同步，当一个 Java 线程在修改一个共享变量时，另外一个线程正在使用或者更新同一个变量，这样容易导致程序出现错误的结果。 

1.解释实现多线程的几种方法? 

一 Java 线程可以实现 Runnable 接口或者继承 Thread 类来实现，当你打算多重继承时，优先选择实现 Runnable。 

2.Thread.start ()与 Thread.run ()有什么区别？ 

Thread.start ()方法(native)启动线程，使之进入就绪状态，当 cpu 分配时间该线程时，由 JVM 调度执行 run ()方法。 

3.为什么需要 run ()和 start ()方法，我们可以只用 run ()方法来完成任务吗？ 

我们需要 run ()&start ()这两个方法是因为 JVM 创建一个单独的线程不同于普通方法的调用，所以这项工作由线程的 start 方法来完成，start 由本地方法实现，需要显示地被调用，使用这俩个方法的另外一个好处是任何一个对象都可以作为线程运行，只要实现了 Runnable 接口，这就避免因继承了 Thread 类而造成的 Java 的多继承问题。 

4.什么是 ThreadLocal 类，怎么使用它？ 

ThreadLocal 是一个线程级别的局部变量，并非“本地线程”。ThreadLocal 为每个使用该变量的线程提供了一个独立的变量副本，每个线程修改副本时不影响其它线程对象的副本(译者注)。 

下面是线程局部变量(ThreadLocal variables)的关键点： 

一个线程局部变量(ThreadLocal variables)为每个线程方便地提供了一个单独的变量。 

ThreadLocal 实例通常作为静态的私有的(private static)字段出现在一个类中，这个类用来关联一个线程。 

当多个线程访问 ThreadLocal 实例时，每个线程维护 ThreadLocal 提供的独立的变量副本。 

常用的使用可在 DAO 模式中见到，当 DAO 类作为一个单例类时，数据库链接(connection)被每一个线程独立的维护，互不影响。(基于线程的单例) 

ThreadLocal 难于理解，下面这些引用连接有助于你更好的理解它。 

《Good article on ThreadLocal on IBM DeveloperWorks 》、《理解 ThreadLocal》、《Managing data : Good example》、《Refer Java API Docs》 

5.什么时候抛出 InvalidMonitorStateException 异常，为什么？ 

调用 wait ()/notify ()/notifyAll ()中的任何一个方法时，如果当前线程没有获得该对象的锁，那么就会抛出 IllegalMonitorStateException 的异常(也就是说程序在没有执行对象的任何同步块或者同步方法时，仍然尝试调用 wait ()/notify ()/notifyAll ()时)。由于该异常是 RuntimeExcpetion 的子类，所以该异常不一定要捕获(尽管你可以捕获只要你愿意).作为 RuntimeException，此类异常不会在 wait (),notify (),notifyAll ()的方法签名提及。 

6.Sleep ()、suspend ()和 wait ()之间有什么区别？ 

Thread.sleep ()使当前线程在指定的时间处于“非运行”（Not Runnable）状态。线程一直持有对象的监视器。比如一个线程当前在一个同步块或同步方法中，其它线程不能进入该块或方法中。如果另一线程调用了 interrupt ()方法，它将唤醒那个“睡眠的”线程。 

注意：sleep ()是一个静态方法。这意味着只对当前线程有效，一个常见的错误是调用t.sleep ()，（这里的t是一个不同于当前线程的线程）。即便是执行t.sleep ()，也是当前线程进入睡眠，而不是t线程。t.suspend ()是过时的方法，使用 suspend ()导致线程进入停滞状态，该线程会一直持有对象的监视器，suspend ()容易引起死锁问题。 

object.wait ()使当前线程出于“不可运行”状态，和 sleep ()不同的是 wait 是 object 的方法而不是 thread。调用 object.wait ()时，线程先要获取这个对象的对象锁，当前线程必须在锁对象保持同步，把当前线程添加到等待队列中，随后另一线程可以同步同一个对象锁来调用 object.notify ()，这样将唤醒原来等待中的线程，然后释放该锁。基本上 wait ()/notify ()与 sleep ()/interrupt ()类似，只是前者需要获取对象锁。 








