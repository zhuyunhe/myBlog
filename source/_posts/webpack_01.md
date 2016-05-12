---
title: 第一天
date: 2016-05-09 14:32:40
tags:
- 开始
- webpack
- 学习
categories: 日志 
---
# Webpack的核心原理
---
- 1.一切皆模块   
 正如js文件可以是一个“模块(module)”一样，其他的如css、图片或html文件都可以视作模块。因此，你可以require('myJSfile.js')亦可以将事物（业务）分割成更小的易于管理的片段，从而达到重复利用的目的。

-  2.按需加载   
传统的模块打包工具（module bundlers）最终将所有的模块编译成一个庞大的bundle.js文件。但是在真实的app里，这个bundle.js可能会非常大，导致第一次打开时应用一直处于加载中状态。因此Webpack使用许多特性来分割代码然后生成多个“bundle”文件，而且异步加载部分代码以实现按需加载。

