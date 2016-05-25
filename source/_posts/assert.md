---
title: 断言-assertion
date: 2016-05-25 14:20:23
tags:
- 学习
- 测试
categories: 学习笔记
---
##断言（assertion）
今天在看一些前端测试的东西，又看到了“断言”这个词，其实自己一直对这种“舶来词”有点恐惧，因为没喝过洋墨水，对这些东西的理解大多来自于国人翻译，良莠不齐也是常态。今天抽空简单梳理一下。  
根据维基百科的解释：
> 在程序设计中，断言(assertion)是一种放在程序中的一阶逻辑（如一个结果为真或假的逻辑判断式），目的是为了标示与验证程序开发者预期的结果，当程序运行到断言的位置时，对应的断言应该为真。若断言不为真时，程序会中止运行，并给出错误消息。
> 程序设计者可以用断言来标示程序，提供程序正确性的相关信息。例如在一段程序前加入断言（先验条件），说明这段程序运行前预期的状态。或在一段程序后加入断言（后验条件），说明这段程序运行后预期的结果。  

接下来让我们实现一个断言函数，并做个demo。  

	function AssertionFail(message){
	this.message = message;
	}
	AssertionFail.prototype = Object.create(Error.prototype);
	
	//断言函数，用来检查开发过程中的逻辑错误
	function assert(test,message){
	 	if(!test){
	 		throw new AssertionFail(message);
	 	}
	}
	function lastElement(array){
		//使用断言
		assert(array.length>0,'我是先验断言：数组是空的');
		return array[array.length-1];
	}
	var array = [];
	console.log(lastElement(array));

上面代码中，执行`lastElement(array)`时，会使用一个断言函数，可以把它看成是一个检查程序逻辑错误的工具，在这里`lastElement`函数的作用是返回数组中的最后一个元素，所以我们期望传入的数组不是空的，也就是`array.length>0`，如果真实的array和我们期望的array不符，例如demo中的array是个空数组，那么程序就会终止执行，并抛出一个我们定义的错误。程序运行结果如下图所示:  
![](/image/assert/1.png)  
但是，如果注释掉`assert(array.length>0,'我是先验断言：数组是空的');`这句话，那么此时js并不会报错，而是会返回array[-1]，由于array是空数组，所以就返回了undefined。可以假设一下如果后面的程序中还会用到这个undefined值，那就会产生一些不可意料的错误了。结果如下图所示：
![](/image/assert/2.png)  
由此可见：
>断言是检查开发人员逻辑错误的基本工具。
>断言能够确保错误发生时触发中止程序运行的操作，而不是让程序悄悄产生一个无意义的值，然后继续执行下去，直到系统出现其他莫名其妙的错误为止。

