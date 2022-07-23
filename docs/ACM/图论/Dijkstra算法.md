## 普通版Dijkstra

引用👉<a href="https://www.cnblogs.com/skywang12345/p/3711512.html#anchor2">skywang12345的博客</a>

迪杰斯特拉(Dijkstra)算法是典型最短路径算法，用于计算一个节点到其他节点的最短路径。
它的主要特点是以起始点为中心向外层层扩展(广度优先搜索思想)，直到扩展到终点为止。

### 原理

 通过Dijkstra计算图G中的最短路径时，需要指定起点s(即从顶点s开始计算)。

 此外，引进两个集合S和U。S的作用是记录已求出最短路径的顶点(以及相应的最短路径长度)，而U则是记录还未求出最短路径的顶点(以及该顶点到起点s的距离)。

 初始时，S中只有起点s；U中是除s之外的顶点，并且U中顶点的路径是"起点s到该顶点的路径"。然后，从U中找出路径最短的顶点，并将其加入到S中；接着，更新U中的顶点和顶点对应的路径。 然后，再从U中找出路径最短的顶点，并将其加入到S中；接着，更新U中的顶点和顶点对应的路径。 ... 重复该操作，直到遍历完所有顶点。

 ### 操作步骤
<ol>
    <li>
    初始时，S只包含起点s；U包含除s外的其他顶点，且U中顶点的距离为"起点s到该顶点的距离"[例如，U中顶点v的距离为(s,v)的长度，然后s和v不相邻，则v的距离为∞]。
    </li>
    <li>
    从U中选出"距离最短的顶点k"，并将顶点k加入到S中；同时，从U中移除顶点k。
    </li>
    <li>
    更新U中各个顶点到起点s的距离。之所以更新U中顶点的距离，是由于上一步中确定了k是求出最短路径的顶点，从而可以利用k来更新其它顶点的距离；例如，(s,v)的距离可能大于(s,k)+(k,v)的距离。
    </li>
    <li>
    重复步骤(2)和(3)，直到遍历完所有顶点。
    </li>
 </ol>

### 经典问题

#### Dijkstra求最短路 I

<a href="https://www.acwing.com/problem/content/851/">题目链接</a>

**题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 −1。

**输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

**输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 −1。

**数据范围**

$1≤n≤500,$

$1≤m≤10^5,$

图中涉及边长均不超过10000。

**输入样例**

```
3 3
1 2 2
2 3 1
1 3 4
```

**输出样例**

```
3
```

#### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 505, M = 1e5 + 5;
int head[N], e[M], w[M], ne[M], idx = 0;
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = head[a];
    head[a] = idx ++ ;
}
int n, m;
int dis[N];
bool vis[N];
void djs(int s) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    for(int i = 1; i <= n; i ++ ) {
        int pos = -1;
        for(int j = 1; j <= n; j ++ ) {
            if(!vis[j] && (pos == -1 || dis[pos] > dis[j])) pos = j;
        }
        if(dis[pos] == 0x3f3f3f3f) {
            cout << "-1" << endl;
            return ;
        }
        vis[pos] = true;
        for(int j = head[pos]; j != -1; j = ne[j]) {
            int t = e[j];
            dis[t] = min(dis[t], dis[pos] + w[j]);
        }
    }
    if(dis[n] == 0x3f3f3f3f) cout << "-1" << endl;
    else cout << dis[n];
}
int main() {
    cin >> n >> m;
    memset(head, -1, sizeof(head));
    for(int i = 1; i <= m; i ++ ) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    djs(1);
    return 0;
}
```

---

## 堆优化版Dijkstra

### 原理

没有优化的迪杰斯特拉算法的时间复杂度为$O(n^2)$。

普通版的迪杰斯特拉算法主要缺陷是：每当找到一个最短路径，如果需要找下一个最短路径，就需要在完成松弛操作之后，遍历dist数组，寻找其中的最小值。遍历dist数组的时间复杂度为$O(n)$。

如果图的边数为n*(n-1)，那么每找到一个最小值，所要进行的松弛操作数就是n-1，这和遍历dist数组可以同时进行，算法优化的空间不大。

然而，如果是稀疏图，每找到一个最小值，所要进行的松弛操作数就远小于n-1，这时就可以对算法进行优化。优化的关键是省去对dist的线性查找，如果每次可以直接返回dist中的最大值，就可以大大减小算法的时间复杂度。

堆优化后的迪杰斯特拉算法复杂度为$O(ElogE)$

### 经典问题

#### Dijkstra求最短路 II

<a href="https://www.acwing.com/problem/content/852/">题目链接</a>

**题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，所有边权均为非负值。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 −1。

**输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

**输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 −1。

**数据范围**

$1≤n,m≤1.5×10^5,$

图中涉及边长均不小于 0，且不超过 10000。

数据保证：如果最短路存在，则最短路的长度不超过 $10^9$。

**输入样例**

```
3 3
1 2 2
2 3 1
1 3 4
```

**输出样例**

```
3
```

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef pair<int, int> PII;
const int N = 1e6 + 5;
int head[N], e[N], w[N], ne[N], idx;
int dis[N];
bool vis[N];
int n, m;
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = head[a];
    head[a] = idx ++ ;
}

void djs(int s) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> q;
    q.push({0, s});
    while(q.size()) {
        auto cur = q.top();
        q.pop();
        int pos = cur.second;
        if(vis[pos]) continue;
        vis[pos] = true;
        for(int i = head[pos]; i != -1; i = ne[i]) {
            int j = e[i];
            if(dis[j] > dis[pos] + w[i]) {
                dis[j] = dis[pos] + w[i];
                q.push({dis[j], j});
            }
        }
    }
    if(dis[n] == 0x3f3f3f3f) cout << "-1";
    else cout << dis[n];
}
int main() {
    cin >> n >> m;
    memset(head, -1, sizeof(head));
    for(int i = 1; i <= m; i ++ ) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    djs(1);
    return 0;
}
```