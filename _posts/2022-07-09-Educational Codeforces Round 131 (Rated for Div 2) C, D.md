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


# Educational Codeforces Round 131 (Rated for Div. 2) C, D

### C. Schedule Management

**题目描述**: $n$个工人, $m$个工作, 每个工作有一个合适的工人, 当工人是当前工作的合适工人的时候, 其需要一个小时完成该工作, 否则需要两个小时. 一个工作在一个时刻只能被一个工人从事, 问完成全部工作的最小时间.

**思路**: 二分答案即可: 对于每个时间, 判断一下当前时间是否能满足要求. 需要注意的是 check 函数.

**代码实现**: 

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;

int n, m;
int cnt[200010];

int check(LL t){
    LL restask = 0, resworker = 0;
    for(int i = 1; i <= n; ++i){
        if(cnt[i] > t){
            restask += (cnt[i] - t);
        }else{
            resworker += (t - cnt[i]) / 2;
        }
    }
    return resworker >= restask;
}

void work(){
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) cnt[i] = 0;
    for(int i = 1; i <= m; ++i){
        int x;
        cin >> x;
        cnt[x]++;
    }
    LL l = 1, r = 2 * m;
    while(l < r){
        int mid = l + r >> 1;
        if(!check(mid)) l = mid + 1;
        else            r = mid;
    }
    cout << l << endl;
    return;
}

int main(){
    int T;
    cin >> T;
    while(T--)  work();
    return 0;
}
```

### D. Permutation Restoration

**题目描述**: $a$是 $1-n$的一个排列, 对于 $b_i$, 其生成规则为: $b_i = \lfloor \frac{i}{a_i} \rfloor$. 现给定数列 $b$, 求一个可能的 $a$.

**思路**: 不难发现, 对于 $a_i$, 其范围应该满足 $\lfloor \frac{i}{b_i + 1} \rfloor + 1
\leq a_i \leq
\left\{
\begin{aligned}
&n, b_i =0 \\
&\lfloor \frac{i}{b_i} \rfloor, b_i \neq 0
\end{aligned}
\right.$, 接下来, 为了求出数列 $a$,我们会有的一个很自然的思路是对左端点进行排序后进行枚举.但是,对于这个例子:

有五个点的取值范围为 $[1, 7]$,两个点的取值范围为 $[2, 3]$,如果我们从小到大枚举左端点,我们会发现,这种情况下 $[2,3]$这两个点在最后无法填入任何值.

这时候, 我们的思路会转变成从小到大枚举这 $n$个排列数, 把将第 $i$个数可以放入的所有点的右端点放入一个优先队列, 每次取出其中最小的一个, 将这个数放在右端点最小的位置.

**代码实现**: 

```cpp
#include <bits/stdc++.h>
using namespace std;
using AII = array<int, 2>;

 
int b[500010];
int res[500010];
 
void work(){
    int n;
    cin >> n;
    vector<array<int, 2> > a[n + 1]; 
    for(int i = 1; i <= n; ++i){
        cin >> b[i];
        int l = i / (b[i] + 1) + 1;
        int r = b[i] == 0? n: (i / b[i]);
        a[l].push_back({r, i});
    }
    priority_queue<AII, vector<AII>, greater<AII> > pq;
    for(int i = 1; i <= n; ++i){
        for(auto iter: a[i])    pq.push(iter);
        auto iter = pq.top();
        pq.pop();
        res[(iter)[1]] = i;
    }
    for(int i = 1; i <= n; ++i){
        cout << res[i] << " ";
    }
    cout << endl;
}
 
int main(){
    int T;
    cin.tie(0);
    cin >> T;
    while(T--)  work();
    return 0;
}
```