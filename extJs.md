# 深入浅出extJs

## [1. ext概述](#1)
## [2.ext框架基础](#2) 
*  [2.1 面向对象的基础架构](#2.1)
*  [2.2 统一的组件模型](#2.2)
*  [2.3 完善的事件机制](#2.3)
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


<h2 id='1'> ext概述 </h2>
     ext支持grid表格，form表单，tree属性，window弹出框和layout页面布局等多种组件，而且可以简便的组装成各种复杂系统；除常用组件外还提供portal,desktop,calendar组件。  <br/> 
     第一节介绍了ext导入js文件，测试hello world； 以及firefox的firebug调试工具

<h2 id='2'> ext基础框架 </h2>
<h4 id='2.1'> 面向对象的基础架构</h4>   

 ext面向对象架构体现在： 依靠ext对象模型的api, 可以迅速实现对象定义，创建，继承和拓展等
 
 * 创建新类 ：  Ext.define('String',Object); par1 表示类名，par2是object对象，存放类的属性和方法； 使用时var c = new demo.Demo();
 
 * 继承及拓展类：   Ext.define('demo.DemoWindow',{extend:'Ext.Window',title:'header'}); 另外可以用statics定义静态成员，可通过constructor
自定义构造函数；

<h4 id='2.2'> 统一的组件模型</h4>

> ext日常接触使用最多的是其组件与布局，通过布局与组件完成应用

 2.2.1 ext所有组件都继承自Ext.Component,所有组件有通用方法和生命周期，最常见的功能initComponent(), render(),show(), hide(); 无论哪种组件都经过初始化，渲染，显示，隐藏实现其整体的生命周期。
 
 2.2.2 Ext.Panel; 直接继承自Ext.Container; 用tbar,bbar设置上下位置工具条，用collapseFirst,collapsed,collapsedCls,collapsible设置与面板折叠有关配置。 floating,shadow设置阴影，浮动效果。
 
 2.2.3 Ext.Container,继承自Ext.Component. layout参数指定当前组件布局方式，items参数包含所有的子组件。 xtype参数简化配置，延迟布局中组件初始化。Ext.Container是一切可布局组件的超类
 
<h4 id='2.3'> 完善的事件机会</h4>

> ext事件模型应用广泛，分2中，自定义事件和浏览器事件。

 2.3.1 自定义事件： ext遵循一种树状的事件模型，继承自Ext.util.Observable类的控件都可以支持事件。自定义Person类，加walk,eat,sleep事件；
 
 person.on('walk',function(){}); 这里on是addListener()简写形式；
 
 2.3.2 浏览器事件：Ext使用Ext.EventManager,Ext.EventObject,Ext.lib.Event对原生浏览器事件封装，展示一套统一的跨浏览器的通用事件接口。
 
 Ext.get('test');获取id='test'的元素；
 
 2.3.3 Ext.EventObjectImpl: 封装的事件，可减少浏览器之间的差异
 
  * getTarget() 返回事件的目标坐标，用来统一IE和其他浏览器使用的ev.target和ev.srcElement.
  * getRelatedTarget() 返回事件相关的html元素；
  * Ext.EventObjectImpl 中还存在按键与ascii的映射关系
  * Ext.EventObject，提供getWheelDelta()，获得鼠标滚轮的delta值，可利用该方法动态改变div宽度
  
 2.3.4 Ext.util.Observable
 
 2.3.5 Ext.EventManager 与事件相关的处理函数
 
  * onDocumentReady,  如Ext.onReady(); 即使在head区域，也会等待html元素都加载后才执行，可控制加载顺序。
  * onWindowResize, 监听浏览器窗口大小变化，以及改变后大小。
  * onTextResize
  
> 本节介绍了ext基本功能框架，包括对象模型，组件模型，事件模型。通过示例解释了模型使用方法，ext所有功能都建立在这些基础架构之上。
 


