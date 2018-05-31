w3c地址   https://www.w3cschool.cn/extjs/extjs_first_program.html

1. Ext.onReady（）方法将在Ext JS准备好渲染Ext JS元素时调用。

Ext.create（）方法用于在Ext JS中创建对象，这里我们创建一个简单的面板类Ext.Panel的对象。

Ext.Panel是Ext JS中用于创建面板的预定义类。

每个Ext JS类都有不同的属性来执行一些基本的功能 

例如ext.panel 
Ext.Panel类有以下各种属性:

renderTo 是此面板必须呈现的元素。 \'helloWorldPanel\'是Index.html文件中的div id。

Height 和宽度属性用于提供面板的自定义尺寸。

Title 属性是为面板提供标题。

Html 属性是要在面板中显示的html内容

2. ext.js class 系统 ext.js 创建class
Ext.define(class name, class members/properties, callback function); 
ext.define() 用来定义js类。

ext.js 面向对象语言；
 创建对象： ext.create()
 继承：     ext.extend;

3. ext.js集装箱 ext.js容器

Ext.container.Container是Ext JS中所有容器的基类。
容器Ext.panel.Panel，Ext.form.Panel，Ext.tab.Panel和Ext.container.Viewport是Ext JS中经常使用的容器。

 * Ext.panel.Panel：这是允许在正常面板中添加项目的基本容器. 行排列；

 * Ext.form.Panel：Form面板为表单提供了一个标准容器，它本质上是一个标准的Ext.panel.Panel，它自动创建一个用于管理任何Ext.form.field.Field对象的BasicForm

 * Ext.tab.Panel : 标签面板就像一个普通的面板，但支持卡标签面板布局

 * Ext.container.Viewport：Viewport是一个容器，它会自动调整大小到整个浏览器窗口的大小。 然后，您可以在其中添加其他ExtJS UI组件和容器

4. ext.js 布局 根据每种布局的代码和效果，需要时查阅实现效果。

5. ext.js 组件  ExtJS UI 定义各种UI组件，例如网格grid,窗体form,消息框message box, chart 图表以及html editorHTML编辑器。 自己按需选择实现效果。

6. ext.js 拖放  拖放动作执行时，代码实现某些效果

7. ext.js 主题  主题，背景效果

8. ext.js 自定义事件和监听器 自定义；

9. ext.js 数据 *重*  model它将存储数据绑定到视图,   store它包含本地的缓存数据（静态数据或后台json数据）,    proxy 代理由model和store用于处理模型数据的加载和保存。

10. ext.js 字体 字体模板

11. ext.js 风格 style，类似css模板

12.ext.js 图像 绘图

13.ext.js 本地化   设置页面展示语言language

14.ext.js 可访问性  为不同人群提供阅读体验，如盲人，提供阅读功能

15.ext.js 调试代码 console.log() 或浏览器调试

16.ext.js 方法 主要有浏览器和服务端信息获取，日期，字符串处理。 Ext.is 检查使用的平台   Ext.supports 表示该类关于当前环境的支持信息   ext.string 处理字符串类
 





























