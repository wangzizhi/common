# 初识Css
@(我的第一个笔记本)[浏览器, CSS语法, 小技巧]

##引言

想知道css是怎么工作的？那我们先得从我们经常用到的这个软件-->**浏览器**说起！
就目前来看网络浏览器已经成为了使用最广的软件之一。我们现在的工作也基本围绕它展开的。
但是你真的了解这个我们熟悉的陌生“人”么？不了解？没关系！下面我们就一起来了解下。
##浏览器简介
目前使用的主流浏览器有五个：Internet Explorer、Firefox、Safari、Chrome 浏览器和 Opera。本文中以开放源代码浏览器为例，即 Firefox、Chrome 浏览器和 Safari（部分开源）。根据 StatCounter 浏览器统计数据，2016 年 1 月 Firefox、Safari 和 Chrome 浏览器的总市场占有率将近 60%。由此可见，如今开放源代码浏览器在浏览器市场中占据了非常坚实的部分。
##浏览器的主要功能
浏览器的主要功能就是向服务器发出请求，在浏览器窗口中展示您选择的网络资源。这里所说的资源一般是指 HTML 文档，也可以是 PDF、图片或其他的类型。资源的位置由用户使用 URI（统一资源标示符）指定。

浏览器解释并显示 HTML 文件的方式是在 HTML 和 CSS 规范中指定的。这些规范由网络标准化组织 [W3C][1]（万维网联盟）进行维护。
多年以来，各浏览器都没有完全遵从这些规范，同时还在开发自己独有的扩展程序，这给网络开发人员带来了严重的兼容性问题。如今，大多数的浏览器都是或多或少地遵从规范。

##浏览器的高层结构
浏览器的主要组件为 ：

1. **用户界面** - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。
2. **浏览器引擎** - 在用户界面和呈现引擎之间传送指令。
3. **呈现引擎** - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。
4. **网络** - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。
5. **用户界面后端** - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。
6. **JavaScript 解释器**。用于解析和执行 JavaScript 代码。
7. **数据存储**。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。
<br>
![Alt text](./1489201505072.png)
 >**值得注意的是**，和大多数浏览器不同，Chrome 浏览器的每个标签页都分别对应一个呈现引擎实例。每个标签页都是一个独立的进程。

##浏览器的工作流程
浏览器工作主要分为三个步骤：
1. **加载**：
- 加载过程中遇到外部css文件，浏览器另外发出一个请求，来获取css文件。
- 遇到图片资源，浏览器也会另外发出一个请求，来获取图片资源。这是异步请求，并不会影响html文档进行加载，但是当文档加载过程中遇到js文件，html文档会挂起渲染（加载解析渲染同步）的线程，不仅要等待文档中js文件加载完毕，还要等待解析执行完毕，才可以恢复html文档的渲染线程。
2. **解析**：
- html文档解析生成解析树即dom树，是由dom元素及属性节点组成，树的根是document对象。
- css解析将css文件解析为样式表对象。该对象包含css规则，该规则包含选择器和声明对象。
3. **渲染**：
- 渲染引擎开始于从网络层获取请求内容，一般是不超过8K的数据块。接下来就是渲染引擎的基本工作流程。

- 渲染引擎的基本工作流程
- ![Alt text](./1489203986527.png)
- ![Alt text](./1489402004859.png)
（解析HTML构建DOM树，渲染树构建，渲染树布局，绘制渲染树）

- 渲染引擎会解析HTML文档并把标签转换成DOM节点到一个树中（这里我们称这个树为 “内容树”）。它会解析style元素和外部文件中的样式数据。样式数据和HTML中的显示控制将共同用来创建另一棵树——渲染树。

- 渲染树包含带有颜色，尺寸等显示属性的矩形。这些矩形的顺序与显示顺序一致。

- 渲染树构建完后就开始参与“布局”处理，这里的意思就是确定每个节点一个在屏幕上的精确显示位置。接下来的阶段是绘制 —— 遍历渲染树并用UI后端层将每一个节点绘制出来。

- 一定要理解这是一个缓慢的过程，为了更好的用户体验，渲染引擎会尝试尽快的把内容显示出来。它不会等到所有HTML都被解析完才创建并布局渲染树。它会在处理后续内容的同时把处理过的局部内容先展示出来。
>**渲染最大的一个困难就是为每一个dom节点计算符合他的最终样式**。

为每一个元素查找到匹配的样式规则，需要遍历整个规则表。
``` css
 #test p{ color:#999999}
```
遍历是自右向左，也就是先查询到p元素，再找到上一级id为test的元素，也就是是从树的低端向上遍历。
**计算样式的一些困难：**

