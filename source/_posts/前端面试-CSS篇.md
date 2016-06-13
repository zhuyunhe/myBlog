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
	1. 新增各种CSS选择器，例如`:not(.input)`选择所有class不是‘input’的元素  
	2. 圆角，`border-radius:8px`  
	3. 阴影和反射，Shadow\Reflect  
	4. 文字特效，text-shadow
	5. 文字渲染，text-decoration
	6. 线性渐变，gradient
	7. 动画：旋转、缩放、移动、倾斜等

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

[参考](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)