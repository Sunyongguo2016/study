# 马说。

## 20190421

 + 语言这个东西，本质上都是一样的，理解了内存就理解了一切。

 + 我很不喜欢听别人讲,理想很丰满,这句话言外之意就是事情不好干。同学，什么事情都不容易，你不去主动争取，什么也没有，我希望你更积极，还是要敢想的。
人最高的情商，是积极上进。马老师

 + 对于业务和技术，业务也很重要，没有业务无从谈起。业务回归后不就是crud吗，也可以这么说，所以任何语言，基本功就是crud，但是你要懂一个成体系的业务，也是很重要的。所以掌握一门语言，基本的要求就是crud

 + 业务其实没有什么特别难理解的，如果一个业务复杂到很难理解，那么他基本就没有存在的必要了，业务复杂如何推广呢？


### Netty实战()/

> netty 是一款优秀的网络通信框架，（比http效率高）本书分为4部分， 第一部分详细介绍了Netty的相关概念以及核心组件，第二部分介绍自定义协议通常用到的编解码器，
第三部分介绍Netty对于应用层高级协议的支持，会覆盖常见的协议及其实践中的应用，第四部分是几个案例研究。

> netty 以apache maven作为它的构建管理工具。netty不只是一个网络通信框架，另外还定义了一种架构，模式。

p22

 马士兵-BIO-NIO-AIO - Netty
 
> bio,nio,aio 都是java对io做的API，但是效果都不是很好，netty是对nio进行了封装，效果很好，所有被很多公司应用；

计算机中， CPU， 比磁盘的IO 快1000万倍；所以io处理很重要，也一直是个难题；

1. 网络处理中，不许抛出ioException, 这是错误的做法，可能会导致通道没有关闭；
2. (BIO)ServerSocket.accept//此时阻塞方法，直到接收到数据，才会执行下一行；（因此必须每个连接启动一个线程，然后下一个accept才能腾出手来接收数据）   

 接收到数据后，启动线程处理数据new thread().start
 
 BIO 在ServerSocket.accept()会阻塞  ; Socket s.getInputStream().read(); 会阻塞  s.getOutPutStream().write() 会阻塞,比如客户端不接收，就一直阻塞了 所以是阻塞iO 效率很低；并发性很差；
 
 BIO中 ，client连接server 过程会阻塞， 连接好以后，client和thread数据读写过程也会阻塞
 
3. (NIO) nio 在bio的基础上，在client--server直接连接的基础上，在中间加了一个selector,做了处理，采用轮询的机制，其实就是（while），管理连接；甚至可以做到一个线程就处理所有io;

    ServerSocketChannel (其实是对serverSocket的一个封装，功能是既可以读，又可以写，可以和selector结合使用，实现轮询机制)
	
	NIO中 ，上面的是单线程模型，正常的使用应该是reactor模式，（响应式编程）；  client--经过selector--用线程池处理client的读写操作；ByteBuffer非常不好用，可能只读了一半数据（这里其实就是netty，只是netty对此API进行了封装，比原生nio更好用）
	这里与bio+线程池的区别是，bio每个连接建立一个线程处理，nio是连接是通过selector管理的，然后接收到有消息的话，消息处理的过程交给线程池里的线程执行；
	
	bio 是每次一个socket 都要accept； 然后建立一个线程处理；即每一次连接都要一个线程；而且有3次阻塞，accept，read，write；
	
	nio 是 通过selector, 对accept, read 这些动作事件监听，然后通过轮询（while(true)）对监听到的事件做处理，socketChannel sc = ssc.accept()； 然后必须有 sc.configureBlocking(false); 这才是非阻塞，如果没有这一句代码，就还是阻塞的iO；
	
	
	SocketChannel sc = ssc.accept();
    sc.configureBlocking(false);
	
	
4. (AIO) nio中轮询机制，是主动轮询的。 aio(asynchronous异步的)是不再需要轮询了!!! 是接受通知然后处理io数据； client，请求服务端操作系统，os告诉selector,selector 只负责连接客户端; 这就不用selector主动轮询了，
    而是接受os的通知处理连接。  aio中completionHandler() 是成功连接客户端网络后，的处理函数，即回调函数，也可称钩子函数；都属于观察者的设计模式；
	
	aio 完全不阻塞，都是通过hook的模式，在comple后执行 对应的accept的handler,和 read 后的handler; 不阻塞，通过观察者模式，用回调函数处理需要执行的操作；

