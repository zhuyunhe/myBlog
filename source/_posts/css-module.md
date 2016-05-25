---
title: 'css Module'
date: 2016-05-22 14:28:55
tags:
- 学习
- css
categories: 学习笔记
---
# CSS Module--CSS模块化
CSS全称是Cascading Style Sheet，层叠样式表(ps:如果要对css有更充分的了解，建议看一下黄玄大神的一篇[slide](http://huangxuan.me/2015/12/28/css-sucks-2015/))。说起层叠，在CSS中我们会经常和两个东西打交道，一个是优先级(Priority)，还有一个就是继承(inheritance)。下面是一个简单的demo：  
![demo](/image/css_module/demo.png)  
了解css的同学应该都能看得明白，之所以在CSS中出现这样的问题，是因为本身CSS不是一门程序语言，所有的样式规则都位于同一个作用域下，无论你引入多少个css文件，以什么方式引入，所有的样式规则都位于同一个作用域。  
### 策略
为了减少css中层叠关系的互相影响，避免预料之外的样式覆盖，我们在写css时一般都会遵守一个命名规约，来合理的组织css代码。CSS Modules就是一种有效组织css代码的策略。
### 技术流的模块化
CSS Modules是一种技术流的组织css代码的策略，它将为css提供默认的局部作用域。在github的[CSS Modules](https://github.com/css-modules/css-modules)官方介绍中是这样定义的：
> “A CSS Module is a CSS file in which all class names and animation names are scoped locally by default. All URLs (url(...)) and @imports are in module request format (./xxx and ../xxx means relative, xxx and xxx/yyy means in modules folder, i. e. in node_modules).”

翻译过来就是一个css module就是一个css文件，在这个文件中所有的类命名和动画命名的作用域默认都是局部的，所有涉及到URL网址和@import引入的地方都采用模块请求格式。  
css modules会编译成一个被称为ICSS（Interoperable CSS）低级别的数据交换格式，但是可以像普通css文件那样写。  
	
	/* style.css */
	.className{
		color:green;
	}
当从一个js模块中导入css module时，这个css module会导出一个对象，这个对象包含了局部命名空间到全局命名空间的映射。  

	import styles from './style.css';
	//import {className} from "./style.css";
	element.innerHTML = '<div class="' + styles.className + '">';
### 命名规范
对于局部的类名建议使用驼峰命名法，但并非强制。

参考：http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651220796&idx=1&sn=4099912162d73a3a39c62e8f62a7fd89&scene=23&srcid=0522EJnxDj0xY39G0ZurlfvY#rd