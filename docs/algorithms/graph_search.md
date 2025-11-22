🌟 搜尋與圖論（BFS / DFS 大全）

搜尋（Search）是所有演算法的核心之一。

無論是迷宮、最短路徑、找環、連通元件、拓樸排序……

都離不開 BFS（廣度優先搜尋） 與 DFS（深度優先搜尋）。

本章節包含：

BFS / DFS 基本介紹

強化版比較表（以表格呈現）

迷宮 BFS 模板（C++ / Python）

找環教學（ASCII 圖示）

BFS / DFS 題型大全（30 題）

ZeroJudge 題單

1️⃣ 圖的基本概念

一張圖（Graph）由：

節點（Vertices / Nodes）

邊（Edges）

常用表示法：

方法	說明

鄰接串列（Adjacency List）	adj[u] 是所有與 u 相鄰的點

邊列表（Edge List）	用 pairs 存 (u, v)

鄰接矩陣（Adjacency Matrix）	g[u][v] = 1 表示 u→v 有邊

<br>

2️⃣ BFS — Breadth-First Search（廣度優先搜尋）

概念：

像水波一層一層擴散。

使用 Queue（佇列）

能找 最短路徑（步數最少）

在迷宮題裡是最常用的算法

BFS — C++ 樣板

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

BFS — Python 樣板

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

<br>

3️⃣ DFS — Depth-First Search（深度優先搜尋）

概念：

沿著一條路徑一直往深處走到底，再回頭換路。

用 遞迴（Recursion） 或 Stack

適合找「整塊區域」

常用在：

連通元件

找環

拓樸排序

DFS — C++ 樣板

vector<int> adj[MAXN];

bool visited[MAXN];

void dfs(int u) {

    visited[u] = true;
    
    for (int v : adj[u]) {
    
        if (!visited[v]) dfs(v);
    }
}

DFS — Python 樣板

def dfs(u, adj, visited):

    visited[u] = True
    
    for v in adj[u]:
    
        if not visited[v]:
        
            dfs(v, adj, visited)

<br>

4️⃣ BFS vs DFS 強化比較表（重要）

下表比較 BFS 與 DFS 的全部面向。

符合你要求：表格後「空一列」。

功能 / 面向	BFS	DFS

搜尋方式	層級擴散	一路深入

資料結構	Queue	Recursion / Stack

是否能找到最短路徑	✅ 一定可以	❌ 不一定

判斷連通性	可以	⭐ 最常用

找環（Cycle）	可（需記錄 parent）	⭐ 最常用

拓樸排序	✔（Kahn）	✔（DFS Post-order）

記憶體負擔	層級大會大量佔用記憶體	遞迴深會 stack overflow

走訪順序	Level-order	Depth-order

適合題型	迷宮、最短路徑	數區塊、找環

複雜度	O(V+E)	O(V+E)

<br>

5️⃣ 迷宮 BFS（最常見題型）

2D 迷宮示例：

S.#..
..#.E
.#...

迷宮 BFS（C++）

int R, C;

vector<string> grid;

int dist[1005][1005];

int dr[4] = {-1, 0, 1, 0};

int dc[4] = {0, 1, 0, -1};

queue<pair<int,int>> q;

...

// dist 初始化

for (int i=0; i<R; i++)

    for (int j=0; j<C; j++)
    
        dist[i][j] = -1;

q.push({sr, sc});

dist[sr][sc] = 0;

while (!q.empty()) {

    auto [r, c] = q.front(); q.pop();
    
    for (int k=0; k<4; k++) {
    
        int nr = r + dr[k], nc = c + dc[k];
        
        if (nr<0 || nr>=R || nc<0 || nc>=C) continue;
        
        if (grid[nr][nc] == '#') continue;
        
        if (dist[nr][nc] != -1) continue;

        dist[nr][nc] = dist[r][c] + 1;
        
        q.push({nr, nc});
    }
}

迷宮 BFS（Python）

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

<br>

6️⃣ 找環（Cycle Detection）教學（含 ASCII 圖示）

🔷 無向圖找環（概念）

例如：

1 -- 2
|    |
4 -- 3


DFS 走到已訪問過的節點且不是 parent，代表發現環。

🔷 無向圖找環（C++）

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

🔷 有向圖找環（需 in_stack）

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

<br>

7️⃣ BFS / DFS 題型大全（30 題）

🟢 Level 1：入門題（1～10）

題型	ZeroJudge
迷宮最短路（BFS）	a290
二維 BFS	d626
可達性	a725
連通元件（DFS）	c291
找環（DFS）	c471
Node 度數	d190
BFS 簡單圖	a224
DFS 可達點	a693
Flood Fill	c221
Level BFS	e507
<br>
🟡 Level 2：中階題（11～20）
題型	ZeroJudge
多源 BFS	e563
grid BFS	f640
騎士走法（BFS）	c005
拓樸排序（Kahn）	a291
DFS Cycle（無向圖）	c471
DFS Cycle（有向圖）	自製
邊數、層數分析	自製
BFS 回溯路徑	自製
二分圖著色	自製
BFS 求最遠點	自製
<br>
🔴 Level 3：挑戰題（21～30）
題型	備註
迷宮可破牆一次（BFS + 狀態）	需要 3D dist
0/1 BFS	Deque
最長路徑（DAG）	DP or DFS
SCC	強連通元件
兩次 BFS 求圖的直徑	樹很常用
割點（Articulation Point）	進階
割邊（Bridge）	Tarjan
BFS + DP 混合題	自製
地圖 DP + DFS	自製
迷宮特殊移動（跳躍、加權）	自製
<br>
8️⃣ ZeroJudge 題單（可直接貼給學生）
🟢 Level 1
a290, d626, a725, c291, c471, d190, c221

🟡 Level 2
e563, e507, f640, c005, a291

🔴 Level 3

（教師可選題）

0/1 BFS、破牆 BFS、SCC、Bridge、Diameter

<br>
🎉 完成！

你現在得到：

✓ BFS/DFS 整合教學
✓ 強化比較表（表格且後面多空行）
✓ 迷宮模板
✓ 找環教學（含 ASCII 圖示）
✓ 題型大全 30 題
✓ ZeroJudge 題單
