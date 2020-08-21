lambda



### 笔记速读

1. jdk8引入lambda 表达式，本质上是一个匿名函数，只有参数列表， 和方法体； -> lambada 运算符

2. 方法引用用法：
   对象::方法；
   如果是static方法， 则是 class::方法； 
   如 System.out::println；

3. 简化代码，可读性好

4. @FunctionalInterface注解；

   这个注解是函数式接口注解，所谓的函数式接口，当然首先是一个接口，然后就是在这个接口里面只能有一个抽象方法。
   这种类型的接口也称为SAM接口，即Single Abstract Method interfaces

5. java8 推出，是以Lambda重要特性，一起推出的，其中系统内置了一系列函数式接口；
   在jdk的java.util.function包下：



### 链接

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIxNTAwNjA4OQ==&mid=2247491923&idx=1&sn=97c41df8d071260e8ebf2b335b9cffd1&chksm=979c4fb5a0ebc6a3a808a8497345cfbff723277c33a0c233bfb54134d12abd89b839123f9abd&mpshare=1&scene=24&srcid=0818VYq8BG8wJVK1TZXepx7e&sharer_sharetime=1597751116461&sharer_shareid=bf08f249d802ad9da04b3ebb9124c537#rd)

[B站地址](https://www.bilibili.com/video/bv1ci4y1g7qD)