5. netty (aio其实基本和netty类似了，但是netty封装的更好而已，主要是对byteBuffer的封装更好)
   
   netty实现了对NIO BIO的封装，封装成AIO的样子（一般使用Nio的封装的API）  linux下的AIO效率未必比NIO高（因为底层都是调用系统层面的轮询机制，aio相当于在NIO上又封装了一次，所以可能效率还不如NIO）
   
   分清楚2个概念，一个是连接，一个是处理； 连接是一个线程池，专门处理连接； 处理是对连接之后做的对数据的操作处理，是另一个线程池；  上面的bio,nio,aio 都是通过这2布细分出来的。

   netty中连接的线程池，和处理的线程池，是同一个类 EventLoopGroup ; 具体使用看demo即懂；

   bootstrap 启动 之前的配置 ； 
   
> 阻塞 -- 非阻塞 -- 同步 -- 异步

  同步，异步是消息通信的机制；阻塞-非阻塞是关心等待消息时的状态;
  
  同步，异步，是指发起人，等待返回结果再执行，是同步；如果发起消息，不管返回结果，把返回结果的处理代码写好等待hook，随便某个人（线程）去执行，是异步；
 
  同步异步，是服务器发送，接受，处理，完整交互之后还是由原来的线程处理，是同步； 阻塞非阻塞，是服务器accept()打开通道后等待接受数据，这个等待过程线程只是等待，别的操作都不做是阻塞； 

  如accept操作，一个操作就可以是异步非阻塞的， 然后read() write() 这2个操作可以是阻塞的， 一个动作为一个单位；然后网络处理中，一次连接涉及到accept,读，写数据； 所以一次连接又是一个大的概念，宏观一点的概念；
  
  netty: 异步非阻塞
  
  
cas：java掉本地接口（c语言），c掉汇编，汇编掉cpu指令

activeMQ 小而灵巧； 小一点的项目时候，更方便
kafka  大而全；  大项目用

微服务：
dubbo： 小而精巧；
springCloud: 大而全；

--马士兵-- spring aop 核心动态代理，核心原理ASM----------


aop是面向切面编程； 在方法上加注解，设置方法前，后需要通知的执行的方法，代码；

实现原理是动态代理，所谓动态代理， 即在访问目标class时，加了一层代理，代理能实现目标class的功能，而且还加了aop设置好的需要执行的代码； 

class文件加载进内存就是固定的，不会改变，而class文件按照jvm规范又可以看做是一个结构体，有固定的结构，所以就可以被读取里面的内容，并且也可以写入自己需要打代码；
这个技术就是ASM（可以解释为汇编级别的读写），asm可以读取目标class文件，然后根据aop设置的需要执行的代码逻辑（如加日志，加权限，加try,exception等），写一个新的目标class文件的代理文件，加入需要执行的代码；
然后代码执行时，就执行目标class文件的代理文件，此时就实现了动态代理。

ASM 自己可以上官网，看一下，其实就是一个API，作用就是可以读一个class文件，读出文件的内容，也可以写class文件，自己通过asm写一个正确的class文件；然后应用到动态代理，就是读取文件，然后在目标的方法上，加自己的逻辑代码，然后写一个新的class文件；

--马士兵--时间复杂度

时间复杂度，空间复杂度；

时间复杂度，空间复杂度计算是指算法的效率； 算法是依托于某个特定数据结构，处理特定问题的； 所以复杂度依托算法，算法依托数据结构，和具体问题；

时间复杂度： 随着问题规模的扩大，计算机处理问题，时间的增长趋势； 用O (BIG O)表示， O(1) 表示常数，时间复杂度固定，如访问数组坐标的值； O(n) 线性，如访问链表的某个位置的值； （复杂度一般以最坏结果计算），o(n^n) 表示双重循环的复杂度；
测算方式，一般是用时间差来比较效果；

空间复杂度： 随着问题规模的扩大，计算机处理问题，空间的增长趋势；


