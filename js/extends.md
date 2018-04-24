### javascript 的继承

我接触javascript的时间比较晚，那个时候已经发布了es6，我基本上就是看着[阮一峰es6教程](http://es6.ruanyifeng.com/)开始的

其中的class继承部分，老实说并没有完全看，甚至只是知道了`class`，`extends`和`super`这几个关键字就开始写代码了。

先总结两个原生的写法，然后再过一遍[Class 的继承](http://es6.ruanyifeng.com/#docs/class-extends)那章

```
function original() {
    this.name = 'original';
    console.log('original constructor');
}
original.prototype.setNmae = function (name) {
    this.name = name;
}
```
```
/**
 * 继承方式1
 * 只继承了constructor
 */
function sub() {
    original.call(this);
    console.log('sub constructor');
}
let sss = new sub();
    sss = new sub(); // 每次 new sub() 时都会 调用父 constructor
// sub 的原型链上没有 setName() 这个方法，sss 实例调用会出错，可以借助遍历父prototype上的方法
sss.setNmae('sub');
console.log(sss.name);
```
```
/**
 * 继承方式2
 * 原型链方式
 */
function subb() {
    console.log('subb constructor');
}
subb.prototype = new original();
subb.prototype.constructor = subb; // 把构造函数指回来
let su = new subb();
    su = new subb(); // 只是第一次 new sub() 会调用父 constructor
su.setNmae('subb');
console.log(su.name);
```

&emsp;&emsp;在es6的`class`语法中，如果子类继承父类后，构造方法中没有使用`super`去初始化父类，是没有办法使用`this`关键字的。
而且这个`super`关键字可以当作方法来用作初始化，也可以当作对象，一个父类的原型对象，去掉用父类中的原型方法，也可以省略，但无法调用父类实例的属性和方法。

&emsp;&emsp;在子类的方法中，使用`super`调用父类的方法时，父类方法中的`this`被替换为子类的`this`，对`super`赋值，结果会反应在子类中，而不是父类的原型上。


### JavaScript中new与Object.create的区别

参考:
    http://fe2x.cc/2017/10/14/Object-create-and-new-JavaScript/
    https://blog.csdn.net/blueblueskyhua/article/details/73135938
    https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction

test:
```
function Delegator() {
    console.log('constructor');
    this._name = 'delegator';
    this.method = function () { console.log('method'); }
}
Delegator.prototype.showName = function() { console.log(this._name) };
```
&emsp;&emsp;先定义了一个(构造)函数`Delegator`，并定义了一个原型方法，这简单模拟了我们日常的使用方式。
&emsp;&emsp;先是使用Object.create()的方式来创建:
```
let obj_create = Object.create(Delegator);
console.log(obj_create); // output: Function {}
console.log(obj_create._name); // output: undefined
console.log(obj_create.method); // output: undefined
console.log(obj_create.prototype); // output: Delegator { showName: [Function] }
```
&emsp;&emsp;可以看到`constructor`没有打印，`_name`属性`method`方法也都被无视掉了，只是保留了原型链上的方法，而且第二行`Object.create()`的结果打印和`Object.create(null)`的然会结果相似，一个是空的function`Function {}`一个是空的对象`{}`。
```
let obj_new = new Delegator(); // output: constructor
console.log(obj_new); // output: Delegator { _name: 'delegator', method: [Function] }
console.log(obj_new._name); // output: delegator
obj_new.method();// output: method
obj_new.showName(); // output: delegator
```
&emsp;&emsp;看到这里就感觉到和网上说的差不多了。即:
```
Object.create =  function (o) {
    var F = function () {};
    F.prototype = o;
    return new F();
};
```
而且
```
obj_create.prototype.showAdd = function() { console.log('additional') };
console.log(Delegator.prototype); // Delegator { showName: [Function], showAdd: [Function] }
```
&emsp;&emsp;`Object.create（）`函数做的只是将一个新的空函数(或对象)的原型指向了目标，就像MDN上说的:
```
function Constructor(){}
o = new Constructor();
// 上面的一句就相当于:
o = Object.create(Constructor.prototype);
// 当然,如果在Constructor函数中有一些初始化代码,Object.create不能执行那些代码
```
