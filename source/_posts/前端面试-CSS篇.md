---
title: 前端面试-CSS篇
date: 2016-06-13 16:35:26
tags: 
- 面试
- CSS
categories: 学习笔记
---
不断补充总结的一些前端面试中关于CSS的问题<!-- more -->  

- 介绍一下CSS盒模型？  
	`CSS中的一个基本概念是盒模型（box model）。可见元素会在页面中占据一个矩形区域，该区域就是元素的盒子（box）。`
	`CSS有两种盒模型：IE盒模型和标准W3C盒模型`  
	`盒模型有四个基本属性，从内到外依次是：content、padding、border、margin`  
	`标准W3C盒模型的content不包括padding和border，IE盒模型包括。`  

- CSS选择器有哪些？哪些属性可以继承？  
	1. id选择器（#id）  
	2. 类选择器（.class）  
	3. 标签选择器（div、p、ul）  
	4. 相邻选择器（h1+p）  
	5. 直接子元素选择器(ul>li)  
	6. 后代选择器(li a)  
	7. 通配符选择器（*）  
	8. 属性选择器（a[rel="external"]）  
	9. 伪类选择器（a:hover,li:nth-child(n)）  
	`可继承的样式：font-size、font-family、color`  
	`不可继承的样式：border、padding、margin、width、height`

- CSS优先级算法如何计算？
	`优先级就近原则，权重相同的情况下以最近定义者为准，即后定义的样式会覆盖前面定义的样式`  
	`!important > 内联样式 > id > class > tag`  
	`div{} 权重为：1`  
	`.class{} 权重为：10`  
	`#id{} 权重为：100`  
	`.class div{} 权重为：11`  
	`#id div{} 权重为：101`  
	`#id .class div{} 权重为：111`   

- CSS3有哪些新特性？  
	1. 新增各种CSS选择器，例如，新的伪类：`:not(.input)`选择所有class不是‘input’的元素;子串匹配的属性选择器E[attribute^="value"]；伪元素使用两个冒号而不是一个来表示：:after变为::after等。 
	2. 圆角，`border-radius:8px`，还有border-top-left-radius,border-top-right-radius,border-bottom-left-radius,border-bottom-right-radius。
	3. 背景属性：background-origin，background-size，background-clip等 
	4. 阴影和反射，Shadow\Reflect  
	5. 文字特效，text-shadow
	6. 文字渲染，text-decoration
	7. 线性渐变，gradient
	8. 动画：旋转、缩放、移动、倾斜等

- 解释一下CSS3的Flexbox（弹性盒布局模型），以及适用场景？
	`CSS3弹性盒布局模型，是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当排布行为的布局方式。对很多应用程序来说，由于不使用浮动，且弹性容器的外边距也不会与其内容的外边距合并，弹性盒模型比方框模型要好用一些。使用flexbox，我们可以告别使用浮动的div元素、绝对定位和一些JavaScript hacks，仅仅几行CSS就可以构建水平或垂直方向的布局。`  
	`一些基本使用案例：`  
	`1. 将一个元素放置在页面中间`
	`2. 一组在垂直方向可以一个紧挨一个的布局容器`
	`3. 创建一行按钮或其他元素，这些元素在小屏幕上可以垂直折叠显示`  

- 使用纯CSS3创建一个三角形的原理？
	`把元素的高度宽度设为0，只设置边框的宽度，然后隐藏掉其中三条边（颜色设为transparent）`  
		
		//指向上的小三角，隐藏掉上、右、左三边
		#demo{
			width: 0;
			height: 0;
			border-width: 20px;
			border-style: solid;
			border-color: transparent transparent red transparent;
		}

- 为什么要初始化CSS样式？
	`因为浏览器的兼容问题，不同浏览器对有些标签的默认样式是不同的，如果没有对CSS初始化，那么页面在不同浏览器之间会表现出差异。`

