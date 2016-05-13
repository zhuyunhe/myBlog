---
title: webpack-05
date: 2016-05-13 18:48:44
tags:
- 学习
- webpack
categories : 学习笔记
---
# webpack中的模块加载和链式模块加载
模块加载器是可以自由添加的Node模块，用于将不同类型的文件“load”或“import”并转换成浏览器可以识别的类型，如js、stylesheet等。更高级的模块加载器现在也支持使用ES6中的“import”或“require”语法引入模块。  
	
	//webpack.config.js配置文件的module配置项
	module:{
		loaders:[{
			test : /\.js$/,	//匹配.js文件，如通过则使用下面的loader处理
			exclude : /node_modules/,	//排查node_modules文件夹
			loader : 'babel'			//使用babel（也可以写成“babel-loader”,webpack支持简写）作为loader
		}]
	}
webpack中的loader是链式（管道式）的加载器，加载顺序从右到左。