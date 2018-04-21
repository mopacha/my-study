

## Hybrid 设计


### Hybrid APP 

底层依赖Native提供的容器（UIWebview）,上层使用Html&Css&JS做业务开发

**优点**

- 开发效率高、跨平台、底层本
- 没有版本问题，有BUG可及时发版修复


**缺点**

Hybrid 体验肯定比不上Native


### JS to Native

- 假跳转的请求拦截
- 弹窗拦截：alert()、prompt()、confirm()
- JS上下文注入：苹果JavaScriptCore注入、安卓addJavascriptInterface注入、苹果scriptMessageHandler注入

####  假跳转的请求拦截

**JS发起调用**

native 自身可以自定义一个url schema,并且把自定义的url注册在调度中心，如

    weixin:// 打开微信
    
    taobao:// 打开淘宝

JS有很多方式发起请求，只要按照约定的schema，native就会截获，并读取、解析其中的参数当做数据，分发task。几种方式如下：


- A标签跳转

    //在HTML中写上A标签直接填写假请求地址
    <a href="JSBridge://bridge.mopacha.com? id =xx">A标签</a>

- 原地跳转

    //在JS中用location.href跳转
    location.href = 'JSBridge://bridge.mopacha.com? id =xx'


- iframe跳转

    $('body').append('<iframe src="'+url+'" />');

**客户端拦截**

1. 根据url，判断是否是所需要的拦截的调用，若是
2. 取出路径，确认要发起的native调用的指令是什么
3. 取出参数，拿到JS传过来的数据
4. 根据指令调用对应的native方法，传递数据

####  JS上下文注入

 **客户端注入**：将一个native 对象或者函数直接注入到JS全局对象之中；
 
 **JS调用**：在web的JS中直接调用和传递数据到native。



### Native to JS

- evaluatingJavaScript 执行JS代码
- loadUrl 执行JS代码
- WKUserScript 执行JS代码










参考文章：[从零开始收拾Hybrid](http://www.cocoachina.com/ios/20180109/21795.html)

































