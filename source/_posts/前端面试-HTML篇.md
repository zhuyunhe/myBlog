---
title: 前端面试-HTML篇
date: 2016-06-13 11:37:34
tags:
- 面试
- HTML
categories: 学习笔记

---
自己总结的一些前端面试中关于HTML方面一些常见问题。<!-- more -->  

- doctype的作用？标准模式与兼容模式有何区别？  
	`1.<!doctype>声明位于HTML文档的第一行，处于<html>标签之前。告知浏览器的解析器用什么文档标准解析这个文档。doctype声明不存在或不正确将会导致文档以兼容模式呈现。`
	`2.标准模式的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。`
- HTML5为什么只需要写<!doctype html>？
	`HTML5不基于SGML（Standard Generalized Markup Language,标准通用标记语言），因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为。而HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。`

- 行内元素有哪些？块级元素有哪些？空（void）元素有哪些？
	`首先：CSS规范规定，每个元素都有display属性，确定该元素的类型，每个元素都有默认的display值，如div的display属性默认值为“block”。`  
	`行内元素：a、b、span、input、img、select、strong`  
	`块级元素：div、p、ul、ol、li、dl、dt、dd、h1、h2...h6`  
	`空元素：br、hr、img、input、link、meta、area、base、col、command、embed、keygen、param、source、track、wbr`

- 行内元素和块级元素的区别？
	`块级元素会独占一行，默认情况下，其宽度自动填满其父元素宽度；行内元素不会独占一行，相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，其宽度随着元素中内容多少而变化。`  
	`块级元素可以设置width、height属性。行内元素设置width、height属性无效。块级元素即使设置了宽度没有占满父元素宽度，仍然是独占一行。`  
	`块级元素可以设置margin和padding属性；行内元素只有左右边距起作用，即margin-left,margin-right,padding-right,padding-left`  
	`块级元素对应的display:block;行内元素为display:inline`

- 页面导入样式时，使用link和@import有何区别？
	`1.link属于XHTML标签，除了加载css外，还可以用于定义RSS，定义rel连接属性等；而@import是CSS提供的，只能用于加载CSS。`
	`页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完成后再加载。  `
	`@import是CSS2.1提出的，只有在IE5之上才能被识别，而link是XHTML标签，无兼容性问题。`
- 简述一下对浏览器内核的理解，并列举一下常见的浏览器的内核？
	`浏览器内核主要分为两个部分：渲染引擎（Layout Engineer或Rendering Engineer）和JS执行引擎。`
	`渲染引擎：负责取得网页内容（HTML、XML、图像等）、整理信息（加入CSS）、计算网页的显示方式然后输出到显示器或打印机。不同的浏览器内核对不同的网页语法解释规则不同。`
	`JS执行引擎：解析和执行JavaScript来实现网页动态交互，著名的有Chrome的V8。  `
	`渲染引擎和JS执行引擎最初没有明确区分，随着JS执行引擎越来越独立，内核更倾向于指渲染引擎。  `
	`Trident内核：IE、TT、The World、360、搜狗浏览器`  
	`Gecko内核：FF、Netscape`  
	`Presto内核：Opera`  
	`Webkit内核：safari、Chrome（Chrome采用的Blink引擎是Webkit的分支）`
- H5有哪些新特性、移除了哪些元素？如何处理H5新标签的浏览器兼容问题？如何区分HTML和HTML5？
	`HTML5已经不再是SGML的子集，主要是关于图像，位置，存储，多任务等功能的增加。`  
	`绘画：canvas`  
	`媒体：video和audio`  
	`本地离线存储localStorage长期存储数据，浏览器关闭后数据也不丢失。`  
	`sessionStorage存储数据，浏览器关闭后自动删除`  
	`语义化更好的内容元素，比如article、footer、header、nav、section`  
	`表单控件：calendar、date、time、email、url、search`  
	`新技术：webworker、websocket、Geolocation`  
	`移除的元素：`  
	`纯表现的元素：basefont、big、center、font、s、strike、tt、u`  
	`对可用性产生负面影响的元素：frame、frameset、noframes`
	`支持HTML5新标签：`
	`IE6，7，8支持通过document.createElement方法产生的标签，可以利用这一特性让这些浏览器支持HTML5新标签。浏览器支持新标签后，还需要添加标签默认样式。当然也可以使用成熟的框架：html5shim`  
	`如何区分HTML5：doctype声明、新增的结构元素、功能元素`
