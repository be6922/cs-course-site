# 🧰 C++ STL（Standard Template Library）

本頁整理常用的 C++ STL 容器與演算法，適合作為課堂講義與學生自學參考。

---

# 📦 一、序列式容器（Sequence Containers）

## 1️⃣ `vector` — 動態陣列（最常用）

### ✔ 特性
- 連續記憶體（像 C 陣列）
- `push_back()` 自動擴張
- 隨機存取 O(1)
- 中間插入 / 刪除 O(n)

### ✔ 基本語法
```cpp
#include <vector>
using namespace std;

vector<int> v;                // 空的 vector
vector<int> v2(10);           // 長度 10，預設值 0
vector<int> v3 = {1, 2, 3};   // 初始化列表

v.push_back(5);               // 尾端加入 5
v.pop_back();                 // 尾端刪除一個元素
int n = v.size();             // 元素個數
v.clear();                    // 清空
int x = v[0];                 // 讀取第 0 個元素
```

### 🔹 典型輸入與輸出
```cpp
int n;
cin >> n;
vector<int> v(n);
for (int i = 0; i < n; i++) cin >> v[i];

// 逐一輸出
for (int i = 0; i < n; i++) cout << v[i] << " ";
cout << "\n";

// range-based for
for (int x : v) cout << x << " ";
cout << "\n";
```

### 🔹 增加 / 刪除 / 查詢
```cpp
// 尾端增加
v.push_back(100);

// 刪除第 k 個元素
int k = 2;
if (0 <= k && k < (int)v.size())
    v.erase(v.begin() + k);

// 在第 k 個位置前插入
v.insert(v.begin() + k, 999);

// 查某值是否存在（線性）
bool found = false;
for (int x : v) {
    if (x == 50) { found = true; break; }
}
```

### ✔ 代表題目
- CSES – Static Range Sum Queries（前綴和）

---

## 2️⃣ `string` — 字串容器

### ✔ 基本語法
```cpp
#include <string>
using namespace std;

string s = "hello";
char c = s[0];           // 存取字元
int len = s.size();      // 長度
s.push_back('!');        // 尾端加字元
s += " world";           // 串接
string t = s.substr(0, 5);  // 取子字串 "hello"
```

### 🔹 典型輸入與輸出
```cpp
string s;
cin >> s;                // 讀一個不含空白的字串

string line;
getline(cin, line);      // 讀一整行（可含空白）
cout << line << "\n";

// 逐字元處理
for (char c : s) {
    if (isalpha(c)) { /* ... */ }
}
```

### 🔹 常見操作
```cpp
// 統計 'a' 出現次數
int cnt = 0;
for (char c : s) if (c == 'a') cnt++;

// 字串反轉
reverse(s.begin(), s.end());
```

### ✔ 代表題目
- ZeroJudge a362 類字串處理題

---

## 3️⃣ `deque` — 雙端佇列

### ✔ 特性
- 可在頭尾 O(1) 加入 / 刪除
- 可用 `operator[]` 存取
- 適合 0/1 BFS、滑動視窗

### ✔ 基本語法
```cpp
#include <deque>
using namespace std;

deque<int> dq;
dq.push_back(3);
dq.push_front(1);
dq.pop_back();
dq.pop_front();
int x = dq[0];
```

### 🔹 典型輸入與輸出
```cpp
int n;
cin >> n;
deque<int> dq;
for (int i = 0; i < n; i++) {
    int x; cin >> x;
    dq.push_back(x);
}

for (int x : dq) cout << x << " ";
cout << "\n";
```

### ✔ 代表題目
- CSES – Sliding Window Maximum / Minimum（滑動視窗）

---

## 4️⃣ `list` — 雙向鏈結串列

### ✔ 特性
- 中間插入 / 刪除 O(1)
- 不支援 `operator[]` 隨機存取（O(n)）

### ✔ 基本語法
```cpp
#include <list>
using namespace std;

list<int> lst;
lst.push_back(3);
lst.push_front(1);

auto it = lst.begin();
advance(it, 1);        // it 指向第二個元素
lst.insert(it, 5);     // 在 it 前插入 5
lst.erase(it);         // 刪除 it 指向的元素

for (int x : lst) cout << x << " ";
cout << "\n";
```

---

# 🎒 二、容器配接器（Container Adapters）

## 5️⃣ `stack` — 堆疊（LIFO）

### ✔ 基本語法
```cpp
#include <stack>
using namespace std;

stack<int> st;
st.push(3);
int x = st.top();   // 看頂端
st.pop();           // 移除頂端
bool emp = st.empty();
```

