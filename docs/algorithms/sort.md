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
// 泡沫排序（Bubble Sort）
// 特色：一次比較相鄰兩個元素，把大的往後推
// 時間複雜度：最差 O(n^2)，最佳 O(n)

void bubble_sort(vector<int> &a) {

    int n = a.size();        // 取得陣列長度
    bool swapped = true;     // swapped 用來偵測本輪是否發生交換

    // 外層迴圈負責跑 n-1 輪，每一輪把一個最大值推到最後
    // 如果某輪沒有交換動作，就代表已經排序完成，可提前結束
    for (int i = 0; i < n - 1 && swapped; i++) {

        swapped = false;     // 每輪開始時先假設沒有交換

        // 內層迴圈：比較相鄰兩數，若順序錯誤就交換
        // j < n-1-i：因為後面 i 個已是排序好的最大值，不需再比較
        for (int j = 0; j < n - 1 - i; j++) {

            // 若前者 > 後者 → 交換
            if (a[j] > a[j+1]) {
                swap(a[j], a[j+1]);
                swapped = true;  // 發生交換 → 本輪至少有效果
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
// 插入排序（Insertion Sort）
// 特色：從左到右，維持左側區域為「已排序區」
// 將新的元素插入到已排序區的正確位置
// 時間複雜度：最差 O(n^2)，最佳（資料本來就排序好）O(n)

void insertion_sort(vector<int> &a) {

    int n = a.size();   // 取得陣列長度

    // i 從第 1 個位置開始（因為第 0 個視為已排序）
    for (int i = 1; i < n; i++) {

        int key = a[i];     // 暫存要被「插入」的值
        int j = i - 1;      // 從已排序區的最後一個開始比較

        // 將所有「比 key 大」的值往右移，為 key 騰出位置
        while (j >= 0 && a[j] > key) {
            a[j+1] = a[j];  // 大的往右移
            j--;            // 往左繼續比較
        }

        // j 停下的位置為「第一個比 key 小或等於」的位置
        // 所以 key 應插入 j+1
        a[j+1] = key;
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
// merge_vec：將 a[l..m] 與 a[m+1..r] 兩段「已排序」的子序列合併成一段排序好的序列
void merge_vec(vector<int> &a, int l, int m, int r) {

    // 建立左半邊 L：從 a[l] 到 a[m]
    vector<int> L(a.begin() + l, a.begin() + m + 1);

    // 建立右半邊 R：從 a[m+1] 到 a[r]
    vector<int> R(a.begin() + m + 1, a.begin() + r + 1);

    // i 指向 L，j 指向 R，k 指向原陣列 a 中要放置的位置
    int i = 0, j = 0, k = l;

    // 比較 L 和 R 的目前元素，較小者放回 a[k]
    while (i < (int)L.size() && j < (int)R.size()) {
        if (L[i] <= R[j])
            a[k++] = L[i++];   // L[i] 比較小 → 放入 a
        else
            a[k++] = R[j++];   // R[j] 比較小 → 放入 a
    }

    // 若 L 還沒放完，把剩下的全部放進 a
    while (i < (int)L.size())
        a[k++] = L[i++];

    // 若 R 還沒放完，放進 a
    while (j < (int)R.size())
        a[k++] = R[j++];
}

// merge_sort：遞迴把陣列切半 → 排序 → 合併
void merge_sort(vector<int> &a, int l, int r) {
    if (l >= r) return;         // 只有一個元素 → 天生排序好

    int m = (l + r) / 2;        // 找中間點

    merge_sort(a, l, m);        // 排序左半邊
    merge_sort(a, m + 1, r);    // 排序右半邊

    merge_vec(a, l, m, r);      // 合併兩個排序好的半邊
}

```

---

### 3.2 快速排序（Quick Sort）
# 快速排序（Quick Sort）教學

快速排序（Quick Sort）是一種高效率、常見的排序演算法，採用「分治法」策略。

---

## 📘 基本概念

1. 選擇一個基準值（pivot）
2. 將小於等於 pivot 的放左邊，大於 pivot 的放右邊
3. 對左右子區域遞迴排序

---

## 📌 Quick Sort 程式碼（C++）

```cpp
// 分區函式：將 pivot 放到正確位置，並回傳 pivot 的新索引
int partition(vector<int> &a, int l, int r) {
    int pivot = a[r];    // 選最右邊的值作為 pivot
    int i = l - 1;       // i 指向「小於 pivot 區域」的最後一位

    // j 從 l 到 r-1 一一檢查
    for (int j = l; j < r; j++) {
        if (a[j] <= pivot) {     // 若 a[j] 比 pivot 小
            i++;                 // 小於區域擴大一格
            swap(a[i], a[j]);    // 交換到左邊
        }
    }

    // 最後把 pivot 放到正確位置（i+1）
    swap(a[i + 1], a[r]);
    return i + 1;   // 回傳 pivot 位置
}

// Quick Sort 主程式：遞迴左右分治
void quick_sort(vector<int> &a, int l, int r) {
    if (l >= r) return;     // 遞迴終止條件（只有 0 或 1 個元素）

    int p = partition(a, l, r);  // 尋找 pivot 正確位置

    quick_sort(a, l, p - 1);     // 排序左半邊（較小）
    quick_sort(a, p + 1, r);     // 排序右半邊（較大）
}
```

---

## ⏱ 時間複雜度

| 情況 | 時間複雜度 | 說明 |
|------|-------------|------|
| 最佳 | O(n log n) | 每次平均切半 |
| 平均 | O(n log n) | 常見情況 |
| 最差 | O(n²) | pivot 太偏（例如已排序資料） |

---

## 📦 空間複雜度

- 平均遞迴深度：O(log n)
- 最差：O(n)

---

## 🧠 小示意圖

示例：排序 `[5, 1, 4, 2, 3]`

pivot = 3  

左半邊（≤ 3）：1, 2, 3  
右半邊（> 3）：5, 4  

遞迴排序左右部分即可。

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
