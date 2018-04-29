# Gulp介绍

**本文要点**

- NodeJS stream
- Gulp
- Gulp与Webpack

Gulp是基于流(Stream)的自动化构建工具



## 一、NodeJS stream

### 什么是Stream

    ls | grep *.js

- 如，在这样的脚本中，使用`|`连接两条命令，把前一个命令的结果作为后一个命令的参数传入，这样数据像是水流在管道中传递，每个命令类似一个处理器，对数据进行加工处理，因此`|`称为“管道符合”


- **再如，如我们使用Gulp对项目进行构建的时候，会使用`gulp.src`接口将匹配到的文件转为stream(流)的形式，再通过`.pipe()`接口对其进行链式加工处理；**

### Stream 分类

1. readable 可读的、设备流向程序
2. writtable 可写的、程序流向设备
3. duplex、transform 读写均可、双向

在NodeJS中对文件的处理多数使用流来完成

例子：读取某个配置文件config.json，这时候简单分析一下

- 数据： config.json的内容
- 方向： 设备（物理磁盘文件）->NodeJS程序

使用readable流来做此事

    const fs = require('fs');
    const FILEPATH = '...';
    
    const rs = fs.createReadStream(FILEFATH);

通过fs模块提供的`createReadStream()`方法创建了一个可读的流，这时候`config.json`的内容从设备流向程序。我们没有直接使用Stream模块，因为fs内部已经引用了Stream模块，并做了封装。


创建让数据从程序流向设备的writable的流，对数据进行处理。

    const ws = fs.createWriteStream(DEST);

通过pipe()方法来链接流

	rs.pipe(ws);  

如果我们还有其他的数据加工，如将文件中的所有字母都变成小写，假如有个专门处理字符转成小写的流lower。

	rs.pipe(lower).pipe(ws)

 这这里lower是双向流的流，即可读也可写。

`pipe()`连接的流为加工器

### Stream的作用

- 不使用流

