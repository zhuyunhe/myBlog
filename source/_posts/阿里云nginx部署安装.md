---
title: 阿里云nginx部署安装
date: 2016-05-21 14:32:02
tags: 
- 学习
- Nginx
categories: 学习笔记
---
- 下载并解压Nginx
我是先在自己的台式机上下载好Nginx安装包，然后通过WinSCP这个软件把安装包扔到阿里云的主机上并解压一下，过程如下图。  
![step1](/image/nginx/step1.png)  
![step2](/image/nginx/step2.png)  
![step3](/image/nginx/step3.png)  
- cd进入解压后的nginx文件夹，第一步在终端输入'./configure'后回车进行配置，这个时候一般会出错，如下图所示  
![step4](/image/nginx/step4.png)  
提示缺少了pcre这个库文件，然后我去其[官网](http://www.pcre.org/)下载了后安装，但安装后配置nginx时还是提示出错，在网上找了一个解决方法：`sudo apt-get install libpcre3 libpcre3-dev`可以解决。接下来继续回到nginx文件夹根目录下执行`./configure`，然后发现依然报错，原因是缺少了zlib和openSSL这两个库，我们同样是去其官网把安装包下载一下，解压后分别进入其文件夹执行`./configure`回车，`make`回车，`make install`回车，三步安装（注意安装openSSL时若`./configure`不好使的话换为`./config`）。把这两个库安装完成之后，我们再次回到nginx文件夹，执行三步走  
`./configure`  
`make`  
`make install`  
这次终于提示安装成功了，nginx默认被安装在了/usr/local/nginx文件夹。接下来，我们`cd /usr/local/nginx/sbin`,然后启动nginx服务器`./nginx`，我们再次打开我们的网站，可以看到如下效果，说明nginx安装成功了。  
![step6](/image/nginx/step6.png)  

---
这只是很粗糙的安装部署，以后再进一步学习完善，以下是参考的几篇博客：  
http://www.2cto.com/os/201201/117129.html  
http://blog.csdn.net/feng88724/article/details/7255714  
http://www.cnblogs.com/65702708/archive/2011/04/08/2009111.html  

