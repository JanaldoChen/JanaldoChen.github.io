---
layout: post
title:  "Divide and Conquer DP"
date:   2018-11-19
excerpt: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
tag:
- Divide and Conquer
- DP
comments: true
---

# Divide and Conquer DP

## 知识背景: 

### 分治法求解DP:

有些动态规划问题有如下的递推形式:
\\[
dp(i,j)=\min_{k \leq j} \{ dp(i-1,k)+C(k,j) \}
\\]
其中$$1\leq i \leq n$$ 和 $$1\leq j \leq m$$, $$C(k,j)$$ 是代价函数, 计算$$C(k,j)$$花费$$O(1)$$.

直接计算递推式的时间是$$O(nm^2)$$. 因为这里有$$n \cdot m$$个状态, 每个状态转移花费$$O(m)$$.

我们定义 $$opt(i,j)$$ 为最小化 $$ dp(i,j)$$ 的$$k$$. 如果对于所有的$$i,j$$ 有 $$opt(i,j) \leq opt(i,j+1)$$ ,那我们应用分治法求解$$DP$$. 这个就是单调性条件, 对于一个固定的 $$i$$ 最优分割点随 $$j$$ 单调增. 由这样的性质可以让我们更有效率地计算状态. 假设我们已经计算 $$opt(i,j)$$ 对于固定的 $$i$$ 和 $$j$$ . 然后对于 $$j^{'} \le j$$ 我们知道 $$opt(i, j^{'}) \le opt(i, j)$$ . 这就意味着当我们计算 $$opt(i, j^{'})$$ 的时候, 没有必要考虑这么多的情况. 首先, 我们计算 $$opt(i,n/2)$$ . 然后我们计算 $$opt(i, n/4)$$ 和 $$opt(i, 3n/4)$$ , 我们知道 $$opt(i, n/4) \le opt(i,n/2)$$, $$opt(i,3 n/4) \ge opt(i,n/2)$$. 通过递归保持 $$opt$$ 的上下界. 可以达到$$O(mnlog{n})$$.

### 代码框架:

尽管程序实现视具体问题不同而不同, 这里给出大类模版. 该函数 $$compute$$ 通过给出第 $$i-1$$ 行的状态 $$dp\_before$$ 计算第 $$ i$$行的状态 $$dp\_cur$$ ,调用方式 $$compute(0,n-1,0,n-1)$$.

```c++
int n;
long long C(int i, int j);
vector<long long> dp_before(n), dp_cur(n);

// compute dp_cur[l], ... dp_cur[r] (inclusive)
void compute(int l, int r, int optl, int optr)
{
    if (l > r)
        return;
    int mid = (l + r) >> 1;
    pair<long long, int> best = {INF, -1};

    for (int k = optl; k <= min(mid, optr); k++) {
        best = min(best, {dp_before[k - 1] + C(k, mid), k});
    }

    dp_cur[mid] = best.first;
    int opt = best.second;

    compute(l, mid - 1, optl, opt);
    compute(mid + 1, r, opt, optr);
}
```

## 例题. Not XOR but OR

### Problem description
给你一个包含 $$n$$ 个正整数序列 $$A$$ 和一个正整数 $$k$$. 你的目标是将这 $$n$$ 
个正整数划分成 $$k$$ 个不相交连续非空的子序列, 以至于每个元素都恰好属于一个子序列.
每个子序列可以有两个整数 $$l$$ 和 $$r$$ $$(l \leq r)$$ 描述,其含义是\\( A[l],A[
l+1],\ldots,A[r] \\). 这样的一个子序列的价值是所有元素的按位或, 即\\( A[l] | A[
l+1]|\ldots |A[r] \\).  总的价值是所有的子序列的价值和.
现在你必须找道一种划分方式使得总的价值和最大.
如果你的不知道按位或的话, 你能了解更多关于[按位或](https://en.wikipedia.
org/wiki/Bitwise_operation#OR).

### Input
第一行包括一个正整数 $$T​$$, 表示测试数据的数目.
接下来 $$T$$ 组测试数据如下:
第一行包括每组测试数据的两个正整数 $$n​$$ 和 $$k​$$, 
表示序列元素的数目和要求划分的子序列数目.
第二行包括 $$n$$ 个正整数, 表示该序列.

### Output
对于每组测试数据, 输出一行, 包含所能达到的最大价值.

### Constraints
* \\( 1 \le T \le 10 \\)
* \\( 1 \le n \le 1000 \\)
* \\( 1 \le k \le n \\)
* \\( 1 \le A[i] \le 2^{30} \\)
* 保证所有组的 $$n$$ 的和不超过 5000

### Example

#### Input:
```
4
3 2
1 2 2
4 3
1 2 3 4
2 2 
1 2
11 4
66 152 7 89 42 28 222 69 10 54 99
```

#### Output:
```
5
10
3
704
```

### Explanation

#### Example case 1.
最优的划分方案: 将前两个数划分为第一个子序列, 最后一个划分为第二个子序列. 
这样获得的最大价值是: \\( (1 | 2) + 2 = 3 + 2 = 5 \\).
#### Example case 2.
最优的划分方案: \\( (1|2)+2+3+4=3+3+4=10 \\).
#### Example case 3.
最优的划分方案: \\( 1+2=3 \\).

### 题解:

定义状态:
\\[
dp[i][j] = \max_{k \le i} \{  dp[k-1][j-1] + c[k][i] \}
\\]
其中 $$f[i][j]$$ 表示前$$i$$个数分成$$j$$组的最优价值. $$c[k][i]$$ 表示第$$k$$个到第$$i$$个数的或和. 满足$$opt(i,j) \le opt(i+1,j)$$, 所以我们可以使用分治法求解.

证明: 反证法

我们假设 $$opt(a,j) > opt(b,j)$$ 其中 \\( a< b \\). 令 $$opt(a,j)=x_{a}$$ , $$opt(b,j)=x_{b}$$ .

则 
\\[
x_b<x_a \Longrightarrow dp[x_b-1][j-1]+cost(x_b,a) < dp[x_a-1][j-1]+cost(x_a,a)
\\]
两边同时加上项 $$cost(a+1,b)$$.
\\[
dp[x_b-1][j-1]+cost(x_b,a)+cost(a+1,b) < dp[x_a-1][j-1]+cost(x_a,a)+cost(a+1,b)
\\]
因为 \\( (a+b)-(a \& b)=(a|b) \\), 结合上式得 
<div style="width: 90%;">
\begin{align} 
dp[x_b-1][j-1]+cost(x_b,b) & =  dp[x_b-1][j-1]+cost(x_b,a)+cost(a+1,b)
                            -  (cost(x_b,a)\And cost(a+1,b)) \\ 
                           & <  dp[x_a-1][j-1]+cost(x_a,a)+cost(a+1,b) 
                            -  (cost(x_a,a)\And cost(a+1,b)) \\
                           & =  dp[x_a-1][j-1]+cost(x_a,b) 
\end{align}
</div>
与已知条件 $$ opt(b,j)=x_b \Longrightarrow dp[x_b-1][j-1] + cost(x_b,b) > dp[x_a-1][j-1]+cost(x_a,b) $$ 矛盾.

所以假设不成立! 

### 代码:

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#define maxn 1005
using namespace std;
typedef long long ll;
int n, K, a[maxn], BitOr[maxn][maxn];
ll f[2][maxn];
void read() {
    scanf("%d%d", &n, &K);
    for(int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
}
void init() {
    for(int i = 1; i <= n; i++) {
        BitOr[i][i] = a[i];
        for(int j = i + 1; j <= n; j++)
            BitOr[i][j] = BitOr[i][j - 1] | a[j];
    }
}
int cur;
ll C(int i, int j) {
    return BitOr[i][j];
}
void compute(int l, int r, int optl, int optr) {
    if(l > r)
        return;
    int mid = (l + r) >> 1;
    pair<ll, int> best = make_pair(0, -1);
    for(int k = optl; k <= min(mid, optr); k++)
        best = max(best, make_pair(f[1 - cur][k - 1] + C(k, mid), k));
    f[cur][mid] = best.first;
    int opt = best.second;
    compute(l, mid - 1, optl, opt);
    compute(mid + 1, r, opt, optr);
}
void DP() {
    memset(f, 0, sizeof(f));
    cur = 0;
    for(int i = 1; i <= K; i++) {
        cur = 1 - cur;
        compute(1, n, 1, n);
    }
    printf("%lld\n", f[cur][n]);
}
int main() {
    int T;
    scanf("%d", &T);
    while(T--){
        read();
        init();
        DP();
    }
    return 0;
}
```

