---
title: 一个移动端做题的demo
date: 2016-05-20 08:50:45
tags:
- 学习
- 工作
categories: demo
---
# 移动端做题的demo
## 结合移动端的触摸事件：touchstart、touchmove、touchend
## 动画和过度：CSS3中的transform和transition
前段时间公司有个小任务，做一个网页版的做题界面，需要模仿原生客户的可以在题目间滑动切换的功能。我把工作中主要的页面抽取出来，做了个[demo](https://github.com/zhuyunhe/HJZ/tree/master/dati)分享一下。  
原理其实很简单：可以参考下图  
![demo1](/image/dati/demo5.png)  
每次页面间来回切换时，只需要动态的修改content容器的transform样式把要显示的题目放到视口中。核心代码如下：  

	function scrollTo(index){
		$('#since').find("#content").css({
			"transition": ".2s",
			"-webkit-transition": ".2s",
			"transform" : "translate3d("+(-index * hjz_data.viewWidth)+"px,0,0)",
			"-webkit-transform" : "translate3d("+(-index * hjz_data.viewWidth)+"px,0,0)",
		});
	}
先看一下效果  
![demo1](/image/dati/demo1.png)  
![demo1](/image/dati/demo4.png)  
以下是实践过程中要注意的一些东西：  

+ 触摸事件touchEvent  
很多书中（比如红宝书的399页）讲到触摸事件除了常见的dom事件属性外，还包括以下三个用于跟踪触摸的属性：touches、targetTouchs、changeTouches。但需要注意的是只有触摸事件，也就是touchEvent才有这些属性，而回调函数中原始的event只是一个普通的事件对象，根本不是touchEvent，真正的touchEvent对象则是保存在这个普通event对象的originalEvent中。如下面截图中显示  
![demo2](/image/dati/demo2.png)  
所以当需要用到touchEvent时，需要在回调函数中多写一句`var e = e.originalEvent;`
+ touchmove事件  
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/TouchEvent)上关于touchmove提到了一点“不同浏览器上touchmove事件的触发频率并不相同。这个触发频率还和硬件设备的性能有关。因此绝不能让程序的运作依赖于某个特定的触发频率”。因此如果在touchmove事件绑定的地方加上断点，滑动一下界面，调试程序会多次进到这个断点，我们也可以在touchmove事件的回调函数中利用手指在x轴和y轴的滑动距离来判断是否要离开当前题目的页面，简单的来说，如果在x轴（水平方向）上移动的距离大于在y轴上移动的距离（竖直方向），那么就认为这个滑动动作是要离开当前题目页。下面是程序截图  
![demo3](/image/dati/demo3.png)
