# Promise入门 #

## Promise 对象 ##
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
    
 
