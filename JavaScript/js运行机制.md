

##  js 运行机制



### 谈谈线程

Js是单线程语言，所谓单线程，是指Js引擎中负责解释和执行代码的线程只有一个，我们称它们为主线程。

实际上还存在其它线程： 处理AJAX请求的线程，处理DOM事件的线程，定时器线程，读写文件的线程（在Node.js中）等。我们称它为工作线程。

单线程意味着排队，前一个任务执行完后，才会执行后一个任务，由于I/O很慢，不得不等结果出来再往下执行。

### 函数调用栈与任务队列

Js有一个主线程（main thread）和call-stack(一个调用栈)，在对一个调用栈中的task处理的时候，其他都要等待。

```
console.log('Hi')

setTimeout(function cb(){
	console.log('there');
},5000);


console.log('sjs')

```




1.首先main()函数的执行上下文入栈，开始执行同步任务。

2.遇到console.log('Hi')，此时log('Hi')入栈，console.log方法只是一个webkit内核支持的普通的方法，它是同步任务，所以log('Hi')方法立即出栈被引擎执行。此时输出'Hi'。

3.当遇到setTimeout的时候，将setTimeout(callback,5000)添加到执行栈，调用栈发现setTimeout是之前提到的WebAPIs中的API，因此将其出栈之后，将延时执行的函数cd交给浏览器的timer模块进行处理。

4.timer模块去处理延时执行的函数，此时执行引擎接着立即继续往下处理后面代码，于是将log('SJS')加入执行栈，接下来log('SJS')出栈执行，输出'SJS'。而执行引擎在执行完console.log('SJS')后，程序处理完毕，main()方法也出栈。此时一项同步任务全部完成。

5.当timer模块中延时方法规定的时间到了之后，就将其放入到任务队列之中，此时调用栈中的task已经全部执行完毕，主线程处于空闲状态，于是主线程在下一次事件轮询时，发现刚刚放入消息队列的cb事件，便将cb入栈。

6.接着执行cb里面的代码，将log('there')入栈然后出栈执行，输出’there’，等到log执行结束之后cb函数运行完毕，cb接着出栈。之后主线程处于空闲状态，一直不停地轮询消息队列，看看没有要执行的事件。



### 宏任务(macro-task)、微任务(micro-task)

```
(function test() {
    setTimeout(function() {console.log(4)}, 0);
    new Promise(function executor(resolve) {
        console.log(1);
        for( var i=0 ; i<10000 ; i++ ) {
            i == 9999 && resolve();
        }
        console.log(2);
    }).then(function() {
        console.log(5);
    });
    console.log(3);
})()
```
setTimeout和Promise都被称之为任务源，来自不同任务源的回调函数会被放进不同的任务队列里面。

setTimeout的回调函数被放进setTimeout的任务队列之中。而对于Promise,它的回调函数并不是传进去的executer函数，而是其异步执行的then方法里面的参数，被放进Promise的任务队列之中。也就是说Promise的第一个参数并不会被放进Promise的任务队列之中，而会在当前队列就执行。

macro-task包括：script(整体代码), setTimeout, setInterval, setImmediate, I/O, UI rendering。

micro-task包括：process.nextTick, Promises, Object.observe, MutationObserver



** setTimeout任务队列和最后的函数调用栈中存放的都是setTimeout的回调函数，并不是整个setTimeout定时器。

事件循环的顺序是从script(整体代码),开始第一次循环，随后全局上下文进入函数调用栈




### 总

不同的任务会放进不同的任务队列之中。

先执行macro-task，等到函数调用栈清空之后再执行所有在队列之中的micro-task。

等到所有micro-task执行完之后再从macro-task中的一个任务队列开始执行，就这样一直循环。

当有多个macro-task(micro-task)队列时，事件循环的顺序是按上文macro-task(micro-task)的分类中书写的顺序执行的。



### 参考文章

[JavaScript运行机制:事件驱动编程详解](https://zhuanlan.zhihu.com/p/30894022?utm_medium=social&utm_source=wechat_session)

[深入浅出Javascript事件循环机制(上)](https://zhuanlan.zhihu.com/p/26229293)

[深入浅出JavaScript事件循环机制(下)](https://zhuanlan.zhihu.com/p/26238030)







































































