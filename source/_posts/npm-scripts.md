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
package.json文件中有一个script字段，可以用于指定脚本命令，供npm直接调用。

	{
		'scripts': {
			'commit': 'sh commit.sh',
			'pull': 'sh pull.sh'
		}
	}
