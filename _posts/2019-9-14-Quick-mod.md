---
layout: post
title: 快速幂
excerpt: "快速幂"
tag:
- 算法
- 数学
---

# 快速幂

快速幂的思想就是减少重复计算
$$
a^b =
\begin{cases}
    1 & b = 0\\
    a^{b/2} \times a^{b/2} & b\%2 = 0 \\
    a^{b/2} \times a^{b/2} \times a & b\% 2 = 1
    \end{cases}
$$
   例如:

* $a^6 = a^3 \times a^3$
* $a^9 = a^4 \times a^4 \times a$

我们可以采用递归来完成这个任务:

```c++
int pow(int a, int b) {
  if(b == 0) return 1;
  if(b & 1) {
    return a * pow(a, b - 1);
  }
  else {
    int t = pow(a, b / 2);
    return t * t;
  }
}
```



我们也可以利用二进制拆分的方式,实现非递归的写法:

* 对于计算$a^b$, 我们将指数$b$写成二进制数为$a_ta_{t-1}\dots a_1a_0$
  * $b = a_t2^t + a_{t-1}2^{t-1}+\dots + a_12+a_0$
  * 其中$a_i(0\leq i\leq t)$是$0$或者$1$
* 那么就有$a^b=a^{a_t2^t + a_{t-1}2^{t-1}+\dots + a_12+a_0}$
  * $a^b=a^{a_t2^t}\times a^{a_{t-1}2^{t-1}} \times \dots \times a^{a_12} \times a^{a_0}$
* 所以最后$a^b$可以变成多个$a^{2^k}$这样的乘积
  * $a^{2^k} = (a^{2^{k-1}})^2, a^{2^0}=a$
  * 我们可以通过递推得到$a^{2^k}$,时间复杂度$O(logb)$



比如现在我们要计算$a^5$

* 把$5$写成二进制之后就是$101$,换一种方法写出来就是$2^2 + 2^0$.
* 那么我们要计算的就是$a^{2^2+2^0}$, 如果把它展开一下,可以得到${a^2}^2 \times {a^2}^0$
* 不管指数是什么,在分解成二进制像上面这样表示出来之后,我们会发现最后的答案是像${a^2}^k$这样的数的乘积
* 对于像${a^2}^k$这样的数,我们可以从$k = 0$开始不断递推
* 当$k=0$时,${a^2}^0=a$
* 为了计算$k=1$时候的值,可以利用前面的结果,也就是${a^2}^1={a^2}^0\times {a^2}^0$
* 然后以此类推,每次将前面一个数平方后就可以得到下一个结果了
* 再利用刚刚对指数的分解,再这一位是$1$的时候就在答案上乘上这个数

采用非递归的方式实现快速幂:

```c++
int pow(int a,int b) { //快速求a^b ，复杂度 log(b)
    int result = 1;
    int base = a;
    while(b) {
        if(b & 1)
            result *= base;
        base *= base;
        b >>= 1;
    }
    return result;
}
```



通常情况下$a^b$是一个很大的数,难以表示(即使使用高精度也无法解决)

所以一般我们会计算$a^b\mod c$

整数取模运算

* 加法

  $$(a+b) \mod c = (a \mod c + b \mod c) \mod c$$

* 减法

  $$(a-b) \mod c = (a \mod c - b \mod c) \mod c$$

* 乘法

  $$(a\times b) \mod c = (a \mod c \times b \mod c) \mod c$$

快速幂取模代码实现

```c++
int powmod(int a,int b,int c)   {//快速求 a^b % c，要避免计算中间结果溢出
    int result = 1;
    int base = a % c;
    while(b) {
        if( b & 1)
            result = (result * base) % c;
        base = (base * base) % c;
        b >>= 1;
    }
    return result;
}
```



有时对于计算$a*b\mod c$, 其中$a,b,c \leq 10^{18}$. 可能会造成$long\ long$溢出,为了避免使用高精度,可以借助快速幂的思想实现大数乘法:

* $a*b \mod c = (\underbrace { a+a+\cdots a }_{ b }) \mod c $
* $a \times b \mod c = 
      \begin{cases}
      0 & b = 0\\
      (a\times (b/2) \mod c + a\times (b/2) \mod c) \mod c & b\%2 = 0 \\
      (a\times (b/2) \mod c +a\times (b/2) \mod c) \mod c + a) \mod c & b\% 2 = 1
      \end{cases}$

乘法加速取模

```c++
typedef long long ll;
ll mulmod(ll a,ll b,ll c)   {//快速求 a*b % c，要避免计算中间结果溢出
    ll result = 0;
    ll base = a % c;
    while(b) {
        if(b & 1)
            result = (result + base) % c;
        base = (base + base) % c;
        b >>= 1;
    }
    return result;
}
```

