---
title: JavaScript-promise
date: 2016-06-29 09:49:07
tags: 
- JavaScript
- 学习
categories: 学习笔记
---
Promise对象是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。在ES6中，Promise对象用于异步(asynchronous)计算。一个Promise对象代表着一个还未完成，但预期将会完成的操作。 
 
# 复习

### 同步模式
程序的执行顺序和任务的排列顺序是一致的、同步的，后一个任务等待前一个任务结束后再执行。

### 异步模式
程序的执行顺序与任务的排列顺序是不一致的、异步的，每一个任务都有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则不用等前一个任务结束就执行。
<!-- more -->

# 语法
`new Promise(executor)`  
`new Promise(function(resolve, reject){....})`  

## 参数

### executor
带有resolve、reject两个参数的函数对象。第一个参数用在处理执行成功的场景，第二个参数则用在处理执行失败的场景。一旦我们的操作完成即可调用这些函数。

# 功能

同步代码和异步代码解耦，程序流程更加清晰。

# Demo

参照MDN上关于Promise的[例子](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise#创建Promise)写了下面这个小demo。

## 常规回调函数写法
	'use strict'
	var count = 0;
	var btn_cb = document.getElementById('btn_cb');
	btn_cb.addEventListener('click', function(){
		testCallback(callback);
	});
	var log_cb = document.getElementById('log_cb');
	function testCallback(callback){
		var thisCount = ++count;
		log_cb.insertAdjacentHTML('beforeend','('+thisCount+')Started (<small>Sync code started</small>)<br/>');

		//模拟一个异步操作			
		log_cb.insertAdjacentHTML('beforeend','('+thisCount+')Started (<small>Async code started</small>)<br/>');
		setTimeout(function(){
			//执行异步操作回调
			callback(thisCount);
		},Math.random()*2000 + 1000);

		log_cb.insertAdjacentHTML('beforeend','('+thisCount+')Started (<small>Sync code terminated</small>)<br/>');
	}
	function callback(val){
		log_cb.insertAdjacentHTML('beforeend','('+val+')Started (<small>Async code terminated</small>)<br/>');
	}

## Promise对象写法
	'use strict'
	var count = 0;
	
	function testPromise(){
		var thisCount = ++count;
		var log = document.getElementById('log');
		log.insertAdjacentHTML('beforeend','('+thisCount+')Started (<small>Sync code started</small>)<br/>');
		var p = new Promise(
			function(resolve, reject){
				log.insertAdjacentHTML('beforeend','('+thisCount+')Started (<small>Async code started</small>)<br/>');
				//用定时器模拟一个耗时的异步任务
				window.setTimeout(
					function(){
						resolve(thisCount);
					},Math.random()*2000 + 1000);
		});
		p.then(
			function(val){
				log.insertAdjacentHTML('beforeend', '('+val+')Started (<small>Async code terminated</small>)<br/>');
			}
			)
		 .catch(
		 	function(reason){
		 		console.log('Handle rejected promise ('+reason+') here')
		 	}
		 	);

		log.insertAdjacentHTML('beforeend', '('+thisCount+')Started (<small>Sync code terminated</small>)<br/>');
	}
	if('Promise' in window){
		var btn = document.getElementById('btn');
		btn.addEventListener('click', testPromise);
	}

阮一峰老师在他[文章](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html)里说，使用Promise的优点在于，回调函数变成了链式写法，程序流程可以看得很清楚。就是让异步编程写起来更像同步编程。