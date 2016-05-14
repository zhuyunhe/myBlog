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
