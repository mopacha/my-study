## Babel

Babel是一个广泛使用的转码器，可以将ES6代码转换成ES5代码，从而实现现有环境的执行。

### 1.配置文件 `.babelrc`

Babel的配置文件是`.babelrc` 存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。

改文件用来设置转码规则和插件，基本格式如下。


```$xslt
{
    "presets": [],
    "plugins": []
}
```

presets字段設定转码规则，官方提供以下的规则集，你可以根据需要安装。


```$xslt
//ES2015转码规则
npm install --save-dev babel-preset-es2015


// react 转码规则
npm install --save-dev babel-preset-react

// ES7不同阶段语法提案的转码规则（共有4个阶段），选择一个

npm install --save-dev babel-preset-stage-0
npm install --save-dev babel-preset-stage-1
npm install --save-dev babel-preset-stage-2
npm install --save-dev babel-preset-stage-3
```
然后，将这些规则加入.babelrc。

{
    "preset": ["es2015", "react", "stage-2"],
    "plugins": []
}


### babel-core 

可以看做babel的的编译器，babel的核心api都在这里面，比如transfrom,
主要都是处理转码的。它会把我们的js代码，抽象成ast ,即abstract syntax tree 的缩写，
是源代码的抽象语法结构的树状表现形式。我们可以理解为，它定义的一种分析js语法的树状结构。
也就是说es6的新语法，跟老语法是不一样的，那我们怎么去定义这个语法呢。所以必须先转成ast,去发现这个语法
的kind，分别做对应的处理，才能转化成es5。

### babel-polyfill

npm install balel-polyfill --save

babel-polyfill是为了模拟一个完整的ES2015+环境，它会让我们程序的执行环境，模拟成完美支持es6+的环境


参考文章

[你真的会用 Babel 吗?](https://segmentfault.com/a/1190000011155061)













