### 数组去重

```
const rem = function (arr) {
    const obj = {};
    arr.map(item => {
        if (!obj[item]) obj[item] = true;
    });
    return Object.keys(obj);
};
```

使用 `''.charCodeAt()`，`String.fromCharCode(num)`这俩个方法生成一个随机数组:
```
let num = 20;
const list = [];
while (num > 0) {
    list.push(String.fromCharCode(100 + Math.floor(Math.random() * 16)));
    num--;
}
```

检查结果:
```
console.log(list);
console.log(rem(list));
```