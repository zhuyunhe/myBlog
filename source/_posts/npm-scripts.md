---
title: npm_scripts
date: 2016-06-05 16:10:49
tags: 
- node
- 学习
categories: 学习笔记

---
## npm执行脚本
npm不仅可以用于模块管理，还可以用于执行脚本。  
package.json文件中有一个script字段，可以用于指定脚本命令，以供npm直接调用。

	{
		'scripts': {
			'commit': 'sh commit.sh',
			'pull': 'sh pull.sh'
		}
	}
其中，commit.sh文件内容为：  
	
	git add .
	git commit -m 'common commit'
	git push
这里要注意的是commit.sh这个文件的位置，我第一次写的时候，就把脚本文件放到的这个博客项目的根目录下，如下图所示：  
![](/image/npm_script/1.png)
然后我在本机（window）直接输入`npm run commit`时，结果是这样滴：  
![](/image/npm_script/2.jpg)  
很明显嘛，在window上本来就不能直接运行一个shell脚本，在阿里云上的ubuntu上直接运行倒是可以的。  

后来知道了，原来`npm run`命令会自动在环境变量$PATH添加`node_modules/.bin`目录，所以我们在package.json文件内的scripts字段内指定的任务时，一般无需指定脚本文件的路径，就像我上面写的那样，**但此时需要将脚本文件放到./node_module/.bin/目录下去**，`npm run`命令会在这个目录下自动寻找对应的脚本文件。然后我把commit.sh放进去试试看。结果不用看啦，当然还是不行的。其实仔细瞅一眼`node_modules/.bin`这个文件夹里放的东西就知道啦，里面就是一堆命令的执行文件程序，每个命令都对应两个文件，一个是window下用.cmd结尾的Window命令脚本，还有一个当然就是Linux系统下常用的shell script啦。我直接扔个.sh的文件进去当然是不行的。  
![](/image/npm_script/3.jpg)   
当然了，只是window下的这个`node_modules/.bin`文件夹长才长这个样子，阿里云上的`node_modules/.bin`肯定就不是这样啦，`node_modules`这个文件夹本来也只是本地开发用嘛。  
但是，当我在Ubuntu上把commit.sh这个文件**剪切**到`node_modules/.bin`文件下，然后再运行`npm run commit`时，也报错了。  
![](/image/npm_script/2.png)  
说是无法打开这个commit.sh文件，然后我瞅了一眼原来是这个文件我还木有x执行权限，坑爹啊，赶紧用chmod命令弄一下（Linux渣也是跪了），输入`chmod a+x commit.sh`，然后我们再回去运行`npm run commit`试试。然后，还是跪了，还是刚才那个错误，不是说放到`npm run commit`里就可以嘛，明天再好好看看npm的[官方文档](https://docs.npmjs.com/cli/run-script)吧，今晚先睡了。
