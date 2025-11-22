# 🧰 C++ STL（Standard Template Library）

本頁提供 STL 課程內容：
- 每個容器的用途、底層、複雜度
- 每個容器附上一題線上評測代表題目
- 常用演算法（Algorithms）
---

# 📦 一、序列式容器（Sequence Containers）

## 1️⃣ vector — 動態陣列（最常用）
### ✔ 特性
- 連續記憶體（像 array）
- push_back 擴張
- 隨機存取 O(1)
- 中間插入 O(n)

### ✔ 基本語法
```cpp
vector<int> v;
vector<int> v2(10);
vector<int> v3 = {1,2,3};
v.push_back(5);
v.pop_back();
v.size();
v.clear();
v[i];
```

### ✔ 代表題目
CSES – Static Range Sum Queries

---

## 2️⃣ string — 字串容器
### ✔ 基本語法
```cpp
string s = "hello";
s[0];
s.size();
s.push_back('!');
s += " world";
s.substr(0,3);
```

### ✔ 代表題目
ZeroJudge a362 – 字串題

---

## 3️⃣ deque — 雙端佇列
### ✔ 基本語法
```cpp
deque<int> dq;
dq.push_back(3);
dq.push_front(1);
dq.pop_back();
dq.pop_front();
dq[0];
```

### ✔ 代表題目
CSES – Sliding Window Maximum

---

## 4️⃣ list — 雙向鏈結串列
### ✔ 基本語法
```cpp
list<int> lst;
lst.push_back(3);
lst.push_front(1);
auto it = lst.begin();
advance(it, 1);
lst.insert(it, 5);
lst.erase(it);
```

### ✔ 代表題目
大量插入刪除模擬題

---

# 🎒 二、容器配接器（Adapters）

## 5️⃣ stack — 堆疊（LIFO）
### ✔ 基本語法
```cpp
stack<int> st;
st.push(3);
st.top();
st.pop();
st.empty();
```

### ✔ 代表題目
UVA 514 – Rails

---

## 6️⃣ queue — 佇列（FIFO）
### ✔ 基本語法
```cpp
queue<int> q;
q.push(10);
q.front();
q.pop();
q.empty();
```

### ✔ 代表題目
ZeroJudge a290 – 迷宮 BFS

---

## 7️⃣ priority_queue — 優先佇列（Heap）
### ✔ 基本語法
```cpp
priority_queue<int> pq;
pq.push(5);
pq.top();
pq.pop();

priority_queue<int, vector<int>, greater<int>> minpq;
```

### ✔ 代表題目
UVA 10954 – Add All

---

# 🌳 三、關聯式容器（Ordered）

## 8️⃣ set — 自動排序、不重複
### ✔ 基本語法
```cpp
set<int> s;
s.insert(3);
s.count(3);
s.erase(3);
auto it = s.lower_bound(2);
```

### ✔ 代表題目
CSES – Distinct Numbers

---

## 9️⃣ multiset — 可重複集合
### ✔ 基本語法
```cpp
multiset<int> ms;
ms.insert(10);
ms.insert(10);
ms.count(10);
ms.erase(ms.find(10));
```

### ✔ 代表題目
CSES – Concert Tickets

---

## 🔟 map — key → value 對應表
### ✔ 基本語法
```cpp
map<string,int> mp;
mp["apple"] = 3;
mp["apple"]++;
mp.count("apple");
```

### ✔ 代表題目
UVA 11572 – Unique Snowflakes

---

## 1️⃣1️⃣ unordered_map / unordered_set — 雜湊容器（Hash）
### ✔ 基本語法
```cpp
unordered_map<string,int> mp;
mp["hi"]++;
mp.find("hi");

unordered_set<int> us;
us.insert(5);
us.count(5);
```

### ✔ 代表題目
LeetCode – Two Sum

---

# ⚙️ 四、常用 STL 演算法（Algorithms）

## 1️⃣ sort（排序）
### ✔ 基本語法
```cpp
sort(v.begin(), v.end());
sort(v.begin(), v.end(), greater<int>());
```
### ✔ 代表題目
CSES – Ferris Wheel

---

## 2️⃣ reverse（反轉）
```cpp
reverse(v.begin(), v.end());
```
代表題目：字串反轉類題、路徑反轉

---

## 3️⃣ find / count
```cpp
auto it = find(v.begin(), v.end(), x);
int c = count(v.begin(), v.end(), x);
```
代表題目：ZeroJudge 統計小題

---

## 4️⃣ max_element / min_element
```cpp
int mx = *max_element(v.begin(), v.end());
```
代表題目：最大最小值查詢

---

## 5️⃣ accumulate（求和）
```cpp
long long sum = accumulate(v.begin(), v.end(), 0LL);
```
代表題目：區間總和類題

---

## 6️⃣ lower_bound / upper_bound
```cpp
auto it = lower_bound(v.begin(), v.end(), x);
auto it2 = upper_bound(v.begin(), v.end(), x);
```
代表題目：CSES – Concert Tickets

---

## 7️⃣ next_permutation（全排列）
```cpp
do {
} while (next_permutation(v.begin(), v.end()));
```
代表題目：UVA 146 – ID Codes

---

## 8️⃣ unique + erase（去重）
```cpp
v.erase(unique(v.begin(), v.end()), v.end());
```
代表題目：Distinct Numbers（vector 寫法）

---

# 📘 五、STL × 練習題總表

| STL 項目 | 主題 | 線上題目 |
|----------|------|-----------|
| vector | 陣列、前綴和 | CSES – Static Range Sum |
| string | 字串 | ZeroJudge a362 |
| deque | 0/1 BFS | CSES Sliding Window |
| stack | 軌道重排 | UVA 514 |
| queue | BFS | ZeroJudge a290 |
| priority_queue | 合併費用 | UVA 10954 |
| set | distinct | CSES Distinct Numbers |
| multiset | 最接近值 | CSES Concert Tickets |
| map | 不重複區段 | UVA 11572 |
| unordered_map | Hash 查詢 | LeetCode Two Sum |
| sort | 排序 | CSES Ferris Wheel |
| lower_bound | 二分搜尋 | Concert Tickets |
| next_permutation | 字典序 | UVA 146 |
| unique+erase | 去重 | Distinct Numbers |

---
※以上資料為chatgpt整理
