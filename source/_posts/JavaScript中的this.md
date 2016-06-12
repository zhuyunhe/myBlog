---
title: JavaScript中的this
date: 2016-06-08 13:16:25
tags: 
- JavaScript
- 学习
categories: 学习笔记

---
# JavaScript中的“this”
在箭头函数出现之前，JavaScript中的每个函数都有一个this值，它总是指向一个对象，而具体指向哪个对象这是运行时基于函数的执行环境（上下文）绑定的。<!-- more -->
## this指向的对象
一般来说“this”的执行环境有以下几种：

- 作为对象的方法被调用  

当函数作为对象的一个方法被调用时，this指向调用它的对象。  
	
	var o = {
		name: 'zhu',
		f: function(){
			return this.prop;
		}
	};
	console.log(o.f());		//zhu

- 作为普通函数被调用  

在非严格模式下，此时this指向window全局对象；在严格模式下是指向undefined。这属于函数的简单调用模式。

	var x = 2000;
	function foo(){
		console.log(this.x);
	}
	foo();		//2000
- 构造函数调用  

当函数是被当作构造器（constructor）来调用时（使用new关键字），this被绑定到新创建的对象实例上。要注意，尽管constructor默认会返回由this指向的对象，我们也可以使它返回一些其他的对象（如果返回的值不是一个对象，那么this对象还是会被返回）  

	function Person(){
		this.age = 25;
	}
	var p = new Person();
	console.log(p.age);		//25
	function Person2(){
		this.age = 25;
		return {age: 26};
	}
	var p2 = new Person2();
	console.log(p2.age);	//26
- Function.prototype.call或Function.prototype.apply调用  

所有的函数对象都有从Function.prototype继承的call和apply方法。apply方法在指定this值和参数（参数以数组或类数组对象的形式存在）的情况下调用某个函数。apply和call方法的语法几乎是相同的，唯一区别在于，call方法的参数接受的一个参数列表，而apply方法接受的是一个包含多个参数的数组（或类数组对象）。  

当函数在函数体内使用this关键字时，通过调用call或apply方法，this能够被绑定到特定的对象。  
	
	function greet(){
		var reply = [this.perso, 'is a awesome', this.role].join(' ');
		console.log(reply);
	}
	var i = {
		person: 'zhu',
		role: 'js developer'
	};
	greet.call(i);	//zhu is a awesome js developer  

- 作为事件处理方法调用(DOM event handler)

当一个函数是以事件处理方法（event handler）调用时，this指向触发该事件的元素。

	function bluify(e){
		//always true
		console.log(this === e.currentTarget);
		//true when currentTarget and target are the same object
		console.log(this === e.target);
		this.style.backgroundColor = "#A5D9F3";
	}
	var elements = document.getElementsByTagName('*');
	for(var i=0; i<elements.length; i++){
		elements[i].addEventListener('click', bluify, false);
	}

## 总结
this实质上是一种绑定（binding）机制，它的值取决于函数被调用时所处的上下文（context），也就是函数如何被调用。