---
title: webpack-07
date: 2016-05-14 11:10:33
tags:
- 学习
- webpack
categories: 学习笔记
---
#webpack中的插件
先看官网介绍：https://webpack.github.io/docs/list-of-plugins.html  
例如，uglifyJSPlugin插件获取bundle.js然后压缩和混淆内容以减小文件体积。  
CommonsChunkPlugin插件可用于将每个入口文件中都引用到的功能模块打包到一个公共的js文件（chunk）中，然后每个入口文件就可以引用这个公共的js，以减少代码冗余。  
![plugin](/image/webpack/plugin.png)  

###webpack中loader和plugin的比较
+ Loader处理单独的文件级别，并且通常用在包生成之前或生成的过程中。  
+ plugin则是处理包（bundle）或者chunk级别，且通常是bundle生成的最后阶段。像上面介绍的CommonsChunkPlugin插件直接修改了bundle的生成方式。