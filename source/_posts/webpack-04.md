---
title: webpack_04
date: 2016-05-13 15:36:42
tags:
- 学习
- webpack
categories : 学习笔记
---
# webpack.config.js配置文件中的output选项  
---
output项告诉webpack怎样存储输出结果以及存储到哪里。output的两个属性“path”和“publicPath”可能会造成困惑。
“path”仅仅告诉Webpack结果存储在哪里，然而“publicPath”项则被许多Webpack的插件用于在生产模式下更新内嵌到css、html文件里的url值。  
例如，在本地开发模式（localhost）里的css文件中你可以用“./test.png”这样的url来加载图片，但是在生产模式下这个“test.png”文件会定位到CDN上并且你的node.js服务器可能会运行在HeroKu上。这就意味着在生产环境你必须手动更新所有文件里的url为CDN路径。  
然而你也可以使用Webpack的“publicPath”选项和一些插件来在生产模式下编译输出文件时自动更新这些url。如下图所示:  
![outer-publicPath](/image/outer.png)  

	//开发环境：Server和图片都是在localhost（域名）下
	.image{
		background-image:url('./test.png');
	}
	//生产环境：图片可以是在CDN上
	.image{
		background-image:url('https://someCDN/test.png');
	}
	