## React简介


React 最主要的特性就是基于组件的设计流程


### JSX
 render()方法可以直接把html嵌套在JS中的写法，他被称为JSX,JSX只是作为编译器，把类型Html的结构编译成JavaScript;

### DOM

文档对象模型，Html会将文档转换成DOM tree结构，浏览器会提供DOM API 给JS来操作DOM;

 
###Virtual DOM 

只不过是DOM结构的JavaScript 对象描述。

Virtual DOM的任何更新都发生在Virtual DOM树上，这些操作都是对JS对象的操作，速度很快。当一系列更新完成的时候，就会产生一颗新的Virtual DOM树，然后使用Diff 算法比较新旧两棵树之间的差异。到目前为止，没有做任何DOM操作者，只是对JavaScript的计算和操作。最后，将这个差异作用到真正的DOM 元素上，通过这种方法，让DOM操作最小化，做到效率最高。

### props 属性


props 就是传入组件的属性，在组件内部可用通过this.props来访问。


定义组件属性的变量类型

    import {PropTypes} from 'react';
    
    myComponet.propTypes = {
    	name: ProTypes.string.isRequired,
    	age: ProTypes.number.isRequired
    }



### state

state是组件内部的属性，组件本身是一个状态机，可以在constructor中通过this.state 直接定义它的值，然后根据这些值渲染不容的UI。可以通过this.setState方法改变state值，此方法会让组件再次调用render方法，来重新渲染新的UI。

> 在ES6 class 类型的component组件的声明方式中，不会把自定义的callback函数绑定到实例上，所以需要手动在constructor里面绑定。
> 
>     this.likedCallback = this.likedCallback.bind(this);



### React组件的类别
- 函数式定义的无状态组件
- es5原生方式 React.createClass 定义的组件
- es6形式的 extends React.Component定义的组件

#### 1. 函数式定义的无状态组件


    const HelloComponent = props => {
    	return <div>Hello {props.name}</div>
    }
    
    ReactDON.render(<HelloComponent name = "Perry")/>, mountNode)


特点：

- 组件无实例化过程，不用分配多余内存，性能好；
- 组件不能访问this对象；
- 组件无法访问生命周期方法；
- 无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用；

#### 2. es5原生方式 React.createClass 定义的组件

    var InputControlES5 = React.createClass({
    	propTypes: {
    		initialValue: React.PropTypes.string
    	},
    	defaultProps: {
    		initialValue: ''
    	},
    	// 设置 initial state
    	getInitialState: function() {
    		return {
    			text: this.props.initialValue || 'placeholder'
    		};
    	},
    	handleChange: function(event) {
    		this.setState({
    			text: event.target.value
    		});
    	},
    	render: function() {
    		return (
    			<div>
    				Type something:
    				<input onChange={this.handleChange}
    				value={this.state.text} />
    			</div>
    		);
    	}
    });

#### 3. es6形式的 extends React.Component定义的组件

    class InputControlES6 extends React.Component {
    	constructor(props) {
    		super(props);
    
    		// 设置 initial state
    		this.state = {
    			text: props.initialValue || 'placeholder'
    		};
    
    		// ES6 类中函数必须手动绑定
    		this.handleChange = this.handleChange.bind(this);
    	}
    
    	handleChange(event) {
    		this.setState({
    			text: event.target.value
    		});
    	}
    
    	render() {
    		return (
    			<div>
    				Type something:
    				<input onChange={this.handleChange}
    				value={this.state.text} />
    			</div>
    		);
    	}
    }
    
    InputControlES6.propTypes = {
    	initialValue: React.PropTypes.string
    };
    InputControlES6.defaultProps = {
    	initialValue: ''
    };
    

#### es5 createClass 与 es6 class区别

**函数绑定**

createClass :每一个成员函数都由React自动绑定。任何时候需要调用，直接使用this.whateverFn即可，函数中的this变量在函数调用时会被正确设置；

ES6 class ： 函数不是自动绑定的，必须手动绑定。

1.构造函数中绑定

2.行内绑定：


----------


	// 使用 `.bind`:
	render() {
	    return (
	        <input onChange={this.handleChange.bind(this)}
	             value={this.state.text} />
	    );
	}
	
	// --- OR ---
	
	// 使用胖箭头函数:
	render() {
	    return (
	        <input onChange={() => this.handleChange()}
	             value={this.state.text} />
	    );
	}

以上任何一种方法都行的通，但都不如前文的方法有效率。因为每次render 方法被调用的时候（这个调用会相当频繁）就会有一个新的函数被创建。相比于在构造函数中做仅仅一次函数绑定，这个方法会慢一些。



3.将函数自身替换为胖箭头函数


----------
	// 通常的做法
	// 需要在别处绑定
	handleChange(event) {
	  this.setState({
	    text: event.target.value
	  });
	}
	
	// ES7 做法
	// 搞定，不需要另外绑定
	handleChange = (event) => {
	  this.setState({
	    text: event.target.value
	  });
	}


4.使用插件绑定

如果你不想总是手动敲这些函数绑定的代码，可以看看[react-autobind](https://www.npmjs.com/package/react-autobind)或[autobing-decorator](https://www.npmjs.com/package/autobind-decorator) 




**Initial State**

createClass 方法接受一个getInitialState函数作为参数一部分，这个函数会在组件挂载（mount）时被调用一次。
ES6 class 使用构造函数。在调用 super 之后，直接设置 state 即可。

**propTypes 和 defaultProps 的位置**

使用createClass 的时候，将 propTypes 和 defaultProps 作为你传入的对象的属性。

如果一个Props不是requied，一定在defaultProps(或者getDefaultProps)中设置它
React.PropTypes主要用来验证组件接收到的props是否为正确的数据类型，如果不正确，console中就会出现对应的warning。

使用 ES6 class 的时候，这些变成了类本身的属性，所以他们需要在类定义完之后被加到类上。

如果你开启了 ES7 的属性初始化器(property initializer)，可以使用下面的简写法：

	class Person extends React.Component {
	  static propTypes = {
	    name: React.PropTypes.string,
	    age: React.PropTypes.string
	  };
	
	  static defaultProps = {
	    name: '',
	    age: -1
	  };

	  ...
	}



### 组件生命周期

React 用不同的方法来描述它的整个生命周期

生命周期函数


#### 1. constructor

constructor可以获取父组件传下来的props，context。如果在构造函数内部使用prop或context，则需要传入（在组件其他地方是可以直接接收的），并传入super对象。


    constructor(props,context){
		super(props,context)
		console.log(this.props,this.context) //在内部可以使用props和context
	}







文章参考：

[React: ES5 (createClass) or ES6 (class)?](https://daveceddia.com/react-es5-createclass-vs-es6-classes/)



[ES6的类和ES7的property initializer](https://blog.csdn.net/future_challenger/article/details/52524742)