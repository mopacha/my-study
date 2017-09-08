
# css 深入理解之float

## 1. float 的历史

### float 设计的初衷——文字环绕效果

<img src="1">



## 2. float 的特性

### 包裹性

Block formatting context — "块级格式化上下文"

<img src="2">

具有包裹性的其他小伙伴

1. display: inline-block/table-cell/…
2. position: absolute（近亲）/fixed/sticky
3. overflow: hidden/scroll

### 破坏性


<img src="3">

<img src="4">


具有破坏性的其他小伙伴

1. display: none

2. position: absolute（近亲）/fixed/sticky


浮动是魔鬼 

无宽度、无图片、无浮动


## 3. 被误解的float——是魔鬼还是情非得已

**众所周知**： 浮动具有破坏性，会让父元素高度塌陷！


**言论**：如何解决浮动让父元素高度塌陷的bug？


Bug???????? 

不是所有长得不顺眼的都是虫子！


**你需要知道的**：浮动使高度塌陷不是bug，而是标准！！


怎么会有规范让人干坏事的？

**温故知新**： 浮动的原本作用**仅仅是为了实现文字环绕效果！**

**考考你** ：如果你是设计者，你如何利用古老的CSS盒模型规则实现文字环绕效果？



**CSS盒模型**：巍峨浩瀚，陋室难容；欲攻此玉，他山之石。


#### 置之死地而后生





#### 结论

浮动的破坏性只是单纯为了实现文字环绕效果而已！ 

—— 因此，父容器高度塌陷根本就不是bug，特性使然！

















































