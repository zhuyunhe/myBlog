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


[参考](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)