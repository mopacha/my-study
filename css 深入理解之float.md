
# css 深入理解之float

> 文章参考： 张鑫旭-鑫空间-鑫生活[http://www.zhangxinxu.com]
> 本文未经授权不得转载或转载请注明出处

## 1. float 的历史

### float 设计的初衷——文字环绕效果

<img src="https://user-images.githubusercontent.com/20238205/30202467-27463d4e-94b1-11e7-9199-6966a6596daf.png">


## 2. float 的特性

### 包裹性

Block formatting context — "块级格式化上下文"

<img src="https://user-images.githubusercontent.com/20238205/30202504-4668d2fe-94b1-11e7-92e7-13e7ceade47d.png">

具有包裹性的其他小伙伴

1. display: inline-block/table-cell/…
2. position: absolute（近亲）/fixed/sticky
3. overflow: hidden/scroll

### 破坏性

<img src="https://user-images.githubusercontent.com/20238205/30202509-469960f4-94b1-11e7-8322-91f4263379b0.png">

<img src="https://user-images.githubusercontent.com/20238205/30202505-46796c22-94b1-11e7-8a4c-34ba9392ed4c.png">


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


<img src="https://user-images.githubusercontent.com/20238205/30202502-4654140e-94b1-11e7-9b91-6a0af5e1e628.png">

<img src="https://user-images.githubusercontent.com/20238205/30202503-46549014-94b1-11e7-991a-e92618618d91.png">

<img src="https://user-images.githubusercontent.com/20238205/30202508-46863ca4-94b1-11e7-9487-6592461105dd.png">

<img src="https://user-images.githubusercontent.com/20238205/30202507-46843328-94b1-11e7-8f9e-4a5c41d7e745.png">

<img src="https://user-images.githubusercontent.com/20238205/30202510-469a9e9c-94b1-11e7-99e3-dbe449c13c84.png">


#### 结论

浮动的破坏性只是单纯为了实现文字环绕效果而已！ 

—— 因此，父容器高度塌陷根本就不是bug，特性使然！

**浮动是魔鬼  更是情非得已**



## 4. 清楚浮动

**更准确的说法**：清楚浮动**带来的影响**

### 两大基本方法

<img src="https://user-images.githubusercontent.com/20238205/30204075-c8e3a2f4-94b6-11e7-8d70-fb31911cbca3.png">


**clear**：与外界还有联系，例如会产生margin重叠的效果
**BFC/haslayout（应用于父元素）**:封闭，里面的声明不会对外界产生影响，例如float不会出现margin重叠，但也有缺陷，无法使用所有浏览器

<img src="https://user-images.githubusercontent.com/20238205/30204074-c8dbcf70-94b6-11e7-9a0c-fa6a777c0eb6.png">

### clear通常应用形式

1. HTML block水平元素底部走起   `  <div …></div>`

2. CSS after伪元素底部生成     `.clearfix:after {}`


**不足**

<img src="https://user-images.githubusercontent.com/20238205/30204076-c8fa233a-94b6-11e7-9d43-fb742555146d.png">

### BFC/haslayout通常声明**

 1. float:left/right
 2. position:absolute/fixed
 3. overflow:hidden/scroll (IE7+)
 4. display:inline-block/table-cell(IE8+)
 5. width/height/zoom:1/… (IE6/IE7)

**不足**

 1. 无法“一方通行”
 2. 你我相逢不相识


### 权衡后的策略

```
.clearfix:after { content: ''; display: block; height: 0; overflow: hidden; clear: both; }
.clearfix { *zoom: 1; }
```
```
.fix:after {}
.fix {}
```

### 更好的方法

```
.clearfix:after { content: ''; display: table; clear: both; }
.clearfix { *zoom: 1; }(为了兼容IE6/7)
```

###  切勿滥用


<img src="https://user-images.githubusercontent.com/20238205/30206064-7a559438-94bd-11e7-9d51-008c57f73cfc.png">


乱入的haslayout往往会让IE6/IE7做出出格的事情！

浮动也会触发haslayout, 所以，浮动在IE6/IE7下更显魔性！

但浮动普遍滥用了！




## 5. float的滥用 --不在其职而谋其政

### 浮动的作用

1. 浮动可以让元素 block 化
2. 破坏性造成的紧密排列特性(去空格化)：实质上空格是空的字符串，浮动去空格实质上是让空字符串产生了环绕效果。


**回车空格和&nbsp的区别**

1. 回车空格在显示中没有显示，后边的元素紧贴上边浮动元素

2. &nbsp占据空间，有一段间距

### float块元素布局的问题

1. 容错性比较糟糕，容易出问题
2. 需要元素固定尺寸，会发生不配对，重新调整
3. 低版本ie7下有很多问题。

**！少使用浮动实现砌砖布局。**

**要实现流体布局**


## 6. 浮动与流体布局



### 看家本领—— 文字环绕萌萌哒

float+ inline 水平的跟随元素


### 文字环绕变身

```
<div class="left">左青龙</div>
<div class="right">右白虎</div>
<div class="center">中间是标题</div>

.left{
	float: left;
}

.right{
	float: right;
}

.center{
	text-align: center;
}
```

<img src="https://user-images.githubusercontent.com/20238205/30257119-6671357c-96e2-11e7-98b2-beddb9d12379.png">

### 文字环绕的衍生——单侧固定（左侧固定右侧自适应）

```
width+float(左侧元素)
padding-left/margin-left(右侧元素，可实现当改变宽度时整体都随之变化)
```

<img src="https://user-images.githubusercontent.com/20238205/30257120-6674bd96-96e2-11e7-8443-af40e09b4f57.png">


### DOM文档流与显示位置匹配（右侧固定左侧自适应）


1. 为不匹配型，头像定宽右浮动，内容margin-right；

```
  头像divA { width：  ；float：right；}
  自适应divB{ margin-right：；}

```

2. 为匹配型（代码顺序与显示效果一致），先文档后图片：外层标签设置，宽度100%，左浮动；内嵌标签margin-right撑开间距，头像定宽左浮动，注意margin负值


```
.mib_full_float{ width:100%; float:right;}
     .mib_feed_flow{ margin-right: 76px;} //在自适应容器外部添加一个标签
.mib_head_l{ width:56px; float:left; margin-left:-56px;}
```

<img src="https://user-images.githubusercontent.com/20238205/30257121-6695720c-96e2-11e7-95ca-4cc8f0feee51.png">


### 高级进化 –- 智能自适应尺寸

**智能自适应尺寸**：一侧使用float，另一侧ie8+使用display：table-cell，ie7使用display：inline-block

<img src="https://user-images.githubusercontent.com/20238205/30257122-66bbf364-96e2-11e7-9da3-8304f8331df4.png">


### 等......







































