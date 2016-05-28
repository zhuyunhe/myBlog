---
title: JavaScript的内存泄露
date: 2016-05-27 23:59:23
tags:

- 学习
- JavaScript

categories: 学习笔记
---
# 什么是内存泄露
本质上，内存泄露可以定义为：应用程序不再需要占用内存的时候，由于某些原因，内存没有被操作系统或可用内存池回收。编程语言管理内存的方式各不相同，一些语言提供了语言特性，可以帮助开发者做此类事情；另一些则寄希望于开发者对内存是否需要回收清晰明了。
# JavaScript内存管理
JavaScript是一种垃圾回收语言。垃圾回收语言通过周期性地检查先前分配的内存是否可达，帮助开发者管理内存。换言之，垃圾回收语言减轻了“内存仍可用”及“内存仍可达”的问题。这二者的关系是微妙但重要的：仅有开发者了解哪些内存是将来仍然要使用的；而哪些内存是可达或者不可达则可以由算法确定和标记，不可达的内存适时被操作系统回收。
# JavaScript内存泄露
垃圾回收语言的内存泄露主因是“不需要的引用”。
# 标记清除(Mark-and-Sweep)
大部分自动垃圾回收语言用的算法是基于标记清除算法，算法由以下几步组成：

- 垃圾回收器创建一个“roots”列表。root通常是代码中全局变量的引用。JavaScript中，“window”对象是一个全局变量，被当做root。window对象总是存在的，因此垃圾回收器可以检查它和它的所有子对象是否存在（即不是垃圾对象）。
- 所有的roots被检查和标记为激活（即不是垃圾）。roots对象的所有子对象也被递归地检查。从root开始的所有对象如果是可达的，它就不被当做垃圾。
- 所有未被标记的内存会被当做垃圾，收集器现在可以释放这部分垃圾内存，把它们归还给操作系统了。   

现代的垃圾回收器改良了算法，但是本质是相同的：可达内存被标记，其余的被当作垃圾回收。
在内存泄露中提到的“不需要的引用”指的就是开发者明知这些内存引用不再需要，却由于某些原因，它仍被留在激活的root树中。在JavaScript中，不需要的引用是某些保留在代码中的变量，它们不再被需要，却指向了一块本该被释放的内存。有些人认为这是开发者的错误。  
为了理解JavaScript中最常见的内存泄露，我们需要了解哪种方式的引用容易被遗忘。
# 三种常见的JavaScript内存泄露
- 意外的全局变量  
JavaScript中对未定义变量的处理方式比较宽松：在非严格模式中，未定义的变量会在全局对象创建一个新变量，在浏览器中，全局对象是window。  
		
		function foo(arg){
			bar = 'this is a hidden global variable';
		}
真相是：  

		function foo(arg){
			window.bar = 'this is a hidden global variable';
		}
函数foo内部定义bar变量时忘记使用var，意外创建了一个全局变量。此例泄漏了一个简单的字符串，无伤大雅，但还有一种更糟糕的情况。  
		
		function foo(){
			this.variable = 'potential accidental global';
		}
		//foo调用自己，this指向了全局对象（window）而不是undefined
		foo();
在JavaScript文件头部加上'use strict'启用严格模式解析JavaScript，可以避免此类意外全局变量的错误发生。  
**全局变量注意事项**
尽管我们讨论了一些意外的全局变量，但仍有一些明确的全局变量产生的垃圾。它们被定义为不可回收（除非定义为空或重新分配）。尤其当全局变量用于临时存储或处理大量信息时，需要多加小心。如果必须使用全局变量存储大量数据时，确保用完后把它设为null或重新定义。与全局变量相关的增加内存消耗的一个主因是缓存。缓存数据是为了重用，缓存必须有一个大小上限才有用。高内存消耗导致缓存突破上限，因为缓存内容无法被回收。  

---
- 被遗忘的计时器或回调函数
在JavaScript中使用setInterval非常平常。一段常见的代码：  
	
		var someResource = getData();
		setInterval(function(){
			var node = document.getElementById('node');
			if(node){
				node.innerHTML = JSON.stringify(someResource);
			}
		}1000);
此例说明：与节点或数据关联的计时器不再需要，node对象可以删除，整个回调函数也不需要了。可是，计时器回调函数仍没被回收（计时器停止才会被回收）。同时，someResource如果存储了大量的数据，也是无法被回收的。  
接下来是一个观察者的例子，一旦它们不再需要（或者关联的对象变成不可达），明确地移除它们非常重要。老的IE 6是无法处理循环引用（事件）的。如今，即使没有明确移除它们，一旦观察者对象变成不可达，大部分浏览器是可以回收观察者处理函数的。  

		var element = document.getElementById('button');
		function onClick(event){
			element.innerHTML = 'text';
		}
		element.addEventListener('click',onClick);
在老版的浏览器（IE）中，无法检测DOM节点与JavaScript代码间的循环引用，如果要回收element这个DOM节点，最好先调用其removeEventListener函数。但现代的浏览器使用了更先进的垃圾回收算法，已经可以检测和处理循环引用了，换言之，回收节点内存时，不必非要调用removeEventListener了。

---
- 脱离DOM的引用
有时候，保存DOM节点内部数据结构很有用。假如你想快速更新表格的几行内容，把每一行DOM存成字典（JSON键值对）或者数组很有意义。此时，同样的DOM元素存在两个引用：一个在DOM树中，另一个在字典中。将来你决定删除这些行时，需要把两个引用都清除。
		
		var elements = {
			button: document.getElementById('button'),
			image: document.getElementById('image'),
			text: document.getElementById('text')
		};
		
		function doStuff(){
			image.src = 'http://url/image';
			button.click();
			console.log(text.innerHTML);
			//更多逻辑
		}

		function removeButton(){
			document.body.removeChild(document.getElementById('button'));
			//此时仍有一个全局的button元素的引用在elements字典中，button元素仍在内存中，不能被GC回收。
		}
此外我们还要考虑DOM树内部或子节点的引用问题。假如你的JavaScript代码中保存了一个表格中某个<td>的引用。将来决定删除整个表格的时候，我们直觉认为GC会回收了已保存的<td>以外的其他节点。实际情况并非如此：此<td>表格的子节点，子元素与父元素是引用关系，由于代码保留了<td>的引用，导致整个表格仍留到内存中而不能被GC回收。因此在保存DOM元素引用的时候，要小心谨慎。   

--- 
- 闭包  
闭包是JavaScript开发的一个关键方面：匿名函数可以访问父级作用域的变量。通常，一个包装了一些局部变量的函数就是一个闭包。  
实例代码：  

		var theThing = null;
		var replaceThing = function(){
			var originalThing = theThing;
			var unused = function(){
				if(originalThing){
					console.log('hi');
				}
			};
			theThing = {
				longStr: new Array(100000).join('*'),	
				someMethod: function(){
					console.log(someMessage);
				}				
			};
		};
		setInterVal(replaceThing, 1000);
代码片段做了一件事：每次调用replaceThing时，theThing得到一个包含一个大数组和一个新闭包(someMethod)的新对象。同时，变量unused是一个引用了originalThing的闭包（在此之前originalThing又引用了theThing）。闭包的作用域一旦创建，它们有同样的父级作用域，作用域是共享的。someMethod可以通过theThing使用，someMethod与unused分享闭包作用域，尽管unused从未使用，它引用的originalThing迫使它留在内存中（防止被回收）。当这段代码反复运行，就会看到内存占用不断上升，垃圾回收器（GC）无法降低内存占用。因此，在replaceThing的最后有必要把originalThing变量设置为null，这样就能解除对theThing的引用。
