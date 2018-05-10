## React 生命周期

React 生命周期主要包括三个阶段：初始化阶段、运行阶段、销毁阶段，在不同的生命周期里，会依次触发不同的钩子函数。

### 一.初始化阶段
 
 #### 1、设置组件的默认属性
 
```$xslt
Static defaultProps = {
    name: 'perry',
    age:'26'
 }
 
 Counter.defaultProps = {name: 'perry'}

```
 #### 2、设置组件的初始化状态
 
构造函数，在创建组件的时候调用一次。
 
```$xslt
 constructor(){
    super();
    this.state = {number: 0}
 }

```
#### 3、componentWillMount()

组件即将被渲染到页面之前触发，此时可以进行开启定时器、向服务器发送请求等操作;

在组件挂载之前调用一次。如果在这个函数里面调用setState，本次的render函数可以看到更新后的state，并且只渲染一次。

#### 4、render()

组件渲染

render是一个React组件所必不可少的核心函数（上面的其它函数都不是必须的）。记住，不要在render里面修改state。

### 5、componentDidMount()

在组件挂载之后调用一次。这个时候，子主键也都挂载好了，可以在这里使用refs。



### 二、运行中阶段

#### 1、componentWillReceiveProps(nextProps)

props是父组件传递给子组件的。

父组件发生render的时候子组件就会调用componentWillReceiveProps（不管props有没有更新，也不管父子组件之间有没有数据交换）。

#### 2、shouldComponentUpdate()

组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。

shouldComponetUpdate(newProps, newState){
    if(newProps.number < 5) return true;
    return false
}

//该钩子函数可以接受到两个参数，新的属性和状态，返回true/false来控制组件是否更新



一般我们通过该函数来优化性能：

一个React项目需要更新一个小组件，很可能需要父组件更新自己的状态。而一个父组件的
重新更新会造成它旗下的所有子组件重新执行render()，形成新的虚拟DOM，再调用
diff算法对新旧虚拟DOM进行结构和属性的比较，决定组件是否需要重新渲染，
这样的操作会造成很多的性能浪费，所以我们开发者可以根据项目的业务逻辑，在shouldComponentUpdate()
中加入条件判断，从而优化性能。




#### 3、componentWillUpdate()

shouldComponentUpdate返回true或者调用forceUpdate之后，componentWillUpdate会被调用。



4、componentDidUpdate()

组件被更新完成后触发，页面中产生了新的DOM元素，可以对DOM操作;

除了首次render之后调用componentDidMount，其它render结束之后都是调用componentDidUpdate。

### 三、销毁阶段

### 1、componentWillUnmount()

组件被销毁时触发。这里我们可以进行一些清理操作，例如清理定时器，取消Redux的订阅事件等等。

一般在componentDidMount里面注册的事件需要在这里删除。



### 更新方式

react中，触发render的有4条路径。


以下假设shouldComponentUpdate都是按照默认返回true的方式。


1. 首次渲染Initial Render
2. 调用this.setState （并不是一次setState会触发一次render，React可能会合并操作，再一次性进行render）
3. 父组件发生更新（一般就是props发生改变，但是就算props没有改变或者父子组件之间没有数据交换也会触发render）
4. 调用this.forceUpdate





![avatar](https://upload-images.jianshu.io/upload_images/1814354-4bf62e54553a32b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


例子

```$xslt
import React from 'react'
import ReactDOM from 'react-dom';

class SubCounter extends React.Component {
    componentWillReceiveProps() {
        console.log('9、子组件将要接收到新属性');
    }

    shouldComponentUpdate(newProps, newState) {
        console.log('10、子组件是否需要更新');
        if (newProps.number < 5) return true;
        return false
    }

    componentWillUpdate() {
        console.log('11、子组件将要更新');
    }

    componentDidUpdate() {
        console.log('13、子组件更新完成');
    }

    componentWillUnmount() {
        console.log('14、子组件将卸载');
    }

    render() {
        console.log('12、子组件挂载中');
        return (
                <p>{this.props.number}</p>
        )
    }
}

class Counter extends React.Component {
    static defaultProps = {
        //1、加载默认属性
        name: 'sls',
        age:23
    };

    constructor() {
        super();
        //2、加载默认状态
        this.state = {number: 0}
    }

    componentWillMount() {
        console.log('3、父组件挂载之前');
    }

    componentDidMount() {
        console.log('5、父组件挂载完成');
    }

    shouldComponentUpdate(newProps, newState) {
        console.log('6、父组件是否需要更新');
        if (newState.number<15) return true;
        return false
    }

    componentWillUpdate() {
        console.log('7、父组件将要更新');
    }

    componentDidUpdate() {
        console.log('8、父组件更新完成');
    }

    handleClick = () => {
        this.setState({
            number: this.state.number + 1
        })
    };

    render() {
        console.log('4、render(父组件挂载)');
        return (
            <div>
                <p>{this.state.number}</p>
                <button onClick={this.handleClick}>+</button>
                {this.state.number<10?<SubCounter number={this.state.number}/>:null}
            </div>
        )
    }
}
ReactDOM.render(<Counter/>, document.getElementById('root'));

```




参考文章

[图解ES6中的React生命周期](https://juejin.im/post/5a062fb551882535cd4a4ce3)

[React组件生命周期小结](https://www.jianshu.com/p/4784216b8194)



