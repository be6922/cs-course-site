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
#include <bits/stdc++.h>  // 把常用的 C++ 標準函式庫一次全部載入
using namespace std;      // 使用 std 命名空間，方便寫 cout / cin 等

int main() {              // 主程式進入點
    int n;                // 宣告整數變數 n，用來存「第幾個 Fibonacci」
    cin >> n;             // 從輸入讀入 n

    // 建立一個長度為 n+1 的陣列 dp
    // dp[i] 用來存「第 i 個 Fibonacci 數」
    vector<long long> dp(n + 1);

    dp[0] = 0;            // 根據定義：F(0) = 0
    if (n >= 1)           // 如果 n 至少是 1（避免 n=0 時越界）
        dp[1] = 1;        // 根據定義：F(1) = 1

    // 用迴圈從 i = 2 一直到 i = n
    // 每次都根據「前兩項」算出下一項
    for (int i = 2; i <= n; i++) {
        // Fibonacci 遞迴關係：
        // F(i) = F(i-1) + F(i-2)
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    cout << dp[n];        // 輸出第 n 個 Fibonacci 數
    return 0;             // 正常結束程式
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

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    const int MOD = 1000000007;
    int n;
    if (!(cin >> n)) return 0;

    // dp[x] = 湊出總和 x 的方法數
    vector<int> dp(n + 1, 0);
    dp[0] = 1;  // 和為 0 只有一種方法：什麼都不做

    for (int x = 1; x <= n; x++) {
        long long ways = 0;
        // 嘗試最後一顆骰子是 1~6
        for (int k = 1; k <= 6; k++) {
            if (x - k < 0) break;       // 後面的 k 只會更大，全都不合法，直接離開迴圈
            ways += dp[x - k];          // 前面先湊到 x-k，再加上這顆 k
        }
        dp[x] = (int)(ways % MOD);      // 記得取模
    }

    cout << dp[n] << '\n';
    return 0;
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
#include <bits/stdc++.h>                   // 載入常用的 C++ 標頭
using namespace std;
const int MOD = 1e9+7;                    // 題目常用的大質數取模

int main() {
    int n; 
    cin >> n;                              // 讀入要湊出的總和 n

    // dp[x] = 湊出總和 x 的方法數
    // pre[x] = dp[0] + dp[1] + ... + dp[x]（prefix sum）
    vector<long long> dp(n + 1), pre(n + 1);

    dp[0] = 1;                             // base case：湊出 0 的方法只有 1 種（什麼都不做）
    pre[0] = 1;                            // prefix[0] = dp[0]

    // 依序計算 dp[1] ... dp[n]
    for (int x = 1; x <= n; x++) {

        // L = 區間左端點（若 x>=6 就是 x-6，否則就是 0）
        // R = 區間右端點 = x-1
        // 這段代表：dp[x] = dp[x-1] + dp[x-2] + ... + dp[x-6]
        int L = max(0, x - 6);
        int R = x - 1;

        // 先取 prefix[R] = dp[0] + ... + dp[R]
        long long sum = pre[R];

        // 若 L > 0，代表左界不是從 dp[0] 開始
        // 真正的區間和 = prefix[R] - prefix[L-1]
        if (L > 0)
            sum = (sum - pre[L - 1] + MOD) % MOD;

        // 計算好的區間和就是 dp[x]
        dp[x] = sum;

        // prefix[x] = prefix[x-1] + dp[x]（記得也要 %MOD）
        pre[x] = (pre[x - 1] + dp[x]) % MOD;
    }

    // 輸出答案 dp[n]
    cout << dp[n];
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
#include <bits/stdc++.h>           // 一次載入常用 C++ 標頭
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;               // 讀入網格大小 n 行、m 列

    // dp[i][j] = 從 (0,0) 走到 (i,j) 的方法數
    vector<vector<long long>> dp(n, vector<long long>(m));

    dp[0][0] = 1;                // 起點 (0,0) 只有 1 種走法（站在原地）

    // 雙層迴圈，走遍整個 n x m 網格
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {

            // 如果可以從上方來（i > 0）
            if (i > 0)
                dp[i][j] += dp[i - 1][j];

            // 如果可以從左邊來（j > 0）
            if (j > 0)
                dp[i][j] += dp[i][j - 1];
        }

    // 最終答案：從 (0,0) 走到 (n-1,m-1) 的方法數
    cout << dp[n - 1][m - 1];
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

* State：dp[i][cap]=用前 i 個物品、容量為 cap 的最大價值
* 
* Transition：取或不取
* 
  **如果不拿第 i 物品： dp[i][cap]=dp[i−1][cap]
  
  **如果拿第 i 物品：dp[i][cap]=dp[i−1][cap−wi​]+vi​
  
  **取最大值：:dp[i][cap]=max(不拿,要拿)
  
* Base：dp[0][*]=0

## C++

```cpp
#include <bits/stdc++.h>              // 一次載入常用標頭
using namespace std;

int main() {
    int n, W;
    cin >> n >> W;                    // n = 物品數量, W = 背包容量

    vector<int> w(n), v(n);           // w[i] = 第 i 個物品重量, v[i] = 價值

    // 讀入每個物品的重量與價值
    for (int i = 0; i < n; i++)
        cin >> w[i] >> v[i];

    // dp[i][cap] =
    // 用前 i 個物品、容量為 cap 能得到的最大價值
    vector<vector<int>> dp(n + 1, vector<int>(W + 1));

    // i = 前 i 個物品
    for (int i = 1; i <= n; i++)
        // cap = 當前背包容量
        for (int cap = 0; cap <= W; cap++) {

            // 1. 不拿第 i 個物品
            dp[i][cap] = dp[i - 1][cap];

            // 2. 若容量夠，考慮拿第 i 個物品（i-1 是 index）
            if (cap >= w[i - 1]) {
                dp[i][cap] = max(
                    dp[i][cap],                         // 不拿
                    dp[i - 1][cap - w[i - 1]] + v[i - 1] // 拿
                );
            }
        }

    // dp[n][W] = 用全部 n 個物品、容量 W 的最大價值
    cout << dp[n][W];
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
#include <bits/stdc++.h>      // 載入常用 C++ 標頭
using namespace std;

int main() {
    int n;
    cin >> n;                 // 讀入序列長度 n

    vector<int> a(n), dp(n, 1);
    // a[i] = 序列的第 i 個數字
    // dp[i] = 以 a[i] 作為「結尾」的 LIS 長度
    // 初始每個 dp[i] = 1（至少包含自己）

    for (int &i : a)
        cin >> i;            // 讀入序列

    int ans = 1;             // 全局最大 LIS 長度

    // 外圈迴圈：計算 dp[i]
    for (int i = 0; i < n; i++) {

        // 內圈迴圈：找所有 j < i 且 a[j] < a[i]
        for (int j = 0; j < i; j++)
            if (a[j] < a[i])
                // 若能接在 a[j] 後面，就更新 dp[i]
                dp[i] = max(dp[i], dp[j] + 1);

        // 更新目前找到的最大 LIS 長度
        ans = max(ans, dp[i]);
    }

    cout << ans;             // 輸出 LIS 長度
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

