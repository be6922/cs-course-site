# 動態規劃（Dynamic Programming）教學

> 給高中／大一程度學生使用，搭配 C++ / Python 範例與圖表說明。

---

# 1. 什麼是動態規劃？

## 1.1 直覺版

動態規劃（Dynamic Programming, DP）是一種「先把小問題算好、存起來，再把答案堆出大問題」的方法。

它的精神是：

> **重複的子問題 + 最佳子結構 = 適合用 DP**

### ✔ 重複子問題

如果 subproblem 會重複出現 → 用 DP 記起來可避免重算。

### ✔ 最佳子結構

若大問題的最佳解可以用小問題的最佳解組合而成 → 適合用 DP。

---

# 2. DP 的三大要素：STB 分析

DP 設計一定要回答三件事：

### ✔ S = State（狀態）

`dp[i]`、`dp[i][j]`… 代表什麼？

### ✔ T = Transition（轉移）

`dp[i]` 如何由較小的 dp 值推得？

### ✔ B = Base Case（初始條件）

最開始的 dp 是什麼？

例：爬樓梯：

* `dp[i] = 爬到 i 階的方法數`
* `dp[i] = dp[i-1] + dp[i-2]`
* `dp[0] = 1`, `dp[1] = 1`

---

# 3. Top-down vs Bottom-up

## 3.1 Top-down（遞迴 + 記憶化）

* 先寫遞迴式 `f(n)`
* 若算過就從 dp 表回傳（避免重複計算）

## 3.2 Bottom-up（迴圈）

* 直接把 dp 從小推到大
* 比較穩定、競賽常用

---

# 4. Unit 1 — 簡單遞迴轉 DP（Warm-up）

## 題目說明

定義序列 dp：

* dp[0] = 1
* dp[1] = 1
* dp[i] = dp[i-1] + dp[i-2]

請輸出 dp[n]。

## C++ 逐行註解

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;                  // 讀入 n
    cin >> n;

    vector<long long> dp(n + 1);   // dp[0..n]

    dp[0] = 1;              // base case
    if (n >= 1) dp[1] = 1;

    for (int i = 2; i <= n; i++) {  // 從小推到大
        dp[i] = dp[i-1] + dp[i-2];
    }

    cout << dp[n];          // 輸出答案
}
```

## Python 逐行註解

```python
n = int(input())

dp = [0] * (n + 1)
dp[0] = 1
if n >= 1:
    dp[1] = 1

for i in range(2, n + 1):
    dp[i] = dp[i-1] + dp[i-2]

print(dp[n])
```

---

# 5. Unit 2 — Fibonacci

## 題目說明

費波那契是最基本的 DP：

dp[n]=dp[n−1]+dp[n−2]

S（狀態）：dp[i] = 第 i 個費波那契數
T（轉移）：dp[i] = dp[i-1] + dp[i-2]
B（初始值）：dp[0]=0, dp[1]=1
輸出第 n 個 Fibonacci 數（0 ≤ n ≤ 90）。

## C++

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n; cin >> n;
    vector<long long> dp(n+1);
    dp[0]=0;
    if(n>=1) dp[1]=1;
    for(int i=2;i<=n;i++) dp[i]=dp[i-1]+dp[i-2];
    cout<<dp[n];
}
```

## Python

```python
n=int(input())
dp=[0]*(n+1)
if n>=1: dp[1]=1
for i in range(2,n+1): dp[i]=dp[i-1]+dp[i-2]
print(dp[n])
```

---

# 6. Unit 3 — 爬樓梯（Climbing Stairs）

## 題目說明

每次可以走 1 或 2 階，問走到第 n 階有幾種走法？
dp[i]=dp[i−1]+dp[i−2]

與 Fibonacci 幾乎相同，只是 Base case 為：

dp[0] = 1   (什麼都不做算1種)
dp[1] = 1
STB：

* State：dp[i] = 到達第 i 階的走法數
* Transition：dp[i] = dp[i-1] + dp[i-2]
* Base：dp[0]=1, dp[1]=1

