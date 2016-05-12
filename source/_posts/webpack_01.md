---
title: Webpack学习笔记-2
date: 2016-05-09 19:08:40
tags:
- webpack
- 学习
categories: 学习笔记 
---
# Webpack的用户交互接口
---
####Webpack提供两种用户交互接口：webpack CLI和webpack-dev-server
- 1.webpack CLI   
 webpack CLI是webpack提供的命令行接口，是默认的交互方式，安装Webpack时已经一起安装好了。这种方式可以从命令行或配置文件（默认配置文件是项目根目录下的webpack.config.js）获取参数，将获取到的参数传入Webpack打包。   
    方式一  
	`//命令行方式`  
	`$ webpack //使用webpack.config.js配置打包生成bundle`  
	方式二  
    `//配合node的package.json，使用自己配置的用于生成环境的config文件打包`  
	`//添加build命令到package.json的scripts配置项`  
	`"script":{  
	"build" : "webpack --config webpack.config.prod.js -p"  
	}`   
	`//命令行执行`  
	`npm run build`

-  2.webpack-dev-server   
这是一个基于Express.js框架开发的web server，默认监听8080端口。server内部调用Webpack，它提供了额外的如热更新"Live Reload"以及热替换"Hot Module Replacement"(HMR)等功能，有利于在开发环境中使用。  
	方式一  
	`//全局安装`  
	`npm install webpack-dev-server -g`  
	`$ webpack-dev-server --inline --hot`  
	方式二  
	`//添加到package.json的scripts配置项`  
	`"script:{  
	"start":"webpack-dev-server --inline --hot"
	}"`  
	`//运行`  
	`npm start`
	然后就可以在浏览器中预览了：http://localhost:8080
