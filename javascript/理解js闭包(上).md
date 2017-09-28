## 理解js闭包（上）

### 1. 什么是闭包？

先看一段代码：

```$xslt
function a(){
    var n = 0;
    function inc(){
        n++;
        console.log(n);
    }
    inc();
    inc();
}

a(); // 控制台输出1， 再输出2

```

再来看一段代码：

```$xslt
function a(){
    var n = 0;
    this.inc = function(){
        n++;
        console.log(n);
    };
}

var c = new a();

c.inc(); // 1
c.inc(); // 2
```

#### 什么是闭包？ 这就是闭包！

**有权访问另一个函数作用域内变量的函数都是闭包。**

这里inc函数访问了构造 函数a里面变量n,所有行成一个闭包。

再来看一段代码

```$xslt
function a(){
    var n = 0;
    function inc(){
        n++;
        console.log(n);
    }
    
    return inc;
}


var c = a();
c();  // 1
c();  //2
```

`var c = couter()`，这一句 `couter()`返回的是函数 `inc`，那这句等同于 `var c = inc`;

`c()`: 这一句等同于 `inc()`;  注意，函数名只是一个标识（指向函数的指针），而()才是执行函数。

后面三句翻译过来就是：  var c = inc;  inc();  inc();，跟第一段代码有区别吗？ 没有。

所有的教科书教程上都喜欢用最后一段来说明闭包，但我觉得这将问题复杂化了。
这里面返回的是函数名，没看过谭浩强C/C++程序设计的同学可能一下子没反应出带不带()的区别，也就是说这种写法自带一个陷阱。
虽然这种写法更显高大上，但我还是喜欢将问题单一化，看看代码1和代码2，你还会纠结函数的调用，你会纠结n的值吗？

### 2. 为啥这么写？

**变量增强"封装性"**：我们知道，js的每个函数都是一个个小黑屋，它可以获取外界信息，但是外界却无法直接看到里面的内容。将变量n放进小黑屋里，处理inc函数之外，没有其它办法能接触到变量n,而且在函数a外定义
同名的变量，n也是互不影响的。



#### 常见的陷阱

```$xslt
function createFunctions(){
    var result = new Array();
    for(var i=0; i<10; i++){
        result[i] = function(){
            return i;
        };
    }
    return  result;
}

var funcs = createFunction();
for (var i=0; i < funcs.length; i++){
    console.log(func[i]());
}
```

输出10个10；

这里的陷阱就是：函数带()才是执行函数！ 单纯的一句 var f = function() { alert('Hi'); }; 是不会弹窗的，后面接一句 f(); 才会执行函数内部的代码。上面代码翻译一下就是：


```$xslt
var result = new Array(), i;
result[0] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
result[1] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
...
result[9] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
i = 10;
funcs = result;
result = null;

console.log(i); // funcs[0]()就是执行 return i 语句，就是返回10
console.log(i); // funcs[1]()就是执行 return i 语句，就是返回10
...
console.log(i); // funcs[9]()就是执行 return i 语句，就是返回10
```

为什么只垃圾回收了 result，但却不收了 i 呢？ 因为 i 还在被 function 引用着啊。好比一个餐厅，盘子总是有限的，所以服务员会去巡台回收空盘子，但还装着菜的盘子他怎么敢收？ 当然，你自己手动倒掉了盘子里面的菜（=null），那盘子就会被收走了，这就是所谓的内存回收机制。

至于 i 的值怎么还能保留，其实从文章开头一路读下来，这应该没有什么可以纠结的地方。盘子里面的菜，吃了一块不就应该少一块吗？

### 3. 总结

闭包就是一个函数引用另外一个函数的变量，因为变量被引用着所以不会被回收，因此可以用来封装一个私有变量。这是优点也是缺点，**不必要的闭包只会徒增内存消耗**！另外使用闭包也要注意变量的值是否符合你的要求，因为他就像一个静态私有变量一样。闭包通常会跟很多东西混搭起来，接触多了才能加深理解，这里只是开个头说说基础性的东西。




























