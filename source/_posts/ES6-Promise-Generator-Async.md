---
title: ES6-Promise-Generator-Async
date: 2016-08-12 17:06:17
tags:
- 学习
categories: 学习笔记
---
JavaScript是单线程执行的，所以异步编程对JS来说很重要，如果没有异步编程的话，JS代码执行起来会很卡。 
异步：简单的说就是一个任务分成两段，先执行第一段，然后转而执行其他任务，等做好了准备，再回头执行第二段。  
同步：任务从上往下依次执行的模式。  
JavaScript语言对异步编程的实现，是一个逐步发展演进的过程，从最初的callback到现在async，程序的架构越来越好，可读性也越来越高。现在我们就沿着callback=>promise=>generator=>async的路线，一步步回顾，这样才能更清晰的了解ES6异步编程的来龙去脉。<!-- more -->
# 基础准备
假设我们现在要实现一个功能：获取数据库中最新的一篇文章的作者的邮箱。后台给我提供了有三个API。
  
* 第一个API抓取文章列表，返回如下格式数据。  

```
[  
	{articleId:'1',title:'aaaaa'},
	{articleId:'2',title:'bbbbb'},
	{articleId:'3',title:'ccccc'}
]
```
* 第二个API是根据文章id，获取文章的内容，返回如下格式数据。

```
{
	authorId:1,
	content:'文章内容',
	timestamp:'2016-08-11'
}

```
* 第三个API是根据作者id，返回作者信息，返回如下格式的数据。

```
{
	name:'zyh',
	tel:'13800001111',
	email:'meituan.com'
}

```
程序执行的流程是：获取文章列表＝》获取文章内容＝》获取作者信息。

# 实现
## 回调函数callback
JavaScript最初对异步编程的实现，就是回调函数。所谓回调函数就是把任务的第二段单独写在一个函数里面，等到重新执行这个任务的时候，就直接调用这个函数，这样就可以解决阻塞的问题。

```
function ajax2ArticleList(callback) {
    setTimeout(function () {
        //假装用ajax成功请求回来了数据data
        var data =[
            {articleId:'1',title:'aaaaa'},
            {articleId:'2',title:'bbbbb'},
            {articleId:'3',title:'ccccc'}
        ];
        callback(data);
    },1000);
}
function ajax2Article(articleId,callback){
    setTimeout(function () {
        //假装用ajax成功请求回来了数据data
        var data = {authorId:1,content:'文章内容',timestamp:'2016-08-11'};
        callback(data);
    },1000);
}
function ajax2Author(authorId,callback){
    setTimeout(function () {
        //假装用ajax成功请求回来了数据data
        var data = {name:'zyh',tel:'13800001111','email':'zhuyunhe@meituan.com'};
        callback(data);
    },1000);

}

//callback hell
ajax2ArticleList(function (articleList) {
    ajax2Article(articleList[0].articleId,function (article) {
        ajax2Author(article.authorId,function (author) {
            console.log(author.email);
        });
    })
});

```
callback的写法我们比较熟悉，但其缺点也很明显，就是程序看上去是横向发展的，不符合我们期望的程序从上到下的顺序执行的思维，并且这种横向的层层嵌套，很容易滋生bug。

## Promise
Promise是ES6中引入的语法，它为了解决callhell而生，在ES6中提供了原生的Promise对象可以直接使用。所谓Promise，就是一个对象，用来传递异步操作的消息。  
我们把我们要实现的功能改用Promise对象实现一下，就能明显看到效果。

```
function ajax2ArticleList() {
    return new Promise(function (resolve,reject) {
        setTimeout(function () {
            //假装用ajax成功请求回来了数据data
            var data =[
                {articleId:'1',title:'aaaaa'},
                {articleId:'2',title:'bbbbb'},
                {articleId:'3',title:'ccccc'}
            ];

            resolve(data);
        },1000);
    });
}
function ajax2Article(articleId){
    return new Promise(function (resolve,reject) {
        setTimeout(function () {
            //假装用ajax成功请求回来了数据data
            var data = {authorId:1,content:'文章内容',timestamp:'2016-08-11'};
            resolve(data);
        },1000);

    });
}
function ajax2Author(authorId){
    return new Promise(function (resolve,reject) {
        setTimeout(function () {
            //假装用ajax成功请求回来了数据data
            var data = {name:'zyh',tel:'13800001111','email':'zhuyunhe@meituan.com'};
            resolve(data);
        },1000);
    })
}



//分步执行
var p1 = ajax2ArticleList().then(function (articleList) {
    return ajax2Article(articleList[0].articleId);
});
var p2 = p1.then(function (article) {
   return ajax2Author(article.authorId);
});
var p3 = p2.then(function (author) {
    console.log(author.email);
});

//分步执行进化为链式执行
ajax2ArticleList().then(function (articleList) {
    return ajax2Article(articleList[0].articleId);
}).then(function (article) {
    return ajax2Author(article.authorId);
}).then(function (author) {
    console.log(author.email);
});

//配合ES6的箭头函数
ajax2ArticleList()
    .then(articleList => ajax2Article(articleList[0].articleId))
    .then(article => ajax2Author(article.authorId))
    .then(author => console.log(author.email));

```
我们可以看到，在上面的代码中，我们拿掉了ajax2ArticleList、ajax2Article、ajax2Author这三个函数中的callback，并且在这三个函数中都返回了一个promise，在原来出现callback的地方换成了resolve。我们现在可以看到，原本的callback hell被我们变成纵向的。  
但我们也可以看到，promise其实也并没有完全摆脱callback，promise对象相当于一个介于异步操作和callback之间的一个中介。

