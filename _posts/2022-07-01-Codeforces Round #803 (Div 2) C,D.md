---
tag: Codeforces
---

# Codeforces Round #803 (Div. 2) C,D

## C. 3SUM Closure

**题目描述**: 给定一个由整数构成的数组, 若其中任意三个下标不同的数之和仍在这个数组中, 那么称这个数组是3SUM-closed.求判断一个数组是否是3SUM-closed数组.有多组测试数据.

**思路**: 若这个数组中有三个及以上正数(或负数),那么我们可以将正数排序,最大的三个正数之和一定不在这个数组中(负数同理).据此思路对正数个数,负数个数,零个数进行分类讨论即可.

**代码实现**: 注意情况较多,在比赛时头昏WA了几发,不得不使用最暴力的写法.

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;

int T;
LL a[200010];

void work(){
    int n;
    cin >> n;
    int pos = 0, neg = 0, zro = 0;
    for(int i = 1; i <= n; ++i){
        cin >> a[i];
        if(a[i] > 0)    pos++;
        else if(a[i] < 0)    neg++;
        else if(a[i] == 0)   zro++;
    }
    if(pos >= 3 || neg >= 3){
        cout << "NO" << endl;
        return;
    }
    if(zro){
        if(pos >= 2 || neg >= 2){
            cout << "NO" << endl;
            return; 
        }
        if(pos && neg){
            sort(a + 1, a + 1 + n);
            if(a[1] + a[n] == 0){
                cout << "YES" << endl;
                return;
            }
            cout << "NO" << endl;
            return;
        }
        cout << "YES" << endl;
        return;
    }else{
        if(n == 4){
            LL s1 = a[1] + a[2] + a[3], s2 = a[1] + a[2] + a[4], s3 = a[2] + a[3] + a[4], s4 = a[1] + a[3] + a[4];
            int flg1 = 1, flg2 = 1, flg3 = 1, flg4 = 1;
            for(int i = 1;i <= n; ++i){
                if(a[i] == s1)  flg1 = 0;
                if(a[i] == s2)  flg2 = 0;
                if(a[i] == s3)  flg3 = 0;
                if(a[i] == s4)  flg4 = 0;
            }
            if(flg1 || flg2 || flg3 || flg4){
                cout << "NO" << endl;
                return;
            }
            cout << "YES" << endl;
            return;
        }else{
            LL s = a[1] + a[2] + a[3];
            for(int i = 1; i <= n; ++i){
                if(s == a[i]){
                    cout << "YES" << endl;
                    return;
                }
            }
            cout << "NO" << endl;
            return;
        }
    }
}

int main(){
    cin.tie(0);
    cin >> T;
    while(T--)  work();
    return 0;
}
```

## D. Fixed Point Guessing

**题目描述**: 交互题,一个由 $n$个正整数构成的一个排列: $1, 2, \dots, n(3 \leq n \leq 10^4, n\ is\ odd)$, 现将其中的 $\frac{n-1}{2}$组两两交换, 那么肯定会剩余一个数在其本来的位置上没有被交换.现对每个用例可以进行最多15次询问,询问一个区间中的数的构成(将从小到大排序后返回给你),输出那个没有被交换的数.

**思路**: 这种交互题一看就是二分.二分询问每个区间中的数,如果对 $[l,r]$的询问中有奇数个数落在 $[l,r]$这个区间中,那么没有被交换的数一定在这个区间中.

**代码实现**: 注意交互题的缓冲区清空问题.

```cpp
#include <bits/stdc++.h>
using namespace std;

int T;

int n;

int query(int l, int r){
    cout << "? " << l << " " << r << endl;
    int cnt = 0;
    for(int i = l; i <= r; ++i){
        int x;
        cin >> x;
        if(x >= l && x <= r)    cnt++;
    }
    if(cnt & 1) return 1;
    else        return 0;
}

int work(int l, int r){
    if(l == r)  return l;
    int mid = l + r >> 1;
    if(query(l, mid)){
        return work(l, mid);
    }else{
        return work(mid + 1, r);
    }
}

int main(){
    cin.tie(0);
    cin >> T;
    while(T--){
        cin >> n;
        int ans = work(1, n);
        cout << "! " << ans << endl;
    }
    return 0;
}
```