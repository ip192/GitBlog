### fibonacci

```
function fibonacci(num) {
    if (num < 2) return num;
    else return fibonacci(num - 1) + fibonacci(num - 2);
}

function memorize(num) {
    let cache = {};
    const fib = function (num) {
        if (num < 2) return num;
        else {
            if (!cache[num]) cache[num] = fib(num - 1) + fib(num - 2);
            return cache[num];
        }
    };
    return fib(num);
}
```

比较两个写法的用时
由此也可以看出，这两个`setTimeout`/`process.nextTick`函数中的回调函数也是顺序执行的:
```
process.nextTick(() => {
    console.time('f1');
    console.log(fibonacci(41));
    console.timeEnd('f1');
});
process.nextTick(() => {
    console.time('f2');
    console.log(memorize(41));
    console.timeEnd('f2');
});
```

我理解的`斐波那契数列`就是一个求合逻辑，给定一个数 n ，我们要通过第 n-1 个和第 n-2 个数相加得到，而 n-1, n-2 是由 n-2, n-3, n-4 做加法得到。
所以我们只要知道 1 到 n-1 个结果就可以了。不管是用 `for` 循环，还是递归，也都应该是算 n-1 次就能得到结果才对。
```
function forfib(num) {
    const fibArr = [];
    for (let i = 0; i < num; i++) {
        if (i < 2) fibArr[i] = i;
        else fibArr[i] = fibArr[i - 1] + fibArr[i - 2];
    }
    return fibArr[num - 1];
}
```
