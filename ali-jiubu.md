### 阿里前端出品 基础教程 九部 笔记
 * https://www.yuque.com/fe9/basic

## debugger
* 在source文件改代码ctrl+s,可以直接改js文件效果，不用改源文件，调试方便
* 调试方式debugger,source断点,另外一个是对dom元素,右键break on 监听dom元素的一些变化（自己不常用）

## css
### css盒模型
* css 盒模型分为标准盒模型和ie盒模型
* 标准盒模型，是 W3C 的规范；另外的一种是老的 IE 浏览器在怪异模式下使用自己的非标准模型，也可称为 IE 盒模型

### 标准盒模型：
* 元素的 width、height 只包含内容 content，不包含 border 和 padding 值

### IE盒模型： 
* 元素的 width、height 不仅包括 content，还包括 border 和 padding；
* 盒子的大小取决于 width、height，修改 border 和 padding 值不能改变盒子的大小。

只要设置了合适的 DTD，大多数浏览器会按照标准盒模型来显示，但是 IE5.X 和 6 在怪异模式下会根据 IE 盒子模型进行显示。
标准盒模型下元素的 box-sizing 属性（IE8+）默认值为 content-box，将它设置成 border-box 可转换为 IE 盒模型。在实际应用场景中，若想控制元素总宽高保持固定，这个设置很有用

