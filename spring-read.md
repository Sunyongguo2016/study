## spring源码深度解析 郝佳
> 不建议读框架的源代码，如何实现一个框架和如何用框架实现业务有很大的不同，在阅读底层框架源代码上的时间精力投入相比收获来说不划算,spring.io上的文档你们都读了几遍了？
_所有项目的文档读不过来没事，spring framework这一个项目的reference有完整从头到尾读一遍的吗？_ 详细认真读过spring所有项目的reference，并对所有API doc了如指掌的人都少之又少。你先开始读源代码干嘛？

> 所有跳过文档这一步就想直接阅读源代码的，只能是英文水平不行，读不懂文档又急于求成。

### 第一章 spring整体架构和环境搭建

### 第二章 企业应用
#### 8. 数据库连接jdbc  225
    `spring整合jdbc`,jdbc自身使用有一套复杂的代码，spring对jdbc做了大量封装，消除了冗余代码，开发量大大减少。配置好spring配置文件后，操作很简单：
      ApplicationContext act = new ClassPathXmlApplicationContext("bean.xml");
      UserService userService = (UserService)act.getBean("userService");
      userService.update(user);

#### 9. 整合mybatis
> 与上面spring整合jdbc类似，spring整合mybatis最大方便是减少代码量，spring装配mybatis相关的bean，通过spring得到bean，然后操作bean的属性，方法；
    `mybatis独立使用`; `spring整合mybatis`，spring 使用mybatis非常方便，甚至察觉不到使用mybatis，代码量极少，如下：
      Application context = new ClassPathXmlApplicationContext("test/mybatis/applicationContext.xml");
      UserMapper userDao = (UserMapper)context.getBean("userMapper");
      userDao.getUserById("1");
#### 10. 事务
     spring的声明式事务让我们从复杂的事务处理中解脱，再也不需要去处理获得连接，关闭连接，事务提交和回滚等操作，不需要在与事务相关方法中处理
大量的try..catch..finally代码。默认spring对runtimeexception回滚，对exception不会滚；

#### 11. springmvc
##### ServletContextListener,ServlertContext设置默认全局变量
     每个web应用都有一个ServletContext与之相关联，ServletContext对象在应用启动时被创建，在应用关闭的时候被销毁。ServletContext在全局范围内有效，
类似于应用中的一个全局变量。
     在ServletContextListener中的核心逻辑便是初始化WebApplicationContext实例并存放至ServletContext中。目的在系统启动时添加自定义的属性。便于全局范围调用。
##### dispatcherservlet
###### dispatcherServlet的初始化工作
     dispatcherservlet工作首先是初始化一些系统参数，通过几个步骤实现初始化，如初始化`MultipartResolver`,用于处理文件上传，spring常用Multipart配置如下：
<class="org.springframework.web.multipart.commons.CommonsMultipartResolver">,当前在初始化此之前需要得到ApplicationContext对象，这些都是Dispartcher的初始化
准备工作；
     `LocaleResolver初始化`，实现国际化，使用class="org.Springframework.web.servlet.il8n.AcceptHeaderLocaleResolver"
     `spring主题资源`，class="org.Springframework.ui.context.support.ResourceBundleThemeSource"
     `主题解析器themeSource`,针对不同用户定制不同主题，org.springframework.web.servlet.theme.FixedTheme Resolver;主题解析器有4类，可以按照用户（cookie），session等方式定制。
>> dispatcherServlet 初始化initStrategies 分别初始化 intMultipartResolver,initLocaleResolver,initThemeResolver,initHandlerMappings,initHandlerAdapters,initHandlerExceptionResolvers,initRequestToViewNameTranslator,initViewResolvers,initFlashMapManager
     接上面，现在初始化`handlerMappings`,客户端发出request时dispatcherServlet会将Request提交给HandlerMapping,然后HandlerMapping根据WebApplicationContext的配置来回传给Dispatcher响应的controller；
     `HandlerAdapters`,适配器；
     `HandlerExceptionResolvers`，初始化异常处理，基于handlerExceptionResolver接口的异常处理，使用这种方式只需要实现ResolveException方法，改方法返回一个ModelAndView对象；
     `RequestToViewNameTranslator`，当controller处理器方法没有返回一个view对象或逻辑视图名，spring采用约定好的方式提供一个逻辑视图名称。配置项有：prefix,suffix,separator等等。
     `viewResolvers`，springmvc中，controller将请求处理结果放入modelAndView后，dispatcherServlet会根据ModelAndView选择合适的视图进行渲染，而view如何创建，如何选择合适的view？ 就是viewResolver
