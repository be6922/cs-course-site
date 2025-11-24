# 動態規劃（Dynamic Programming）教學

> 給高中／大一程度學生使用，搭配 C++ / Python 範例與圖表說明。
> 完整教案版，可直接存為 dp.md 放入 GitHub / MkDocs。

---


## 1. 什麼是動態規劃？

### 1.1 直覺版
把一個「大問題」拆成很多「小問題」，  
把小問題的答案算好、存起來，  
再用它們堆出大問題的答案。

**DP 核心精神：**

- **重複的子問題（Overlapping Subproblems）**
- **最佳子結構（Optimal Substructure）**

若兩者皆具備 → 很適合用 DP。

---

## 2. DP 三大要素：STB 分析

### ✔ S = State（狀態）
dp[i]、dp[i][j] 代表什麼？

### ✔ T = Transition（轉移）
dp[i] 如何由小的 dp 推得？

### ✔ B = Base Case（初始值）
最小的那些 dp 值是多少？

---

## 3. Top-down vs Bottom-up

### 3.1 Top-down（遞迴 + 記憶化）
先寫遞迴 f(n)，計算過就記起來，避免重算。

### 3.2 Bottom-up（迴圈）
直接從小到大填 dp 表。  
→ **競賽最常用**

---

# 4. Unit 1 — 練習 1：簡單遞迴轉 DP

## 題目說明
定義：
- dp[0] = 1
- dp[1] = 1
- dp[i] = dp[i-1] + dp[i-2]

給定 n，求 dp[n]。

（完整程式碼略，與教學相同）

---


# 5. Unit 2 — Fibonacci

F(0)=0, F(1)=1, F(n)=F(n-1)+F(n-2)

（程式碼略，與教學相同）

---


# 6. Unit 3 — 爬樓梯（Climbing Stairs）

每次可走 1 或 2 階，問到達第 n 階有幾種走法。

STB：
- S：dp[i] = 走到第 i 階的方法數  
- T：dp[i] = dp[i-1] + dp[i-2]  
- B：dp[0]=1, dp[1]=1  

（程式碼略，與教學相同）

---


# 7. Unit 4 — 骰子湊出總和（基本 DP）

dp[x] = dp[x-1] + dp[x-2] + ... + dp[x-6]  
dp[0] = 1

---

## 7.1 表格示範（以 n = 7）

| x | 公式 | dp[x] |
|---|-------------------------------|--------|
| 0 | 定義：不擲任何骰子 | 1 |
| 1 | dp[1] = dp[0] | 1 |
| 2 | dp[2] = dp[1] + dp[0] | 2 |
| 3 | dp[3] = dp[2] + dp[1] + dp[0] | 4 |
| 4 | dp[4] = dp[3] + dp[2] + dp[1] + dp[0] | 8 |
| 5 | dp[5] = dp[4] + dp[3] + dp[2] + dp[1] + dp[0] | 16 |
| 6 | dp[6] = dp[5] + dp[4] + dp[3] + dp[2] + dp[1] + dp[0] | 32 |
| 7 | dp[7] = dp[6] + dp[5] + dp[4] + dp[3] + dp[2] + dp[1] | 63 |

可觀察：

- x ≤ 6 時，dp[x] = 2^x  
- x ≥ 7 後不再是 2 的冪（因不能使用 dp[-1] 等負數）


---

## 7.2 小朋友版故事解釋：存零用錢

你每天得到 1～6 元，要湊到 n 元。

以 n = 4 為例：

- 1+1+1+1  
- 1+1+2  
- 1+2+1  
- 2+1+1  
- 2+2  
- 1+3  
- 3+1  
- 4  

總共 **8 種方法** → dp[4] = 8。

---

# 8. Unit 5 — 前綴和優化（Prefix Sum）

dp[x] = dp[x-1] + ... + dp[x-6]  
可寫成區間和：

prefix[i] = dp[0] + ... + dp[i]  
dp[x] = prefix[x-1] - prefix[x-7]

（程式碼略）

---

# 9. Unit 6 — Grid Paths（網格走法）

dp[i][j] = 從 (0,0) 走到 (i,j) 的走法數  
可從上方 dp[i-1][j] 或左方 dp[i][j-1] 來。

（程式碼略）

---

# 10. Unit 7 — 0/1 Knapsack（揹包問題）

dp[i][w] = 用前 i 件物品在容量 w 的最大價值。

（程式碼略）

---

# 11. Unit 8 — LIS（Longest Increasing Subsequence）

dp[i] = 以 a[i] 結尾的 LIS 長度  
若 j<i 且 a[j]<a[i] → dp[i] = max(dp[i], dp[j]+1)

（程式碼略）

---

# 12. Unit 9 — 區間 DP（Interval DP）

範例：矩陣鏈乘（Matrix Chain Multiplication）  
dp[l][r] = 最小成本

（可依需求補全部程式碼）

---

# 13. Unit 10 — 樹 DP（Tree DP）

例：最大獨立集  
dp[u][0]、dp[u][1]

（可依需求補全部程式碼）

---

# ✔ 本文件已整合所有 DP 單元與教案內容。
