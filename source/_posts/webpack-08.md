---
title: webpack-08
date: 2016-05-14 13:26:55
tags:
- 学习
- webpack
categories: 学习笔记
---
在很多webpack.config.js配置文件中都会看到一个resolve属性，然后就像下面代码所示有一个空字符串的值。空字符串在此是为了resolve一些在import或require文件时不带文件扩展名的表达式，如```import myJSFile from './myJSFile'``` 或者```require('./myJSFile')```。实际上这就是自动添加后缀，默认是当成js文件来查找。  
	
	{
		resolve: {
			extensions:['','.js','.jsx']
		}
	}