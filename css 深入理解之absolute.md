# css 深入理解之absolute 

> 文章参考： 张鑫旭-鑫空间-鑫生活[http://www.zhangxinxu.com]
> 本文未经授权不得转载或转载请注明出处

## 1. absolute 和 float

**相同的特性表现(鲜为人知的兄弟关系)**

   1. 包裹性
   2. 破坏性
   3. 页面布局可以相互替换

### 包裹性(inline-block化)

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


## 2. 别把我和relative 栓在一起


### 大大的误区

   使图片定位在左上角方法： 父容器： position:relative ； 子容器：position: absolute; left: 0 ; top: 0;

   实际上，position:relative ； left: 0 ; top: 0; 根本不需要；

### 越独立越强大

  1. relative和absolute 是分离的、独立的，并且absolute越独立越强大；

  2. clear用来管控float带来的破坏性

  3. relative用来管控absolute 

### 超越overflow
 
 position:absolute可以不受overflow:hidden && scroll的影响


##  3. absolute-上辈子折翼的天使.

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
   

## 4. 强大的折翼天使

1. 图片图标来覆盖，无依赖、真不赖；
2. 如何定位下拉框，最佳实践来分享；
3. 对齐居中或边缘，定位实现有脸面；
4. 星号时有时没有，破坏队形不用愁；
5. 图文对齐兼容差，绝对定位来开挂；
6. 文字溢出不够放，不值一提就小样！

### 图片图标绝对定位覆盖

**无依赖绝对定位下的相对定位**

<img src="https://user-images.githubusercontent.com/20238205/30144258-aae26aec-93bc-11e7-9016-698b69a42b44.png">

<img src="https://user-images.githubusercontent.com/20238205/30144977-10bf9520-93c1-11e7-9749-5d6402a7cfbc.jpg">

   1. 无需在父元素设置position：relative，直接对里面的元素使用absulute定位，利用了absolute的跟随性和不占空间的特点，然后使用margin来精确定位，实现相对定位的效果;

   2. html顺序很重要，体现了跟随性;

   3. 传统实现： 父容器position: relative; 页面之后维护时，这个属性可能会去掉，其依赖的绝对定位可能会飞到很远的地方；

   4. 优点：无依赖的绝对定位由于不依赖父元素，这种定位，更强大，更容易维护，自适应性更强;

**注释`<!-- -->`**

<img src="https://user-images.githubusercontent.com/20238205/30144978-10c1e460-93c1-11e7-9b80-0a3d337d72a0.jpg">


可以用来消除两个标签之间的空格，保证两个元素之间完美的贴合。(同时保持代码可读性)

**下拉框定位最佳实践**

<img src="https://user-images.githubusercontent.com/20238205/30144706-7206c814-93bf-11e7-843e-f80146ccf1f1.jpg">


在HTML中将ul放在input前面，再利用absolute位置跟随特性，宽和高脱离了文档流不占空间特性，再配合margin来实现。

**居中以及边缘对齐定位**

1. 无依赖绝对定位----居中

<img src="https://user-images.githubusercontent.com/20238205/30144256-aac88a64-93bc-11e7-9da8-93bdbd510ab6.png">

   1. 父div text-align：center；
   2. 子为&nbsp（文本居中）+要居中的元素
   3. 要居中元素绝对定位，margin-left自身宽度一半实现

2. 无依赖绝对定位-----右侧fixed

<img src="https://user-images.githubusercontent.com/20238205/30144259-aafd14f0-93bc-11e7-9b93-01137e7251be.jpg">

   1. 父元素text-align：right height：0
   2. 子元素为 空格（文本右对齐）+要定位div
   3. 要定位div display：inline（跟在 空格后）margin后position：fixed 
   4. display：inline是很有用的，否则div要换行显示


### 各种对齐溢出技巧实例

**星号左对齐实现方法** 

绝对定位 + margin ，不占据空间。这样文字也能对齐；

<img src="https://user-images.githubusercontent.com/20238205/30144260-aafda578-93bc-11e7-8ce5-0169a70e1ad7.jpg">


**图标文字对齐**

绝对定位 + margin-left负值；

<img src="https://user-images.githubusercontent.com/20238205/30144261-ab09d852-93bc-11e7-9b7e-c04d76d8f7a7.png">


**溢出效果**

<img src="https://user-images.githubusercontent.com/20238205/30144262-ab2f5b18-93bc-11e7-9f33-0011365ec533.jpg">

<img src="https://user-images.githubusercontent.com/20238205/30144433-c5a214ee-93bd-11e7-806c-066a3b7812ac.jpg">

本来已经超出了整体的容器范围，但是因为绝对定位不占据任何空间，不会自动换行，所以有溢出效果；





## 5. absolute 和层级

### 脱离文档流二三事情

**脱离文档流：**

**回流与重绘：** 动画尽量作用在局对定位元素上，因为已经脱离了文档流，不会影响文档流

**垂直空间的层级：** 绝对定位元素 会在普通元素之上。绝对定位元素之间也有层级： 遵循准则--后来居上

**z-index潜在误区：** 绝对定位元素都需要z-index控制层级，确定其显示的垂直位置！

### z-index无依赖

1. 如果只有一个绝对定位元素，自然不需要z-index，自动覆盖普通元素；
2. 如果两个绝对定位，控制DOM流的前后顺序达到需要的覆盖效果，依然无z-index；
3. 如果多个绝对定位交错，非常非常少见，z-index：1控制；
4. 如果非弹框类的绝对定位元素z-index>2, 必定z-index冗余，请优化！


## 6、absolute 天使的翅膀

### 天使的翅膀

**技能克星：** 被定位了的父元素 position：relative/absolute/fixed/

<img src="https://user-images.githubusercontent.com/20238205/30144264-ab5654fc-93bc-11e7-946e-837e5c810009.jpg">

**藕断丝连半天使：** 只使用top属性的话，只能在垂直方向上瞬间移动，而水平方向仍旧受凡间小伙伴的牵连

<img src="https://user-images.githubusercontent.com/20238205/30144434-c5a2fa8a-93bd-11e7-8417-b4281eef5365.jpg">

**折翼天使变大天使：** 定位，一般组合使用

<img src="https://user-images.githubusercontent.com/20238205/30144435-c5c9d970-93bd-11e7-9e56-06b625a51ce2.jpg">


## 7. absolute的left/top/right/bottom和width/height

### 相互替代性

已知页面已有样式：`html, body { height: 100%; }`

实现一个**全屏自适应**的50%黑色半透明**遮罩层**，你会怎么实现？

通常技术方案：

```
.overlay { 
    position: absolute; 
    width: 100%; height: 100%; 
    left: 0; top: 0; 
    … 
}

```

可能有些人并不知道，还可以这么实现：

```
.overlay { 
    position: absolute; 
    **left: 0; top: 0; right:0; bottom: 0;**
    …
}
```
没有宽度(width)没有高度(height)实现宽高满屏效果。


绝对定位方向是对立(如：left vs right, top vs bottom)的时候，结果不是瞬间位移，而是身体的爆裂拉伸。

<img src="https://user-images.githubusercontent.com/20238205/30144510-2e5c9bee-93be-11e7-8b07-41f59c809626.png">


也就是说，很多情况下，absolute的翅膀拉伸和width/height是可以相互替代的！

`position: absolute;  left: 0; top: 0; **width: 50%**;`等同于：`position: absolute;  left: 0; top: 0; **right: 50%**;`

_**注意**：天使翅膀的爆裂拉伸特性**IE7+**支持！_


### 差异所在-拉伸更强大

实现一个距离右侧200像素的**全屏自适应**的容器层，你会怎么实现？

**使用拉伸直接**：`position: absolute;  left: 0; **right: 200px**;`

**但是，width只能使用CSS3 calc计算：** `position: absolute;  left: 0; width: calc(100% - 200px);` ，兼容性不好


### 其次，相互支持性

#### 1. 容器无需固定 width/height值，内部元素亦可拉伸；**

<img src="https://user-images.githubusercontent.com/20238205/30146659-e4347eca-93cb-11e7-851f-a31a2351ec53.png">

**内部拉伸特性应用**

CSS驱动的左右半区翻图浏览效果：

<img src="https://user-images.githubusercontent.com/20238205/30146714-3f592eea-93cc-11e7-9569-48290d1c2630.png">


#### 2. 容器拉伸，内部元素支持百分比width/height值；

**通常情况**：元素百分比height要想其作用，需要父级容器的height值不是auto;


**绝对定位拉伸**：即使父级容器的height值是auto，只要容器绝对定位拉伸形成，百分比高度值也是支持的。


<img src="https://user-images.githubusercontent.com/20238205/30146902-5557e820-93cd-11e7-96e2-0e546d797fdd.png">


### 最后，相互合作性

如果天使拉伸和width/height尺寸同时存在，会怎么样呢？

width/height设置的尺寸 
**大于** 
left/top/right/bottom拉伸的尺寸

因此，`position: absolute; top: 0; left: 0; right: 0; width: 50%; `的实际宽度是50%而不是100%.

这里的拉伸岂不是打酱油了？“非也非也”！当遭遇margin: auto的时候，两者的合作性就体现出来了！

<img src="https://user-images.githubusercontent.com/20238205/30147051-8f643090-93ce-11e7-9807-c65b6474781d.png">

<img src="https://user-images.githubusercontent.com/20238205/30147049-8e4b4fa4-93ce-11e7-983e-80d9a9cd815e.png">


当**尺寸限制、拉伸以及margin: auto同时出现**的时候，就会有绝对定位元素的**绝对居中**效果！

**注意**：这种绝对居中需要ie8及ie8以上才能支持！

### 8. absolute网页整体布局——适合移动web的布局策略

### 摆脱狭隘的定位

* 与fixed, relative一样，absolute设计的初衷确实是定位(position)

* 但与fixed, relative不同的是，absolute包含更多特有且强大的特性

* 如果仅仅是使用其实现一些覆盖与定位，则未免大材小用了

* 不妨发挥其潜力，试试使用absolute实现网页的整体布局

**你会发现：兼容性好，自适应强，扩展方便，性能优异，可以方便实现诸多效果，适合移动端，PC端同样精彩。**


### absolute与整体布局

#### body降级, 子元素升级

<img src="https://user-images.githubusercontent.com/20238205/30148238-1425f02e-93d5-11e7-8215-e90e23f10ca5.png">


升级的子div(假设类名为page)满屏走起：

`.page { position: absolute; left: 0; top: 0; right: 0; bottom: 0 }`

绝对定位受限于父级，因此，page要想愉快地拉伸，需要：

`html, body { height: 100%; }` ，因为默认情况下height为0

#### 各模块-头尾、侧边栏(PC端)各居其位

<img src="https://user-images.githubusercontent.com/20238205/30147806-d9bd8430-93d2-11e7-8fd7-783563f59b33.png">

```
header, footer { position: absolute; left: 0; right: 0; }
header { height: 48px; top: 0; }
footer { height:  52px; bottom: 0; }
```
```
aside { 
    width: 250px;
    position: absolute; left: 0; top: 0; bottom: 0 
}
```
#### 内容区域想象成body

<img src="https://user-images.githubusercontent.com/20238205/30147805-d9b81bbc-93d2-11e7-9aae-0c7816b77d16.png">

```
. content { 
    position: absolute;
    top: 48px; bottom: 52px; 
    left: 250px(如果侧边栏有);
    overflow: auto;
}
```
此时的头部尾部以及侧边栏都是fixed效果，不跟随滚动。避免了移动端position: fixed实现的诸多问题。

#### 全屏覆盖与page平级

<img src="https://user-images.githubusercontent.com/20238205/30148024-1ae71880-93d4-11e7-97cc-c8660201471a.png">

```
. overlay { 
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0;
    background-color: rgba(0,0,0,.5);
    z-index: 9;
}
```

```
<div class= " page " ></div>
<div class= " overlay "></div>
```

#### 实例页面感受

<img src="https://user-images.githubusercontent.com/20238205/30147807-d9d873bc-93d2-11e7-9548-60b5d7a3a3a8.png">



