## C++

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n; cin>>n;
    vector<long long> dp(n+1);
    dp[0]=1;
    if(n>=1) dp[1]=1;
    for(int i=2;i<=n;i++) dp[i]=dp[i-1]+dp[i-2];
    cout<<dp[n];
}
```

## Python

```python
n=int(input())
dp=[0]*(n+1)
dp[0]=1
if n>=1: dp[1]=1
for i in range(2,n+1): dp[i]=dp[i-1]+dp[i-2]
print(dp[n])
```

---

# 7. Unit 4 — 骰子湊出總和（基本 DP）

## 題目說明

擲 1~6 點的骰子，問湊出總和 n 有多少種方法？

STB：

* State：dp[x] = 湊出總和 x 的方法數
* Transition：dp[x] = dp[x-1] + ... + dp[x-6]
* Base：dp[0]=1


## 小朋友版故事解釋

把它想成**存零用錢**：

每天媽媽用骰子決定給你 **1~6 元**，你必須剛好湊到 **n 元**。

例如 n=4：

* 1+1+1+1
* 1+1+2
* 1+2+1
* 2+1+1
* 2+2
* 1+3
* 3+1
* 4

共 8 種 → dp[4] = 8。


## 表格示範（以 n=7）

| x | 公式                                                    | dp[x] |
| - | ----------------------------------------------------- | ----- |
| 0 | 定義：不擲任何骰子                                             | 1     |
| 1 | dp[1] = dp[0]                                         | 1     |
| 2 | dp[2] = dp[1] + dp[0]                                 | 2     |
| 3 | dp[3] = dp[2] + dp[1] + dp[0]                         | 4     |
| 4 | dp[4] = dp[3] + dp[2] + dp[1] + dp[0]                 | 8     |
| 5 | dp[5] = dp[4] + dp[3] + dp[2] + dp[1] + dp[0]         | 16    |
| 6 | dp[6] = dp[5] + dp[4] + dp[3] + dp[2] + dp[1] + dp[0] | 32    |
| 7 | dp[7] = dp[6] + dp[5] + dp[4] + dp[3] + dp[2] + dp[1] | 63    |

可以看出：

* x ≤ 6 時，dp[x] = 2^x
* x ≥ 7 開始不再是 2 的冪次（因為不能用負的 dp）



## C++（逐行註解）

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;

int main() {
    int n;
    cin >> n;

    vector<long long> dp(n+1);
    dp[0] = 1;

    for(int x=1; x<=n; x++){
        long long sum = 0;
        for(int k=1; k<=6; k++){
            if(x-k<0) break;
            sum += dp[x-k];
        }
        dp[x] = sum % MOD;
    }

    cout << dp[n];
}
```
## python（逐行註解）

```python
MOD = 10**9 + 7
n = int(input())

dp = [0]*(n+1)
dp[0] = 1

for x in range(1, n+1):
    s = 0
    for k in range(1,7):
        if x-k < 0:
            break
        s += dp[x-k]
    dp[x] = s % MOD

print(dp[n])
```
---

# 8. Unit 5 — 骰子 DP（前綴和優化版）

## 題目說明
用 prefix 進行區間加速，使得 dp[x] 可在 O(1) 取得。適合 n 很大時使用。

STB：
- State：dp[x] = 湊出總和 x 的方法數
- Transition：dp[x] = prefix[x-1] - prefix[x-7]
- Base：dp[0]=1

## C++
```cpp
#include <bits/stdc++.h>
using namespace std; const int MOD=1e9+7;
int main(){
    int n; cin>>n;
    vector<long long> dp(n+1),pre(n+1);
    dp[0]=1; pre[0]=1;
    for(int x=1;x<=n;x++){
        int L=max(0,x-6),R=x-1;
        long long sum=pre[R];
        if(L>0) sum=(sum-pre[L-1]+MOD)%MOD;
        dp[x]=sum;
        pre[x]=(pre[x-1]+dp[x])%MOD;
    }
    cout<<dp[n];
}
````

## Python

```python
MOD=10**9+7
n=int(input())
dp=[0]*(n+1)
p=[0]*(n+1)
dp[0]=1;p[0]=1
for x in range(1,n+1):
    L=max(0,x-6);R=x-1
    s=p[R]
    if L>0: s=(s-p[L-1])%MOD
    dp[x]=s
    p[x]=(p[x-1]+dp[x])%MOD
