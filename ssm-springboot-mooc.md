# 慕课 从ssm到springboot
> **项目评价**
项目讲解清晰完整，带领稍有基础的javaweb开发人员完成一个完整的项目，有完整的项目开发流程，包含了一些彩蛋，是非常好的一套项目开发实战教程。
_2018-04-01_

> ## 1.项目开发的一般流程：
1.根据背景提出项目的功能需求；划分项目的几个业务模块，再设计表；
2.根据库表，完成实体类映射。
3.构建项目；依据maven的方式，构建maven经典目录结构，配置maven需要引进的spring，日志，测试等等依赖（这一步导入jar包）
4.整合ssm框架、整合jdbc.properties,mybatis-config.xml,spring--dao.xml,spring--service.xml，Spring--web.xml;层层依赖，配置好经典的ssm框架整合；（_2.5,2.6 video_）
5.配置好ssm框架，依次验证dao,service,controller层是否成功使用，编写代码完成一次初级的controller到dao的操作；（_ssm重点_）
6.配置logback;logback可以在第三方查看log;支持扩展log4j,(_logback是log4j团队开发的_),logback支持一些日志常用操作，包括格式化，文件大小，保存时间，保存位置这些信息；
7.代码开发主要功能，crud;前端后端代码配合使用数据；接受渲染数据。
8.主从同步与读写分离技术。理论到服务器配置到代码实现；

> ## 2.项目彩蛋
_1_. 2.5--2.6 ssm配置，ssm面试常考重点。
_2_. 3_* logback介绍， log4j升级版，可第三方查看log..等强大功能
_3_. 4.3 thumbnailator 图片处理与封装类。 4.9 4.12kaptcha 实现验证码 
_4_. 5.* 主从同步技术.
_5_. 10.* 阿里云部署，远程微信开发调试技巧

> ## 3.第一章 项目准备
环境： jdk1.8 maven 3.3.9 mysql 5.5 chrome tomcat8 eclipse


> ## 4.第二章 项目设计与框架搭建
> 2.1-2.4 创建数据库表技巧：
Mysql数据库，datetime最常用，时间范围长；datestamp主要优势是对跨时区处理的好。如果项目是跨国际的，可以 选择datastramp。Datestramp截止时间是2037年；
Mysql引擎是两种；一种是myisam;另外是innodb;
Myisam,基于表锁。一个进程修改表的第二行数据，会锁整张表；其他线程无法访问；性能高，因为是基于全表去上扫描（检索）的。
Innodb的锁是细致到莫一行的；是基于行检索的，一般用innodb
auto_increment=1; engine=innodb 
auto_increment=1 default charset=utf8

> 2.5 配置maven，maven配置ssm
其实很简单；配置maven有固定pom文件；
主要导入spring关键包，
测试的包和日志的包；
Mybatis关键包；
jdbc关键包；

