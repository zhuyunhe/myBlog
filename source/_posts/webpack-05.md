---
title: webpack-05
date: 2016-05-13 18:48:44
tags:
- 学习
- webpack
categories : 学习笔记
---
# webpack中的模块加载和链式模块加载

先放出官网关于loader的介绍：  
https://webpack.github.io/docs/loader-conventions.html  
https://webpack.github.io/docs/list-of-loaders.html  

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
webpack中的多个loader可以用在同一个文件上并且被链式调用。链式调用时从右到左执行且loader之间用“!”来分割。  
例如，假设我们有一个名为“myCssFile.css”的css文件，然后我们想将它的内容使用style标签内联到最终输出的html里。我们可以使用css-loader和style-loader两个loader来达到目的。  

	module: {
		loaders: [
			test : /\.css$/,
			loader : 'style-loader!css-loader'  //(也可以简写为'style!css')
		]
	}
下图能更好地展示其如何工作。  
![style!css](/image/webpack/css-style.png)  
1. webpack在模块入口文件的顶部搜索css依赖项，即Webpack检查js文件是否有“require('myCssFile.css')”的引用，如果发现有css的依赖，Webpack将css文件交给“css-loader”去处理。  
2. css-loader加载所有的css文件以及css自身的依赖（如在myCssFile.css中@import了其他css文件）到JSON对象中，然后webpack将处理结果传给“style-loader”。
3. style-loader接受JSON值然后添加一个style标签并将其内嵌到html文件里。