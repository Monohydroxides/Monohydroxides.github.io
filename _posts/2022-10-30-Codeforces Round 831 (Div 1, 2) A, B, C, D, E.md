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

# Codeforces Round #831 (Div. 1 + Div. 2) A,B,C,D,E

### [A. Factorise N+M](https://codeforces.com/contest/1740/problem/A)
注意到任何奇素数 $p$ 都有 $p + 3$ 为非素数，特判 $2$ 即可。

### [B. Jumbo Extra Cheese 2](https://codeforces.com/contest/1740/problem/B)
画图可以发现周长可以补成一个矩形，又由题目的要求，直接让 $x$ 轴上的边最短即可，注意爆 long long。

### [C. Bricks and Bags](https://codeforces.com/contest/1740/problem/C)
观察样例后发现最优的放法应当将其中两堆分别只放 1 个，第三堆放其他的以免对答案产生影响。因此可以贪心，对数组排序后枚举中间放哪个即可。

### [D. Knowledge Cards](https://codeforces.com/contest/1740/problem/D)
类似于华容道，注意到 $(1, 1)$ 和 $(n, m)$ 两个格子是不能复用的，那么除去这两个格子外，我们如果能再空出两个格子，那么任意一个卡片就可以通过辗转腾挪到达任何一个点。可以使用的格子的上界个数是 $n * m - 4$，模拟过程即可。

### [E. Hanging Hearts](https://codeforces.com/contest/1740/problem/E)
树形 dp。
观察样例，每个节点的最大值可以通过子节点的最长链 +1 得到，使用 $dp[u][1]$ 维护最长链，$dp[u][0]$ 维护答案，进行一次 dfs 即可。
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

int h[N], e[N * 2], ne[N * 2], idx;
long long dp[N][2];

void add (int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs1 (int u, int father, int depth) {
    int cnt = 0;
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (v == father) {
            continue;
        }
        cnt++;
        dfs1(v, u, depth + 1);
        dp[u][0] += dp[v][0];
        dp[u][1] = max(dp[u][1], dp[v][1] + 1);
    }
    if (cnt == 0) {
        dp[u][1]++;
        dp[u][0]++;
    }
    dp[u][0] = max(dp[u][1], dp[u][0]);
}

int main () {
    int n;
    cin >> n;
    memset(h, -1, sizeof h);
    for (int i = 2; i <= n; ++i) {
        int x;
        cin >> x;
        add(x, i), add(i, x);
    }
    dfs1(1, -1, 1);
    cout << dp[1][0] << '\n';
    return 0;
}
```
