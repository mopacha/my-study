## 浮动与清除

> 本文未经授权不得转载或转载请注明出处

浮动， 就是把元素从常规文档流中拿出来

* 实现文字环绕图片的效果
* 可以让原来上下堆叠的块级元素，变成左右并列，从而实现布局中的分栏。


### 1. 浮动

#### 文本环绕图片

```
<img ___/>
<p> ..the paragraph text__</p>


p {margin: 0; border:1px solid red;}
img {float: left; margin: 04px 4px 0;} 

```

浮动图片会从文档流中被移除，如果标记中有文本元素跟在它后面，则其中的文本会绕开图片

注：浮动非图片元素时，必须给它设定宽度，否则后果难以预料！

#### 创建分栏

浮动图片的固定宽度段落一经浮动，就会变成布局中的一栏，其文本也不会环绕图片了；

如果几个相邻的元素都具有设定的宽度，都是浮动的，而且水平空间也足以容纳它们，它们就会并列排在一行。

注：浮动元素位于"文档流外部"，因为它已经不被包含在标记中的父元素之内了。

```
<img ___/>
<p> ..the paragraph text__</p>


p {float: left; margin: 0; border:1px solid red;}
img {float: left; margin: 04px 4px 0;} 

```

### 2. 围住浮动元素的三种方法


#### 方法一、 为父元素添加`overflow: hidden`

```$xslt
<section>
    <img src=''>
    <p>It's fun to float</p>
</section>
<footer>Here is the footer element that runs across the bottom of the page.</footer>
```

```$xslt
section {border: 1px solid red; margin: 0 0 10px 0; overflow: hidden;}
img {float: left}
p {boder: 1px solid red;}
footer {border: 1px solid red;}
```

第一个方法很简单，缺点是不太直观，即为父元素添加`overflow: hidden`，以强制它包围浮动元素。

实际上，`overflow:hidden` 声明的真正用途是防止包含元素被超大内容撑大。应用后，包含元素依然保持其设定的宽度，
而超大的子内容则会被容器剪切掉。除此之外，overflow:hidden还有另外一个作用，即它可靠的迫使父元素包含其浮动的子元素。


#### 方法二、 同时浮动父元素


```
section {border: 1px solid red; margin: 0 0 10px 0; float: left; width:100%}
img {float: left}
footer {border: 1px solid red; clear:left}

```
浮动section后，不管其子元素是否浮动，它都会紧紧包围住其子元素。因此，需要用`width:100%` 再让其与浏览器同宽。

另外，由于section 现在出现浮动了，所以footer会努力挤到它旁边去。为了强制footer依然呆在section下方，要给它应用

`clear: left`.被清除的元素不会被提升到浮动元素的旁边。


#### 方法三、 添加非浮动的清除元素

给元素的最后添加一个非浮动的子元素，然后清除该子元素。由于包含元素一定会包围非浮动的子元素，
而且清除会让这个子元素位于（清除一侧）浮动元素的下方，因此包含元素一定会包含这个子元素——以及前面的浮动元素。

在包含元素最后添加子元素作为清除元素的方式有两种。


1). 

```$xslt
    <section>
        <img src='float.png'>
        <p>It's fun to float</p>
        <div class="clear_me"></div>
    </section>
    <footer>Here is the footer element that runs across the bottom of the page.</footer>
    
    section {border: 1px solid red; margin: 0 0 10px 0;}
    img {float: left}
    .clear_me{clear: left}

    footer {border: 1px solid red;}

```
2).  

```
    <section class="clearfix">
        <img src='float.png'>
        <p>It's fun to float</p>
        <div class="clear_me"></div>
    </section>
    <footer>Here is the footer element that runs across the bottom of the page.</footer>
    
    .clearfix:after{
        content:"":
        display:block;  //让对象成为块级元素
        height:0;
        visibility:hidden;  // 不可见，但保留对象物理空间
        clear:both;
    }
    
```

**举一反三：** 

```$xslt

dispaly: none //隐藏对象，不为被隐藏对象保留其物理空间
         inline // 内联对象， 默认值
```

### 总结

迫使父元素包围浮动子元素三种方式：

* 为父元素应用`overflow： hidden`

* 浮动父元素

* 在父元素内容的末尾添加非浮动元素，可以直接在标记中加， 也可以通过给父元素添加clearfix类来加；


**注意** 

不能在下拉菜单的顶级元素上应用overflow：hidden,否则 作为资源上的下拉菜单就不会显示了。

因为下拉菜单会显示在其父元素区域的外部，而这恰恰是overflow:hidden 所阻止的。































