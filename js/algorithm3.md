### biggest divisor(最大公约/因数)

```
function biggestDivisor(a, b) {
    if (a % b === 0) return b;
    if (b % a === 0) return a;
    else return biggestDivisor(b, a % b);
}
```

根据两个已知数，去找他们的最大公因数。小时候做这样的数学题，还要求解过程的话，我大致就只会把这两个数分别作因数分解，然后挑出相同的因数，相乘，
便得到了最终结果。

假设 x 为 a，b 两个数的最大公因数(a，b不相等)，那么有:
```
x * m = a;
x * n = b;
```
假设a > b，那么a，b 的差里面就包含了 m - n 个最大公因数。所以不停做取余(%)的运算，某种程度上就是在找公因数。
```
x * (m - n) = a - b = a % b;
```
在这一步出现了特殊情况的话，那么就是那两个`if`了。我们的最终目的是将等式左边变成 `x * 1`，也就是让等式两边都变小。
```
left = m - n;
right = a - b = a % b;
x * left = right;
x * abs(left - n) = abs(right - b);
......
```
所以这个过程就是不停作差(取余)，得到结果后再继续作差(取余)，最上面的写法应该改为:
```
function biggestDivisor(a, b) {
    if (a % b === 0) return b;
    if (b % a === 0) return a;
    if (a > b) return biggestDivisor(b, a % b);
    if (b > a) return biggestDivisor(a, b % a);
}
```
