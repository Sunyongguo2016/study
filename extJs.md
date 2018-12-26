# 深入浅出extJs

## [1. ext概述](#1)
## [2.ext框架基础](#2) 
*  [2.1 面向对象的基础架构](#2.1)
*  [2.2 统一的组件模型](#2.2)
*  [2.3 完善的事件机制](#2.3)
## [3. 表格控件](#3)
*  [3.1 制定简单表格](#3.1)
*  [3.2 表格常用功能](#3.2)
*  [3.3 表格渲染renderer](#3.3)
*  [3.4 给表格行列设置颜色renderer](#3.4)
*  [3.5 自动显示行号和复选框renderer](#3.5)
*  [3.6 选择模型获取选中内容](#3.6)
*  [3.7 表格视图-GridView](#3.7)
*  [3.8 表格分页](#3.8)
*  [3.9 后台排序](#3.9)
*  [3.10 多重排序](#3.10)
*  [3.11 可编辑表格控件-EditorGrid](#3.11)
*  [3.12 属性表格控件-EditorGrid之上更智能高级表格组件-可看做key,value的表格](#3.12)
*  [3.13 分组表格控件 GroupingGrid](#3.13)
*  [3.14 可拖放的表格](#3.14)
*  [3.15 表格与右键菜单](#3.15)
*  [3.16 基于表格的拓展插件](#3.16)
*  [3.17 小结](#3.17)
## [4.表单与输入控件](#4)
*  [4.1 FormPanel BasicForm详解](#4.1)
*  [4.2 Ext支持的输入组件](#4.2)
*  [4.3 ComboBox详解](#4.3)
*  [4.4 复选框和单选按钮](#4.4)
*  [4.5 滑动条表单控件](#4.5)
*  [4.6 表单布局](#4.6)
*  [4.7 数据校验](#4.7)
*  [4.8 使用表单提交数据](#4.8)
*  [4.9 自动数据填充到表单](#4.9)
*  [4.10 小结](#4.10)
## [5.树形结构](#5)
*  [5.1 TreePanel基本使用](#5.1)
*  [5.2 树的事件](#5.2)
*  [5.3 修改节点图标，提示信息，超链](#5.3)
*  [5.4 树形拖放 p172](#5.4)
*  [5.6 对树进行排序](#5.6)
*  [5.7 带checkbox树形 178](#5.7)
*  [5.8 树形与表格的结合 179](#5.8)
*  [5.9 更多树形的高级应用 (选中刷新等事件)182](#5.9)
*  [5.10 小结183](#5.10)

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
 
<h2 id='3'> 表格控件 </h2>
<h4 id='3.1'> 制定简单表格</h4>

> page 41; 表格功能很强大，排序，缓存，隐藏某一列，列汇总，单元格编辑等 xtype为grid,支持本地分页和远程分页；

 表格包含的`列columens`: header(文本) dataIndex(记录集字段) sortable(排序) renderer(渲染函数) format(格式化信息) 一维数组里包括对象

 表格数据: 通过Ext.data.ArrayStore, 我们可以把任何格式的数据转化为表格可以使用的形式。 store包含2部分，proxy, reader. proxy指获取数据的方式，reader是指如何解析这一堆数据。执行一次store.load()来初始化数据。
 
 grid: columns; data; store:{data,fields}; store.load(); grid:renderTo:'grid';  -- 此时应该有一个id=grid的div对应
 
<h4 id='3.2'> 表格常用功能 </h4>

 * 列拖放，改列的size： enableColumnMove, enableColumnResize 设置false，可以固定列
 * gird: loadMask:true, 读取数据时遮罩和提示功能 ;
 * gird: forceFit:true, 表格按照width的比例分配，非常智能。当改变某一列宽度会自动重新计算其他列宽度，既不会超出也不会多余
 * columns = new Ext.grid.ColumnModel(); 设置内部对象sortable:true, 在列上会出现下拉标签，可以设置排序方式； 另（书中有中文排序重写代码）
 * 日期类型： data:[['1970-01-15T02:58:04']]  store=new Ext.data.ArrayStore(fields[{name:'date',type:'date',dateFormat:'Y-m-dTH:i:s'}]) columns=[{header:'日期',dataIndex:'date'},renderer:Ext.util.Format.dateRenderer('Y-m-d')]
 
<h4 id='3.3'> 表格渲染(如颜色,图片) renderer </h4> 

var columns = [{header:'性别',dataIndex:'sex',renderer:function(value){
		if(value == 'male'){
			return "<spen style='color:red; font-weight:bold;'>男</span>"
		}
	}}];

 可以看到，这里renderer里面，既然可以写HTML元素，那么也可以插入图片等；这里可以把renderer 后自定义函数抽出来

 ---
 
 另外可以写一个复杂的渲染：
 renderer可以格式化该列显示的数据格式或者按照你自定义的脚本显示最终数据样子（我目前是这么理解的）
	先看下renderer: function()里的参数

	renderer:function(value, cellmeta, record, rowIndex, columnIndex, store){

	}
	1.value是当前单元格的值
	2.cellmeta里保存的是cellId单元格id，id不知道是干啥的，似乎是列号，css是这个单元格的css样式。
	3.record是这行的所有数据，你想要什么，record.data["id"]这样就获得了。
	4.rowIndex是行号，不是从头往下数的意思，而是计算了分页以后的结果。
	5.columnIndex列号太简单了。
	6.store，这个厉害，实际上这个是你构造表格时候传递的ds，也就是说表格里所有的数据，你都可以随便调用，唉，太厉害了。
 ---

<h4 id='3.4'> 给表格的行和列设置颜色 renderer </h4> 
<h4 id='3.5'> 自动显示行号和复选框 renderer </h4> 

行号：
var columns = [new Ext.grid.RowNumberer(),{header:'编号',dataIndex:'id'}]
这里有个问题，如果删除了某一行，行号就不连续了， 然后可以通过 gird.view.refresh() 在执行删除后刷新表格视图。refresh()会自动重新计算行号

复选框：
我们需要使用CheckboxModel,在每行前加一个复选框； 同样还要修改columns,SelectionModel对象即selModel;
```
var sm = new Ext.selection.CheckboxModel();
// var sm = new Ext.selection.CheckboxModel({checkOnly:true}); 只允许通过复选框选中，防止不慎选中一行后只选中一行的情况。
var grid = new Ext.grid.GridPanel({
	autoHeight: true,
	renderTo: 'grid',
	store: store,
	columns: columns,
	selModel: sm
});
```
 
<h4 id='3.6'> 选择模型：获取选中内容 </h4> 
 
 当我们单击普通HTML元素，选中的是HTML元素， 而在表格中，单击某个单元格，选中的却是一行，表格这种功能称为选择模型。
 
 行选择模型：
 ```
 var grid = new Ext.grid.GridPanel({
	renderTo: 'grid',
	store: store,
	sm: new Ext.grid.RowModel({singleSelect:true})
 });
 ```
 
 CellModel,-- 单元格选择模型，每次只允许选择一个单元格，EditorGrid里默认使用CellModel; 也可以设置为RowModel;
 
 `用途`： 获取选中行一些信息
 ```
 grid.on('itemclick',function(){
	var selected = grid.getSelectModel().selected;
	for(var i=0; i<selected.getCount(); i++){
		var recored = selected.get(i);
		Ext.Msg.alert(..recored.get("id"),recored.get("name"),recored.get("descn"));
	}
 });
 ```
 
 <h4 id='3.7'> 表格视图-Ext.grid.GridView </h4> 
 
> Ext的表格控件都遵循MVC模型， Ext.data.Store可看做model； Ext.grid.GridPanel中设置的各种监听器可看做（controller），Ext.grid.GridView对应View; 我们可通过grid.getView()获取当前表格使用的视图实例

 GridView控制视图显示功能，如果对表格的显示效果做调整，可通过GridView进行设置，但是grid.getView()必须在创建Ext.grid.GridPanel之后调用； 若要初始化GridView，可使用Ext.grid.GridPanel的viewConfig参数。
 
 ```
 var grid = new Ext.grid.GridPanel({
	viewConfig:{
		columnsText:'显示的列'，
		sortAscText:'升序'，
		//等等参数
	}
 });
 ```
 
 <h4 id='3.8'> 表格分页 </h4> 
 
 * 分页工具条
 grid:   bbar: new Ext.PagingToolbar();  如果配置了分页工具条，store.load() 就必须构造表格以后执行否则分页工具条将不起作用。   -- 分页的store不能选择Ext.store.SimpleStore; 分页需要store.load()函数，而load()函数与proxy有关，
SimpleStore不但没有设置proxy，而且没有重写load()函数，所以会出现错误，无法显示分页信息；
 
 * 通过后台脚本获取分页数据
  1.proxy:{type:'ajax',url:'09.jsp',reader:{type:'json',root:'root',totalProperty:'totalProperty',idProperty:'id'} } -- 3 store.load({params:{start:0,limit:10}});
  
 ext分页，会一次性把所有获取到的数据展示出来；pageSize参数无意义，不起作用；  但是PagingMemoryProxy可以实现内存分页；
 
 <h4 id='3.9'> 后台排序 </h4> 
 
 默认情况下，表格只是实现了前台排序，若实现后台排序，Ext.data.Store的remoteSort属性设置为true; （是否允许远程排序） 选择远程排序，向后台传参sort(排序字段)，dir(升序或降序)
  
 <h4 id='3.10'> 多重排序 </h4>   
 <h4 id='3.11'> 可编辑表格控件-EditorGrid </h4>
 
 像excel一样，可随意添加删除行和列，编辑单元格，这些单元格暂定不许为空否则验证信息会给出提示；
 
 <h4 id='3.12'> 属性表格控件-EditorGrid之上更智能高级表格组件-可看做key,value的表格</h4>
 <h4 id='3.13'> 分组表格控件 GroupingGrid </h4>
 
 通过某一列的数据，分组展示，一目了然，方便查看
 
 <h4 id='3.14'> 可拖放的表格 </h4>
 <h4 id='3.15'> 表格与右键菜单 </h4>
 <h4 id='3.16'> 基于表格的拓展插件 </h4>
	
 * 行编辑器 （整行编辑）
 * 进度条分页组件
 * 分组表头 （合并表头，2级表头）
 * 锁定列
 * 树形表格 TreeGrid ，在表格的一列展示一棵树；
 * 表格过滤组件

 <h4 id='3.17'> 小结 </h4>
	
 本章介绍了Ext中的表格控件，一步步介绍表格的各种配置，然后讲了选择模型，配置不同的sm修改表格的内部行为。 -- 对表格的分页和排序，前台和后台进行分页和  排序。
 
 可编辑表格EditorGrid, 介绍如何添加和删除行，将修改的数据提交后台保存，对输入数据校验和类型限制；
 Ext的特殊组件： 如PropertyGrid GroupingGrid, 以及表格的几种拖放形式。


 <h2 id='4'> 表单与输入控件 </h2>
 
> 97 page Ext表单提供一层封装，可以默认数据校验，提示，ComBox, 选择控件等

 <h4 id='4.1'> FormPanel BasicForm详解 </h4>
 
 FormPanel是Ext.Panel的一个子类，可对其执行各种Panel操作，实际上，表单的功能是在Ext.form.BasicForm对象。 formPanel对象getForm()可获得BasicForm对象。我们可以BasicForm上执行‘提交表单数据’和‘复位表单初始值’等操作。
 引入FormPanel的最大好处在于布局；
 
 <h4 id='4.2'> Ext支持的输入组件 </h4>
 
  * ext支持输入的控件继承关系图 TextField,TextArea,Checkbox, Radio, ComboBox, DateField, HtmlEditor, Hidden ,TimeField; 都继承与Ext.form.Field
  * 表单控件详解（详细demo）
  * Ext.form.Field 是所有表单输入控件的基类，其他控件都是基于此拓展来的。其中定义了一些通用的属性和功能函数，大致3类（页面显示样式，控件参数配置和数据有效性校验）
    - 页面显示样式： clearCls，cls fieldClass focusClass itemCls invalidClass labelStyle等 定义不同状态下输入框的样式
	- 控件参数配置： autoCreate disabled fieldLabel hideLabel inputType labelSeparator name readOnly tabIndex value等，设置输入控件生成的dom内容和标签内容，以及是否禁用，可读等。
	- 数据有效性校验： invalidText, msgFx msgTarget validateOnBlur validateDelay validateEvent等， 校验数据方式以及显示错误信息
  * TextField: 校验非空，最大值 最小值
  * TextArea: 在TextField基础上，可以多行输入，只需设置对一个长度宽度即可；grow:true, area会自动拓展自身高度，而不是宽度，preventScrollbars参数可防止出现滚动条
  * DateField：支持所有Field和TextField功能外，弹出日历，disabledDays可禁止用户选择一周内的特定日期
  * TimeField: 选择时间的输入控件， minValue, maxValue, increment; 设置显示内容
  * Ext.form.HtmlEditor: 可以带一定格式输入内容
  * Ext.form.Hidden: 直接继承自Field, 可通过setValue() getValue()函数对它赋值取值；
  * 如何使用input type="image", 这里讲拓展Field没有的属性，自定义
 
 <h4 id='4.3'> ComboBox详解 p108 </h4>
 
  * comboBox简介
  * select 转换成ComboBox
  * ComboBox结构详解
  * ComboBox读取远程数据
  * comboBox高级配置：如分页，是否允许用户自己填写内容
  * 监听用户选择的数据
  * 使用本地数据实现省，市，县级联
  * MultiSelect,ItemSelector 扩展及示例
  
 <h4 id='4.4'> 复选框和单选按钮 p125 </h4>
 
  * 复选框  
  * 单选框，继承自复选卡箍
  * CheckboxGroup, RadioGroup控件 可以设置排列方式，竖向或横向或自定义
  
 <h4 id='4.5'> 滑动条表单控件 p131 </h4>  
 <h4 id='4.6'> 表单布局 p133 </h4>    
 
 表单布局，在布局中使用fieldset; 在fieldset中使用布局，以及自定义布局
 
 <h4 id='4.7'> 数据校验 p143 </h4>   
 
 输入不为空， 最大长度最小长度， 借助vtype（如email,url,alpha(英文),alphanum（英文数字））； -- 另外可以自定义校验规则；如通过regex: 设置只输入汉字；
 NumberField是TextField子类，有一些自己的校验，如只能输入数字；
 使用后台返回的校验信息；
 
 <h4 id='4.8'> 使用表单提交数据 p148 </h4>    
 
 ext提供了3种提交表单数据的方式， 如原始HTML表单方式，2中ajax形式的提交；
 
 form.getForm().submit(); -- 因为formPanel是布局容器，没有submit()方法，所以先获得BasicForm；  -- 这里要form设置一个url, 给textField加上name;
 
 form.getForm().submit({
	 sucess:function(form,action){
		// 判断方法是返回200
	 }，
	 failure:function(){
		// 判断500 404
	 }
 });
 
 使用HTML原始提交形式：
  el都是由Dom的，dom模型就是ext控件在页面上真实对应的部分，通过handler:function(){ form.getForm().getEl().dom.action=''; form.getForm().getEl().dom.submit();}提交表单数据’和‘复位表单初始值’等操作。
  
 单纯的ajax;
 既然是单纯的ajax,我们要获取数据，这里有几种方式： --form.getValues()函数，有一个参数，true返回json字符串，false返回json对象； --findField()函数，可获取表单里的控件，如有一个名称为text的TextField；
 var text= form.getForm().findField('text');
 
 我们通过最简单的getValues(true)函数来配合ajax获取数据， 用ajax数据传给后台，最后回调代码如下
 
 ```
 handler: function(){
	var text = form.getForm().findField('text');
	
	Ext.Ajax.request({
		method: 'post',
		url: '09_01_01.jsp',
		success:function(response){
			var result = Ext.decode(reponse.responseText);
			Ext.Msg.alert("信息", result.mes);
		},
		failure:function(){
		},
		params: form.getForm().getValues(true);
		
	});
 }
 ```

 这里使用form.getValues(true) 需要注意，得到的字符串不是decode()编码后的值，是name=value的形式，无须处理发给后台，这里有个问题，因为包含了=，& 无法使用encodeURIComponent()函数对它进行编码。
 这样，如果参数中包含中文，就不能用get发送数据了，否则会出现乱码。 在实际使用需要权衡。

>> 文件上传与文件上传控件  p151

 <h4 id='4.9'> 自动数据填充到表单 p153 </h4>   
 
 <h4 id='4.10'> 小结 </h4>   

 本章介绍了 Ext中`表单以及表单输入控件`，通过控件继承图我们可清楚知道`控件之间的相互关系` , 介绍了组件功能以及校验方式， 并介绍了Ext.form.ComboBox, Ext.form.Checkbox,Ext.form.Radio; 以及上传控件的使用方法。
 此外， 介绍了表单的提交，获取数据，校验和布局。 最后介绍了吧数据填充到表单控件中


 <h2 id='5'> 树形结构 </h2> 

> 156 page  树形结构是一个很经典的结构，Ext提供树形控件，方便在B/S中快速开发树信息结构的应用。treepanel继承自 Panel

 <h4 id='5.1'> TreePanel基本使用 </h4>  

 ```
 var tree = new Ext.tree.TreePanel();
 tree.render('tree'); 这里tree表示div的节点id
 
 既然是树形结构，那么就有根root;有了根才有枝叶生长。
 store: new Ext.data.TreeStore({
	root:{
		text:'我是根',
		leaf:true
	}
 })
 1.虽然只有一个根但是是一棵树形结构了，下面添加枝叶节点；
 store: new Ext.data.TreeStore({
	root:{
		text:'我是根',
		children: [{
				text:'1枝',
				children{
					text:'1-1叶子',
					leaf: true
				}
			},{
				text:'1-叶子',
				leaf:true
			}]
		}
 })
 
 // 让加载后的树直接展开：
 Tree.getRootNode().expand(true,true);//parm1 是否展开 parm2 是否要动画效果
 
 2.树形配置
 在tree期望渲染的目标id放到{}中，对应树形为renderTo.
 var tree = new Ext.tree.TreePanel({store:new Ext.data.TreeStore(), renderTo:'tree'}) // renderTo: id='tree'的div
 
 3.使用TreeStore获得数据 （后台交互）
  store: new Ext.data.TreeStore({
	proxy:{
		type:'ajax',
		url:'01-04.txt'
	},
	root:{
		text:'我是根'
	}
  })
  // 小细节； 返回json 中叶子都设置leaf , 非叶子设置children属性，就不会出现无限循环展开问题； 每次点击非叶子节点，都会请求后台，当前id作为参数
  // 通过xml获取数据，165，通过proxy: extraParams:{isXml:true}; reader:{type:'xml'}等参数控制
 ```

 <h4 id='5.2'> 树的事件 </h4>  

> 树的事件模型可以提供很丰富的树触发的事，如节点展开，折叠，单击，双击等； 

```
tree.on("itemexpand",function(node){
	console.info(node,"展开了");
});
// itemcollapse->折叠 itemclick ->单击  itemcontextmenu->右键事件  

tree.on("itemcontextmenu", function(view,record,item,index,e){
	e.preventDefault();
	contextmenu.showAt(e.getXY());
	contextmenu.hide();
});
```

 <h4 id='5.3'> 修改节点图标，提示信息，超链接 </h4>  

 * node: {} node节点中，icon:'user_female.png', iconCls:'icon-male' 可以设置节点图标，icon优先级高
 * node: qtip:'xxx' 参数作为提示信息 ，另//开启提示功能 -> Ext.QuickTips.init() 才可以使用，且一般在其他元素加载后， Ext.onReady(function(){// 这里写})
 * node: href:'07-1.html', hrefTarget:'_blank'
 
 <h4 id='5.4'> 树形拖放 p172 </h4>  
 
 tree= new Ext.tree.TreePanel({
	viewConfig:{
		plugins:{ptype:'treeviewdragdrop'} //实现拖放
	},
	store:
 })

 * 节点拖放3种形式
   - append
   - above
   - below
 * 叶子不能append
 * 判断拖放目标
 * 树之间的拖放 176p

 <h4 id='5.6'> 对树进行排序 </h4>  
 
 设置folderSort参数就可以实现为树形进行排序； 在store对象内加属性 folderSort: true
 
<h4 id='5.7'> 带checkbox树形 178</h4>  
 
 只要为树形节点加checked:true就会显示对应的Checkbox; {text:'leaf no2',leaf: true, checked:flse}
 
 <h4 id='5.8'> 树形与表格的结合 179</h4>  
 
 在Ext中，可以通过拓展treecolumn插件的方式实现表格与树形结合的效果。
 
  <h4 id='5.9'> 更多树形的高级应用 (选中刷新等事件)182</h4>

 * 选中树的某个节点； TreePanel的selectPath()函数，如selectPath('/root/leaf'); 选种id='root'下的id='leaf'节点；  treePanel.getSelectionModel()获取选中模型，sm有个select()函数，传入index或record可选中节点。
 * 刷新树的所有节点： 根节点下有reload()函数，要刷新树，就取到根节点rootNode,再调用reload()函数；
 * 借用grid的缓冲视图插件： 可以在tree中使用grid的缓冲视图插件，在需要渲染成千上万个树形节点时，只渲染当前显示的视图部分；自动监听滚动条的拖曳情况。保证海量数据加载时不卡顿。 plugins:[{ptype:'bufferedrenderer'}]
 * 借用grid的锁定插件：  将某一列锁定，即使出现横向滚动条，锁定内容依然在视图内； 使用方法：{xtype:'treecolumn',text:'任务',locked:true}

  <h4 id='5.10'> 小结183 </h4>

 本章讨论ext树的原理和使用方法，treeStore部分配置方式； 以及树形事件，包括右键菜单，修改节点图标，为节点设置提示信息，设置超链接，修改树形节点名称等内容；
 另外，几种介绍了树形拖放，多种树形拖放形式以及树形节点排序，最后一节介绍了表格与树的结合的拓展件以及树形的一些高级应用内容。

