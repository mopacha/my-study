# css 深入理解之absolute 

> 文章参考： 张鑫旭-鑫空间-鑫生活[http://www.zhangxinxu.com]
> 本文未经授权不得转载或转载请注明出处

##一、 absolute 和 float

### 相同的特性表现(鲜为人知的兄弟关系)

   1. 包裹性
   2. 破坏性
   3. 页面布局可以相互替换

### 包裹性(inline-block化):

absolute的包裹性是对容器的absolute化。容器设置absolute后，容器就变成了内联块元素inline-block，没有宽度的时候内容撑开宽度，表现结果就是容器把内容包裹起来。

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
结果如下图所示：

<img width="595" height="605" src="https://user-images.githubusercontent.com/20238205/30103060-19188b5c-9324-11e7-948e-90c6f26daf9a.jpg">

### 破坏性（脱离文档流）

absolute的破坏性是对图片的absolute化。图片设置absolute后，图片就从标准文档流中脱离出来，没有东西可以继续支撑容器的高度，父容器的高宽都塌陷。


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
结果如下图所示：
<img width="560" height="329" src="https://user-images.githubusercontent.com/20238205/30103253-a2577bee-9324-11e7-946c-27556f91a5b9.jpg">


## 二、别把我和relative 栓在一起


### 大大的误区

   使图片定位在左上角方法： 父容器： position:relative ； 子容器：position: absolute; left: 0 ; top: 0;

   实际上，position:relative ； left: 0 ; top: 0; 根本不需要；

### 越独立越强大

  relative 和absolute 是分离的、独立的，并且absolute 越独立越强大；

### clear用来管控float带来的破坏性

### relative用来管控absolute 

### 超越overflow
 
 position:absolute可以不受overflow:hidden && scroll的影响


##  三、absolute-上辈子折翼的天使.

### 无依赖的absolute 定位

   1. 不受relative限制的absolute定位，行为表现上市不使用top/right/bottom/left任何一个属性或使用auto作为值！
   2. 称为不影响其他布局的绝对定位下的相对定位之王


### 定位的行为表现

   1. 脱离文档流
   2. 折翼的天使

### 折翼天使的特性表现

   1. 去浮动：绝对定位实现的时候，浮动是无效的；比如说：图片浮动后绝对定位和直接绝对定位的位置是一样的；
   2. 位置跟随：原先是block水平，那absolute后依旧是换行显示的，原先是inline或者inline-block，那么还是在原位置；特例：在IE7浏览器下，任何元素绝对定位都是inline-block化的；解决IE7浏览器绝对定位永远inline-block水平的bug只需要在其外面套一层block表标签即可；
   3. chrome浏览器下，如果一个元素绝对定位了，那再去改变它的display属性也不会有任何渲染，即不能动态改变

### 配合margin 的精确定位

   1. 支持负值定位；
   2. 超赞的兼容性 - IE6;
   

## 四、强大的折翼天使

1. 图片图标来覆盖，无依赖、真不赖；
2. 如何定位下拉框，最佳实践来分享；
3. 对齐居中或边缘，定位实现有脸面；
4. 星号时有时没有，破坏队形不用愁；
5. 图文对齐兼容差，绝对定位来开挂；
6. 文字溢出不够放，不值一提就小样！

### 图片图标绝对定位覆盖

**无依赖绝对定位下的相对定位**

   1. 无需在父元素设置position：relative，直接对里面的元素使用absulute定位，利用了absolute的跟随性和不占空间的特点，然后使用margin来精确定位，实现相对定位的效果;

   2. html顺序很重要，体现了跟随性;

   3. 传统实现： 父容器position: relative; 页面之后维护时，这个属性可能会去掉，其依赖的绝对定位可能会飞到很远的地方；

   4. 优点：无依赖的绝对定位由于不依赖父元素，这种定位，更强大，更容易维护，自适应性更强;

**注释<!-- -->**

可以用来消除两个标签之间的空格，保证两个元素之间完美的贴合。(同时保持代码可读性)

**下拉框定位最佳实践**

在HTML中将ul放在input前面，再利用absolute位置跟随特性，宽和高脱离了文档流不占空间特性，再配合margin来实现。

**居中以及边缘对齐定位**

1. 无依赖绝对定位----居中

   1. 父div text-align：center；
   2. 子为&nbsp（文本居中）+要居中的元素
   3. 要居中元素绝对定位，margin-left自身宽度一半实现

2. 无依赖绝对定位-----右侧fixed

   1. 父元素text-align：right height：0
   2. 子元素为 空格（文本右对齐）+要定位div
   3. 要定位div display：inline（跟在 空格后）margin后position：fixed 
   4. display：inline是很有用的，否则div要换行显示


### 各种对齐溢出技巧实例

**星号左对齐实现方法** 

绝对定位 + margin ，不占据空间。这样文字也能对齐；

**图标文字对齐**

绝对定位 + margin-left负值；

**溢出效果**

本来已经超出了整体的容器范围，但是因为绝对定位不占据任何空间，不会自动换行，所以有溢出效果；





## 五、absolute 和层级

### 脱离文档流二三事情

**脱离文档流：**

**回流与重绘：**动画尽量作用在局对定位元素上，因为已经脱离了文档流，不会影响文档流

**垂直空间的层级：**绝对定位元素 会在普通元素之上。绝对定位元素之间也有层级： 遵循准则--后来居上

**z-index潜在误区：** 绝对定位元素都需要z-index控制层级，确定其显示的垂直位置！

### z-index无依赖

1. 如果只有一个绝对定位元素，自然不需要z-index，自动覆盖普通元素；
2. 如果两个绝对定位，控制DOM流的前后顺序达到需要的覆盖效果，依然无z-index；
3. 如果多个绝对定位交错，非常非常少见，z-index：1控制；
4. 如果非弹框类的绝对定位元素z-index>2, 必定z-index冗余，请优化！


## 六、absolute 天使的翅膀

### 6-2 天使的翅膀

技能克星：被定位了的父元素 position：relative/absolute/fixed/

藕断丝连半天使：只使用top属性的话，只能在垂直方向上瞬间移动，而水平方向仍旧受凡间小伙伴的牵连

折翼天使变大天使：定位，一般组合使用


## 七、 absolute和width/height

### 7-1 left/top/right/bottom 与width/height  

异曲同工与特殊表现

首先，相互替代性

已知页面已有样式：html, body { height: 100%; }

实现一个全屏自适应的50%黑色半透明遮罩层，你会怎么实现？

通常技术方案：

```
.overlay { 
    position: absolute; 
    width: 100%; height: 100%; 
    left: 0; top: 0; 
    … 
}

```

可能有些人并不知道，还可以这么实现……

```
.overlay { 
    position: absolute; 
    left: 0; top: 0; right:0; bottom: 0;
    …
}
```
没有宽度(width)没有高度(height)实现宽高满屏效果。

绝对定位方向是对立(如：left vs right, top vs bottom)的时候，结果不是瞬间位移，而是身体的爆裂拉伸。


也就是说，很多情况下，absolute的翅膀拉伸和width/height是可以相互替代的！

position: absolute;  left: 0; top: 0; width: 50%;

等同于：

position: absolute;  left: 0; top: 0; right: 50%;

注意：天使翅膀的爆裂拉伸特性IE7+支持！

### 差异所在-拉伸更强大

实现一个距离右侧200像素的全屏自适应的容器层，你会怎么实现？

使用拉伸直接：position: absolute;  left: 0; right: 200px;

但是，width只能使用CSS3 calc计算：position: absolute;  left: 0; width: calc(100% - 200px);


### 其次，相互支持性

1. 容器无需固定 width/height值，内部元素亦可拉伸；
2. 容器拉伸，内部元素支持百分比width/height值；












