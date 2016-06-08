---
title: JavaScript中的this
date: 2016-06-08 13:16:25
tags: 
- JavaScript
- 学习
categories: 学习笔记

---
#JavaScript中的“this”
在箭头函数出现之前，JavaScript中的每个函数都有一个this值，它总是指向一个对象，而具体指向哪个对象这是运行时基于函数的执行环境（上下文）绑定的。<!-- more -->
##this指向的对象
一般来说“this”的执行环境有以下几种：

- 作为对象的方法被调用  
- 作为普通函数被调用  
- 构造函数调用  
- Function.prototype.call或Function.prototype.apply调用
