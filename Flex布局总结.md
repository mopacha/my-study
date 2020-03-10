传统布局基于盒模型：display+position+float，特殊布局不方便，比如垂直居中。

### 主要概念
- 弹性布局
- 弹性容器
- 弹性元素
- 主轴
- 交叉轴

### 容器属性

- `flex-direction`：定义主轴的方向，也就是项目的排列方向
row ： 水平，起点左侧
row-reverse： 水平，起点右侧
column：垂直，起点上沿
column-reverse：垂直，起点下沿
 
- `flex-wrap`：元素在主轴排列不下，是否换行
nowrap：不换行
wrap：换行
wrap-reverse：换行，第一行在下方
- `flex-flow`： <flex-direction> || <flex-wrap>简写
- `justify-content`： 项目在主轴上的对齐方式
flex-start：左对齐
flex-end：右对齐
center： 居中
space-between：两端对齐，项目之间的间隔相同
space-around： 每隔项目不干扰的环绕的空间相同，视觉上，边缘两侧的空间只有中间空白宽度的一半。
space-enenly：匀称、平等。每个元素两侧的空白完全一样’
- `align-items`： 元素在交叉轴上的对齐方式
flex-start：交叉轴起点对齐
flex-end：交叉轴终点对齐
center：交叉轴中点对齐
strech（默认）：如果元素没有设置高度，或auto，将撑满整个容器的高度
baseline：元素第一行文字的基线对齐

- `align-content`：元素多根轴线的对齐方式，也就是元素换行后（wrap），出现多根轴线，单个轴线不起作用
stretch（默认值）：轴线占满整个交叉轴
flex-start：交叉轴的起点对齐
flex-end： 交叉轴的终点对齐
center：交叉轴的中点对齐
space-between：交叉轴两端对齐，轴线之间间隔平局分配
space-around：每个轴线两端的间距一样，所以，轴线之间的间距比轴线与边框的间距大一倍
space-evenly:每个轴线上下独享一样的间距

### 元素属性
- `order`：元素在主轴上的排列顺序，值越小，排列越靠前
- `flex-grow`： 元素的放大比例
>容器剩余宽度：50px
分成每份：50px / (3+2) = 10px  （两个元素 flex-grow分别为3 和2）
元素1放大为：50px + 3 * 10 = 80px

总结： 放大的计算方法并没有与缩小一样，将元素大小纳入考虑。
仅仅按flex-grow声明的份数算出每个需分配多少，叠加到原来的尺寸上。

- `flex-shrink`: 元素的缩小比例
缩小算法： 会考虑弹性元素本身的大小。

>容器剩余宽度：-70px
缩小因子的分母：1*50 + 1*100 + 1*120 = 270 (1为各元素flex-shrink的值)
元素1的缩小因子：1*50/270
元素1的缩小宽度为缩小因子乘于容器剩余宽度：1*50/270 * (-70)
元素1最后则缩小为：50px + (1*50/270 *(-70)) = 37.03px

加入弹性元素本身大小作为计算方法的考虑因素，原因是避免将一些本身宽度较小的元素在收缩之后宽度变为0的情况出现。


- `flex-basis`： 元素在主轴上的初始尺寸，也就是元素在flex-grow 和flex-shrink 之前的尺寸，默认值为auto
```
flex-basis： 0 //根据内容撑开,设置了width 也无效
flex-basis： auto // 若设置了width 由width 决定，若没有，则有内容决定
flex-basis优先级高于width 和height
```
- `flex`:  flex-grow, flex-shrink ,flex-basis] 简写
```
默认值： 0 1 auto
flex:none = flex: 0 0 auto
flex: auto = flex: 1 1 auto
flex: 1 = flex: 1 1 0%
flex: 0% = flex: 1 1 0%
flex:2 3 = flex: 2 3 0%
flex: 2333 3222px  = flex:2333 1 3222px
```
>flex:1 和 flex:auto 的区别?
也就是flex-basis: 0;与flex-basis: auto的区别
flex-basis指元素的初始尺寸，若设置0（绝对弹性元素），在放大或者缩小时，不考虑自身尺寸；若设置auto(相对弹性元素)，此时放大和缩小的比例要考虑元素自身的尺寸。

- `align-self`： 单独某个元素设置在交叉轴对齐方式
与align-items的属性一样
flex-start | flex-end | center | strech | baseline



















