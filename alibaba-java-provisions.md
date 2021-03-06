# 阿里巴巴 Java开发手册 by(孤尽)

> 序： 阿里巴巴根据开发手册的约定，开发了一套ide的插件，用来提示java开发中不规范的内容.
>> 地址：https://github.com/alibaba/p3c
>>参考笔记：
> * 掘金：（10-21赞）https://juejin.im/entry/58dcb6e3da2f60005fb756f9
> * 赵小忠：（前文图片赞，mysql数据库部分赞）http://www.cnblogs.com/1315925303zxz/p/6934575.html
> * IBM解释: https://www.ibm.com/developerworks/cn/java/deconding-code-specification-part-2/index.html?ca=drs-

## 1.编程规约
##### 1.1 命名风格
> _p6_ 各层命名规范：
* 获取单个对象的方法用get作为前缀
* 获取多个对象的方法用list作为前缀
* 获取统计值的方法用count作为前缀
* 插入的方法用save/inser作为前缀
* 删除的方法用remove/delete作为前缀
* 修改的方法用update作为前缀
##### 1.3 代码格式
> _p10_ 正例代码
##### 1.4 oop规约
> _p18_ 类内定义方法的顺序是：公有>保护>私有>getting/setting 依照关心程度排名，在getter/setter方法中不增加业务逻辑，否则会增加排查问题的难度。
> _p19_ 在循环体内，字符串的拼接方式是stringbuilder.append()拼接，不允许str += str+"aaa";因为后者的反编译字节码显示循环体内每次拼接都是new stringbuilder再append拼接的。
##### 1.6 并发处理(看不懂)
##### 1.7 控制语句
> _p36_ 接口入参保护：必须参数检验(对外提供接口,rpc,api,http接口);(敏感权限入口);(需要极高稳定性和可用性的方法);
##### 1.8 注释规约
> _p38_ 类，方法，接口方法上必须用 /** content */ 注释

## 2.异常日志
> _p44_ catch异常需要自己处理，或者返回调用者，必须有作为。
> 异常 catch 不可以做流程控制，条件控制.
> _p50_ 日志打印输出，使用占位符描述关键内容，如: logger.debug("Processing trade with id: {} and symbol : {}",id,symbol);

## 3.单元测试
> _p54_ 单元测试过程必须是全自动才有意义，必须(可重复),不允许使用system.out人肉眼测试，必须用assert验证。
单元测试应保持独立性，不可互相调用，不可前后依赖。

## 4.安全规约
> 安全无小事，> _p60_ 个人用户页面或功能，必须进行权限验证，防止修改个人私信内容，订单等。
>> 个人敏感信息脱敏，如手机号，身份证 152****0796
> 用户传入任何参数必须做有效性验证， 表单，ajax提交执行csrf安全过滤； 用户输入的sql参数严格使用参数绑定或metadata字段值限定，防止sql注入；
用户使用平台资源，要做数量限制，或疲劳限制，防止烂刷，资损；
> https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project#tab=Main
> 根据OWASP前十项目执行安全代码训练。(owasp) 互联网：OWASP Top 10是一款强大的Web应用程序安全性意识文档

## 5. **mysql数据库** 
> _本章是很重要实用的内容，完美，这部分while(1){读原文}！_
##### 5.1建表规约 
>> 库表设计规约，强推重读原文！！！
>> 表必须具备3个字段，id,gmt_create,gmt_modified....
##### 5.2索引规约
>> 业务上具有唯一特性的字段，即使是多个字段的组合，也必须建立成唯一索引，(索引对插入的影响几乎不计，对查询性能提升很大，另外可以确定数据唯一性)
>> 后续学习.
##### 5.3sql语句
> sql 重点,强推!
> 性能优化目标: range,ref,consts
> 数据更新，必须先select,在update
##### 5.4orm映射
> 返回结果必须标注，select * 不允许.
> update 要有针对性，不许全部更新
> 更新记录，gmt_modified时间
> @transactonal勿滥用

> 分页优化：利用延迟关联或自查询优化超多分页的场景。
正例： 先快速定位需要获取的id段，然后再关联。
select a.* from 表1 a,(select id from 表 1 where 条件 limit 100000,20) b where a.id=b.id

## 6.工程接口
##### 6.2 二方库依赖
> gav遵循规则：
  * 1.GroupId: 格式：com.{公司/BU}.业务线.[子业务线],最多四级； 例如com.alibaba.tamll.aliexpress
  * 2.版本号: 主版本号.次版本号.修订号. 注意从1.0.0开始
##### 6.3 服务器
> 服务器tcp的time_wait调小， 最大文件句柄数调大， 服务器jvm设置； 非常实用的服务器配置技巧，小而精的细节，点赞！ 

## 7.设计规约
大型软件系统，需要做设计，设计类图，流程图，以及各种文档....类似于建大厦需要图纸，而一般的项目如搭个二层小楼，不用太繁琐即可设计开发.
