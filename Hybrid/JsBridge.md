
## JSBridge设计


JSBridge是Native代码与JS代码的通信桥梁。目前的一种统一方案是:H5触发url scheme->Native捕获url scheme->原生分析,执行->原生调用h5



### 一、定义Native与JS 交互的全局桥对象,

对象名为"JSBridge"，是H5页面中全局对象window的一个属性。

    var JSBridge = window.JSBridge || (window.JSBridge = {})




### 二、H5中注册供原生调用的API——registerHandlerr

    /** 
    * registerHandler( String,Function )：H5调用，注册本地JS方法,注册后Native可通过JSBridge调用，调用后会将方法注册到本地变量messageHandlers 中。
    * messageHandlers：本地注册的方法集合，只有在本地注册的方法才允许原生调用
    * 原生必须通过JSBridge来调用H5的方法
    * API格式注意，（data, callback） 其中第一个data即原生传过来的数据,第二个callback是内部封装过一次的,执行callback后会触发url scheme,通知原生获取回调信息  
    * 
    * @param {String} handlerName 方法名
    * @param {Function} handler 对应的方法				
    * /


    JSBridge.registerHandler = function registerHandler(handlerName, handler) {
   		 messageHandlers[handlerName] = handler;
    };
    
    //注册一个测试函数
    JSBridge.registerHandler('testH5Func', function(data, callback) {
   		 alert('测试函数接收到数据:' + JSON.stringify(data));
    	 callback && callback('测试回传数据...');
    });



### 三、JS如何调用Native——callHandler

- **callHandler(String,JSON,Function)**：H5调用，调用原生开放的api，调用后实际上还是本地通过url scheme触发。调用时会将回调id存放到本地变量responseCallbacks中。
- **responseCallbacks**：回调函数集合，原生调用了对应api会根据回调id调用回调函数

**callHandler函数内部实现过程**

(1) 判断是否有回调函数,如果有,生成一个回调函数id,并将id和对应回调添加进入回调函数集合responseCallbacks中

(2) 通过特定的参数转换方法,将传入的数据,方法名一起,拼接成一个url scheme

    //url scheme的格式如
    //基本有用信息就是后面的callbackId,handlerName与data
    //原生捕获到这个scheme后会进行分析
    var uri = CUSTOM_PROTOCOL_SCHEME://API_Name:callbackId/handlerName?data

(3) 使用内部早就创建好的一个隐藏iframe来触发scheme

    //创建隐藏iframe过程
    var messagingIframe = document.createElement('iframe');
    messagingIframe.style.display = 'none';
    document.documentElement.appendChild(messagingIframe);
    
    //触发scheme
    messagingIframe.src = uri;


- 注意,正常来说是可以通过window.location.href达到发起网络请求的效果的，但是有一个很严重的问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。所以JS端发起网络请求的时候，需要使用iframe，这样就可以避免这个问题




### 四、Native如何调用JS——_handleMessageFromNative

1. Native接收到Url后，按照定义好的数据格式来解析数据
2. 根据提取出来的参数,根据定义好的参数进行转化。如果是JSON格式需要手动转换,如果是String格式直接可以使用
3. 原生本地执行对应的api功能方法
4. 功能执行完毕后,找到这次api调用对应的回调函数id,然后连同需要传递的参数信息,组装成一个JSON格式的参数
  - **回调的JSON格式为**:`{responseId:回调id,responseData:回调数据}`
  - **responseId String型** H5页面中对应需要执行的回调函数的id,在H5中生成url scheme时就已经产生
  - **responseData JSON型** Native需要传递给H5的回调数据,是一个JSON格式: `{code:(整型,调用是否成功,1成功,0失败),result:具体需要传递的结果信息,可以为任意类型,msg:一些其它信息,如调用错误时的错误信息}`
  
5. 通过JSBridge通知H5页面回调


**_handleMessageFromNative( JSON )**：Native调用，Native通过JSBridge调用H5的JS方法或者通知H5进行回调，具体如下

    //将回调信息传给H5
    JSBridge._handleMessageFromNative(messageJSON);	

如上,实际上是通过JSBridge的_handleMessageFromNative传递数据给H5,其中的messageJSON数据格式根据两种不同的类型,有所区别,如下

**Native通知H5页面进行回调**

数据格式为: Native通知H5回调的JSON格式

**Native主动调用H5方法**

Native主动调用H5方法时,数据格式是:`{handlerName:api名,data:数据,callbackId:回调id}`

handlerName String型 需要调用的,h5中开放的api的名称
data JSON型 需要传递的数据,固定为JSON格式(因为我们固定H5中注册的方法接收的第一个参数必须是JSON,第二个是回调函数)
callbackId String型 原生生成的回调函数id,h5执行完毕后通过url scheme通知原生api成功执行,并传递参数
注意,这一步中,如果Native调用的api是h5没有注册的,h5页面上会有对应的错误提示。

另外,H5调用Native时,Native处理完毕后一定要及时通知H5进行回调,要不然这个回调函数不会自动销毁,多了后会引发内存泄漏。


 






参考文章：


[JSBridge的原理](http://www.cnblogs.com/dailc/p/5931324.html)

[Hybrid APP基础篇(五)->JSBridge实现示例](http://www.cnblogs.com/dailc/p/5931328.html)
