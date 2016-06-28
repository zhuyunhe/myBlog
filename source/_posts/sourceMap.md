---
title: sourceMap
date: 2016-06-28 15:55:22
tags:
- 学习
categories: 学习笔记

---
# 为什么要用Source Map？
参考阮一峰老师的一篇[文章](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)先。  
JavaScript脚本正变得越来越复杂，大部分的源码（尤其是各种函数库和框架）都要经过转换，才能投入生产环境。在以下三种情况下，我们需要进行源码转换：  
1. 压缩、丑化源码文件，减小体积。  
2. 多个文件合并，减小HTTP请求数。  
3. 其他语言或ES6编译成浏览器支持的JavaScript。  
在上述三种情况下，浏览器里实际运行的代码都不同于开发代码，似的调试时困难重重。  
通常情况下，JavaScript解析器会告诉你第几行第几列出错，但这对于上述三种情况下转换后的代码毫无用处，我们需要知道的是错误在开发代码中的原始位置。这就是Source Map想要解决的问题。
# 什么是Source Map
Source Map是一个信息文件，里面存储着位置信息。这里的`map`是映射的意思，通过Source Map文件，可以将编译后的代码映射到编译前原始的代码，在浏览器debug的时候，虽然浏览器解析的是编译后的，但是呈现给我们的是通过开发时的代码，编译调试。<!-- more -->
如果想要在项目中项目中使用ES6的一些新语法，那么我们要用babel这样的工具将ES6代码编译成浏览器支持的JavaScript，编译后的代码和开发的代码不一样了，调试时就需要利用SourceMap将开发代码映射到debug工具中。Chrome浏览器支持SourceMap功能，只需在设置（win下按F1）中开 启。  
![source](/image/sourcemap/1.png)  

我在Gulpdemo中试用了一下SourceMap，效果如下：  
![demo](/image/sourcemap/2.png)  
`main-3795f5a805.min.js`是生产环境中压缩过的js文件，它的源文件是`index2.12.js`，chrome启用SourceMap后，开发者工具的左边文件树中多了一个橘黄色的source文件夹，里面就是开发时源代码，我们调试时还可以像以前一样设置断点什么的。但不知道为啥我的中文注释是乱码啊，谁能告诉我怎么办。还有为啥我在源代码里设置断点不管用啊。