- 设置元素浮动后，该元素的display值是什么？
	`不管浮动前是什么类型的元素，设为浮动后一律变为块元素，display的值为block。`

- 如果要手动写动画，你认为最小时间间隔是多久，为什么？
	`多数显示器默认频率是60HZ，即1秒刷新60次，所以理论上最小时间间隔为(1/60)*1000ms = 16.7ms`

- display: inline-block
	`移除元素标签间的空格、使用margin负值、使用font-size：0、letter-spacing、word-spacing`

- ::before和:after中双冒号和单冒号有什么区别？解释一下这2个伪元素的作用。
	`简而言之就是：单冒号（:）记法用于CSS3伪类，双冒号（::）记法用于CSS3伪元素`  
	[W3C关于CSS3的伪类选择器](https://www.qianduan.net/before-and-before-the-difference-between/)有这样一段描述：
	> Pseudo-elements create abstractions about the document tree beyond those specified by the document language. For instance, document languages do not offer mechanisms to access the first letter or first line of an element's content. Pseudo-elements allow designers to refer to this otherwise inaccessible information. Pseudo-elements may also provide designers a way to refer to content that does not exist in the source document (e.g., the ::before and ::after pseudo-elements give access to generated content).  
	A pseudo-element is made of two colons (::) followed by the name of the pseudo-element.  
	This :: notation is introduced by the current document in order to establish a discrimination between pseudo-classes and pseudo-elements. For compatibility with existing style sheets, user agents must also accept the previous one-colon notation for pseudo-elements introduced in CSS levels 1 and 2 (namely, :first-line, :first-letter, :before and :after). This compatibility is not allowed for the new pseudo-elements introduced in CSS level 3.
	
	大概翻译过来就是：伪元素会创建超出文档语言定义的关于文档树的抽象。例如，文档语言并不提供来访问一个元素内容的第一字母或第一行的机制。伪元素允许设计人员引用到这些本来难以接触的信息。伪元素有时也提供设计者一种方法去引用那些在源文档中并不存在的内容。（例如,::before和::after这两个伪元素能访问其生成的内容）  
	伪元素由两个冒号::紧接伪元素名称构成。双冒号是在当前规范中引入中，用于区分伪类和伪元素。为了兼容现有的样式表（CSS1，CSS2），浏览器还需要支持旧的单冒号的伪元素写法，比如:first-line,:first-letter,:before和:after。不过，CSS3中新推出的伪元素不支持这种单冒号的兼容性写法。  
	所以，对于CSS2之前的伪元素，比如:before,:after等，单冒号和双冒号的写法是一样的。  
	如果你的网站只需要兼容webkit、firefox、opera等现代浏览器，建议对于伪元素采用双冒号的写法。对于IE的话，还是写单冒号安全一些。 

- 什么是CSS预处理器/后处理器？
	1. 预处理器：常见的预处理器有LESS、SASS、Stylus，用来预编译less或sass文件，增强css代码的复用性。  
	预处理器都有层级、mixin、变量、循环、函数等功能，能够方便地进行UI组件模块化开发能力，极大提高工作效率。
	2. 后处理器：PostCSS，通常被视为在完成的样式表中根据CSS规范处理CSS，让其更有效。目前最常做的是给CSS属性添加浏览器私有前缀，实现跨浏览器兼容性问题。

- 简述一下CSS中position属性的四种常见定位属性的区别？。  
	`在CSS中使用position属性来指定元素的定位类型，该属性有四种不同类型的定位，分别为static、relative、absolute、fixed。`  
	`要理解以上四种定位，先看一下CSS的文档流（普通流）的概念：`  
	`除非专门指定，否则所有框（box，也就是html元素）都在普通流中定位。也就是说，普通流中的元素位置由元素在HTML中的位置决定。`  
	`块级框从上到下一个接一个地排列，框之间的垂直距离是由框的竖直方向外边距计算出来。若某元素的position属性的是默认的static，那么这个元素就在文档流中。`  
	1. relative相对定位  
	设置为相对定位的元素不脱离文档流，参考自身的位置通过top、bottom、left和right进行定位，让这个元素以自身原来的位置为基准进行移动，元素仍然保持其未定位前的形状，它原本所占的空间仍然保留（因为它没有脱离文档流）。因此，采用相对定位的元素有可能会覆盖其他元素，因为它其实占据了两个位置，一个是移动前的位置，一个是移动后的位置，若移动后的位置和别的元素冲突了，就会把别的元素覆盖了。
	2. absolute绝对定位和fix固定定位  
	与relative不同，position值为absolute和fixed的元素脱离了文档流，元素原先在正常文档流中所占的空间会关闭，就像该元素不存在一样。  
	absolute元素的位置相对于最近的已定位的（封闭的，即position属性不为static）祖先元素，若该元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。  
	fixed元素相对于浏览器窗口定位，即将设置为fixed的元素固定在浏览器窗口的某个位置上，即使拖动浏览器的滚动条，该元素的位置也不会改变。  
	在CSS中可以使用float属性用于改变元素对象的默认显示方法。当元素设置了float属性后，它将不在独占一行，而是可以浮动到左侧或右侧，直到浮动框的外边缘碰到包含框或另一个浮动框的边框为止。注意：浮动元素也不在文档流中，因此对文档流中普通块元素来说，浮动框就像不存在一样，这一点和absolute类似，但float和absolute还有以下不同：  
		1. absolute元素的位置相对于离它最近的已定位（封闭的）祖先元素，它可以以父元素4条边为基准进行定位。而float属性定位时则是根据left或right属性值，以父元素的左上或右上为基准进行定位。
		2. 采用absolute定位的元素不能被文本所包围，而浮动元素可以被文本包围（float最初设计的用意就是这个，用以取代HTML中的align属性）。
		3. float的影响可控，absolute的影响不可控  
		设置float和absolute属性的元素都脱离了文档流，因此它们都会影响到其下方的元素。但是，absolute是布局属性，使用时没有一种有效的方法使之与其下方的元素不重合在一起。相反，若一个元素指定了float属性，当我们向其下方（后后面）的元素应用了clear属性（clear:left,clear:right,clear:both）后，其后的元素就不再受影响了。

- 如何实现一个左右固定宽度200px，中间自动占满剩余位置的三栏布局。
	1. 传统圣杯布局  
	首先把这三栏放置在一个容器中，然后把容器的左右内边距设为200px。  
	把三栏都设为左浮动，中间栏的宽度设为100%，左右两栏的宽度设为200px，同时设左栏的margin-left:-200px，设右栏的margin-right:-200px。  
	
			<style>
				section{
					position: relative;
					padding: 0 200px;
					height: 200px;
					background-color: black;
				}
				.left{
					position: relative;
					float: left;
					width: 200px;
					height: 200px;
					background-color: red;
					margin-left: -200px;
				}
				.right{
					position: relative;
					float: left;
					width: 200px;
					height: 200px;
					background-color: yellow;
					margin-right: -200px;
				}
				.center{
					position: relative;
					float: left;
					width: 100%;
					height: 200px;
					background-color: green;
				}
			</style>
			<body>
				<section>
					<div class="left">left</div>
					<div class="center">center</div>
					<div class="right">right</div>
				</section>
			</body>
	2. flexbox弹性盒模型的圣杯布局  
	使用弹性盒模型，我们可以避免使用浮动元素，弹性子元素可以在任何方向上排布，并且“弹性伸缩”其尺寸，既可以增加尺寸以填满未使用的空间，也可以收缩尺寸避免父元素溢出。  
	代码就不贴了，我做了个[demo](http://zhuyunhe.com:8080/src/flexbox/home.html)可以F12看一下。
	
	
[参考](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)