>1. 样式数据是非常大的结构，保存这样是的数据是很耗内存的。
>2. 选择器迭代太深，造成太多的无用遍历。
>3. 样式规则涉及非常复杂的级联，定义了规则的层次（理解：<head>里引用的外部样式表，会被局部样式表中同一属性的设置取代。还有例如body内对font的设置本来会应用于孩子元素，但是如果body的孩子元素定义font属性，则会被后者取代）。

**根据对计算样式困难的理解，我们在编写css样式表时应该注意一下：**

>1. dom深度尽量浅。
>2. 减少inline javascript、css的数量。
>3. 使用现代合法的css属性。
>4. 不要为id选择器指定类名或是标签，因为id可以唯一确定一个元素。
>5. 不要给类选择器指定标签，类，代表具有一类属性的标签，不仅是一个，虽然可以实现，但是降低了效率。
>6. 避免后代选择符，尽量使用子选择符。原因：子元素匹配符的概率要大于后代元素匹配符。后代选择符;#tp p{} 子选择符：#tp>p{}
>7. 避免使用通配符，举一个例子，.mod .hd *{font-size:14px;} 根据匹配顺序,将首先匹配通配符,也就是说先匹配出通配符,然后匹配.hd（就是要对dom树上的所有节点进行遍历他的父级元素）,然后匹配.mod,这样的性能耗费可想而知.
<br>

**介绍在渲染部分俩个重要的概念：一个是Reflow，另一个是Repaint。这两个不是一回事。**

- Repaint——屏幕的一部分要重画，比如某个CSS的背景色变了。但是元素的几何尺寸没有变。
- Reflow——意味着元件的几何尺寸变了，我们需要重新验证并计算Render Tree。是Render Tree的一部分或全部发生了变化。这就是Reflow，或是Layout。（HTML使用的是flow based layout，也就是流式布局，所以，如果某元件的几何尺寸发生了变化，需要重新布局，也就叫reflow）reflow 会从<html>这个root frame开始递归往下，依次计算所有的结点几何尺寸和位置，在reflow过程中，可能会增加一些frame，比如一个文本字符串必需被包装起来。

http://v.youku.com/v_show/id_XMzI5MDg0OTA0.html?firsttime=0

Reflow的成本比Repaint的成本高得多的多。DOM Tree里的每个结点都会有reflow方法，一个结点的reflow很有可能导致子结点，甚至父点以及同级结点的reflow。在一些高性能的电脑上也许还没什么，但是如果reflow发生在手机上，那么这个过程是非常痛苦和耗电的。

所以，下面这些动作有很大可能会是成本比较高的。

>当你增加、删除、修改DOM结点时，会导致Reflow或Repaint
当你移动DOM的位置，或是搞个动画的时候。
当你修改CSS样式的时候。
当你Resize窗口的时候（移动端没有这个问题），或是滚动的时候。
当你修改网页的默认字体时。
注：display:none会触发reflow，而visibility:hidden只会触发repaint，因为没有发现位置变化。

多说两句关于滚屏的事，通常来说，如果在滚屏的时候，我们的页面上的所有的像素都会跟着滚动，那么性能上没什么问题，因为我们的显卡对于这种把全屏像素往上往下移的算法是很快。但是如果你有一个fixed的背景图，或是有些Element不跟着滚动，有些Elment是动画，那么这个滚动的动作对于浏览器来说会是相当相当痛苦的一个过程。你可以看到很多这样的网页在滚动的时候性能有多差。因为滚屏也有可能会造成reflow。

基本上来说，reflow有如下的几个原因：

>Initial。网页初始化的时候。
Incremental。一些Javascript在操作DOM Tree时。
Resize。其些元件的尺寸变了。
StyleChange。如果CSS的属性发生变化了。


**如何减少reflow/repaint**

>1）不要一条一条地修改DOM的样式。与其这样，还不如预先定义好css的class，然后修改DOM的className。
``` css
// bad
var left = 10,
top = 10;
el.style.left = left + "px";
el.style.top  = top  + "px";

// Good
el.className += " theclassname";

// Good
el.style.cssText += "; left: " + left + "px; top: " + top + "px;";
```
>2）不要把DOM结点的属性值放在一个循环里当成循环里的变量。不然这会导致大量地读写这个结点的属性。
3）尽可能的修改层级比较低的DOM。当然，改变层级比较底的DOM有可能会造成大面积的reflow，但是也可能影响范围很小。
4）为动画的HTML元件使用fixed或absoult的position，那么修改他们的CSS是不会reflow的。
5）千万不要使用table布局。因为可能很小的一个小改动会造成整个table的重新布局。

