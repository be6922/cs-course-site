# 🌟 搜尋與圖論（BFS / DFS 大全）

搜尋（Search）是所有演算法的核心之一。無論是迷宮、最短路徑、找環、連通元件、拓樸排序……都離不開 **BFS（廣度優先搜尋）** 與 **DFS（深度優先搜尋）**。

本章節包含：

- BFS / DFS 基本介紹
- 迷宮 BFS 模板（C++ / Python）
- 找環教學（ASCII 圖示）
- BFS / DFS 題型大全（30 題）
- ZeroJudge 題單

---

# 1️⃣ 圖的基本概念

一張圖（Graph）由：

- **節點（Vertices / Nodes）**
- **邊（Edges）**

常用表示法：

| 方法 | 說明 |
|------|------|
| 鄰接串列（Adjacency List） | `adj[u]` 是所有與 u 相鄰的點 |
| 邊列表（Edge List） | 使用 (u, v) pair 表示一條邊 |
| 鄰接矩陣（Adjacency Matrix） | `g[u][v] = 1` 表示 u→v 有邊 |


---

# 2️⃣ BFS — Breadth-First Search（廣度優先搜尋）

**概念：** 像水波一層一層擴散。

- 使用 **Queue（佇列）**
- 能找 **最短路徑（步數最少）**
- 在迷宮題裡是最常用的算法

## BFS — C++ 樣板

```cpp
queue<int> q;
vector<int> adj[MAXN];
int dist[MAXN]; // -1 = 未訪問

void bfs(int start) {
    fill(dist, dist + MAXN, -1);
    dist[start] = 0;
    q.push(start);

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (dist[v] == -1) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
}
```

## BFS — Python 樣板

```python
from collections import deque

def bfs(start, adj):
    n = len(adj)
    dist = [-1] * n
    q = deque([start])
    dist[start] = 0

    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)
    return dist
```

---

# 3️⃣ DFS — Depth-First Search（深度優先搜尋）

**概念：** 一路往深處走到底，再回頭換路。

- 使用 **Recursion（遞迴）** 或 **Stack**
- 適合找「整塊區域」
- 常用在連通元件、找環、拓樸排序

## DFS — C++ 樣板

```cpp
vector<int> adj[MAXN];
bool visited[MAXN];

void dfs(int u) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) dfs(v);
    }
}
```

## DFS — Python 樣板

```python
def dfs(u, adj, visited):
    visited[u] = True
    for v in adj[u]:
        if not visited[v]:
            dfs(v, adj, visited)
```

---

# 4️⃣ BFS vs DFS 強化比較表

以下所有比較已統一改為表格形式：

| 功能 / 面向 | **BFS** | **DFS** |
|------------|---------|---------|
| 搜尋方式 | 層級擴散 | 深度優先 |
| 資料結構 | Queue | Recursion / Stack |
| 最短路徑 | ✔ 一定能找到 | ✘ 不一定 |
| 找連通元件 | 可 | ⭐ 最常用 |
| 找環（Cycle） | 可（需 parent） | ⭐ 最常用（parent 判斷） |
| 拓樸排序 | ✔（Kahn） | ✔（DFS） |
| 記憶體使用 | 層級大會爆 | 遞迴深可能爆 stack |
| 適合題型 | 迷宮、最短路徑 | 數島嶼、找環 |
| 走訪順序 | Level-order | Depth-order |
| 複雜度 | O(V+E) | O(V+E) |


---

# 5️⃣ 二維迷宮 BFS（最常見題型）

示例：
```
S.#..
..#.E
.#...
```

## 迷宮 BFS（C++）
```cpp
#include <bits/stdc++.h>
using namespace std;

int R, C;
vector<string> grid;
int dist[1005][1005];
int dr[4] = {-1, 0, 1, 0};
int dc[4] = {0, 1, 0, -1};

int main(){
    cin >> R >> C;
    grid.resize(R);
    for(int i = 0; i < R; i++) cin >> grid[i];

    int sr, sc, er, ec;
    for(int i=0;i<R;i++){
        for(int j=0;j<C;j++){
            if(grid[i][j]=='S') sr=i, sc=j;
            if(grid[i][j]=='E') er=i, ec=j;
            dist[i][j] = -1;
        }
    }

    queue<pair<int,int>> q;
    q.push({sr, sc});
    dist[sr][sc] = 0;

    while(!q.empty()){
        auto [r, c] = q.front(); q.pop();
        for(int k=0;k<4;k++){
            int nr = r + dr[k], nc = c + dc[k];
            if(nr<0 || nr>=R || nc<0 || nc>=C) continue;
            if(grid[nr][nc] == '#') continue;
            if(dist[nr][nc] != -1) continue;

            dist[nr][nc] = dist[r][c] + 1;
            q.push({nr, nc});
        }
    }

    cout << dist[er][ec] << "
";
    return 0;
}
```

