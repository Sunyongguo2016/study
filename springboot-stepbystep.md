## 一步一步学springboot2 微服务项目实战（黄文毅）

> create by sunyongguo  2018ISBN

> 本书偏于实践，根据作者工作经验而来，使用软件版本：win10 idea2016.2 jdk1.8 tomcat1.8 springboot 2.0.0RCI

> 源代码 ： git@github.com:huangwenyi10/stepbystep-learn-springboot.git  教学视频:https://pan.baidu.com/s/1A5xjEcvE5A2T6I5bmAmkYQ
> 如果在下载中遇到问题，可发送邮件至booksage@126.com获取帮助，邮件标题：一步一步学SpringBoot2:微服务项目实战。
> 笔者邮箱：huangwenyi10@163.com  博客：http://blog.csdn.net/huangwenyi1010


### 第一章 第一个springboot项目
>  侧重工具安装，简单使用，在工作中基本要求目测可以达到，都是比较基础的内容；
      springboot是目前流行的微服务框架，倡导“约定大于配置”（理解，如springboot默认的项目路径规则，即体现了这个理念），主要是简化spring应用的初始化搭建以及开发过程，
   * 介绍安装jdk,idea,maven到本地； idea配置maven；
   * idea中使用springInitializr新建项目，搭建springboot项目，springboot默认目录结构，springboot会默认在src/main/resources下找到application.properties 或application.yml配置文件，可以2者并存
   * springboot的maven依赖
   * maven helper插件的安装和使用， 在idea中安装插件，查看依赖，查看冲突，exclude排除冲突包，跳转到源码。

### 第二章 集成mysql数据库
> mysql安装，idea中pom配置依赖包，test连接数据库，亮点是最后使用阿里druid数据库连接池，使用步骤就是pom.xml配置依赖包，配置初始化项。
   * pom.xml 引入依赖，mysql-connector-java spring-boot-starter-jdbc,集成测试连接数据库demo
   * 使用navicat连接数据库，使用IDEA自身连接数据库，查询；
   * 集成druid, pom导入依赖，application.properities配置内容，使用java文件实现注册配置bean（bean相当于xml里的bean，java文件替代xml），进行测试
      使用druid依赖，貌似要先在spring-boot-starter-boot排除tomcat-jdbc，然后再导入druid依赖，druid是jdbc的一个组件，包括3部分，1.DruidDriver代理driver，能够提供基于Filter-chain模式的插件体系；2
      DruidDataSource高效可管理的数据库连接池，3.SQLParser，支持所有jdbc兼容的数据库 ；监控功能看监控数据库连接池和sql查询工作，提高数据库访问性能
 






## 参考文献
 * javaee开发的颠覆者springboot实战
 * spring源码深度解析
 * springboot揭秘
 * https://spring.io/guides
 * https://docs.spring.io/spring-boot/docs/current-SNAPSHOP/reference/htmlsingle
