🌟 搜尋與圖論：BFS / DFS（完整教學版）

搜尋（Search）是所有演算法的核心之一。
無論是迷宮、找路、圖論、樹的遍歷、最短路徑、找環……
都離不開 BFS（廣度優先搜尋） 與 DFS（深度優先搜尋）。

本章將逐步介紹：

BFS / DFS 核心概念

程式模板（C++ / Python）

完整比較表（強化版）

二維迷宮

連通元件

找環（Cycle）

常見題型（含 ZeroJudge / LeetCode）

1️⃣ 什麼是「圖」？

一張圖（Graph）由：

節點 Nodes / Vertices

邊 Edges

常見表示法：

表示方式	說明

鄰接串列 Adjacency List	adj[u] 為所有與 u 相鄰的 v

鄰接矩陣 Adjacency Matrix	g[u][v] = 1 表示 u → v 有邊

2️⃣ BFS — Breadth-First Search

一層一層擴散的搜尋法。

核心特點：

使用 Queue（佇列）

能找到 最短路徑

適合「邊權重相同」的圖

常用在迷宮、最短步數

BFS 模板（C++）

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

3️⃣ DFS — Depth-First Search

沿著一條路徑一直走到底的搜尋法。

核心特點：

使用 Recursion（遞迴） 或 Stack

深度優先，適合「找一整塊」

可用來判斷 是否連通、是否有環、拓樸排序

DFS 模板（C++）

vector<int> adj[MAXN];

bool visited[MAXN];

void dfs(int u) {

    visited[u] = true;
    
    for (int v : adj[u]) {
    
        if (!visited[v]) dfs(v);
        
    }
}

4️⃣ BFS vs DFS 強化比較表（完整版）

以下是強化後、適合教育網站的 高完整度比較表：

功能 / 面向	BFS	DFS

核心概念	逐層擴散	一條路走到底

資料結構	Queue	Recursion / Stack

最短路徑	✅ 必定最短	❌ 不保證

找連通元件	可	⭐ 最常用

找環（Cycle detection）	可（需額外紀錄）	⭐ 常用（parent 技巧）

拓樸排序	可（Kahn's Algorithm）	可（DFS Post-order）

走訪順序	層級（Level-order）	深度（Depth-order）

記憶體使用	層級太大時會爆（Queue 過長）	遞迴太深會 stack overflow

找路徑（Path Reconstruction）	⭐ 常用（parent）	可

適合大圖嗎？	較適合廣度型問題	適合深度搜尋

找有沒有路（Reachability）	✔	✔

找所有可達點	✔	✔

演算法複雜度	O(V + E)	O(V + E)

適用資料結構	Graph / Tree / Grid	Graph / Tree / Grid

2D 迷宮	⭐ 首選（最短步數）	用於是否可達

記憶體負擔	依層級大小而定	依遞迴深度而定

應用類型	最短路徑、層序遍歷、多源 BFS	連通元件、找環、拓樸、樹遍歷

難度（實作）	中（需要 Queue）	簡（遞迴三行）

ZeroJudge 題型	迷宮 BFS、0-1 BFS	DFS 找區塊、找環

5️⃣ 二維迷宮 BFS（最常見題型）

迷宮範例：

S.#..
..#.E
.#...

Python 程式：

from collections import deque

def bfs_maze(grid, sr, sc):

    R, C = len(grid), len(grid[0])
    
    dist = [[-1]*C for _ in range(R)]
    
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

6️⃣ 連通元件（Connected Components）

常見問題：

圖裡有幾塊互相不相連的區域？

C++ 程式碼

int n;

vector<int> adj[MAXN];

bool visited[MAXN];

void dfs(int u) {

    visited[u] = true;
    
    for (int v : adj[u]) {
    
        if (!visited[v])
        
            dfs(v);
    }
}

int count_cc() {

    int cnt = 0;
    
    for (int i=1; i<=n; i++) {
    
        if (!visited[i]) {
        
            cnt++;
            
            dfs(i);
            
        }
    }
    return cnt;
    
}

7️⃣ 無向圖找環（Cycle Detection）

DFS 判斷環（無向圖）

邏輯：

若走到的點 已經 visited

又 不是我來的方向（父節點）
→ 代表有環

C++ 程式

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

8️⃣ 推薦練習題（ZeroJudge / LeetCode）
ZeroJudge

a290 — 迷宮 BFS

d626 — 二維 BFS

c291 — DFS 找連通元件

c471 — 找環

e563 — BFS 多起點擴散

e507 — BFS 層級走訪

LeetCode

200. Number of Islands（DFS）

695. Max Area of Island（DFS）

542. 01 Matrix（多源 BFS）

994. Rotting Oranges（BFS）

207. Course Schedule（拓樸排序）

🏁 小結

想要 最短路徑 → 一律 BFS

想要 是否連通 / 找區塊 → DFS 或 BFS

想要 找環 → DFS（最常用）

想要 層級資訊 → BFS

想要 遍歷整棵樹 → DFS / BFS 都能

搜尋是所有演算法的基礎，
熟練 BFS / DFS 會讓你在 APCS、競賽、ZeroJudge 變得非常強。

