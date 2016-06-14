---
title: 前端面试-JS篇
date: 2016-06-14 09:20:24
tags: 
- 面试
- JavaScript
categories: 学习笔记

---
不断补充总结的一些前端面试中关于JS的问题<!-- more --> 

- 介绍JS的基本数据类型  
	`Undefined、Null、Boolean、Number、String`
	`可以用typeof操作符来检测`

- 介绍JS有哪些内置对象？
	`Object是JavaScript中所有对象的父对象。`  
	`数据封装类对象：Object、Array、Boolean、Number和String`  
	`其他对象：Function、Arguments、Math、Date、RegExp、Error`

- 什么是JS中的事件委托？
	`事件委托是解决“事件处理程序过多”的方案。事件委托利用了事件冒泡，只指定一个事件处理，就可以管理某一类型的所有事件。`  
	`也就是说：利用事件冒泡的原理，把相关事件绑定到父级元素上，触发执行效果。`  
	`例如：click事件会一直冒泡到document层。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。`

- 什么是Ajax，其核心是什么？
	`Ajax全称为Asynchronous JavaScript And XML`  
	`向服务器发送异步请求的时候，我们不必等待结果返回，而是同时做其他的事情，等到结果返回后，程序会根据设定（callback）进行后续操作，此时，页面也不会发生刷新，提高用户体验。`  
	`Ajax的核心对象就是xhr，XMLHttpRequest`  
	`发生Ajax请求的过程可分为以下六步：`
	1. 创建XMLHttpRequest对象，也就是创建一个异步调用对象。
	2. 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息。
	3. 设置响应HTTP请求状态变化的函数
	4. 发送HTTP请求
	5. 获取异步调用返回的数据
	6. 使用JS操作DOM或数据驱动框架来实现局部刷新  
		
			var xhr = new XMLHttpRequest();
			xhr.onreadystatechange = function(){
				if(xhr.readyState === 4){
					if((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304)
					var data = JSON.parse(xhr.responseText);
					callback(data);
				}
			};
			xhr.open('get',url,true);
			xhr.send(null);

- JS中如何实现继承？
	`在传统的面向对象语言中，类从其他类继承属性。然后在JavaScript中，继承可以发生在没有类的继承关系的对象之间。这种继承机制就是利用原型对象。`
	`先简单回顾一下构造函数、原型和对象实例这三者的关系：每个构造函数都有一个原型对象，相对应地这个原型对象包含一个指向构造函数的指针，而该构造函数new出来的每个对象实例都有一个指向原型对象的内部指针。`
	1. 原型链继承
	JavaScript内建的继承方法被称为原型对象链，又可称为原型对象继承。原型对象的属性可由对象实例访问，这就是继承的一种形式。对象实例继承了原型对象的属性。因为原型对象也是一个对象，它也有自己的原型对象并可以继承其属性，这就原型链：对象继承其原型对象，而原型对象继承它的原型对象，以此类推。
	所有对象，包括我们自己定义的对象都自动继承自Object。确切地说，所有对象都继承自Object.prototype。   
	下面是一种常用的继承方式，利用了原型链和构造函数相结合的技术，利用原型链来实现对方法的继承，借用构造函数来实现对属性的继承（主要针对一些数组、对象等引用类型的属性）。

			function SuperType(name){  
    			this.name = name;  
    			this.colors = ["red","yellow"];  
			}
			SuperType.prototype.sayName = function(){  
			    alert(this.name);  
			};  
			  
			function SubType(name,age){  
			    //借用父类的构造函数，这样SubType的实例中就有了name和colors这两个实例属性，这两个属性就是每个实例独有的了  
			    SuperType.call(this,name);  
			    this.age = age;  
			}  
			  
			//继承方法  
			SubType.prototype = new SuperType();  
			//这是把SubType的constructor属性改回来，创建SubType实例时调用SubType的构造函数  
			SubType.prototype.constructor = SubType;  
			  
			//在SubType的新原型上添加一个方法  
			SubType.prototype.sayAge = function(){  
			    alert(this.age);  
			};  
			  
			var instance1 = new SubType("zhu",24);  
			instance1.colors.push("black");  
			alert(instance1.colors);  //red,yellow black  
			instance1.sayName();  //zhu  
			instance1.sayAge();     //24  
			  
			var instance2 = new SubType("li",23);  
			instance2.colors.push("blue");  
			alert(instance2.colors);  //red,yellow blue  
			instance2.sayName();  //li  
			instance2.sayAge();     //23

	2.深拷贝继承
	参考阮一峰老师的一篇[博客](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)。 
	
		var Chinese = { nation: '中国'};
		var Doctor = { career: '医生'};

		//让“医生”继承“中国人”，变成一位中国医生。
		//把父对象的属性全部拷贝给子对象，也能实现继承
		//由于父对象的属性可能是数组或对象，所以需要用深拷贝
		function deepCopy(p, c){
			var c = c || {};
			for(var i in p){
				if(typeof p[i] === 'object'){
					c[i] = (p[i].constructor === Array) ? [] : {};
					deepCopy(p[i], c[i]);
				} else{
					c[i] = p[i];
				}
			}
			return c;
		}
		
		Chinese.place = ['北京','上海','广州'];
		
		//深拷贝
		var Doctor = deepCopy(Chinese);
		Doctor.place.push('大理');
		console.log(Chinese.place);
		console.log(Doctor.place);

- 