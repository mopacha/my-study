## SASS 簡介

CSS 预处理器，提供了许多便利的写法，如 变量、运算、嵌套、继承、Mixin(定义代码块)、条件命令、循环命令、自定义函数。

### 变量

```
    //所有变量以$开头
    $blue: #1875e7;
    
    div{
        color: $blue;
    }
    
    //变量在字符串中，写在#{}之中
    $side : left;
    
    .rounted{
        border-#{$side}-radius: 5px;
    }

```

### 运算

```
body{
       margin: (14px/2);
    　　top: 50px + 100px;
    　　right: $var * 10%;
    }
```
     
###  嵌套

```$xslt
//选择器嵌套
       
div h1 {
　　　　color : red;
　　}
可以写成：

div{
    hi {
        color: red;
    }
}

//属性嵌套 border-color
p{
    border: {
        color: red
    }
}

//嵌套代码块中，引用父元素。比如a:hover

a{
    &:hover {
        color:red;
    }
}

```
       
       
### 继承
  
允许一个选择器继承另一个选择器
  
```$xslt
 .class1{
    　　　　border: 1px solid #ddd;
    　　}
    
//class2要继承class1，就要使用@extend命令：
　 .class2 {
　　　　@extend .class1;
　　　　font-size:120%;
　　}

```
     
  
### Mixin
  
Mixin有点像C语言的宏（macro），是可以重用的代码块。
 
使用@mixin命令，定义一个代码块。
  
 ```
 @mixin left {
   　　　　float: left;
   　　　　margin-left: 10px;
   　　}
 
 ``` 
  　 
  
使用@include命令，调用这个mixin。 
  
```
div {
  　　　　@include left;
  　}
``` 
  
mixin的强大之处，在于可以指定参数和缺省值。
  
  ```$xslt

@mixin left($value: 10px) {
　　　　float: left;
　　　　margin-right: $value;
　　}
```
使用的时候，根据需要加入参数：

```$xslt

　div {
　　　　@include left(20px);
　　}
```

    
  
### 条件语句

@if可以用来判断：

```$xslt

p {
　　　　@if 1 + 1 == 2 { border: 1px solid; }
　　　　@if 5 < 3 { border: 2px dotted; }
　　}
```
  
配套的还有@else命令：
  
```$xslt

@if lightness($color) > 30% {
　　　　background-color: #000;
　　} @else {
　　　　background-color: #fff;
　　}
```  

### 循环语句

SASS支持for循环：

```$xslt
@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}

```
　　

也支持while循环：

```$xslt
$i: 6;

　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}
```

　　
each命令，作用与for类似：


```$xslt
　　@each $member in a, b, c, d {
　　　　.#{$member} {
　　　　　　background-image: url("/image/#{$member}.jpg");
　　　　}
　　} 
```
### 自定义函数

 
 SASS允许用户编写自己的函数。
 
```$xslt
    @function double($n) {
 　　　　@return $n * 2;
 　　}
 
　　#sidebar {
　　　　width: double(5px);
　　}

```
 　　
  
  
  
  
  
    
