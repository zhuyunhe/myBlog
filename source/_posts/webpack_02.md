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
-  3.Webpack VS Webpack-dev-server选项  
	首先要注意的是像inline和hot这些选项是Webpack-dev-server特有的，而另外的如hide-modules则是CLI模式特有的选项。  
	然后值得注意的是你可以通过以下两种方式向webpack-dev-server传入参数：  
	+ 通过webpack.config.js文件的“devServer”对象  
	`//通过webpack.config.js传参`  
	`devServer:{`  
		`inline:true,`  
		`hot:true,`  
	`}`	   
	根据实践，发现有时候devServer配置项（hot:true和inline:true）不生效，我更偏向使用如下的CLI方式传递参数。
	+ CLI方式传参  
	`//package.json`  
	`{`  
		`"script":"webpack-dev-server --hot --inline"`  
	`}`  
	要注意的是此时要确保你没同时在webpack.config.js中配了hot:true。
-  4.“hot”和“inline”选项的含义  
	“inline”选项会为入口页面添加“热加载”功能，“hot”选项则开启“热替换（Hot Module Reloading）”，即尝试重新加载组件改变的部分，而不是重新加载整个页面。如果同时传入两个参数的话，当资源改变时，webpack-dev-server将会先尝试HRM（即热替换），如果失败则重新加载整个入口页面。  
	`//当资源发生改变时，以下三种方式都会生成新的bundle，但是又有区别:`  
	`//1.不刷新浏览器`  
	`$ webpack-dev-server`  
	`//2.刷新浏览器`  
	`$ webpack-dev-server --inline`  
	`//3.重新加载改变的部分，如HMR没有成功则刷新浏览器`  
	`$ webpack-dev-server --inline --hot`