# Generator
在ES6中还有一个叫Generator的东西，利用它我们可以写出看上去是同步但其实是异步的代码。Generator函数是一个异步操作的容器，其最大的特点就是能交出函数的执行权（即暂停执行），在异步操作需要暂停的地方，就用yield语句注明，把函数执行权交给异步操作。  
Generator函数调用后，该函数并不执行，返回的也不是函数运行的结果，而是一个指向内部状态的指针。
还有要注意的一点就是，Generator函数可以将异步操作表示的很简洁，但是流程管理（即何时执行第一阶段，何时执行第二阶段）却不是很方便（我理解就是执行完前一个next后，什么时候执行下一个next不好控制，还不能自动执行）。当然我们也有办法来处理这个问题。  
Generator是一个异步操作的容器。它的自动执行需要一种机制，当异步操作有了结果能够自动交回执行权。有两种方法可以做到这一点：  

* 回调函数。将异步操作包装成Thunk函数，在回调函数中交回执行权。
* Promise对象。将异步操作包装成Promise对象，用then方法交回执行权。  

下面是配合上Generator＋Promise配合的demo  

```
/**
 * Created by zyh on 16/8/12.
 */
function ajax2ArticleList() {
    return new Promise(function (resolve,reject) {
        //假装用ajax成功请求回来了数据data
        var data =[
            {articleId:'1',title:'aaaaa'},
            {articleId:'2',title:'bbbbb'},
            {articleId:'3',title:'ccccc'}
        ];
        setTimeout(function () {
            resolve(data);
        },1000);
    });
}
function ajax2Article(articleId){
    return new Promise(function (resolve,reject) {
        //假装用ajax成功请求回来了数据data
        var data = {authorId:1,content:'文章内容',timestamp:'2016-08-11'};
        setTimeout(function () {
            resolve(data);
        },1000);
    });
}
function ajax2Author(authorId){
    return new Promise(function (resolve,reject) {
        //假装用ajax成功请求回来了数据data
        var data = {name:'zyh',tel:'13800001111','email':'meituan.com'};
        setTimeout(function () {
            resolve(data);
        },1000);
    })
}

//generator函数
function* run(){
    var articleList = yield ajax2ArticleList();
    var article = yield ajax2Article(articleList[0].articleId);
    var author = yield ajax2Author(article.authorId);
    console.log(author.email);
}

//手动执行Generator
var gen = run();
gen.next().value.then(function (data) {
    console.log(data);
    gen.next(data).value.then(function (data) {
        console.log(data);
        gen.next(data).value.then(function (data) {
            console.log(data);
        })
    })
});

//runGenerator();
//自动按次序执行所有步骤
function runGenerator() {
    var gen = run();

    function go(result) {
        if(result.done){
            return;
        } else{
            result.value.then(function (r) {
                go(gen.next(r));
            });
        }
    }

    go(gen.next());
}

//autoRun(run);
//配合Promise的generator自动执行器
//Generator函数的yield命令后面只能是Promise对象
function autoRun(run) {
    var gen = run();
    function go(data) {
        try {
            var result = gen.next(data);
            if(result.done) return result.value;
            //用then方法交回Generator函数执行权执行权
            result.value.then(go);
        } catch(err){
            console.log(err);
        }
    }
    go();
}

```
注意到run这个Generator函数，虽然里面的执行的是异步操作，但是在写法上已经和同步很像了，拿掉“yield”这个关键字后，就是一模一样的同步代码，但其实是非同步的。这就是Generator的精髓：用同步的语法来写异步的代码。虽然我们给出了：手动执行＝》自动执行＝》执行器执行，用三种不同的方法来执行Generator函数，在实际操作中，你完全不用自己写Generator函数的执行函数，可以用TJ大神的[co](https://github.com/tj/co)这个模块来做，co模块的和上面的第三种执行方法做的事也差不多。

# async
ES7中给我们提供了一个Generator函数的语法糖，就是async函数。async函数自带了执行器，不必用co这样的模块，并且语义上也比Generator更好，asynch和await比起星号和yield来说更加清楚。同时，async函数的适用性更广。但由于现在用的还不是很多（实验阶段），我也没有用过，下面是一段针对这个例子的简单的demo。

```
var asyncRun = async function () {
    var articleList = await ajax2ArticleList();
    var article = await ajax2Article(articleList[0].articleId);
    var author = await ajax2Author(article.authorId);
    console.log(author.email);
}

```

# 参考
[promise,generator,async与ES6](http://huli.logdown.com/posts/292655-javascript-promise-generator-async-es6)
[阮一峰博客](https://github.com/ruanyf/articles/blob/master/2015/2015-05-12-es6-asynchronous-programming.md)


