### [A. Divide and Conquer](https://codeforces.com/contest/1762/problem/A)
按题意中的操作，使用最小次数改变一个数列的和的奇偶性，只需要改变这个数列中能被最快改变奇偶性的数的奇偶性即可。

### [B. Make Array Good](https://codeforces.com/contest/1762/problem/B)
因为每个数增加的数不能超过自身，一开始的想法是把数列中所有的数都改成数列中最小的数的倍数，但这样操作之后无法保证两两之间是可整除的，根据这个思路进一步思考，对排序后每个数根据它前面的那个数增加大小的思路就很自然了。

### [C. Binary Strings are Fun](https://codeforces.com/contest/1762/problem/C)
题目难在理解题意和题目中的众多定义。

理解题目后不难发现直接贪心的从后往前思考每个子串最后一个 0 和最后一个 1 的位置之差即可，他们对答案的贡献是 $2^{\mid las1 - las0 \mid - 1}$。

### [D. GCD Queries](https://codeforces.com/contest/1762/problem/D)
交互题，一个未知的 $0 \sim n - 1$ 的一个排列，每次可以询问两个不同位置的 $\gcd$，需要在最多 $2n$ 次询问后给出这个排列中 $0$ 可能在的两个位置。

假设有三个各不相同的位置 $i, j, k$，令 $l = \gcd(a_i, a_k)$，$r = \gcd(a_j, a_k)$，下面对 $l, r$ 的大小情况进行讨论：

$l = r$，此时 $a_k \neq 0$，因为这是一个排列，而 $\gcd(0, m) = m$，一个排列中不可能有两个相同的数。

$r \lt l$，此时 $a_j \neq 0$，因为 $\gcd(0, a_k) = a_k \geq \gcd(m, a_k)$，由于是排列，则前式无法取等。

$l \lt r$，同理，$a_i \neq 0$。

由上述讨论知，每两次询问可以排除掉一个位置，因此我们需要进行 $2(n - 2)$ 询问即可得到答案，符合要求。

```cpp
#include <bits/stdc++.h>
using namespace std;

void work () {
    int n;
    cin >> n;
    queue<int> res;
    for (int i = 1; i <= n; ++i) {
        res.push(i);
    }
    while (res.size() > 2) {
        int cur1, cur2;
        int fr = res.front(); res.pop();
        int se = res.front(); res.pop();
        int tr = res.front(); res.pop();
        cout << "? " << fr << " " << se << endl;
        cin >> cur1;
        cout << "? " << se << " " << tr << endl;
        cin >> cur2;
        if (cur1 == cur2) {
            res.push(fr), res.push(tr);
        } else if (cur1 < cur2) {
            res.push(se), res.push(tr);
        } else {
            res.push(se), res.push(fr);
        }
    }
    cout << "! " << res.front();
    res.pop();
    cout << " " << res.front() << endl;
    int ret;
    cin >> ret;
}

int main () {
    cin.tie(0), cout.tie(0);
    ios::sync_with_stdio(false);
    int T;
    cin >> T;
    while (T--) {
        work();
    }
    return 0;
}
```
