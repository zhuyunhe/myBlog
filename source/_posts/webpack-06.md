---
title: webpack-06
date: 2016-05-14 10:34:39
tags:
- 学习
- webpack
categories: 学习笔记
---
#webpack中loader的自身配置
webpack中的模块加载器（loader）自身可以传入参数进行配置。  
在下面的例子中，我们可以配置url-loader来将小于1024字节的图片使用DataUrl替换以减少http请求，而大于1024字节的图片仍然使用正常图片url，我们可以用如下两种方式传入“limit”参数来实现这一目的：  
![url-loader](/image/webpack/url-loader.png)

