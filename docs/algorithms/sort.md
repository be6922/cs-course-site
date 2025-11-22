# 🔢 排序與資料結構總整理（Sorting & Data Structures）

本章介紹程式設計中最常用的 **排序演算法** 與 **基本資料結構**，搭配多個 C++ 範例，方便在 APCS / TQC+ / ZeroJudge / CSES 題目中直接套用。

---

## 📘 0. 為什麼要學排序與資料結構？

幾乎所有演算法題都會用到：

- 先把資料「**整理好**」→ 再做判斷或搜尋  
- 用「**適合的資料結構**」→ 讓程式變快、程式碼更簡潔  

常見情境：

- 排名、排序輸出、找第 k 大 / 第 k 小  
- 時間軸排序（區間、會議室、活動安排）  
- 搭配二分搜尋（`lower_bound` / `upper_bound`）  
- 事件模擬（優先隊列、佇列、堆疊）  

本章分為兩大部分：

1. **排序演算法（Algorithms）**  
2. **基本資料結構（Data Structures）**

---

# 1️⃣ 排序演算法總覽

常見排序演算法與時間複雜度比較表：

| 演算法          | 平均時間複雜度 | 最壞時間複雜度 | 穩定性 | 是否實務常用       |
|-----------------|----------------|----------------|--------|--------------------|
| 選擇排序        | O(n²)          | O(n²)          | ❌      | 教學為主           |
| 泡沫排序        | O(n²)          | O(n²)          | ✔      | 教學為主           |
| 插入排序        | O(n²)          | O(n²)          | ✔      | 小資料、幾乎排序好 |
| 合併排序        | O(n log n)     | O(n log n)     | ✔      | 理論重要           |
| 快速排序        | O(n log n)     | O(n²)          | ❌      | 實務常用           |
| 堆排序          | O(n log n)     | O(n log n)     | ❌      | 使用 heap 時可用   |
| 計數排序        | O(n + k)       | O(n + k)       | ✔      | 整數且範圍小       |
| 基數排序        | 依位數         | 依位數         | ✔      | 特殊用途           |
| C++ `sort`      | O(n log n)     | O(n log n)     | ❌      | **實戰主力**       |
| C++ `stable_sort` | O(n log n)   | O(n log n)     | ✔      | 需要穩定排序時     |

在競賽與實務中：  
👉 **95% 以上的情況直接使用 C++ `sort` 即可。**  
其餘演算法主要用來理解原理或應付特殊題目。

---

## 2️⃣ 基本排序演算法（教學用）

### 2.1 選擇排序（Selection Sort）

**想法：**

1. 從所有元素中找出「最小值」  
2. 把它換到最前面  
3. 對剩下的部分重複

**C++ 範例：**

```cpp
#include <bits/stdc++.h>
using namespace std;

void selection_sort(vector<int> &a) {
    int n = a.size();
    for (int i = 0; i < n; i++) {
        int p = i;                  // 假設 a[i] 是最小值位置
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[p]) p = j; // 找到更小的就更新位置
        }
        swap(a[i], a[p]);           // 把最小值換到前面
    }
}

int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) cin >> a[i];
    selection_sort(a);
    for (int x : a) cout << x << " ";
    cout << "\n";
}
```

> 優點：好理解  
> 缺點：慢（O(n²)），只適合教學、幾乎不會在比賽中真的使用。

---

### 2.2 泡沫排序（Bubble Sort）

**想法：**

- 一次遍歷陣列，不斷交換「相鄰且順序錯誤」的元素，像泡泡往上浮。

**C++ 範例（含最佳化）：**

```cpp
void bubble_sort(vector<int> &a) {
    int n = a.size();
    bool swapped = true;
    for (int i = 0; i < n - 1 && swapped; i++) {
        swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j+1]) {
                swap(a[j], a[j+1]);
                swapped = true;
            }
        }
    }
}
```

> 常用於：教學「交換」概念與相鄰比較。

---

### 2.3 插入排序（Insertion Sort）

**想法：**

- 像整理撲克牌：一張一張拿起來，插入前面「已排序」的部分。

**C++ 範例：**

```cpp
void insertion_sort(vector<int> &a) {
    int n = a.size();
    for (int i = 1; i < n; i++) {
        int key = a[i];
        int j = i - 1;
        // 把比 key 大的全部往右移
        while (j >= 0 && a[j] > key) {
            a[j+1] = a[j];
            j--;
        }
        a[j+1] = key;  // 把 key 插入正確位置
    }
}
```

> 優點：當資料「幾乎排序好」時非常快  
> 在某些題目可當小規模優化。

---

## 3️⃣ 高級排序：合併排序 & 快速排序（概念）

### 3.1 合併排序（Merge Sort）

**想法：Divide & Conquer**

1. 把陣列切成兩半，分別排序  
2. 再把兩個「已排序」的陣列合併（兩指標往前走）

**時間複雜度：O(n log n)**，穩定。

**合併步驟 C++ 範例：**

```cpp
void merge_vec(vector<int> &a, int l, int m, int r) {
    vector<int> L(a.begin() + l, a.begin() + m + 1);
    vector<int> R(a.begin() + m + 1, a.begin() + r + 1);
    int i = 0, j = 0, k = l;
    while (i < (int)L.size() && j < (int)R.size()) {
        if (L[i] <= R[j]) a[k++] = L[i++];
        else a[k++] = R[j++];
    }
    while (i < (int)L.size()) a[k++] = L[i++];
    while (j < (int)R.size()) a[k++] = R[j++];
}

void merge_sort(vector<int> &a, int l, int r) {
    if (l >= r) return;
    int m = (l + r) / 2;
    merge_sort(a, l, m);
    merge_sort(a, m+1, r);
    merge_vec(a, l, m, r);
}
```