## 迷宮 BFS（Python）
```python
from collections import deque

def bfs_maze(grid, sr, sc):
    R, C = len(grid), len(grid[0])
    dist = [[-1] * C for _ in range(R)]
    q = deque([(sr, sc)])
    dist[sr][sc] = 0

    dr = [-1, 0, 1, 0]
    dc = [0, 1, 0, -1]

    while q:
        r, c = q.popleft()
        for k in range(4):
            nr, nc = r + dr[k], c + dc[k]
            if 0 <= nr < R and 0 <= nc < C:
                if grid[nr][nc] != '#' and dist[nr][nc] == -1:
                    dist[nr][nc] = dist[r][c] + 1
                    q.append((nr, nc))
    return dist
```


---

# 6️⃣ 找環（Cycle Detection）教學

ASCII 圖示：
```
1 -- 2
|    |
4 -- 3
```

## 無向圖找環（C++）
```cpp
bool visited[MAXN];
bool has_cycle = false;

void dfs(int u, int p){
    visited[u] = true;
    for(int v : adj[u]){
        if(v == p) continue;
        if(visited[v]) has_cycle = true;
        else dfs(v, u);
    }
}
```

## 有向圖找環（C++）
```cpp
bool visited[MAXN], in_stack[MAXN];
bool has_cycle = false;

void dfs(int u){
    visited[u] = true;
    in_stack[u] = true;

    for (int v : adj[u]){
        if (!visited[v]) dfs(v);
        else if (in_stack[v]) has_cycle = true;
    }

    in_stack[u] = false;
}
```

---

# 7️⃣ BFS / DFS 題型大全（30 題）

以下為依難度分級的 BFS / DFS 題型大全，共 30 題，方便課堂使用與學生練習：

## 🟢 Level 1：入門題（1～10）
| 類型 | 說明 | ZeroJudge |
|------|------|------------|
| 迷宮最短路（BFS） | 基礎 BFS | a290 |
| 二維 BFS | 4 方向移動 | d626 |
| 可達性（Reachability） | BFS/DFS 皆可 | a725 |
| 連通元件（DFS） | 數區塊 | c291 |
| 找環（DFS） | 無向圖 cycle | c471 |
| 節點度數 | 圖基本輸入 | d190 |
| BFS 基本圖走訪 | 單一 source | a224 |
| DFS 可達點 | 遞迴基本應用 | a693 |
| Flood Fill | 填色問題 | c221 |
| Level-order BFS | 層級走法 | e507 |


## 🟡 Level 2：中階題（11～20）
| 類型 | 說明 | ZeroJudge |
|------|------|------------|
| 多源 BFS | 多個起點同時擴散 | e563 |
| grid BFS | 二維圖最短路 | f640 |
| 騎士走法（Knight BFS） | 西洋棋 knight | c005 |
| 拓樸排序（Kahn） | BFS 解 DAG | a291 |
| DFS Cycle（無向圖） | parent 判斷環 | c471 |
| DFS Cycle（有向圖） | in_stack 技巧 | — |
| BFS 回溯路徑 | 用 parent[] | — |
| 二分圖著色 | BFS 染色 | — |
| 圖最遠點（兩次 BFS） | 圖直徑 | — |
| BFS + DP 混合題 | 狀態轉移 | — |


## 🔴 Level 3：挑戰題（21～30）
| 類型 | 說明 | 備註 |
|------|------|------|
| 迷宮可破牆一次 | BFS + 3D 狀態 | 狀態 (r,c,break) |
| 0/1 BFS | Deque | 權重 0/1 |
| 最長路徑（DAG） | DFS+DP | 競賽常用 |
| SCC（強連通） | Kosaraju/Tarjan | 競賽題 |
| 割點（AP） | Tarjan | 進階 |
| 割邊（Bridge） | Tarjan | 進階 |
| Dijkstra + BFS 混合 | 模擬題 | — |
| BFS + 優先佇列 | 特殊權重 | — |
| 迷宮跳躍移動 | 特殊移動規則 | — |
| 狀態圖 BFS | 例如：推箱子、機器人問題 | — |


# 8️⃣ ZeroJudge 題單

## Level 1
```
a290, d626, a725, c291, c471, d190, c221
```

## Level 2
```
e563, e507, f640, c005, a291
```

## Level 3
```
0/1 BFS、破牆 BFS、SCC、Bridge、Diameter
```

---

※以上資料為chatgpt整理

