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
Reflow就是回流，它的影响更大。它会在**某一个DOM元素的位置发生改变后触发**，而且它会重新计算所有元素的位置和页面中的占有面积，这样的话将会引起页面某一个部分甚至整个页面的重新渲染。改变某一个元素会影响它所有的子节点、祖先节点和兄弟节点。  
## 什么时候Reflow会触发
Reflow的性能开销更加昂过，以下这些操作会触发浏览器Reflow：
- 添加、删除或者改变DOM元素的可见性时：使用JS去改变DOM元素时会触发Reflow。
- 添加、删除或者改变CSS样式：直接改变CSS Style或者元素的class可能会影响布局，还有改变一个元素的宽度能够影响它所在的DOM节点中的所有元素，以及它周围的那些元素。
- CSS3动画(animation)和过渡(transition)：动画的每一frame都会触发Reflow。
- 使用offsetWidth和offsetHeight：获取一个DOM元素的offsetWidth和offsetHeight属性同样会触发一下Reflow，因为这两个属性需要依赖一些元素去计算。
- 用户交互：用户可以通过hover一下a链接，在input里输入文字，拖动浏览器的大小，改变字体样式，更换样式表或者字体等触发Reflow。
## 常用的提高性能的原则
### 布局
### 简写
### 优化DOM
### 慎改class
### 避免负载动画
### 善用display:none
### 批量更新元素 
参考：http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651221070&idx=1&sn=e633009c20d24ecb64e559160ad5e985&scene=23&srcid=0612AuqMNvRIDV3sz8oJbcNt#rd