# int

* `int` 型的上限为 `2^31-1`，因此有时程序中无穷大的数 `INF` 可以设置成 `(1 << 31) - 1`（注意：必须加括号，因为位运算符的优先级没有算术运算符高）。
* 但是一般更常用的是 `2 ^ 30 - 1`，因为它可以避免相加超过 `int` 的情况。注意：如果把 `2 ^ 30 - 1` 写成二进制的形式就是 `0x3fffffff`，因此下面两个式子是等价的。

```cpp
const int INF = (1 << 30）- 1;
const int INF = 0x3fffffff;
```