### 🔹 括號匹配示例
```cpp
string s;
cin >> s;
stack<char> st;
bool ok = true;
for (char c : s) {
    if (c == '(') st.push(c);
    else if (c == ')') {
        if (st.empty()) { ok = false; break; }
        st.pop();
    }
}
if (!st.empty()) ok = false;
cout << (ok ? "YES\n" : "NO\n");
```

### ✔ 代表題目
- UVA 514 – Rails

---

## 6️⃣ `queue` — 佇列（FIFO）

### ✔ 基本語法
```cpp
#include <queue>
using namespace std;

queue<int> q;
q.push(10);
int x = q.front();
q.pop();
bool emp = q.empty();
```

### 🔹 BFS 典型用法
```cpp
int n, m;
cin >> n >> m;
vector<vector<int>> adj(n+1);
for (int i = 0; i < m; i++) {
    int a, b;
    cin >> a >> b;
    adj[a].push_back(b);
    adj[b].push_back(a);
}

queue<int> q;
vector<int> dist(n+1, -1);
q.push(1);
dist[1] = 0;
while (!q.empty()) {
    int u = q.front(); q.pop();
    for (int v : adj[u]) {
        if (dist[v] == -1) {
            dist[v] = dist[u] + 1;
            q.push(v);
        }
    }
}
```

### ✔ 代表題目
- ZeroJudge a290 – 迷宮 BFS

---

## 7️⃣ `priority_queue` — 優先佇列（Heap）

### ✔ 特性
- 預設為「最大堆」
- 搭配 comparator 可變成最小堆

### ✔ 基本語法
```cpp
#include <queue>
using namespace std;

priority_queue<int> pq;   // 最大堆
pq.push(5);
pq.push(10);
int x = pq.top();         // 10
pq.pop();

// 最小堆
priority_queue<int, vector<int>, greater<int>> minpq;
```

### 🔹 典型輸入與輸出
```cpp
int n;
cin >> n;
priority_queue<int> pq;
for (int i = 0; i < n; i++) {
    int x; cin >> x;
    pq.push(x);
}
while (!pq.empty()) {
    cout << pq.top() << " ";
    pq.pop();
}
cout << "\n";
```

### ✔ 代表題目
- UVA 10954 – Add All

---

# 🌳 三、關聯式容器（Ordered Associative Containers）

## 8️⃣ `set` — 自動排序、不重複集合

### ✔ 基本語法
```cpp
#include <set>
using namespace std;

set<int> s;
s.insert(3);
s.insert(1);
s.insert(3);              // 不會重複
bool has = s.count(3);    // 是否存在
s.erase(1);               // 刪除值 1
auto it = s.lower_bound(2); // 第一個 >= 2 的 iterator
```

### 🔹 典型輸入與輸出
```cpp
int n;
cin >> n;
set<int> s;
for (int i = 0; i < n; i++) {
    int x; cin >> x;
    s.insert(x);
}

cout << "distinct = " << s.size() << "\n";
for (int x : s) cout << x << " ";
cout << "\n";
```

### ✔ 代表題目
- CSES – Distinct Numbers

---

## 9️⃣ `multiset` — 可重複集合

### ✔ 基本語法
```cpp
#include <set>
using namespace std;

multiset<int> ms;
ms.insert(10);
ms.insert(10);
int c = ms.count(10);       // 2

auto it = ms.find(10);      // 指向某個 10
if (it != ms.end()) ms.erase(it); // 只刪一個 10
```

### 🔹 典型輸入與輸出
```cpp
int n;
cin >> n;
multiset<int> ms;
for (int i = 0; i < n; i++) {
    int x; cin >> x;
    ms.insert(x);
}
for (int x : ms) cout << x << " ";  // 會有重複
cout << "\n";
```

### 🔹 常見技巧：找「≤ 顧客預算」的最大值
```cpp
int x; // 顧客價格
auto it = ms.upper_bound(x); // 第一個 > x
if (it == ms.begin()) {
    // 沒有任何 ≤ x 的票
} else {
    --it;         // 改成最後一個 ≤ x
    int price = *it;
    ms.erase(it); // 賣掉這張票
}
```

### ✔ 代表題目
- CSES – Concert Tickets

---

## 🔟 `map` — 對應表（key → value）

### ✔ 基本語法
```cpp
#include <map>
using namespace std;

map<string,int> mp;
mp["apple"] = 3;
mp["apple"]++;                 // 4
bool has = mp.count("apple");  // 有無 key
mp.erase("apple");
```

### 🔹 典型輸入與輸出：統計字串次數
```cpp
int n;
cin >> n;
map<string,int> mp;
while (n--) {
    string s;
    cin >> s;
    mp[s]++;
}

for (auto [word, c] : mp) {
    cout << word << " " << c << "\n"; // 按字典序輸出
}
```

### ✔ 代表題目
- UVA 11572 – Unique Snowflakes（可用 map 紀錄位置）

