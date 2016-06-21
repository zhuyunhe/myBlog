---
title: 前端面试-JS篇
date: 2016-06-14 09:20:24
tags: 
- 面试
- JavaScript
categories: 学习笔记

---
不断补充总结的一些前端面试中关于JS的问题<!-- more --> 

- 介绍JS的基本数据类型  
	`Undefined、Null、Boolean、Number、String`
	`可以用typeof操作符来检测`

- 介绍JS有哪些内置对象？
	`Object是JavaScript中所有对象的父对象。`  
	`数据封装类对象：Object、Array、Boolean、Number和String`  
	`其他对象：Function、Arguments、Math、Date、RegExp、Error`

- 什么是JS中的事件委托？
	`事件委托是解决“事件处理程序过多”的方案。事件委托利用了事件冒泡，只指定一个事件处理，就可以管理某一类型的所有事件。`  
	`也就是说：利用事件冒泡的原理，把相关事件绑定到父级元素上，触发执行效果。`  
	`例如：click事件会一直冒泡到document层。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。`

- 什么是Ajax，其核心是什么？
	`Ajax全称为Asynchronous JavaScript And XML`  
	`向服务器发送异步请求的时候，我们不必等待结果返回，而是同时做其他的事情，等到结果返回后，程序会根据设定（callback）进行后续操作，此时，页面也不会发生刷新，提高用户体验。`  
	`Ajax的核心对象就是xhr，XMLHttpRequest`  
	`发生Ajax请求的过程可分为以下六步：`
	1. 创建XMLHttpRequest对象，也就是创建一个异步调用对象。
	2. 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息。
	3. 设置响应HTTP请求状态变化的函数
	4. 发送HTTP请求
	5. 获取异步调用返回的数据
	6. 使用JS操作DOM或数据驱动框架来实现局部刷新  
		
			var xhr = new XMLHttpRequest();
			xhr.onreadystatechange = function(){
				if(xhr.readyState === 4){
					if((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304)
					var data = JSON.parse(xhr.responseText);
					callback(data);
				}
			};
			xhr.open('get',url,true);
			xhr.send(null);

- JS中如何实现继承？
	`在传统的面向对象语言中，类从其他类继承属性。然后在JavaScript中，继承可以发生在没有类的继承关系的对象之间。这种继承机制就是利用原型对象。`
	`先简单回顾一下构造函数、原型和对象实例这三者的关系：每个构造函数都有一个原型对象，相对应地这个原型对象包含一个指向构造函数的指针，而该构造函数new出来的每个对象实例都有一个指向原型对象的内部指针。`
	1. 原型链继承
	JavaScript内建的继承方法被称为原型对象链，又可称为原型对象继承。原型对象的属性可由对象实例访问，这就是继承的一种形式。对象实例继承了原型对象的属性。因为原型对象也是一个对象，它也有自己的原型对象并可以继承其属性，这就原型链：对象继承其原型对象，而原型对象继承它的原型对象，以此类推。
	所有对象，包括我们自己定义的对象都自动继承自Object。确切地说，所有对象都继承自Object.prototype。   
	下面是一种常用的继承方式，利用了原型链和构造函数相结合的技术，利用原型链来实现对方法的继承，借用构造函数来实现对属性的继承（主要针对一些数组、对象等引用类型的属性）。

			function SuperType(name){  
    			this.name = name;  
    			this.colors = ["red","yellow"];  
			}
			SuperType.prototype.sayName = function(){  
			    alert(this.name);  
			};  
			  
			function SubType(name,age){  
			    //借用父类的构造函数，这样SubType的实例中就有了name和colors这两个实例属性，这两个属性就是每个实例独有的了  
			    SuperType.call(this,name);  
			    this.age = age;  
			}  
			  
			//继承方法  
			SubType.prototype = new SuperType();  
			//这是把SubType的constructor属性改回来，创建SubType实例时调用SubType的构造函数  
			SubType.prototype.constructor = SubType;  
			  
			//在SubType的新原型上添加一个方法  
			SubType.prototype.sayAge = function(){  
			    alert(this.age);  
			};  
			  
			var instance1 = new SubType("zhu",24);  
			instance1.colors.push("black");  
			alert(instance1.colors);  //red,yellow black  
			instance1.sayName();  //zhu  
			instance1.sayAge();     //24  
			  
			var instance2 = new SubType("li",23);  
			instance2.colors.push("blue");  
			alert(instance2.colors);  //red,yellow blue  
			instance2.sayName();  //li  
			instance2.sayAge();     //23

	2. 深拷贝继承
	参考阮一峰老师的一篇[博客](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)。 

			var Chinese = { nation: '中国'};
			var Doctor = { career: '医生'};
	
			//让“医生”继承“中国人”，变成一位中国医生。
			//把父对象的属性全部拷贝给子对象，也能实现继承
			//由于父对象的属性可能是数组或对象，所以需要用深拷贝
			function deepCopy(p, c){
				var c = c || {};
				for(var i in p){
					if(typeof p[i] === 'object'){
						c[i] = (p[i].constructor === Array) ? [] : {};
						deepCopy(p[i], c[i]);
					} else{
						c[i] = p[i];
					}
				}
				return c;
			}
			
			Chinese.place = ['北京','上海','广州'];
			
			//深拷贝
			var Doctor = deepCopy(Chinese);
			Doctor.place.push('大理');
			console.log(Chinese.place);
			console.log(Doctor.place);

- JavaScript中的作用域链？
	`全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。`  
	`当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到该属性或方法，就会上溯到上层作用域查找，直至全局函数，这种组织形式就是作用域链。`

- 什么是闭包？  
	`闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数。`  
	`闭包的特性：`  
	`1. 函数内再嵌套函数`  
	`2. 内部函数可以引用外层的参数和变量`
	`3. 参数和变量不会被垃圾回收机制回收`  
	例子：  
		
		function createFunction(){
			var result = new Array();
			for(var i=0; i<10; i++){
				result[i] = (function(num){
					return function(){
						return num;	
					};
				})(i);
			}
			return result;
		}
	上面这个例子中有两个闭包，外层的那个立即执行的匿名函数是一个闭包。在调用每个匿名函数时，我们传入了变量i，由于函数参数是按值传递，所以就会将变量i的当前值赋值给参数num。而在每个匿名函数内部，我们创建并返回了一个可以访问num的闭包。这样一来，result数组中的每个函数都有了自己num变量的一个副本，因此就可以返回各自不同的数值了。  

- new操作符具体干了什么？
	1. 创建一个空对象，并且this变量引用该对象，同时还继承了该函数的原型。
	2. 属性和方法被加入到this引用的对象中。
	3. 新创建的对象由this所引用，并且最后隐式返回this。

- JSON的了解？
	`JSON是一种轻量级的数据交换格式。`
	`它是基于JavaScript的一个子集。数据格式简单，易于读写，占用带宽小。`  

- eval是做什么的？
	`eval函数的功能是把对应的字符串解析成JS代码并运行。`  
	`应该避免使用eval，不安全，且非常消耗性能（2次处理，一次解析成JS语句，一次执行）`

- null和undefined的区别？
	`null表示没有对象，即此处不应该有值`
	`undefined`表示缺少值，此处应该有一个值，但是还没有定义。

- 如何解决跨域问题？
	`JSONP、window.name、window.postMessage、iframe`

- 模块化开发怎么做？
	`立即执行函数，不暴露私有成员。`

- 函数中的arguments是什么，是数组吗？若不是，如何将它转化为真正的数组？
	`arguments是一个类数组对象（它并不是Array的实例），它可以通过方括号语法访问每个元素，也可以使用length属性来确定调用函数时传递进来了多少个参数。`
	`Array.prototype.slice.call(arguments)`

- 列举JavaScript中的typeof函数的可能结果，如何区分：{}和[]类型？  
	`typeof的值可能有："number","string","boolean","object","undefined","function"`   
	`区分{}和[]:`  
	`1. [].constructor === Array，t={}，t.constructor === Object`  
	`2. [] instanceof Array ==> true`  
	`t = {},t instanceof Object ==> true`

- Function中的call、apply、bind的区别是什么？请分别写出示例代码
	1. bind()方法会创建一个新函数，当这个新函数被调用时，它的this值是传递给bind()的第一个参数，它的参数是bind()的其他参数和其原本参数。  
	bind()最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的this值。JavaScript新手经常犯的一个错误就是将一个方法（假设方法中用到了this对象）从对象中拿出来，然后再调用，并且希望方法中的this是原来的对象。如果 不做特殊处理的话，一般会丢失原来的对象。用原来的函数和原来的对象创建一个绑定函数，可以漂亮地解决这个问题：
			
			this.x = 9;
			var module = {
				x: 81,
				getX: function(){ return this.x }
			}
			
			module.getX();	//81
			
			var retrieveX = module.getX;

			//注意这段代码要在非严格模式下运行，严格模式下会报错  
			//因为严格模式下this为undefined
			retrieveX();	//9,因为此时方法中的this指向的是全局对象
			//用bind方法解决这个问题
			var boundGetX = retrieve.bind(module);
			boundGetX();	//81

	2. apply()和call()很类似，都是在指定指定this值和参数的情况下调用某个函数。差别只是apply()方法的参数是以数组或类数组对象的形式存在。call()方法的参数是一个参数列表（逗号隔开）。  
	实例：比如之前讲到的把函数的arguments类数组对象转成一个数组的方法：Array.prototype.slice.call(arguments)

- 使用jQuery，找到id为“selector”的select标签中拥有'data-target'属性，且值为“isme”的option的值？
	`$('select#selector>option[data-target="isme"]').val()`

- 设计一个算法，合并两个有序数组为一个有序数组？  
	`与归并排序类似`
		
		var a = [3,6,8,9,15];
		var b = [1,2,5,7,12,16];
		var result = [];
		function mergeArray(a,b,result){
			var i=0,j=0,index=0;
			while(i<a.length && j<b.length){
				if(a[i] <= b[j]){
					result[index++] = a[i++]; 
				} else{
					result[index++] = b[j++];
				}
			}
			
			while(i<a.length){
				result[index++] = a[i++];
			}
			while(j<b.length){
				result[index++] = b[j++];
			}
		}
	
- 介绍一下JS中的纯函数和非纯函数？  
	纯函数是指不依赖和修改其作用域之外变量的函数。
	[参考](http://mp.weixin.qq.com/s?__biz=MzA4NjE3MDg4OQ==&mid=2650963421&idx=1&sn=5ea0637e84b54166a93f9cda76fe3816&scene=23&srcid=0616Euwq4iGer5P5sgPSrNlp#rd)
	纯函数的好处：
	1. 方便测试（可测试性）
	2. 自我注释（可读性）
	3. 避免函数内的全局变量（可复用性）
	
	要在所有场景中都使用纯函数是不现实的，因为总有一些情况需要你跳出函数获取数据，最常见的救赎查询DOM以获取某个DOM元素。这是JavaScript的一个实际情况并且我们不得不这样做，但我们可以用纯函数来做一些优化。  
	eg：从DOM中获取一个元素并将它背景设为红色  
		
		function changeElementToRed(){
			var foo = document.getElementById('foo');
			foo.style.backgroundColor = 'red';
		}
		changeElementToRed();
		
	这段代码有两个问题，都可以通过转变为纯函数来解决：
	1. 这个函数完全不能被复用。它和特定的DOM元素紧密联系在一起。我们不能使用它修改其他元素。
	2. 由于是非纯函数，它很难被测试。想要测试它，我们不得不创建带有特定ID的元素。  
	
	我们可以用纯函数的思想重写一下这个函数：  

		function changeElementToRed(ele){
			elem.style.backgroundColor = 'red';
		}
		function changeFooToRed(){
			var foo = document.getElementById('foo');
			changeElementToRed(foo);
		}
	我们修改changeElementToRed()函数使它不再与任何特定的元素关联。与此同时，我们把它变纯了，就这带来了我们之前讨论过的所有优点。  
	需要注意的是，修改后的代码里还有非纯函数changeFooToRed()。这是不可避免的，但这里要说的是把一个函数改造成纯函数会增加它的可读性，可复用性以及可测试性。通过创建尽可能多的纯的、可复用的代码，并把不纯的代码限制在最低程度，我们可以编写出更好的代码并在未来避免更多烦恼。  
	
- endodeURI和encodeURIComponent的区别？  
	它们都是属于Global全局对象。用于对URI进行编码，以便发送给浏览器。    
	首先我们要知道如果要URL的参数中有特殊字符，需要对其进行转义，转义编码的格式为：%加字符的ASCII码，即一个百分号%，后面跟对应字符的UTF-8码（16进制）码值，例如空格的编码值是"%20"。每一个非ASCII字符都会被替换为"%hh"格式，它对应于该字符按UTF-8编码后的编码值。
	**encodeURI不会对ASCII字母、数字和ASCII标点符号（-_.!~*'()）以及在URI中具有特殊意义的符号（;/?:@&=+$,#）进行编码**。利用encodeURI这个函数对汉字进行编码时，比如“日”这个汉字出现在URL中，就会将它转义为“%E6%97%A5”，由于UTF-8是8位不定长的编码，一个中文字使用三个位元组(byte),按照UTF-8编码的“日”字的码值是“11100110 10010111 10100101”，对应的十六进制也就是"E6 97 A5"。  
	**encodeURIComponent转义除了字母、数字、(、)、.、!、~、*、'、-和_之外的所有字符。**为了避免服务器收到不可预知的请求，对任何用户输入的作为URI部分的内容你都需要用encodeURIComponent进行转义。比如，一个用户可能会输入"Thyme &time=again"作为comment变量的一部分。如果不使用encodeURIComponent对此内容进行转义，服务器得到的将是comment=Thyme%20&time=again。请注意，"&"符号和"="符号产生了一个新的键值对，所以服务器得到两个键值对（一个键值对是comment=Thyme，另一个则是time=again），而不是一个键值对。

- String类型的字符串有哪些常用方法？  
	1. 字符方法：charAt()和charCodeAt()
	2. 字符串操作方法：concat(), slice(), substring(), substr() 
	3. 字符串位置方法：indexOf()和lastIndexOf()
	4. trim()方法
	5. 大小写转换方法：toLowerCase(), toLocaleLowerCase(), toUpperCase(), toLocaleUpperCase()
	6. 模式匹配方法：match(), search(), replace()
	7. 比较方法：localeCompare()
	8. 转换方法：fromCharCode()

- Array类型的数组对象有哪些常用方法？
	1. 转换方法：toString(), valueOf(), toLocaleString(), join()
	2. 栈方法（后进先出）：push()方法（返回修改后数组的长度）、pop()方法（返回移除的项）。
	3. 队列方法（先进先出）：shift()移除数组中的第一项并返回、unshift()能在数组前端添加任一项并返回数组长度。
	4. 重排序方法：reverse()、sort()
	5. 操作方法：concat()、slice()、splice()
	6. 位置方法：indexOf()、lastIndexOf()
	7. 迭代方法：every(), some(), filter(), forEach(), map()
	8. 归并方法：reduce()从第一项开始遍历, reduceRight()从最后一项开始遍历  


- JSONP的原理？  
	[参考](https://segmentfault.com/a/1190000002438126)  
	JSONP是JSON with padding（填充式JSON或参数式JSON）的简写，是应用JSON的一种新方法。JSONP和JSON差不多，只不过是被包含在函数调用中的JSON，就像下面这样。  
	`callback({"name": "Nicholas"})`这是从跨域服务器上返回的数据。  
	JSONP由两部分组成：回调函数和数据。回调函数是当响应到来时应该在页面中调用的函数。回调函数的名字一般是在请求中指定的。而数据就是传入回调函数中的JSON数据。下面是一个典型的JSONP请求。  
	`http://freegeoip.net/json/?callback=handleResponse`  

	JSONP原理：
	1. 首先在客户端注册一个callback，然后把callback的名字传给服务器。此时，服务器会先生成客户端想要的JSON格式的数据，然后以JavaScript语法的方式，生成一个函数（function），函数名就是JSONP请求中的callback参数的值，对应上例中就是handleResponse。
	2. 服务器端接着把客户端想要的JSON数据以形参的方式放到function中，这样就生成了一段js代码，把这段代码返回给客户端。
	3. 客户端浏览器解析script标签，并执行返回的js代码，也就是调用了客户端定义的回调函数。
	总结：说白了就是客户端定义一个函数，如handleResponse，请求地址后服务器端返回的结果是调用了handleResponse，并且所要的数据都放在了handleResponse的参数里，以供客户端使用。  
	注意：**JSONP是需要服务器端配置的**。

- localStorage怎么设置过期时间？
	localStorage是本地存储，没有过期一说，除非手动清除或者清除浏览器缓存，否则都会保留。
	
	
