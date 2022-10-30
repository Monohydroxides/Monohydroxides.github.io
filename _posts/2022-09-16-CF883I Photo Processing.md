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

### CF883I Photo Processing

**题目描述**

给定 $n$ 个数，需要将它们分组，每组不少于 $k$ 个数，定义一组内的代价为最大数与最小数之差，现要求最大价值最小的值。

**思路**

首先，很容易考虑到对数组进行排序，可以证明对数组进行排序后的连续划分得到的最大价值一定小于不排序的情况。

此外，可以发现答案要求“最大值最小”，这具有单调性，即若 $x \lt y$，$x$ 满足题目条件则 $y$ 一定满足题目条件。由此可以对答案进行二分。

由题目给出的数据规模可知需要将二分的 check 函数控制在 $O(n)$ 的时间复杂度。

采用动态规划进行 check，令 $dp[i]$ 表示位置在 $i$ 及之前的数是否可以找到一个合法的划分方案，那么对于区间 $[l,r]$，从 $l$ 开始向后进行动态规划，若最后能得到 $dp[r]=1$，那么说明在现有的二分限制下该区间可以被划分。

排序后数组是单调的，那么就可以采用类似单调队列的方法对 $dp$ 状态进行维护，这样得到的复杂度为 $O(n \log n)$ 的。

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 3e5 + 10;

int n, k;
int arr[N];
int dp[N];

bool check(int v){
    memset(dp, 0, sizeof dp);
	int l = 1;
	dp[0] = 1;
    for(int i = 1; i <= n; ++i){
        while (arr[i] - arr[l] > v) l++;
        int r = i - k + 1;
        for (int j = l; j <= r; ++j) {
            if (dp[j - 1]) {
                dp[i] = 1;
                break;
            } else {
                l++;
            }
        }
    }
	return dp[n];
}

int main () {
    cin >> n >> k;
    for (int i = 1; i <= n; ++i) {
        cin >> arr[i];
    }
    sort(arr + 1, arr + 1 + n);
    int l = 0, r = arr[n] - arr[1];
    while (l < r) {
        int mid = l + r >> 1;
        if (check(mid)) r = mid - 1;
        else    l = mid + 1;
    }
    if (check(l)) {
        cout << l << '\n';
    } else {
        cout << l + 1 << '\n';
    }
    return 0;
}
```