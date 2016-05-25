---
title: JSLint_in_sublime
date: 2016-05-25 09:51:42
tags:
- JSLint
- 学习
categories: 学习笔记
---
[JSLint](http://jslint.com/help.html)是一个用来检查JavaScript代码中问题的工具，其主要工作是保障JS的代码质量。尽管现在很多前端的打包工具grunt、gulp、webpack都提供了相应的插件对js代码做检查，但这样毕竟有点后知后觉，如果能编写js代码时就锁定错误并解决岂不更好。  

- 必要条件
系统中必须要先安装Node.js，并且能从命令行运行“node”命令。
- 安装
具体安装过程在JSLint的[github主页](https://github.com/73rhodes/Sublime-JSLint)上写的很清楚来，前端的同学都应该会从sublime的命令行面板->包管理器（package control）->Install Package进行安装。（如果连package control都打不开的同学可以自行google或参考一下这两篇文章[1](http://www.cnblogs.com/xiaolu-web/p/4741527.html)、[2](http://www.imjeff.cn/blog/62/)）  
![install](/image/JSLint/1.png)  
- 使用  
	1.打开命令行控制面板（window中用`ctrl+shift+p`，OS X系统中用`command+shift+p`）,然后输入JSLint。  
	2.点击sublime菜单栏中的`Tools-JSLint`。  
	3.直接按`ctrl+L`  
	4.或者新建一个文件后直接存为.js文件，JSLint会在保存后自动打开。  
	效果图如下：  
	![result](/image/JSLint/2.png)  
- 配置
在sublime的菜单栏中的`Preferences>Package Settings>JSLint>Settings`下可以看到两个配置文件，一个是“Default”，另一个是“User”，点击就可以打开配置文件。  
如果要保存自己的配置，可以先把“Default”配置文件中的内容拷贝到“User”中去，然后按照自己的需求去修改“User”配置文件。