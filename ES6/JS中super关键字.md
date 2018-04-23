
## JS中super关键字

#### super作为函数调用时，代表父类的构造函数

```$xslt
class A {}

class B extend A {
    constructor(){
        super(); 
    }
}

```

* super():调用父类A的constructor()，返回的子类B的实例。这里，super内的this指的是B，因此 `super()`~`A.prototype.constructor.call(this)`
* 构造函数中的this指向实例对象


#### super 作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类

* 在普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例
* super作为对象，用在静态方法中，这时的super将指向父类，而不是父类的原型对象

知识点：
* 所有在类中定义的方法，都会被实例继承。如果在一个方法前面上加上static 关键字,就表示该方法不会被实例继承。而是直接通过类来调用，这就称为"静态方法"
* 父类的静态方法可以被子类继承



```$xslt
class A {

    construtor(){
        this.x = 1;
    }
    print(){
        console.log(this.x);
    }
    
    static myMethod(msg){
        console.log('static', msg);
    }
    
    myMethod(msg){
        console.log('instance', msg);
    }
    
    
}

class B extend A {
    constructor(){
        super();
        this.x = 2;
        m(){
            super.print();//super.print() ~ A.prototype.print()
        }
    }
    
    static myMethod(msg){
        super.myMethod(msg);
    }
    
    myMethod(msg){
        super.myMethod(msg);
    }
}

let b = new B()
b.m() //2
b.myMethod(2);// instance 2   在普通方法中指向父类的原型对象
Child.myMethod(1); // static 1  在静态方法中指向父类

```
上面代码中，super.print()虽然调用的是A.prototype.print(),但是A.prototype.print()内部的this指向子类B的实例，导致输出2 ，而不是1。也就是说实际上执行的是super.print.call(this);


```
//在子类的静态方法中通过super调用父类的方法时候，方法内部的this指向当前的子类，而不是子类的实例。

class A {
    constructor(){
        this.x = 1;
    }
    
    static print(){
        console.log(this.x);
    }
}

class B extend A {
    constructor(){
        super();
        this.x = 2;
    }
    
    static m(){
        super.print();
    }
}


B.x = 3;
B.m() //3
```
上面代码中，静态方法B.m里面，super.print指向父类的静态方法。这个方法里面的this指向的是B，而不是B的实例。

参考文章

[《ECMAScript 6 入门》阮一峰](http://es6.ruanyifeng.com/#docs/class-extends)
