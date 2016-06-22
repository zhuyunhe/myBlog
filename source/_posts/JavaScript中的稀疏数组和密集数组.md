---
title: JavaScript中的稀疏数组和密集数组
date: 2016-06-22 16:03:21
tags: 
- JavaScript
- 学习
categories: 学习笔记
---
在JavaScript中，数组是一类特殊的对象，它表示一种数据集合，集合中的元素可以是任意类型的对象。既然数组也是对象，那么数组中的元素也是以属性的方式进行存储的（键值映射）。但由于JavaScript中数组中的属性的名称（键名）都是数字（从0开始），而一个合法的变量名不能以数字开头，所以我们必须使用方括号来获取数组中的这些属性，而不是使用点号(.)的方式。 
可以看到，JavaScript中数组并不是其他语言中定义的常规数组，它只是一个对象，只不过会自动管理一下“数字”属性和length属性，其他语言中数组的索引应该是个数字，而JavaScript中数组的索引其实就是个字符串（属性名）。 
## 稀疏数组：数组中元素之间存在空隙。
## 密集数组：数组中元素之间不存在空隙。
<!-- more -->
一般来说，JavaScript中的数组都是稀疏数组。例如：  
	
	var a = [1,2,3];
	a[9] = 10;
	console.log(a.length);  // 10
	console.log(a[5]);		//undefined
	
	//查看一下数组a的自有属性
	console.log(Object.getOwnPropertyNames(a));		
	//控制台输出：["0", "1", "2", "9", "length"]s

当我们对稀疏数组a调用一些数组的迭代方法，例如forEach()、filter()时，迭代方法会跳过这些稀疏数组中的这些空隙。原因就是这些空隙不是数组a的属性。
	
	a.filter(function(item,index){
		return item===undefined
	})
	//[]

当我们使用`var a = new Array(10)`创建一个数组时，a其实是一个稀疏数组。  
我们也可以用以下方法创建一个密集数组：
	
	var a = Array.apply(null, Array(3));
	console.log(a);
	//[undefined, undefined, undefined]
上面创建数组的语句等同于`var a = new Array(undefined,undefined,undefined)`  
现在，我们调用数组的迭代方法时，就可以访问到这些undefined元素了。  
	
	a.filter(function(item,index){
		return item===undefined
	})
	//[undefined, undefined, undefined]

## 实际用途
密集数组和map配合使用，可以创建一个固定大小数组并用指定的值来填充。

	var a = Array.apply(null, Array(3)).map(function () { return 'a' })
	//["a", "a", "a"]

[参考](http://www.cnblogs.com/ziyunfei/archive/2012/09/16/2687165.html)