print(dp[n])
```

---

# 9. Unit 6 — Grid Paths（網格走法）

## 題目說明
只能往右、往下走：
dp[i][j]=dp[i−1][j]+dp[i][j−1]
n×m 網格，只能往右或往下，求走法數。

STB：

* State：dp[i][j] = 走到 (i,j) 的方法數
* Transition：由上或左而來
* Base：dp[0][0]=1

## C++

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n,m;cin>>n>>m;
    vector<vector<long long>> dp(n,vector<long long>(m));
    dp[0][0]=1;
    for(int i=0;i<n;i++)for(int j=0;j<m;j++){
        if(i>0) dp[i][j]+=dp[i-1][j];
        if(j>0) dp[i][j]+=dp[i][j-1];
    }
    cout<<dp[n-1][m-1];
}
```

## Python

```python
n,m=map(int,input().split())
dp=[[0]*m for _ in range(n)]
dp[0][0]=1
for i in range(n):
    for j in range(m):
        if i>0: dp[i][j]+=dp[i-1][j]
        if j>0: dp[i][j]+=dp[i][j-1]
print(dp[n-1][m-1])
```

---

# 10. Unit 7 — 0/1 Knapsack（揹包問題）

## 題目說明

每個物品只能取一次，容量限制下取最大價值。
dp[i][w]=前i個物品，容量w時的最大價值
dp[i][w]=max(dp[i−1][w], dp[i−1][w−weight[i]]+value[i])

STB：

* State：dp[i][w] = 用前 i 件、容量 w 的最大價值
* Transition：取或不取
* Base：dp[0][*]=0

## C++

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n,W;cin>>n>>W;
    vector<int>w(n),v(n);
    for(int i=0;i<n;i++)cin>>w[i]>>v[i];
    vector<vector<int>> dp(n+1,vector<int>(W+1));
    for(int i=1;i<=n;i++)for(int cap=0;cap<=W;cap++){
        dp[i][cap]=dp[i-1][cap];
        if(cap>=w[i-1]) dp[i][cap]=max(dp[i][cap],dp[i-1][cap-w[i-1]]+v[i-1]);
    }
    cout<<dp[n][W];
}
```

## Python

```python
n,W=map(int,input().split())
items=[tuple(map(int,input().split())) for _ in range(n)]
dp=[[0]*(W+1) for _ in range(n+1)]
for i in range(1,n+1):
    w,v=items[i-1]
    for cap in range(W+1):
        dp[i][cap]=dp[i-1][cap]
        if cap>=w:
            dp[i][cap]=max(dp[i][cap],dp[i-1][cap-w]+v)
print(dp[n][W])
```

---

# 11. Unit 8 — LIS（Longest Increasing Subsequence）

## 題目說明

求序列中最長嚴格遞增子序列長度。
dp[i]=以a[i]結尾的LIS長度
dp[i]=max(dp[j]+1)(j<i且a[j]<a[i])

STB：

* State：dp[i] = 以 a[i] 結尾的 LIS 長度
* Transition：找所有 j<i 且 a[j]<a[i]
* Base：dp[i]=1

## C++

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n;cin>>n;
    vector<int>a(n),dp(n,1);
    for(int&i:a)cin>>i;
    int ans=1;
    for(int i=0;i<n;i++){
        for(int j=0;j<i;j++)if(a[j]<a[i]) dp[i]=max(dp[i],dp[j]+1);
        ans=max(ans,dp[i]);
    }
    cout<<ans;
}
```

## Python

```python
n=int(input())
a=list(map(int,input().split()))
dp=[1]*n
ans=1
for i in range(n):
    for j in range(i):
        if a[j]<a[i]: dp[i]=max(dp[i],dp[j]+1)
    ans=max(ans,dp[i])
print(ans)
```

---
※以上資料為chatgpt整理

