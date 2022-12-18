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

### [A. Cut the Triangle](https://codeforces.com/contest/1767/problem/A)
数一下不同横坐标和纵坐标的点的个数即可，均为 2 说明不可切割。

### [B. Block Towers](https://codeforces.com/contest/1767/problem/B)
对 $2 \sim n$ 的 tower 排序后从 2 开始把能放到 1 上全部放到 1 上，即可贪心得到最大的 tower 1 的高度。

### [C. Count Binary Strings](https://codeforces.com/contest/1767/problem/C)
考虑 dp：$dp[i][j]$ 表示从 1 到 $i$ 为止均符合要求的 01 子串中，上一个与 $i$ 位置不同的字符位于 j 的方案个数。那么转移是比较好考虑的：
$dp[i + 1][j] += dp[i][j]$ 表示在 $i$ 位置仍放相同的字符；
$dp[i + 1][i + 1] += dp[i][j]$ 表示在 $i$ 位置放与 $j$ 位置相同的字符，即更新这个 01 串中最后一个不同字符的位置。
但转移之前需要对这个转移是否合法进行判断，即对题目中给定的限制，当限制为 1 时，需要保证 $j \leq k$，当限制为 2 时，需要保证 $j \gt k$。
统计答案为 $\sum_{i = 1}^{n} dp[n][i]$。
具体的细节在代码中。
```cpp
#include <bits/stdc++.h>
using namespace std;

const int mod = 998244353;

int n;
int a[110][110];
int dp[110][110];

int add (int a, int b) {
    return ((a + b) % mod + mod) % mod;
}

int mul (int a, int b) {
    return 1ll * a * b % mod;
}

int main () {
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        for (int j = i; j <= n; ++j) {
            cin >> a[i][j];
        }
    }
    dp[1][1] = 2 * (a[1][1] != 2);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= i; ++j) {
            int flag = 0;
            for (int k = 1; k <= i; ++k) {
                if(a[k][i] == 1 && k < j) flag = 1;
                if(a[k][i] == 2 && k >= j) flag = 1;
            }
            if(flag) dp[i][j] = 0;
            dp[i + 1][j] = add(dp[i + 1][j], dp[i][j]);
            dp[i + 1][i + 1] = add(dp[i + 1][i + 1], dp[i][j]);
        }
    }
    int ans = 0;
    for (int i = 1; i <= n; ++i) { 
        ans = (ans + dp[n][i]) % mod;
    }
    cout << ans << endl;
    return 0;
}
```

### [D. Playoff](https://codeforces.com/contest/1767/problem/D)
此题比较简单，观察样例后发现胜负场数和 2 的 1 和 0 的个数次幂有关，直接计算即可。
```cpp
#include <bits/stdc++.h>
using namespace std;

void work () {
    int n;
    cin >> n;
    string str;
    cin >> str;
    str = " " + str;
    int l = 1, r = pow(2, n);
    int cnt1 = 0, cnt0 = 0;
    for (int i = 1; i <= n; ++i) {
        if (str[i] == '1') {
            l += pow(2, cnt1);
            cnt1++;
        } else {
            r -= pow(2, cnt0);
            cnt0++;
        }
    }
    for (int i = l; i <= r; ++i) {
        cout << i << " ";
    }
    cout << '\n';
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
