---
title: '''pm2'''
date: 2016-05-23 09:50:38
tags:
- pm2
- 学习
categories: 学习笔记
---
[pm2](https://github.com/Unitech/pm2)是一个内置了负载均衡功能的针对Node.js应用程序的进程管理器。当你要把你的独立代码利用服务器上的所有CPU，并保障进程永远活着，0秒重载，pm2是非常合适的。下面是利用pm2后台运行hexo的一些小实践。  
根据[hexo官网](http://www.liuzhixiang.com/hexo_site_cn/docs/server.html)的提示如果想保持Hexo服务器以守护进程运行，可以使用[Forever](https://github.com/foreverjs/forever)或PM2。  

- 首先要在hexo项目下安装hexo  

		$ npm install hexo --save

- 然后在项目目录下添加一个app.js文件并加入以下内容  

		//app.js  
		require('hexo').init({command:'server'});

- 使用PM2以fork模式运行这个app.js脚本

		pm2 start app.js -x
执行这个命令后，我们可以看到如下效果：  
![pm2 start app.js -x](/image/pm2/1.png)
显示app应用的状态是online的，但我此时访问我的博客却是无法访问的。  
我在stackoverflow上搜了一下，找了一个类似的[问题](http://stackoverflow.com/questions/34218868/how-do-i-configure-pm2-to-run-hexo)，里面提到两个解决方案。我两个都试了一下，以下是实践:  

	1. 方法1  
	![try1](/image/pm2/3.png)  
	![try1](/image/pm2/2.png)  
	这种方法并不给力
	2. 方法2  
	![try2](/image/pm2/4.png)  
	![try2](/image/pm2/5.png)  
	方法2是好使的，博客也能正常访问了。