- 简述一下你对HTML语义化的理解？  
	`用正确的标签做正确的事情。`  
	`html语义化让页面的内容结构化，结构更清晰，便于浏览器、搜索引擎的解析`  
	`即使在没有CSS样式的情况下也以一种文档格式显示，并且更容易阅读。使阅读源代码的人更容易将网站分块，便于维护理解。`  
	`搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO。`    
- 请描述一下cookies、sessionStorage和localStorage的区别？  
	`cookie是网站为了标示用户身份而存储在用户本地终端（client side）上的数据`  
	`cookie数据始终在同源的http请求中携带，即会在浏览器和服务器间来回传递。`  
	`sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。`  
	`存储大小：`
	`cookie数据大小不能超过4k`
	`sessionStorage和localStorage虽然也有存储大小限制，但比cookie大得多，可以达到5M或更大`  
	`有效时间：`  
	`localStorage-数据持久存储，浏览器关闭后数据不丢失，除非自己手动删除`  
	`sessionStorage-数据在当前浏览器窗口关闭后自动删除`
	`cookie-在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭`

- iframe有什么缺点？  
	`iframe会阻塞主页面的onload事件`  
	`搜索引擎的检索程序无法解读这种页面，不利于SEO`  
	`iframe和主页共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。`  
	`如果要使用iframe，最好是通过JavaScript动态地给iframe添加src属性值。`  

- label标签的作用是什么？怎么用？
	`label标签用来定义表单控件间的关系，当用户选择该标签时，浏览器会自动将焦点转到和这个label标签相关的表单控件上。`  
	`表单控件可以直接包含在label标签中，也可以用label标签的for属性来关联。`  

		<label>click me <input type="text"></label>  
		<label for="username">click me</label>
		<input type="text" id="username">  

- 如何实现浏览器内多个标签之间的通信？
	`WebSocket、SharedWorker`  
	`也可以调用localStorage、cookies等本地存储方式`
	`localStorage在另一个浏览上下文里被添加、修改或删除时，都会触发一个事件，通过监听事件控制它的值来进行页面间的通信。`

- Unicode和UTF-8的区别和联系？
	`Unicode是一套复杂的字符编码标准，是一个字符集，UTF-8是在这个字符集基础上的一种具体的编码实现方案。`  
	`Unicode相当于中文，UTF-8、UTF-16相当于楷书、行书等各种中文写法。`  
	[字符编码小结](http://blog.csdn.net/zhuyunhe/article/details/50611340)

- gif、png、jpg、webp等图片格式的区别和优势？
	`首先，gif,png,jpg,webp,apng都属于位图（点阵图），而不是矢量图。`  
	`GIF(Graphics Interchange Format,图形交换格式)是一种位图图形文件格式，以8位色（即256种颜色）重现真彩色图像。它实际上是一种压缩文档，采用LZW压缩算法进行编码，有效减少图像文件在网络上传输时间。`  
	`优点：体积小；可插入多帧实现动画效果；可设置透明色产生悬浮效果。`  
	`缺点：采用8位压缩，最多只能处理256种颜色，不宜用于真彩图像。`  

	`PNG(Portable Network Graphics, 便携式网络图片)，是一种无损数据压缩位图图形文件格式。PNG格式是无损数据压缩的，有8位，24位，32位三种形式。PNG就是为了取得GIF而生的。`  
	`优点：压缩比高，色彩好`  
	`缺点：有时文件过大` 

	`jpg/jpeg(Joint Photographic Experts Group)是一种针对相片影像而广泛使用的一种失真压缩标准方法。jpg的压缩方式通常是破坏性有损压缩，每次对照片处理都是一个不可逆的累积劣化过程。`  
	`优点：对色调及颜色平滑的相片能有很好的效果，常用于网上存储和传输照片。`  
	`缺点：不适合于线条绘图（drawing）和文字图示（iconic）等图形。`  
	
	`webp格式是2010年谷歌推出的图片格式，专门用在web中使用，压缩率只有jpg的2/3或更低`  
	`优点：体积小巧`  
	`缺点：兼容性不好，只有opera和chrome支持。但可以使用插件让所有浏览器都支持webp格式`
	[参考](http://www.cnblogs.com/diligenceday/p/4472035.html)