node 的单例模式

参考: 
- https://github.com/FredKSchott/the-node-way
- http://fredkschott.com/post/2013/12/node-js-cookbook---designing-singletons

参考该项目中其他几个链接，知道了 node 的模块化规范有一个缓存的概念，所以说 node 的一种单例模式实现，是不需要像其他语言那样写好多的。

像这样最普通的写法:
```
/**
 * singleton.js
 */
let times = 0;
function Single() {
    return times++;
}
module.exports = Single;
```
```
/**
 * app1.js
 */
const Single = require('./singleton.js');
console.log(Single()); // 0
```
```
/**
 * app2.js
 */
 const Single = require('./singleton.js');
 console.log(Single()); // 1
```
`times`作为一个私有变量，无法被外界修改，`singleton.js`被编译了一次以后`times`就存在缓存(`require.cache`)中，所以每次调用export出的
函数，就算是用`new`的方式，只要其操作的变量是私有的，那么至少这些变量可以保证是单例的。