
## LESS CSS 简介

为css赋予了动态语言的特性，如变量、嵌套、运算、函数等，更方便CSS的编写和维护;

    Node.js 环境中使用 Less ：
    
    npm install -g less
    > lessc styles.less styles.css 


### 变量

    @nice-blue: #5B83AD;
    @light-blue: @nice-blue + #111;
    
    #header {
      color: @light-blue;
    }
    
    编译为：
    
    #header {
      color: #6c94be;
    }


### 混合

#### 1.混合变量

    .bordered {
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }
    
    #menu a {
      color: #111;
      .bordered;
    }
    
    .post a {
      color: red;
      .bordered;
    }

#### 2.带参数的混合

    //混合 - 可带参数的
    .border_02(@border_width){
         border: solid yellow @border_width;
    }
    
    .test_hunhe{
         .border_02(30px);
    }


#### 3.带默认值参数的混合

    .border_03(@border_width:10px){
         border: solid green @border_width;
    }
    .text_hunhe_03{
         .border_03();
    }


### 嵌套
    
    #header {
      color: black;
      .navigation {
        font-size: 12px;
      }
      .logo {
        width: 300px;
      }
    }

    .clearfix {
      display: block;
      zoom: 1;
    
      &:after {
        content: " ";
        display: block;
        font-size: 0;
        height: 0;
        clear: both;
        visibility: hidden;
      }
    }
    

 #### 嵌套的规则和冒泡
 
     .component {
       width: 300px;
       @media (min-width: 768px) {
         width: 600px;
         @media  (min-resolution: 192dpi) {
           background-image: url(/img/retina2x.png);
         }
       }
       @media (min-width: 1280px) {
         width: 800px;
       }
     }
     
 编译为：

    .component {
      width: 300px;
    }
    @media (min-width: 768px) {
      .component {
        width: 600px;
      }
    }
    @media (min-width: 768px) and (min-resolution: 192dpi) {
      .component {
        background-image: url(/img/retina2x.png);
      }
    }
    @media (min-width: 1280px) {
      .component {
        width: 800px;
      }
    }
    
    
### 运算

    @test_01:300px;
    
    .box_02{
     width: (@test_01 + 20) * 5;
    }
    
    
    //编译时计算
    .test_03 {
        width: calc(200px - 30px);
    }
    
    //浏览器计算
    .test_03{
         width: ~'calc(200px - 30px)';
    }
    
### 函数

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。

函数的用法非常简单。下面这个例子将介绍如何利用 percentage 函数将 0.5 转换为 50%，将颜色饱和度增加 5%，以及颜色亮度降低 25% 并且色相值增加 8 等用法：
    
    @base: #f04615;
    @width: 0.5;
    
    .class {
      width: percentage(@width); // returns `50%`
      color: saturate(@base, 5%);
      background-color: spin(lighten(@base, 25%), 8);
    }
    
### 命名空间 和访问器

为css提供一些封装，捆绑一些变量或mixin，便于重用


    #bundle() {
      .button {
        display: block;
        border: 1px solid black;
        background-color: grey;
        &:hover {
          background-color: white
        }
      }
      .tab { ... }
      .citation { ... }
    }
    
    #header a {
      color: orange;
      #bundle > .button;  // can also be written as #bundle.button
    }
    
    
### 作用域
    @var: red;
    
    #page {
      #header {
        color: @var; // white
      }
      @var: white;
    }
    
    
参考文章

[https://less.bootcss.com/](https://less.bootcss.com/)
    
    
