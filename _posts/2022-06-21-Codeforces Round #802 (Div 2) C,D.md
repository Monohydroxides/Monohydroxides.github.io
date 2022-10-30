---
tag: Codeforces
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

# Codeforces Round #802 (Div. 2) C,D

## C. Helping the Nature

**题目描述**：给定一个数组 $a_1, a_2, \dots, a_n$，有如下三种操作：

1. 所有数+1
2. $a_1 \sim a_i$,所有数-1
3. $a_i \sim a_n$,所有数-1

求将整个数组变为全0的最小操作次数。

**思路**：等价与差分数组变为全 $0$。操作 $1$ 相当于对差分数组进行 $dif[1] + 1$，操作二相当于对差分数组进行 $dif[1]-1, dif[i + 1]+1$，操作三相当于对差分数组进行 $dif[i]-1$。而后直接从后向前对这个过程进行模拟即可。

**代码**:

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

const int MOD = 998244353;

int T;
LL a[200010];
LL dif[200010];

void work(){
	int n;
    cin >> n;
    for(int i = 1; i <= n; ++i){
        cin >> a[i];
        dif[i] = a[i] - a[i - 1];
    }
    LL cnt = 0, res = 0;
    for(int i = n; i > 1; --i){
        if(dif[i] < 0){
            cnt += -dif[i];
            dif[i] = 0; 
        }
    }
    dif[1] -= cnt;
    res += cnt;
    if(dif[1] < 0)  res += -dif[1], dif[1] = 0;
    for(int i = 1; i <= n; ++i){
        if(dif[i]){
            res += dif[i];
        }
    }
    cout << res << endl;
	return;
}

int main(){
	cin.tie(0);
	cin >> T;
	while(T--)	work();
	return 0;
}
```

## D. River Locks

**题目描述**：一排水桶接水，每个水桶上有一个水管，每个水管一单位时间内可放出一单位的水，当这个水桶的水量达到要求时，多出的水会跑到右侧的水桶中。给定水桶的数量和每个水桶的接水量要求，求多组询问下要在给定的时间内让所有的水桶中的水量达到要求，最少需要开启的水管的数量。

**思路**：左边的水桶满了会溢出到右边的水桶，越优先考虑开左侧的水管是越优的。

可以从左到右扫一遍，记录下来这个位置及之前的水桶被这个位置及之前的水管灌满所需要的最长的时间，然后取最大值，再分别处理每个询问即可。

**代码**:

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;

int T;

int a[200010];
LL pre[200010];

void work(){
    int n, q;
    cin >> n;
    for(int i = 1; i <= n; ++i){
        cin >> a[i];
        pre[i] = pre[i - 1] + a[i];
    }
    LL res = a[1];
    for(int i = 2; i <= n; ++i){
        res = max(res, (pre[i] + i - 1) / i);
    }
    cin >> q;
    while(q--){
        int x;
        cin >> x;
        if(x < res){
            cout << -1 << endl;
        }else{
            cout << (pre[n] + x - 1) / x << endl;
        }
    }
}

int main(){
    T = 1;
    while(T--)  work();
    return 0;
}
```