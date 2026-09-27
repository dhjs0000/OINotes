
它使用了[[主论-倍增思想]]

快速幂利用一个经典的等式，用 $O(logn)$ 次乘法，达到朴素算法 $O(n)$ 次乘法的效果。

经典等式：
$$a^n \times a^m = a^{n+m} $$
首先回顾2进制每位意义
有数`n`，将它的二进制每一位定义为$b_i$，得到：
$$n = \Sigma^{\lfloor \log_2 n \rfloor}_{i=0} b_i \times 2^i$$
如果我们要求$a^k$，刚才我们知道$a^n \times a^m = a^{n+m}$ ，因此我们先把k转换成多个数相加的形式，就能转换成更少个数相乘的样式：
$$a^k = a^{\Sigma^{\lfloor \log_2 k \rfloor}_{i=0} b_i \times 2^i}=
\prod_{i=0}^{\lfloor \log_2 k \rfloor} \left(a^{2^i}\right)^{b_i}
$$
代码实现：
```cpp
long long qpow(long long a, long long b, long long p) {
    long long res = 1;
    a %= p;                          // 先取模，防止溢出
    while (b > 0) {
        if (b & 1)                   // 检查 b 的最低位 b_0 是否为 1
            res = res * a % p;       // 是 1 就乘进答案
        a = a * a % p;               // a 平方，a^(2^i) -> a^(2^(i+1))
        b >>= 1;                     // b 右移一位，丢弃已处理的 b_0
    }
    return res;
}
```