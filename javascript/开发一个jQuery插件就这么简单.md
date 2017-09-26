## 开发一个jQuery插件就这么简单

> 本文未经授权不得转载或转载请注明出处
> 子非鱼

**定义：** 所谓jQuery插件就是扩展Jquery原型对象的一个方法。通过扩展jQuery对象，每次调用jQuery对象的时候,对象里面都包含了自己所扩展的方法。

**目的：** 一般是为了能在选择器所选择的结果集上做某些动作，本质上和jQuery自带的`fadeOut`和`addClass`类似的方法没有区别；

插件完成后，你可以私有，也可以开源，目前网上免费的插件也是很多。开发一个jQuery插件是一件非常简单的事情；

### 1. 典型制作自己插件的方法：

```$xslt
(function($){
    $.fn.myNewPlugin = function(){
        return this.each(function(){
            //do somthing
        });
    };
}(jQuery));
```

上面代码的关键点是扩展jQuery原型对象，也就是下面代码：

`$.fn.myNewPlugin = function(){//do something}`

然后将其封装在一个立即执行函数中：

```$xslt
(function($){
    // ...
}(jQuery));
```

简单来说就是创建一个私有的作用域来扩展jQuery，在这个作用域中可以随意使用`$`符号，而不用担心和其它js库中的`$`产生冲突。

所以真正意思上的插件代码是以下部分；

```$xslt

$.fn.myNewPlugin = function(){
    return this.each(fuction(){
        // do sth.
    });
}
```

其中`this`关键字，指向的是调用这个插件的jQuery 对象。

```$xslt
var somejQueryObject = $(`#something`);

$.fn.myNewPlugin = function(){
    alert(this === somejQueryObject);
};

somejQueryObject.myNewPlugin(); //alert  'true'
```

一个典型的jQuery对象通常包含许多DOM元素。所以说jQuery对象也被看作是DOM对象集。因此，如果要对对象集中的每个元素做处理，就要借助于jQuery的each()方法；

```$xslt
$.fn.myNewPlugin = function(){
    return this.each(function(){
        
    });
}
```

和其它大多数jQuery方法一样，jQuery的each()方法返回的也是一个jQuery 对象，所以我们才可以使用`$(...).css().attr()...`这种广受喜爱的链式操作。为了使我们的插件也支持链式操作，插件也要返回jQuery对象。
通过each()方法，可以对jQuery结果集中的DOM元素逐个进行处理，下面是个示例：

```$xslt
(function($){
    $.fn.showLinkLocation = function(){
        return this.filter('a').each(function(){
            $(this).append(
                '('+$(this).attr('href')+')'
            );
        });
    }
}(jQuery));
```
这个插件的功能就是将每个超链接的地址，在草链接后面也显示出来;


```$xslt
<!-- 插件调用前： -->
<a href="page.html">Foo</a>

<!-- 插件调用后： -->
<a href="page.html">Foo (page.html)</a>
```


插件代码可以再优化：

```$xslt
(function($){
    $.fn.showLinkLocation = function(){
        return this.filter('a').append(function(){
           return ' (' + this.href + ')';
        })
    };
}(jQuery));
```

我们使用一个匿名的回调函数作为`append`方法的参数，并且这个回调函数的返回值即为超链接的地址，和上面例子中使用`attr`方法不一样，此例子中使用了DOM内置的API，通过元素的`href`属性来取得相关值。

不用使用each()循环，直接使用jQuery的其它方法也可到达链式调用的效果；

```$xslt
(function($){
    $.fn.fadeInAndAddClass = function(duration, className){
        return this.fadeIn(duration, function(){
            $(this).addClass(className);
        });
    };
}(jQuery));


// Usage example
$('a').fadeInAndAddClass(400, 'finishedFading');
```

### 2. 何种情况才需要自己写插件

有时候我们需要使用一小段代码实现某种功能，并且这个功能在其它地方都可能被复用，那么这时可以考虑将这个功能写成插件。

大多数插件都仅仅在`$.fn`命名空间下添加方法而已，jQuery 确保在此方法中使用的 this 都是指向调用该方法的 jQuery 对象。反过来，编写插件的时候，也要确保插件方法返回的也是这个对象。


下面又是一个简单的 jQuery 插件示例：

```$xslt
// 定义插件
(function($){
    $.fn.hoverClass = function(c) {
        return this.hover(
            function() { $(this).toggleClass(c); }
        );
    };
})(jQuery);

// 调用插件
$('li').hoverClass('hover');
```


要想了解更多关于 jQuery 插件开发的方法，可以参考 Mike Alsup 撰写的 [A Plugin Development Pattern] (http://www.learningjquery.com/2007/10/a-plugin-development-pattern)一文，文中详细介绍了一款名为 `$.fn.hilight` 插件的开发方法。

该插件的设计模式如下：

```$xslt
//
// create closure
//
(function($) {
  //
  // plugin definition
  //
  $.fn.hilight = function(options) {
    debug(this);
    // build main options before element iteration
    var opts = $.extend({}, $.fn.hilight.defaults, options);
    // iterate and reformat each matched element
    return this.each(function() {
      $this = $(this);
      // build element specific options
      var o = $.meta ? $.extend({}, opts, $this.data()) : opts;
      // update element styles
      $this.css({
        backgroundColor: o.background,
        color: o.foreground
      });
      var markup = $this.html();
      // call our format function
      markup = $.fn.hilight.format(markup);
      $this.html(markup);
    });
  };
  //
  // private function for debugging
  //
  function debug($obj) {
    if (window.console && window.console.log)
      window.console.log('hilight selection count: ' + $obj.size());
  };
  //
  // define and expose our format function
  //
  $.fn.hilight.format = function(txt) {
    return '<strong>' + txt + '</strong>';
  };
  //
  // plugin defaults
  //
  $.fn.hilight.defaults = {
    foreground: 'red',
    background: 'yellow'
  };
//
// end of closure
//
})(jQuery);
```

如果有兴趣，可至[原文](http://www.learningjquery.com/2007/10/a-plugin-development-pattern)深度阅读。

via: http://jqfundamentals.com/#chapter-8
