> 2.5-2.6 完成经典的基于pom配置ssm的web项目
Mybatis-config.xml 网上配置千篇一律；
Spring-dao.xml     共整合进4个包；
通过jdbc.propertise连接数据库的一些属性
通过mybatis-config.xml在sqlsessionfactory里导入datasource再关联到mybatis-config.xml; 以及mapper文件； 
mapperScannerConfigurer动态扫描接口dao;
思路：配置数据库，数据库连接连接池，创建一个负责创建数据库连接池的对象；配置扫描地址，把大的数据连接大对象整过来;
Spring-service.xml  配置事务管理，比如一次业务涉及4步操作，需要这四个都完成才操作。所以这里配置一个原子级别的事务；
Spring-web.xml    开启注解模式；spring就是自动识别conetroller;
                  <Mvc:resource  mapping=”/resources/**” location=”resources”/> 及下一行代码
                 上面的配置时说，告诉springmvc核心类DispatcherServlert 不要拦截resource下的这些资源啊，js啊 图片啊 不要拦截他
                 <Prefix Web-inf/html>       这里利用了web-inf外界访问不到这一安全性；
                解释：内部可以访问web-inf;dispatcherservlet访问controller,controller返回web-inf/里的内容就可以了
                <suffix .html> 严格前后分离。只返回html 没有jsp;后端返回数据；
                设置需要扫描的controller包；路径；
Web.xml   配置好spring三层架构，再和web.xml连接一下；项目就搭起来了;

> 2.5讲maven整合所有的jar包， 2.6讲通过配置文件自己整合一个项目，搭建一个系统，2个video详细解释了maven这个ssm各部分代码的意义，pom里面有每一段代码的具体内容，视频上有详细注释

> 2.7 验证dao层。
mybatis层dao写接口；mapper.xml文件写配置文件实现sql；主要内容为mapper.namespace（dao包的位置）;select标签的id(dao函数名)；resultType(返回对应实体类)；
基于ssm的springMVC各层代码规范，以及注解的使用；
Postman 模拟各种http请求,可以跑自动化测试

> 2.8-2.9 验证service,controller层代码

>> 2.10总结回顾
Ssm重点知识总结: 总结s,s,m各框架重点；面试爱问重点；能理解这三个框架的核心；
SpringMVC:      DispatcherSERVLET;核心类（读源码，面试喜欢问） 
   负责拦截符合要求的请求，进入逻辑；
   生成相应的响应返回给客户端；
Spring：        IOC AOP是重点
   Ioc:Spring来控制对象的生命周期he对象之间的关系；而非传统的由人主动生成对象；
    ioc是概念，ioc依赖di及依赖注入实现；比如以前写代码，数据连接connection ;
    要在需要连接的对象里创建connection；现在则是告诉spring;该对象需要connection；	对象不知道什么时候创建conntion；	
    而是需要的时候spring容器自己创建出这个connection，然后像打针一样把connecton注入到对象上；
   AOP：面向切面编程，例如crud;大多是需要权限的；这里就可以利用aop整日志；
    Aop实现是代理的原理；代理有两种；自行准备；面试重点
Mybatis: orm；映射；对象和数据库表； 实现对象--关系映射；
     通过操作对象实体，自动持久化到数据库；



> ## 5.第三章 Logback配置和使用
1.介绍logback;主要有三个类；支持第三方访问；支持兼容logback;
2.配置logback的xml；实现存储路径，保存级别，格式化日志，保存时间等设置；
3.实例验证配置内容；

> ## 6.第四章 店铺注册功能模块
1.2 dao层新增店铺和更新店铺;dao实现接口；
     对应mapper.xml 实现接口的实现，使用动态sql 在update中使用<set>的标签；
3.thumbnailator图片处理和封装； maven库下载。Demo使用，加水印；
Utils/imageutil. Pathutil.	
4.dto包封装返回bean;关键字段
5.Service层实现；加上@service则有spring 管理； 加@transcation 则有事务管理，必须抛出runtimeexceptin才回滚事务；   
店铺shop添加 service层实现，一个业务4个大步骤，代码严格，添加店铺，然后输入流文件到服务器，在更新店铺图片地址；实现一次完整的添加店铺操作；
6.Sui mobile 阿里巴巴前端方便的框架；可试着用用，只适用ui网页。

> ## 7.第五章 主从库同步与读写分离 
1.主从数据库同步； 原理；从数据库与主数据库保持网络连接和日志通知；主库修改；从库紧接着更新；
2.Linux下 两台服务器下的mysql配置，实现主从同步
3.代码层读写分离； 
5. 代码层读写分离，从properties--？ dao.xml -- > transctioin --?  加注解拦截curd;  --> 
Test crud 是否实现了主从库不同的读写分离功能； 
在dynamicDataSourceIntercepter 上 加@intercepter(@signature)
关键类提示：abstractRoutingdatasource 动态数据源；从两个点接受数据；
_主从同步，主库负责写，从库负责读，读写分离，商业场景中读写比例约为10:1_

>> _**项目开发六-九章需要运行项目自己测试，写代码才有感觉,这几部分负责开发项目模块,主要是后台代码与前端接受数据渲染代码**_

> ## 8.第六章 店铺编辑与列表功能
> ## 9.第七章 商品类别模块
> ## 10.第八章 商品模块
> ## 11.第九章 前端展示系统
> ## 12.第十章 阿里云部署及远程微信开发调试心得与技巧
> ## 13.第十一章 我们可以做的更好
> ## 14.第十二章 项目2.0设计
> ## 15.第十三章 框架大换血
> ## 16.第十四章 店家管理系统增强
> ## 17.第十五章 前端展示系统增强与超级管理员模块
> ## 18.第十六章 课程总结
