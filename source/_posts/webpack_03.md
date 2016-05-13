---
title : webpack学习笔记-3
date : 2016-05-13 09:55:00
tags : 
- webpack
- 学习
categories : 学习笔记
---
# webpack.config.js配置文件中的entry配置项
---  

Entry配置项告诉Webpack应用的根模块或起始点在哪里，它的值可以是字符串、数组或对象。像大多数app一样，倘若你的应用只有一个单一的入口，enter项的值你可以使用任意类型，最终输出的结果都是一样。 

 	`//字符串`  
	`entry:'./src/index.js'`  
	`//数组`  
	`entry:['./src/index.js']`  
	`//对象`  
	`entry:{index:'./src/index.js'`  

- entry : 数组类型  
	如果你想添加多个彼此不互相依赖的文件，你可以使用数组格式的值。  
	例如，你可能在html文件里引用了“googleAnalytics.js”文件，可以告诉Webpack将其加到bundle.js的最后。  

		`{`    
		`entry:['./src/index.js','./src/googleAnalytics.js']`  
		`output:{`  
			`path:'/dist',`  
			`filename:'bundle.js'`  
		`}`  
		`}`

- entry :对象  
	现在，假设你的应用是多页面的(multi-page application)而不是SPA，有多个html文件（假设index.html和profile.html）。然后你通过一个对象告诉Webpack为每个html生成一个bundle文件。  
	以下的配置将会生成两个js文件：indexEntry.js和profileEntry.js分别会在index.html和profile.html。  

		`{`  
			`entry:{`  
			`"indexEntry" : './src/index.js',`  
			`"profileEntry" : './src/profile.js'`  
			`},`  
			`output:{`  
				`path : '/dist',`  
				`filename : "[name].js"  //中括号里的name属性对应entry对象中的key值(键名)`  
			`}`  
			`}`  

	用法  

		`//profile.html`  
		`<script src="/dist/profileEntry.js></script>"`  
		`//index.html`  
		`<script src="/dist/indexEntry.js></script>"`

- entry : 混合类型
	你也可以在entry对象里使用数组类型，例如下面的配置将会生成3个文件：vender.js、index.js、profile.js。  


		`{`  
			`entry:{`  
			`"vender" : ['jquery.js','analytics.js','optimizely.js']`   
			`"index" : './src/index.js',`  
			`"profile" : './src/profile.js'`  
			`},`  
			`output:{`  
				`path : '/dist',`  
				`filename : "[name].js"  //中括号里的name属性对应entry对象中的key值(键名)`  
			`}`  
			`}`  