![](https://images2015.cnblogs.com/blog/561179/201701/561179-20170126170225816-1851442511.gif)

- 使用流

![](https://images2015.cnblogs.com/blog/561179/201701/561179-20170126172845566-1089400487.gif)
    
有个用户需要在线看视频的场景，假定我们通过 HTTP 请求返回给用户电影内容，那么代码可能写成这样

    const http = require('http');
    const fs = require('fs');
    
    http.createServer((req, res) => {
       fs.readFile(moviePath, (err, data) => {
      res.end(data);
       });
    }).listen(8080);
这样的代码又两个明显的问题

- 电影文件先缓存到内存中，一次性吐给用户，等待时间超长。
- 电影文件需要一次放入内存中，相似动作多了，内存吃不消。


用流可以讲电影文件一点点的放入内存中，然后一点点的返回给客户（利用了 HTTP 协议的 Transfer-Encoding: chunked 分段传输特性），用户体验得到优化，同时对内存的开销明显下降

    const http = require('http');
    
    const fs = require('fs');
    
    http.createServer((req, res) => {
    
       fs.createReadStream(moviePath).pipe(res);
    
    }).listen(8080);

除了上述好处，代码优雅了很多，拓展也比较简单。比如需要对视频内容压缩，我们可以引入一个专门做此事的流，这个流不用关心其它部分做了什么，只要是接入管道中就可以了

    const http = require('http');
    
    const fs = require('fs');
    
    const oppressor = require(oppressor);
    
    http.createServer((req, res) => {
    
       fs.createReadStream(moviePath)
    
      .pipe(oppressor)
    
      .pipe(res);
    
    }).listen(8080);

可以看出来，使用流后，我们的代码逻辑变得相对独立，可维护性也会有一定的改善。


## 二、Gulp

gulp之所以性能上好于grunt，主要是因为有了Stream助力来做数据的传输和处理。

在gulp.src接口将匹配到的文件转成为可读流或者双向流，通过.pipe()接口流经各插件处理，最终推送给gulp.dest所生成的的可写或者双向流并生成文件。

###1.  gulp.src(globs[,options])

输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组吗（array of globs）的文件。将返回一个Vinyl files的输出流。
.dest 接口又能消费这个 Stream，并生成对应文件。

**说明**：src方法是指定需要处理的源文件的路径，gulp借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，gulp.src返回当前文件流至可用插件；

**globs：**  需要处理的源文件匹配符路径。类型(必填)：String or StringArray；

通配符路径匹配示例：

- “src/a.js”：指定具体文件；
- “*”：匹配所有文件    例：src/*.js(包含src下的所有js文件)；
- “**”：匹配0个或多个子文件 例：src/**/*.js(包含src的0个或多个子文件夹下的js文件)；
- “{}”：匹配多个属性    例：src/{a,b}.js(包含a.js和b.js文件)  src/*.{jpg,png,gif}(src下的所有jpg/png/gif文件)；
- “!”：排除文件    例：!src/a.js(不包含src下的a.js文件)；


    var gulp = require('gulp'),
    less = require('gulp-less');
     
    gulp.task('testLess', function () {
    //gulp.src('less/test/style.less')
    gulp.src(['less/**/*.less','!less/{extend,page}/*.less'])
    .pipe(less())
    .pipe(gulp.dest('./css'));
    });


**options：**  类型(可选)：Object，有3个属性buffer、read、base；

- options.buffer：  类型：Boolean  默认：true 设置为false，将返回file.content的流并且不缓冲文件，处理大文件时非常有用；
- options.read：  类型：Boolean  默认：true 设置false，将不执行读取文件操作，返回null；
- options.base：  类型：String  设置输出路径以某个路径的某个组成部分为基础向后拼接，具体看下面示例： 


    gulp.src('client/js/**/*.js') 
      .pipe(minify())
      .pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'
     
    gulp.src('client/js/**/*.js', { base: 'client' })
      .pipe(minify())
      .pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'

### 2. gulp.dest(path[,options])

**说明**：dest方法是指定处理完后文件输出的路径；

    
    gulp.src('./client/templates/*.jade')
      .pipe(jade())
      .pipe(gulp.dest('./build/templates'))
      .pipe(minify())
      .pipe(gulp.dest('./build/minified_templates'));

**path：**  类型(必填)：String or Function 指定文件输出路径，或者定义函数返回文件输出路径亦可；
**options：**  类型(可选)：Object，有2个属性cwd、mode；

- options.cwd：  类型：String  默认：process.cwd()：前脚本的工作目录的路径 当文件输出路径为相对路径将会用到；
- options.mode：  类型：String  默认：0777 指定被创建文件夹的权限


### 3. gulp.task(name[,deps],fn)

**说明：**task定义一个gulp任务；

**name：**  类型(必填)：String 指定任务的名称（不应该有空格）；

**deps： ** 类型(可选)：StringArray，该任务依赖的任务（注意：被依赖的任务需要返回当前任务的事件流，请参看下面示例）；

**fn：**  类型(必填)：Function 该任务调用的插件操作；


	gulp.task('testLess', function () {
	    return gulp.src(['less/style.less'])
	        .pipe(less())
	        .pipe(gulp.dest('./css'));
	});
	 
	gulp.task('minicss', ['testLess'], function () { //执行完testLess任务后再执行minicss任务
	    gulp.src(['css/*.css'])
	        .pipe(minifyCss())
	        .pipe(gulp.dest('./dist/css'));
	});

### 4. gulp.watch(glob [, opts], tasks) or gulp.watch(glob [, opts, cb])

**说明：**watch方法是用于监听文件变化，文件一修改就会执行指定的任务；

**glob： ** 需要处理的源文件匹配符路径。类型(必填)：String or StringArray；

**opts：**  类型(可选)：Object 具体参看https://github.com/shama/gaze；

**tasks：**  类型(必填)：StringArray 需要执行的任务的名称数组；

**cb(event)：**  类型(可选)：Function 每个文件变化执行的回调函数；

	gulp.task('watch1', function () {
	    gulp.watch('less/**/*.less', ['testLess']);
	});
	 
	gulp.task('watch2', function () {
	    gulp.watch('js/**/*.js', function (event) {
	        console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
	    });
	});

### 三、Gulp与Webpack

**gulp 是 task runner, Webpack 是module bundler**, Webpack 和 gulp 对方的功能是基本上不重叠的。

#### gulp核心功能

- 任务的定义和组织
- 基于stream的构建
- 插件体系

gulp本身对具体的构建任务可以没有任何的要求。但通常是文件流操作。
 
> 当然也可以编写 gulp 任务来启动服务器，打开浏览器，调用ssh/ftp发布生产服务器等等，笔者挺反对这种什么事情都交给 gulp 来做的方式，gulp 更多的还是集中在构建这一块。

gulp 适合于任何基于 JavaScript 构建的场合，无论是前端还是后端（Node.js）；甚至可以在 gulp 里直接调用 Webpack：

	var webpack = require('gulp-webpack');
	gulp.task('default', function() {
	  return gulp.src('src/entry.js')
	    .pipe(webpack())
	    .pipe(gulp.dest('dist/'));
	});

### webpack 最核心的功能


- 按照模块的依赖构建目标文件；
- loader 体系支持不同的模块；
- 插件体系提供更多额外的功能；


**1. 需要把各种资源（js/ts/css/html/ejs/img/fonts等等）都看成 module；**

**2. module 之间必须是互相依赖的**，在 js 里 import 模板、图片、样式文件等等，样式文件通过  url() 和图片字体关联；这种依赖关系必须是 webpack 既定的或者是通过插件可以理解的关系。

 **3.Webpack 的核心就是模块化地组织，模块化地依赖，然后模块化地打包。相对来上，场景局限在前端模块化打包上**；虽然用 gulp + 插件的方式也能实现，但目前看 Webpack 在依赖的模块化构建上是无人能够替代的。


#### 两者有哪些功能是重叠的？

基于模块依赖的构建这方面，虽然 gulp 也可以实现，但交给更专业的 Webpack 来做；甚至可以使用shama/webpack-stream 来把 Webpack 封装成 gulp 任务；

其他功能，例如文件压缩，Webpack 也有自己的 UglifyJsPlugin 插件，gulp 也有 gulp-uglify，通常答主会选择 gulp 来实现，因为 gulp 除此之外还有更多的比如 CSS 压缩，图片压缩等等可用；

#### 哪些功能是不能被对方替代的？

Webpack 之前，做模块化依赖构建的有 Browserify。可以使用 Browserify 的 gulp 插件 deepak1556/gulp-browserify 结合上 Browserify 的插件 substack/node-browserify 实现 Webpack 提供的基于模块依赖的构建。只是 Webpack 功能完善更强大，所以这方面的事情，交给 Webpack 搞。

核心功能无法替代，gulp 任务定义和管理 Webpack 做不到，Webpack 基于模块的依赖构建 gulp 做不好。举几个例子：


用 gulp-rev-all 来做所有资源引用的版本号替换：

	gulp.task('build', function () {
	  var revAll = new RevAll({
	    prefix: 'http://xxxxx.clouddn.com',
	    dontRenameFile: ['.html', '.xml', '.md', '.yml', '.ico'],
	    dontUpdateReference: ['.html', '.xml', '.md', '.yml', '.ico']
	  })
	  return gulp.src('dist/**')
	    .pipe(revAll.revision())
	    .pipe(gulp.dest('rev'))
	})

我可以用 gulp-qiniu 来把静态文件直接发布到七牛的 CDN：

	gulp.task('qiniu', function () {
	  return gulp.src([
	    'rev/css/**',
	    'rev/fonts/**',
	    'rev/js/**',
	    'rev/images/**'
	  ], {base: 'rev'})
	  .pipe(qiniu({
	    accessKey: qiniuConfig.accessKey,
	    secretKey: qiniuConfig.secretKey,
	    bucket: "qianduan",
	    private: false
	  }, {
	    dir: '/',
	    concurrent: 10
	  }))

这两个工作，在我的场景下需要，Webpack 并做不了。当然你也可以用其他工具来实现，我管不了。

#### 在何种情况下用哪个工具更好？

除了前端模块化开发，模块之间充分依赖的项目，都不值得用 Webpack 去构建。反之，如果要用 Webpack，请确保模块化，模块之间充分依赖。除此之外的构建工作，都应该交给 gulp 继续完成。

**目前大点的项目，Webpack 和 gulp 都是同时存在的，只是各自负责擅长的那部分。比如 Webpack 将模块的、互相依赖的分散的代码打包成数个文件，然后再使用 gulp 任务去做压缩，加版本号，替换等等其他工作。**


**参考文章：**

[webpack2.x开发实践](https://zhuanlan.zhihu.com/p/26645496)

[Stream API Doc](https://nodejs.org/api/stream.html)

[stream-handbook](https://github.com/substack/stream-handbook)

[Node.js Stream](http://www.cnblogs.com/zapple/p/5759670.html)

[gulp 有哪些功能是 webpack 不能替代的？寸志 ](https://www.zhihu.com/question/45536395/answer/164361274)

[Gulp源码解析](http://www.cnblogs.com/vajoy/p/6349817.html)