**通过对上面的了解更能凸显出react的三大优势**

1. 虚拟节点。在UI方面，不需要立刻更新视图，而是生成虚拟DOM后统一渲染。
2. 组件机制。各个组件独立管理,层层嵌套，互不影响，react内部实现的渲染功能。
3. 差异算法。根据基本元素的key值，判断是否递归更新子节点，还是删除旧节点，添加新节点。

##CSS的样式检查

之前用的大多数是针对每个不同CSS文件进行的针对性的样式检查，主要有scss-lint、sass-lint、css-lint。进行分类型的检查。现在主要用的是sytle-lint 。 style-linter由PostCSS提供支持，因此它理解PostCSS可以解析的任何语法，包括SCSS，SugarSS和Less的实验支持。

据我了解，最新的webstorm 2016.3已经集成了[style-lint][2]语法检查。Atom可以直接安装扩展**linter-stylelint**并配置个.stylelintrc文件 重启atom便可以生效。

用之前我的css代码是这样的![Alt text](./QQ20170314-124741.png)

用之后我的css代码是这样的![Alt text](./QQ20170314-124852.png)

##最后分享几个css小技巧

1.尽量减少改动是需要编辑的地方

   当某些值相互依赖时应该他们的相互关系用代码表达出来
   line-height ;line-height可以用字号的倍数来表示
   单位用em来代替绝对值。能使代码维护起来更加便捷
   代码的维护和代码的量**少**不可兼得：
``` css
   //bad
   border-width: 10px 10px 10px 0;
   //Good
   border-width: 10px;
   border-left-width: 0;
```
2.合理使用简写
``` css
  background: red;
  background-color: red;
```
前者是简写，它可以确保你得到red的纯色背景，后者背景变为图片，如果同时又background-image申明在起作用的话，展开式的写法不会帮助你清空所有相关属性，从而会干扰你想要表达的效果。
**合理使用简写是一个良好的防卫性编码方式，可以抵御未来的风险。**

3.calc()函数-->计算

CSS函数calc()可以用在任何一个需要length、frequency, angle、time、number、或integer的地方。有了calc()，你就可以通过计算来决定一个CSS属性的值了。
``` css
  .color-line {
      padding: 3px;
      width: calc(100% - 30px);
      display: inline-block;
      vertical-align: middle;
    }

  .color-cirle {
      width: 30px;
      height: 30px;
      border-radius: 15px;
      display: inline-block;
      text-align: center;
      line-height: 30px;
      font-size: 1.2em;
      color: #fff;
  }

```
![Alt text](./1489399946014.png)
``` css
 .foo {
      --widthA: 100px;
      --widthB: calc(var(--widthA) / 2);
      --widthC: calc(var(--widthB) / 2);
      width: var(--widthC);
  }
```
在所有的变量都被展开后, widthC 的值就会变成 calc( calc( 100px / 2) / 2)，然后当它被赋值给 .foo 的 width属性 时，所有内部的这些calc()（无论嵌套的有多深）都将会直接被“拍”成一个括号（原文：be flattened to just parentheses），所以这个 width属性 的值就直接相当于 calc( ( 100px / 2) / 2)了，或者说就变成25px了。 简而言之：一个 calc() 里面的 calc() 就仅仅相当于是一个括号。
``` css
.footer {
   padding: 1em;
   padding: 1em calc(50%-450px);
   background: #333;
}
```
实现满幅的背景，顶宽的内容
现在基本的写法
``` vbscript-html
<footer>
	<div class="wapper"></div>
</footer>
```
``` css
.footer {
   background: #333;
}
.wrapper {
   max-width: 900;
   margin: 1em atuo
}
```
**垂直居中也可以**
``` vbscript-html
<footer>
	<h1>12312312312313</h1>
	<p>123123123sdfsf</p>
</footer>
```
``` css
.footer {
   position: absolute;
   top: calc(50%-3em);
   left: calc(50%-9em);
   width: 18em;
   height: 6em;
}
```
当然这个也有缺点就是它要求元素的宽高是固定的。

最佳方案是基于FlexBox的方案，

欲知怎样解决，请看下回分解！
<div style="text-align:center;font-size:3em;color:red">谢谢大家</div>

  [1]: https://zh.wikipedia.org/wiki/%E4%B8%87%E7%BB%B4%E7%BD%91%E8%81%94%E7%9B%9F
  [2]: https://stylelint.io/
