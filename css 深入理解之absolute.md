## css 深入理解之absolute 

## 第一章：absolute和float

### 1-1 absolute 和 float

* 相同的特性表现(鲜为人知的兄弟关系)
    1. 包裹性
    2. 破坏性
    3. 页面布局可以相互替换

1. 包裹性(inline-block化):

absolute的包裹性是对容器的absolute化。容器设置absolute后，容器就变成了内联块元素inline-block，没有宽度的时候内容撑开宽度，表现结果就是容器把内容包裹起来。即元素absolute后，使其（div）标签100%默认宽度变成自适应内部元素(如img)的宽度

CSS部分：
```
.div { padding:20px; margin-bottom:10px; background-color:#f0f3f9; }
.abs { position:absolute; }
```

HTML部分：
```
<div class="div">
    <img data-src="http://image.zhangxinxu.com/image/study/s/s256/mm1.jpg" />
    <p>无absolute</p>
</div>
<div class="div abs">
    <img data-src="http://image.zhangxinxu.com/image/study/s/s256/mm1.jpg" />
    <p>absolute后</p>
</div>
```

2 破坏性（脱离文档流）

absolute的破坏性是对图片的absolute化。图片设置absolute后，图片就从标准文档流中脱离出来，没有东西可以继续支撑容器的高度，父容器的高宽都塌陷。好像把房子的柱子挪到房子外边，房子果断就塌了。


CSS代码：
```
.div { padding:20px; margin:10px 0 0 10px; background-color:#f0f3f9; float:left; }
.abs { position:absolute; }
```

HTML如下：

```
<div class="div">
    <img data-src="http://image.zhangxinxu.com/image/study/s/s256/mm1.jpg" />
    <p>图片无absolute</p>
</div>
<div class="div">
    <img class="abs" data-src="http://image.zhangxinxu.com/image/study/s/s256/mm1.jpg" />
    <p>图片absolute后</p>
</div>
```


## 第2章 absolute和relative

### 2-1 别把我和relative 栓在一起

* 大大的误区

例如使图片定位在左上角： 父容器： position:relative ； 子容器：position: absolute; left: 0 ; top: 0;实际上，position:relative ； left: 0 ; top: 0; 根本不需要；relative 和absolute 是分离的、独立的，并且absolute 越独立越强大；

* clear用来管控float带来的破坏性
* relative用来管控absolute
* 超越overflow：position:absolute可以不受overflow:hidden && scroll的影响


## 第3章 absolute-上辈子折翼的天使.

### 3-1 无依赖的absolute 定位

*absolute无依赖：*不受relative限制的absolute定位，行为表现上市不使用top/right/bottom/left任何一个属性或使用auto作为值！称为不影响其他布局的绝对定位下的相对定位之王


* 定位的行为表现
    1. 脱离文档流
    2. 折翼的天使

* 折翼天使的特性表现

    1. 去浮动
    绝对定位实现的时候，浮动是无效的；比如说：图片浮动后绝对定位和直接绝对定位的位置是一样的；

    2. 位置跟随
    原先是block水平，那absolute后依旧是换行显示的，原先是inline或者inline-block，那么还是在原位置；特例：在IE7浏览器下，任何元素绝对定位都是inline-block化的；解决IE7浏览器绝对定位永远inline-block水平的bug只需要在其外面套一层block表标签即可

    chrome浏览器下，如果一个元素绝对定位了，那再去改变它的display属性也不会有任何渲染，即不能动态改变

* 配合margin 的精确定位

    1. 支持负值定位；
    2. 超赞的兼容性 - IE6;

## 第4章 强大的折翼天使

### 4-1 图片图标绝对定位覆盖

1. 图片图标来覆盖，无依赖、真不赖；
2. 如何定位下拉框，最佳实践来分享；
3. 对齐居中或边缘，定位实现有脸面；
4. 星号时有时没有，破坏队形不用愁；
5. 图文对齐兼容差，绝对定位来开挂；
6. 文字溢出不够放，不值一提就小样！


* 1.无依赖绝对定位下的相对定位

无需在父元素设置position：relative,直接对里面的元素使用absulute定位利用了absolute 的跟随性和不占空间的特点，，然后使用margin来精确定位，实现相对定位的效果。

优点：自适应性更强

* 2.注释<!-- -->

可以用来消除两个标签之间的空格，保证两个元素之间完美的贴合。(同时保持代码可读性)















































