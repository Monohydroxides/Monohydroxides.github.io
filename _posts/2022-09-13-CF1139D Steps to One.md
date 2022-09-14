---
tag: 杂题选做
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>


# CF1139D Steps to One

### 题目描述

一个数列, 初始时为空, 每次随机选一个 $1$ 到 $m$ 之间的数加入数列末尾, 数列中所有数的 $\gcd = 1$ 时停止操作. 求数列的期望长度.

### 题解

设 $E(len)$ 为长度为 $len$ 的期望, 那么显然有 $E(len) = \sum_{i = 1}^{\infty} p(len = i) * i$, 其中, $p(len = x)$ 为数列长度为 $x$ 的概率.

对 $E(len)$ 进行处理:

$$
\begin{aligned}
E(len) &= \sum_{i= 1}^{\infty} p(len = i) * i \\
&= \sum_{i = 1}^{\infty}p(len = i) \sum_{j = 1}^{i}1 \\
&=\sum_{j = 1}^{\infty} \sum_{i = j}^{\infty}p(len = i) \\
&=\sum_{j = 1}^{\infty} p(len \geq j) \\
&= 1 + \sum_{i = 1}^{\infty} p(len \gt i)
\end{aligned}
$$

考虑 $p(len>i)$:

$$
\begin{aligned}
p(len \gt i) &= p((\gcd_{j = 1}^{i} a_{j}) > 1) \\
&= 1 - p((\gcd_{j = 1}^{i}a_{j}) = 1) \\
&= 1 - 
\frac{\sum_{a_{1} = 1}^{m}
\sum_{a_{2} = 1}^{m} 
\dots 
\sum_{a_{i} = 1}^{m}
[(\gcd_{j = 1}^{i}a_{j}) = 1]}
{m^{i}} \\
&= 1 - 
\frac{\sum_{a_{1} = 1}^{m}
\sum_{a_{2} = 1}^{m} 
\dots 
\sum_{a_{i} = 1}^{m}
\sum_{d \mid \gcd} \mu(d)}
{m^{i}} \\
&= 1 - 
\frac{\sum_{d = 1}^{m} \mu(d) \cdot (\lfloor \frac{m}{d} \rfloor)^{i}}
{m^{i}}
\end{aligned}
$$

将 $p(len \gt i)$ 带回 $E(len)$:

$$
\begin{aligned}
E(len) &= 1 + \sum_{i = 1}^{\infty} (1 - 
\frac{\sum_{d = 1}^{m} \mu(d)  (\lfloor \frac{m}{d} \rfloor)^{i}}{m^{i}}) \\
&= 1 - \sum_{i = 1}^{\infty} (\frac{\sum_{d = 2}^{m} \mu(d)  (\lfloor \frac{m}{d} \rfloor)^{i}}{m^{i}}) \\
&= 1 - \sum_{i = 1}^{\infty} \frac{1}{m^{i}} \sum_{d = 2}^{m} \mu(d)(\lfloor \frac{m}{d} \rfloor)^{i} \\
&= 1 - \sum_{d=2}^{m}\mu(d) \sum_{i=1}^{\infty}(\frac{ \lfloor \frac{m}{d} \rfloor}{m})^{i} \\
&= 1 - \sum_{d=2}^{m}\mu(d) \frac{\lfloor \frac{m}{d} \rfloor}{m - \lfloor \frac{m}{d} \rfloor}
\end{aligned}
$$

由此, 可以预处理出莫比乌斯函数后进行递推, $O(m)$.

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10, mod = 1e9 + 7;

int m;
int primes[N], cnt;
bool st[N];
int mu[N];
int inv[N];

int qmi (int a, int b) {
    int res = 1;
    while (b) {
        if (b & 1) {
            res = (long long) res * a % mod;
        }
        a = (long long) a * a % mod;
        b >>= 1;
    }
    return res;
}

void init () {
    mu[1] = 1;
    for (int i = 2; i <= m; ++i) {
        if (!st[i]) {
            primes[cnt++] = i;
            mu[i] = -1;
        }
        for (int j = 0; primes[j] * i <= m; ++j) {
            st[primes[j] * i] = 1;
            if (i % primes[j] == 0) {
                break;
            }
            mu[primes[j] * i] = -mu[i]; 
        }
    }
}

int main () {
    cin >> m;
    init();
    int res = 0;
    for (int i = 1; i <= m; ++i) {
        inv[i] = qmi(i, mod - 2);
    }
    for (int d = 2; d <= m; ++d) {
        res = ((res - (long long) mu[d] * (m / d) * inv[m - (m / d)]) % mod + mod) % mod;
    }
    res = (res + 1) % mod;
    cout << res << endl;
    return 0;
}
```