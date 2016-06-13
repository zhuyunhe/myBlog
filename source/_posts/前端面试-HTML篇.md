---
title: 前端面试-HTML篇
date: 2016-06-13 11:37:34
tags:
- 面试
- HTML
categories: 学习笔记

---
自己总结的一些前端面试中关于HTML方面一些常见问题。<!-- more -->  

- doctype的作用？标准模式与兼容模式有何区别？  
	`1.<!doctype>声明位于HTML文档的第一行，处于<html>标签之前。告知浏览器的解析器用什么文档标准解析这个文档。doctype声明不存在或不正确将会导致文档以兼容模式呈现。`
	`2.标准模式的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。`
- HTML5为什么只需要写<!doctype html>？
	`HTML5不基于SGML（Standard Generalized Markup Language,标准通用标记语言），因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为。而HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。`
- 行内元素有哪些？块级元素有哪些？空（void）元素有哪些？
	`首先：CSS规范规定，每个元素都有display属性，确定该元素的类型，每个元素都有默认的display值，如div的display属性默认值为“block”。  
	
	行内元素：a、b、span、input、img、select、strong  
	块级元素：div、p、ul、ol、li、dl、dt、dd、h1、h2...h6  
	空元素：br、hr、img、input、link、meta、area、base、col、command、embed、keygen、param、source、track、wbr
	`
- 页面导入样式时，使用link和@import有何区别？
	`1.`