---

### 3.2 快速排序（Quick Sort）

**想法：**

1. 選一個 pivot（樞紐）  
2. 把比 pivot 小的放左邊，比 pivot 大的放右邊  
3. 對左右再遞迴排序

平均時間：O(n log n)  
最壞：O(n²)（例如已排序的資料又選邊界為 pivot）

> C++ 的 `sort` 內部使用「混合排序」：  
> 非單純 quick sort，而是 intro sort（quick + heap + insertion）。

---

# 4️⃣ 實務：C++ `sort` 與 `stable_sort`

## 4.1 基本用法

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) cin >> a[i];

    sort(a.begin(), a.end());  // 從小到大排序

    for (int x : a) cout << x << " ";
    cout << "\n";
}
```

---

## 4.2 由大到小排序

```cpp
sort(a.begin(), a.end(), greater<int>());
```

或：

```cpp
sort(a.rbegin(), a.rend());   // 使用反向 iterator
```

---

## 4.3 排序 `pair` / 結構（struct）

### 例：依成績排序，若成績相同則依學號由小到大

```cpp
struct Stu {
    int id;
    int score;
};

int main() {
    int n;
    cin >> n;
    vector<Stu> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i].id >> v[i].score;
    }

    sort(v.begin(), v.end(), [](const Stu &a, const Stu &b) {
        if (a.score != b.score) return a.score > b.score; // 分數高的在前
        return a.id < b.id;                               // 學號小的在前
    });

    for (auto &s : v) {
        cout << s.id << " " << s.score << "\n";
    }
}
```

---

## 4.4 `stable_sort`：穩定排序

**穩定排序**：若兩個元素 key 相同，排序後仍維持原本輸入順序。

```cpp
stable_sort(v.begin(), v.end(), cmp);
```

適合情境：

- 先依「原始順序」代表某種時間或優先權  
- 再依第二項目排序時，想保留原來的相對順序

---

# 5️⃣ 基本資料結構與排序關係

## 5.1 陣列、向量（Array / Vector）

- **陣列 `int a[1000];`**：固定大小、記憶體連續  
- **向量 `vector<int>`**：動態大小、可 `push_back`  

排序時通常使用：

```cpp
int a[1000];
sort(a, a + n);    // 傳指標範圍

vector<int> v;
sort(v.begin(), v.end());
```

---

## 5.2 堆疊（Stack）與佇列（Queue）

- **Stack（LIFO）**：如括號匹配、逆序輸出  
- **Queue（FIFO）**：如 BFS、排隊模擬  

這兩種不「排序」內容，而是控制資料流順序。

---

## 5.3 優先佇列（Priority Queue, Heap）

- 內部使用 **Heap（堆積樹）**  
- 可以視為「**動態排序**」：
  - 每次 `top()` 都是目前最大 / 最小值  
  - 插入 / 刪除最大值時間是 O(log n)

例：合併費用最小（每次合併最小兩個數）

```cpp
priority_queue<long long, vector<long long>, greater<long long>> pq;
```

---

## 5.4 樹與二元搜尋樹（簡介）

- **BST（Binary Search Tree）**：
  - 左子樹 < 根節點 < 右子樹  
  - 中序走訪（in-order traversal）會得到排序後的序列  
- 平衡 BST（如 Red-Black Tree）是 `set` / `map` 的底層實作。

---

## 5.5 Hash Table（雜湊表）

- 不保證順序  
- 但提供平均 O(1) 的查詢與插入  
- 與排序常搭配使用：
  - 先用 hash 記錄出現次數，再將 key 排序輸出

---

# 6️⃣ 綜合範例：成績排序 + 分組

題目：  
- 讀入 n 位學生，每位有：學號、姓名、分數  
- 先依「分數由高到低」排序  
- 若分數相同，依「姓名字典序」排序  
- 最後輸出排序結果

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Stu {
    int id;
    string name;
    int score;
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    vector<Stu> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i].id >> v[i].name >> v[i].score;
    }

    sort(v.begin(), v.end(), [](const Stu &a, const Stu &b) {
        if (a.score != b.score) return a.score > b.score;
        if (a.name != b.name) return a.name < b.name;
        return a.id < b.id;
    });

    for (auto &s : v) {
        cout << s.id << " " << s.name << " " << s.score << "\n";
    }
}
```

---

# 7️⃣ 練習題建議（Sorting & Data Structures）

可配合 ZeroJudge / CSES / UVA 題目做練習：

- **基礎排序：**
  - 將一串數字排序後輸出（ZeroJudge a130 類）
  - CSES – *Distinct Numbers*（搭配 sort 或 set）

- **多重條件排序：**
  - 學生成績排序（多欄位 sort）
  - 活動安排（結束時間排序 + 貪心）

- **與資料結構結合：**
  - CSES – *Ferris Wheel*（sort + two pointers）
  - CSES – *Concert Tickets*（sort + multiset / lower_bound）
  - UVA 10954 – *Add All*（priority_queue）
  - LeetCode – *Two Sum*（unordered_map）

---
※以上資料為 ChatGPT 整理
