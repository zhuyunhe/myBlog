---
title: jquery使用的一些技巧
date: 2016-07-21 14:32:40
tags:
- 学习
categories: 学习笔记
---
参考阮一峰老师的一篇(博客)[http://www.ruanyifeng.com/blog/2011/08/jquery_best_practices.html]<!-- more -->

＃ 选择器
## jquery中最快的选择器
id选择器和标签选择器

## 较慢的选择器
class选择器$('.className')、伪类选择器$(':hidden')、属性选择器$('[attribute=value]')

# 父子元素关系
最佳实践：$parent.hind('.child')  
  
其他形式
  

```
$('.child',$parent)  

$parent.children('.child');  
  
$('#parent > .child');  
  
$('#parent .child'); 
``` 

# 避免过度使用jquery
原生js方法永远比jquery快。  
## 例子
### 为a元素绑定一个处理点击事件的函数，点击时取到a元素的id属性值  

	$('a').on('click',function(){
		//连续调用了两次jquery
		alert($(this).attr('id'));
	});

可以用原生js改写  

	$('a').on('click',function(){
		alert(this.id);
	});

类似的，要取得一个checkbox的选中时的值  

	$('input[type="checkbox"]').on('click', function(){
		if(this.checked){
			alert(this.value);
		}
	});

# 缓存jquery对象并保持链式调用写法

	var $myDiv = $('#myDiv');
	var val = $myDiv.find('.input1').val();
	//关键时$.end()方法
	$myDiv.find('.input2').val(val)
		  .end()
		  .find('.input3').attr('disabled','disabled');

# 使用事件委托
利用事件冒泡原理，尽量减少事件绑定。

# 少改动DOM结构
1. 不要频繁使用.append()、insertBefore()、insertAfter()
如果要插入多个元素，尽量合并后再插入，或者使用documentFragment

2. 在一个元素上存储数据，不要写成下面这样：  
	
```
var elem = $('#elem');
elem.data(key,value);
```
而要写成  

	var elem = $('#elem');
	$.data(elem[0],key,value);

elem.data()方法是定义在jQuery函数的prototype对象上面的，而$.data()方法是定义jQuery函数上面的，调用的时候不从复杂的jQuery对象上调用，所以速度快得多。