---

## 1️⃣1️⃣ `unordered_map` / `unordered_set` — 雜湊容器（Hash）

### ✔ 基本語法
```cpp
#include <unordered_map>
#include <unordered_set>
using namespace std;

unordered_map<string,int> mp;
mp["hi"]++;
auto it = mp.find("hi");
if (it != mp.end()) {
    cout << it->second;
}

unordered_set<int> us;
us.insert(5);
bool has = us.count(5);
```

### 🔹 Two Sum 經典寫法
```cpp
int n, target;
cin >> n >> target;
vector<int> a(n);
for (int i = 0; i < n; i++) cin >> a[i];

unordered_map<int,int> pos; // 值 -> index
for (int i = 0; i < n; i++) {
    int need = target - a[i];
    if (pos.count(need)) {
        cout << pos[need] << " " << i << "\n";
        break;
    }
    pos[a[i]] = i;
}
```

### ✔ 代表題目
- LeetCode – Two Sum

---

# ⚙️ 四、常用 STL 演算法（Algorithms）

所有以下函式都在 `<algorithm>`（部分在 `<numeric>`）中。

## 1️⃣ `sort` — 排序

### ✔ 基本語法
```cpp
#include <algorithm>
using namespace std;

sort(v.begin(), v.end());                        // 由小到大
sort(v.begin(), v.end(), greater<int>());        // 由大到小

// 自訂排序條件（以 pair 為例）
sort(v.begin(), v.end(), [](auto &a, auto &b){
    if (a.first != b.first) return a.first < b.first;
    return a.second < b.second;
});
```

### ✔ 代表題目
- CSES – Ferris Wheel

---

## 2️⃣ `reverse` — 反轉
```cpp
reverse(v.begin(), v.end());
```
- 字串反轉
- 路徑反轉（例如 BFS 回溯）

---

## 3️⃣ `find` / `count`
```cpp
auto it = find(v.begin(), v.end(), x);
int c = count(v.begin(), v.end(), x);
```
- 小資料量線性搜尋

---

## 4️⃣ `max_element` / `min_element`
```cpp
int mx = *max_element(v.begin(), v.end());
int mn = *min_element(v.begin(), v.end());
```

---

## 5️⃣ `accumulate` — 求和
```cpp
#include <numeric>

long long sum = accumulate(v.begin(), v.end(), 0LL);
```

---

## 6️⃣ `binary_search` / `lower_bound` / `upper_bound`
```cpp
sort(v.begin(), v.end());

bool ok = binary_search(v.begin(), v.end(), x);

auto it1 = lower_bound(v.begin(), v.end(), x); // 第一個 >= x
auto it2 = upper_bound(v.begin(), v.end(), x); // 第一個 > x

int cnt = it2 - it1;  // x 出現次數
```

### ✔ 代表題目
- CSES – Concert Tickets

---

## 7️⃣ `next_permutation` — 全排列
```cpp
sort(v.begin(), v.end());
do {
    // 使用目前的排列
} while (next_permutation(v.begin(), v.end()));
```

### ✔ 代表題目
- UVA 146 – ID Codes

---

## 8️⃣ `unique` + `erase` — 去重
```cpp
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
```

### ✔ 代表題目
- CSES – Distinct Numbers（vector 寫法）

---

# 📘 五、STL × 練習題總表（速查）

| STL 項目           | 主題                      | 線上題目                        |
|--------------------|---------------------------|---------------------------------|
| `vector`           | 陣列、前綴和              | CSES – Static Range Sum        |
| `string`           | 字串                      | ZeroJudge a362                 |
| `deque`            | 滑動視窗 / 0-1 BFS        | CSES – Sliding Window Maximum  |
| `stack`            | 模擬、括號、重排          | UVA 514 – Rails                |
| `queue`            | BFS                       | ZeroJudge a290                 |
| `priority_queue`   | 合併費用 / 貪心           | UVA 10954 – Add All            |
| `set`              | distinct 計數 / 自動排序  | CSES – Distinct Numbers        |
| `multiset`         | 找最接近值                | CSES – Concert Tickets         |
| `map`              | 字典、計數、不重複區段    | UVA 11572 – Unique Snowflakes  |
| `unordered_map`    | Hash 查詢 / Two Sum       | LeetCode – Two Sum             |
| `sort`             | 排序 + 貪心 / 雙指標      | CSES – Ferris Wheel            |
| `lower_bound` 等   | 二分搜尋                  | CSES – Concert Tickets         |
| `next_permutation` | 字典序排列                | UVA 146 – ID Codes             |
| `unique` + `erase` | 去重 / 壓縮               | CSES – Distinct Numbers        |


※以上資料為chatgpt整理
