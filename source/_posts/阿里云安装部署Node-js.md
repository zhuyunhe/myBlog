---
title: 阿里云安装部署Node.js
date: 2016-05-21 16:12:13
tags:
- 学习
- node
categories: 学习笔记
---
之前买的阿里云服务器是按需付费的，玩了几天后发现按需付费的实例不能申请备案服务号，自己买的域名也不能备案，访问博客时经常出现下图的情况，不得不从狗粮中再挤出一点钱来买按月的。之前搭建的一些环境也得重新再搭一遍了，这次索性花点时间把搭建过程小白式地记录下来，以备我这个小白以后还能用得上。  
![bad](/image/node/node.png)  

- 首先去node.js的[官网下载页](https://nodejs.org/en/download/)下载压缩安装包。  
![bad](/image/node/step1.png)  
- 然后利用xftp或WinSCP把文件复制到阿里云服务器上。  
![bad](/image/node/step2.png)  
- 接下来就是在阿里云服务器上编译安装Node.js  
		
		//进入阿里云服务器时最好先升级一下系统
		//apt-get update
		//解压安装包
		tar zxvf node-v4.4.3.tar.gz
		//进入node文件夹
		cd node-v4.4.3
		//配置
		./configure
		//编译,这步比较慢一点
		make
		//安装
		make install
正常情况下，到此为止就安装成功了。  
![done](/image/node/step3.png)  

参考：http://www.w3ctech.com/topic/1610