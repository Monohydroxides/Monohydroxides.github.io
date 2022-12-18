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

### [A. Add Plus Minus Sign](https://codeforces.com/contest/1774/problem/A)
直接遍历看 1 的奇偶性即可。

### [B. Coloring](https://codeforces.com/contest/1774/problem/B)
画图观察后发现，我们只需要对 $n / k$ 及其余数进行观察即可，当存在余数时，我们最多可以将 $n \bmod k$ 种颜色使用 $\frac{n}{k} + 1$ 次，当 $n \bmod k = 0$ 时，我们最多可以将 $k$ 种颜色使用 $\frac{n}{k}$ 次。

### [C. Ice and Fire](https://codeforces.com/contest/1774/problem/C)
手算众多样例后发现，取长度为 x 的字符串的前缀子串，答案只与这个子串的最长相等后缀的长度有关。

一个感性的理解是因为对战的顺序是任意的，前面的结果可以采用特定的排列消除任意的玩家，只有相等的最长后缀能使得存在一些玩家，无论采用什么对战顺序，他们都一定会被淘汰。

### [D. Same Count One](https://codeforces.com/contest/1774/problem/D)
观察后发现只要 $count1 \bmod n = 0$ 时一定可以完成该过程，直接模拟即可。

### [E. Two Chess Pieces](https://codeforces.com/contest/1774/problem/E)
一个很妙的树 dp。

当 A 需要经过点 i 时，那么 B 就一定需要经过点 i 的 d 层父亲，反之同理，**这样处理完成之后，访问情况和访问顺序就无关了**。

第一次 dfs 处理出每个点的 d 层父亲，第二次 dfs 对每个点根据 A 和 B 的访问情况的不同打上 tag，统计答案时，对每个点（根节点除外），每有一个 tag 答案就增加 2（tag 对应的 A 或 B 到达这个节点一次，离开这个节点一次）。
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;

int n, d;
int m1, m2;
int h[N], e[2 * N], ne[2 * N], idx;

int cr[N], nr[N];
int fa[N], sz[N], dep[N];
int tag1[N], tag2[N];
int res[N][3];

void add (int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs1 (int u, int father, int depth) {
    sz[u] = 1, fa[u] = father, dep[u] = depth;
    cr[depth] = u;
    if (depth > d) {
        nr[u] = cr[depth - d];
    } else {
        nr[u] = 1;
    }
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (v == father) {
            continue;
        }
        dfs1(v, u, depth + 1);
    }
}

void dfs2 (int u, int father, int type) {
    int cur = 0;
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (v == father) {
            continue;
        }
        dfs2(v, u, type);
        if (type == 1) {
            cur |= tag1[v];
        } else {
            cur |= tag2[v];
        }
    }
    if (type == 1) {
        tag1[u] |= cur;
    } else {
        tag2[u] |= cur;
    }
}

void work () {
    cin >> n >> d;
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n - 1; ++i) {
        int u, v;
        cin >> u >> v;
        add(u, v), add(v, u);
    }
    dfs1(1, -1, 1);
    cin >> m1;
    for (int i = 1; i <= m1; ++i) {
        int x;
        cin >> x;
        tag1[x] = 1, tag2[nr[x]] = 1;
    }
    cin >> m2;
    for (int i = 1; i <= m2; ++i) {
        int x;
        cin >> x;
        tag2[x] = 1, tag1[nr[x]] = 1;
    }
    dfs2(1, -1, 1);
    dfs2(1, -1, 2);
    int ans = 0;
    for (int i = 2; i <= n; ++i) {
        if (tag1[i]) {
            ans += 2;
        }
    }
    for (int i = 2; i <= n; ++i) {
        if (tag2[i]) {
            ans += 2;
        }
    }
    cout << ans << '\n';
}

int main () {
    cin.tie(0), cout.tie(0);
    ios::sync_with_stdio(false);
    int T = 1;
    while (T--) {
        work();
    }
    return 0;
}
```