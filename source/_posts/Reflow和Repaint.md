---
title: Reflow和Repaint
date: 2016-06-12 23:05:09
tags: 
- JavaScript
- 学习
categories: 学习笔记  

---
在学习前端性能时候，经常听到Reflow和Repaint这两个词，当我们添加一些CSS3动画或者操作多个DOM元素时，Reflow和Repaint对性能的影响很大。  
用户和Web页面都不能在Reflow和Repaint执行时做任何操作和相应，而且在极端的情况下，CSS会拖慢JS的执行速度，这就是浏览器有时候在操作之后没有反应的原因之一。
<!-- more -->
## Repaint
Repaint就是重绘，它会在你**改变DOM元素的视觉效果**时进行，改变布局时不会触发。比如，改变元素的opacity、background-color、visibility和outline等会触发重绘。重绘的开销还是比较昂过的，因为浏览器会在某一个DOM元素的视觉效果改变后去check这个DOM元素内的所有节点。
## Reflow
Reflow就是回流，它的影响更大。它会在**某一个DOM元素的位置发生改变后触发**，而且它会重新计算所有元素的位置和页面中的占有面积（**对元素位置和几何上的计算**），这样的话将会引起页面某一个部分甚至整个页面的重新渲染。改变某一个元素会影响它所有的子节点、祖先节点和兄弟节点。  
## 什么时候Reflow会触发
Reflow的性能开销更加昂过，以下这些操作会触发浏览器Reflow：
- 添加、删除或者改变DOM元素的可见性时：使用JS去改变DOM元素时会触发Reflow。
- 添加、删除或者改变CSS样式：直接改变CSS Style或者元素的class可能会影响布局，还有改变一个元素的宽度能够影响它所在的DOM节点中的所有元素，以及它周围的那些元素。
- CSS3动画(animation)和过渡(transition)：动画的每一frame都会触发Reflow。
- 使用offsetWidth和offsetHeight：获取一个DOM元素的offsetWidth和offsetHeight属性同样会触发一下Reflow，因为这两个属性需要依赖一些元素去计算。
- 用户交互：用户可以通过hover一下a链接，在input里输入文字，拖动浏览器的大小，改变字体样式，更换样式表或者字体等触发Reflow。
## 常用的提高性能的原则
### 布局
不要用inline style或table布局，flexbox布局也会给性能带来一些小困扰。inline style会在html下载完后进行一次额外的Reflow，table布局的开销远比其他DOM元素的布局开销要大。flexbox的item会在HTML下载完成后改变尺寸。
### 简写CSS
尽量简写CSS，避免使用复杂的CSS选择器，使用[Unused CSS](https://unused-css.com/)、[uCSS](https://github.com/oyvindeh/ucss)、[gulp-uncss](https://github.com/ben-eb/gulp-uncss)等工具能有效的减少CSS样式的定义和文件的大小。
### 优化DOM
减少DOM的层级，减少DOM数量，如果不需要适配老浏览器，删掉一些无用的wrapper性质的DOM元素，总之越少越好。
### 慎改class
在一个DOM树中，尽可能改那些没有特别多子元素DOM的class，子元素少的可以改，多的不推荐。
### 避免负载动画
删掉复杂的动画，运用动画的元素尽量是`position:absolute`或`position:fixed`的，这样会让他们脱离文档流，不去影响其他的元素。
### 善用display:none
`display:none`的元素不会引发Reflow和Repaint，可以在让这些元素在display之前进行一些诸如颜色、尺寸的改变。
### 批量更新元素 
比如下面这个例子会触发三次reflow  
	
	var element = document.getElementById('myElement');
	element.width = '100px';
	element.height = '200px';
	element.style.margin = '10px';

可以使用下面这种修改元素class属性的方法来优化上面的代码：

	var element = document.getElementById('myElement');
	element.classList.add('newstyle');
	css:
	.newstyle{
		width:100px;
		height:200px;
		margin:10px;
	}
此外，还可以在生成新DOM时使用[DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)先在内存中构建DOM。先创建一个文档片段，然后将创建的DOM元素插入到文档片段中，最后把文档片段插入到DOM树中。在DOM树中，文档片段会被替换为它所有的子元素。
因为文档片段存在于内存中，并不在DOM树中，所以将子元素插入到文档片段时不会引起页面回流Reflow。因此使用文档片段通常会起到性能优化的作用。  
	
	var i,li;
	var frag = document.createDocumentFragment();
	var ul = frag.appendChild(document.createElement('ul'));
	for(i=1; i<=3; i++){
	li = ul.appendChild(document.createElement('li'));
	li.textContent = 'item'+i;
	}
	document.body.appendChild(frag);
### 避免大量DOM互相影响
比如Tabs这种场景，如果你点击一个Tab会显示它控制的区块，显示的那个区块会影响其他的区块，这样可能会引起Reflow，因为它们的高度不一样，可以通过定个高度来优化这种场景。  
### 性能永远比酷炫重要
记住一个原则，网页动画再牛逼，性能还是第一位，如果每一帧移动一个像素会造成你的页面卡顿，那宁愿每一帧移动10像素让动画变得迟钝一些，也不要让页面的性能降下来。