# Promise入门 #


### Promise 是什么

![](https://user-images.githubusercontent.com/20238205/37249866-8d1cea46-252a-11e8-9ae3-e9c0f8c04d0c.png)


- Promise是一个构造函数，此函数中有all、race、reject、resolve等方法；
- Promise函数有原型属性prototype，此属性是一个指针，指向此构造函数的原型对象。默认情况下，原型对象都会自动获得一个constructor属性，这个属性是一个指向prototype属性所在函数（这里是Promise）；
- 函数原型对象上有then、catch等方法；

因此，Promise 对象中有对象实例本身和原型对象的属性和方法；

### Promise 用途

**MDN 中文**

- Promise对象用于异步计算
- 一个Promise表示一个现在、将来或永不可能可用的值。

**按照用途来解释**

- 主要用于异步计算
- 可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果。
- 可以在对象之间传递和操作Promise ，帮助我们处理队列。


### Promise 对象 

Promise，简单的说就是一个状态容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。


    const promise = new Promise(function(resolve, reject){
		if(/*异步操作成功*/){
			resolve(value);
		}else {
			reject(error);
		}

	})

Promise 构造函数接受一个执行函数最为参数，该执行函数有两个参数，它们是两个函数

resolve函数： 将promise 对象的状态变成成功，并将异步操作的结果传递出去

reject函数： 将promise 对象的状态变成失败，异步操作失败时调用，并将异步报错作为参数传递出去。

## Promise.prototype.then() ##

本质上，一个promise是某个函数返回的对象，你可以把回调函数绑定在这个对象上，而不是把回调函数当作参数传进函数。 

[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)

 then方法为Promise 实例添加状态改变时的回调函数 

    promise.then(function(value){
    	//sucess
    }, function(error){
    	//failure
    });
    
 

