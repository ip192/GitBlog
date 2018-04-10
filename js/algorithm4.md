### heap sort 堆排序

分为三部分:

1.维护一个堆的逻辑结构:
```
function make_heap(arr) {
    for (let index = Math.ceil(arr.length / 2) - 1; index >= 0; index--) {
        if (arr[index] > arr[2 * index + 1]) arr_switch(arr, index, 2 * index + 1);
        if (arr[index] > arr[2 * index + 2]) arr_switch(arr, index, 2 * index + 2);
    }
    return arr;
}
```

2.元素交换逻辑:
```
function arr_switch(arr, from, to) {
    arr[to] = arr[from] ^ arr[to];
    arr[from] = arr[from] ^ arr[to];
    arr[to] = arr[from] ^ arr[to];
}
```

3.使循环运行起来:
```
function heap_loop(arr) {
    const final_arr = [];
    const len = arr.length;
    while (final_arr.length < len) {
        make_heap(arr);
        final_arr.push(arr.shift());
    }
    return final_arr;
}
```

测试:
```
const arr = [9, 8, 7, 6, 5, 4, 3, 2, 2, 9];
console.log(heap_loop(arr));
```