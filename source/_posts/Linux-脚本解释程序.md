---
title: Linux_脚本解释程序
date: 2016-06-04 10:57:06
tags:
- 学习
- Linux
categories: 学习笔记

---
## shell script（shell脚本）
shell script是利用shell的功能所写的一个“程序”（program），这个程序是利用纯文本文件，将一些shell的语法与命令（含外部命令）写在里面，搭配正则表达式、管道命令与数据流重定向等功能，以达到我们想要的处理目的。  
简单的说，shell script就像早起DOS年代的批处理文件（.bat），最简单的功能就是将许多命令写在一起，让用户很轻易就能够一下子处理复杂的操作。  
### 脚本的第一行
在编写脚本程序时，我们脚本程序的第一行通常是这样的`#!/bin/bash`,这句话是用来声明这个script使用的shell名称，也就是指定用哪个shell程序来解释这个script程序。`#!/bin/bash`这句话就是告诉系统，我们要直接以/bin/bash这个程序来执行script脚本文件内的相关命令操作。   
但在很多地方，我们通常会这么写脚本程序的第一行`#!/usr/bin/env bash`，我一开始看到这句话时候也挺懵逼的，鸟哥的书里好像也没讲到，然后果断求助google，在[stackOverflow](http://stackoverflow.com/questions/16365130/the-difference-between-usr-bin-env-bash-and-usr-bin-bash)上找到了解释。

### /usr/bin/env
> Using #!/usr/bin/env NAME makes the shell search for the first match of NAME in the $PATH environment variable. It can be useful if you aren't aware of the absolute path or don't want to search for it.  

使用`#!/usr/bin/env NAME`这句话可以让shell寻找到在$PATH环境变量中第一个匹配'NAME'的shell来执行这个脚本程序。如果你不知道当前系统的shell的绝对路径或不想搜索它们时，`#!/usr/bin/env NAME`是很有用的。  
其实大意应该就是在不同的系统中，执行脚本的shell们可能被放到了不同了地方（绝对路径不一样），那么使用`#!/usr/bin/env NAME`就很灵活，因为只要'NAME'在$PATH环境变量中，我们就可以找到并使用它。  

	#!/usr/bin/env bash` #lends you some flexibility on different systems，在不同的系统中能灵活使用
	#!/usr/bin/bash`     #gives you explicit control on a given system of what executable is called，在指定操作系统中，能让你明确知道哪个执行脚本的shell被使用

ps：以后要好好学英语
