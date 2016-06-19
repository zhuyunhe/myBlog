---
title: 'JS基准测试:console.log与console.timeEnd'
date: 2016-06-19 09:48:08
tags: 
- JavaScript
categories: 学习笔记

---
在众成翻译上看到一篇关于利用console.time和console.timeEnd测试JS程序执行时间。[中文原文](http://www.zcfy.cc/article/console-time-amp-console-timeend-592.html)<!-- more -->
console.time和console.timeEnd方法允许开发者在任意代码中使用，显示的结果是夹在两个方法中间的程序运行的时间，以毫秒为单位。由于JavaScript性能越来越重要，所以了解基本的基准测试技术是挺有用的。最简单的基准测试工具就是**console.time**和**console.timeEnd**组合。  
`console.time`启动一个计时器，而console.timeEnd则停止计时器，并在控制台输出经过时间。  

	//启动一个名为'testForEach'的计时器
	console.time('testForEach');  
	
	//执行某些要测试的代码，如forEach循环之类

	//结束计时器，获取运行时间
	console.timeEnd('testForEach');

传入`console.time`的第一个参数是计时器的name，以便管理多个并发计时器。调用console.timeEnd则立刻输出运行的时间，单位是毫秒。  

总结：
1. console.time/timeEnd提供了一种快速进行速度测试的手动方法。  
2. Chrome提供强大的开发者调试工具，更多具体功能可以参考这篇[文章](http://gold.xitu.io/entry/56d56f4dc4c971005193ecec)