完成的内容。ViewResolvers作为bean,可以在spring配置文件中进行配置。 org.Springframework.web.servlet.view.InternalResourceViewResolver.

##### 11.4 diapatcherServlet的逻辑处理。 331
>> multipartContent类型的request处理，根据request信息寻找对应的handler，没找到对应的handler的错误处理，根据当前handler寻找对应的handlerAdapter,缓存处理,HandlerInterceptor的处理，逻辑处理，异常视图的处理，根据视图跳转页面
     `handlerMappings`,在之前的内容我们提到过，系统启动时spring会将所有的映射类型的bean注册到this.handlerMappings变量中。
     `request找到对应的handler`,根据request对象，查找对应的handler，request对象里分析出request请求路径，然后与handler匹配，如上，handlerMapping封
装了bean和映射的对应关系,匹配成功，就可以进入对应的controller； 拦截器在springmvc中使用的场景很多，链处理机制，是spring中非常常用的处理方式，是aop
的重要组成部分，可以方便的对目标对象进行拓展及拦截，这是非常优秀的设计。
     `没找到handler的错误处理`,对于匹配handler失败，而且默认请求也未设置，则handler为空的情况出现，就只能通过response向用户返回错误信息。
     `handler寻找对应的handlerAdapter`, 获取适配器。获取是适配器的逻辑无非是遍历所有适配器来选择合适的适配器并返回它。而某个适配器是否适用于当前handler逻辑被封装在具体的适配器中。simpleControllerHandlerAdapter就是用于处理
普通的web请求的，而且对于springmvc来说，我们会把逻辑封装至controller的子类中，例如UserController就是继承自abstractController,而abstractController实现了controller接口。
     `缓存处理`,last-modified缓存机制。 response:last-modified request: if-modified-since; 如果请求内容没有变化，则自动返回http304状态码，只有响应头。内容为空，节省了网络带宽。 spring提供last-modifyed机制的支持，只需要实现
LastModified接口；
     `handlerInterceptor的处理`,344





#### 12. 远程服务
     `RMI`,remote message invocation,远程方法调用，是java用于实现远程过程调用的应用程序编程接口，它使客户机上运行的程序可以调用远程服务器上的`对象`，
远程方法调用特性使java程序员能够在网络环境中分布操作。351
     `Spring整合RMI使用`,RMI使用服务端需要声明服务类，服务名，服务接口，服务端口这些属性，服务端实现一个Object的某个方法Method,客户端通过RMI调用Object的Method,
成功调用后即在客户端执行了Method; 通过使用Spring整合的RMI功能比原生的RMI简单了很多。
     `HttpInvoker`,Spring开发小组意识到RMI服务和基于Http的服务之间的空白，一方面RMI使用java标准的对象序列化，很难穿越防火墙，另一方面Hessian/Burlap能很好的穿越
防火墙，但使用自己私有的一套序列化机制，spring的HttpInvoker就集合了这两者有点开发出来。Httpinvoker使用可以具体看官网demo；

#### 13. spring消息
    JMS应用程序接口是一个java平台中关于面向消息中间件的API，用于在两个应用程序之间或分布式系统中发送消息，进行异步通信。
    `Java消息服务`，java消息服务规范包括点对点和发布者/订阅者模式，可以支持异步/同步消息处理；jms独立使用，创建connection，session，destination，message，
分别创建生产者和消费者的代码，使用时开启发送端，然后向服务器发送消息，接着再开启接收端，正常情况下会接收到发送端发送的消息。
    `spring整合ActiveMQ`,上面单独使用ActiveMQ, spring做的核心事情就是作为一个容器，装配Bean,自动注入bean中的属性，然后通过spring方便的获取bean，操作
bean的属性和方法。 Spring整合ActiveMQ的过程，spring自己封装了jmsTemplate,然后把ConnectionFaction作为属性注入到jmsTemplate,在配置文件吧bean的关系装
配好，然后在代码中实现spring 操作mq发送接收； 主要优点就是在利用spring获取对象的过程，自动装配了其中的bean，省去了connection以及sessioin创建与
销毁过程，简化了工作量。
    ApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"test/activeMQ/spring/applicationContext.xml"});
    JmsTemplate jmsTemplate = (JmsTemplate)context.getBean("jmsTemplate");
    `消息监听器`，另外，在org.springframework.jms.listener.DefaultMessageListenerContainer中可以配置属性 messageListener,作为消息监听器方便使用。
