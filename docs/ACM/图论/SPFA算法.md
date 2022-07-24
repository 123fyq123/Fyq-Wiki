## 概述

本章讲解SPFA算法

解决存在负环的图的单源最短路径，bellman-ford算法是比较经典的一个，(不清楚👉<a href="">Bellman-Ford算法</a>)这个算法的效率并不咋的，因为它只知道要求单源最短路，至多做|v|（j图的结点数）次松弛操作，SPFA很大程度上优化了bellman-ford算法。它的精妙之处在于不是盲目的做松弛操作，而是用一个队列保存当前做了松弛操作的结点，只要队列不空，就可以继续从队列里面取点，做松弛操作。

---

## 操作步骤

1. 初始时将源加入队列
2. 每次从队列中取出一个元素，并对所有与他相邻的点进行松弛
3. 若某个相邻的点松弛成功，如果该点没有在队列中，则将其入队
4. 直到队列为空时算法结束

---

## 经典问题

### spfa求最短路

<a href="https://www.acwing.com/problem/content/853/">题目链接</a>

**题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 impossible。

数据保证不存在负权回路。

**输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

**输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 `impossible`。

**数据范围**

$1≤n,m≤105,$

图中涉及边长绝对值均不超过 10000。

**输入样例**

```
3 3
1 2 5
2 3 -3
1 3 4
```

**输出样例**

```
2
```

### 题解

spfa模板题

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5 + 5;
int head[N], e[N], w[N], ne[N], idx ;
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = head[a];
    head[a] = idx ++ ;
}

int dis[N];
bool vis[N];
queue<int> q;
int n, m;
void spfa(int s) {
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    vis[s] = true;
    q.push(s);
    while(q.size()) {
        int pos = q.front();
        q.pop();
        vis[pos] = false;
        for(int i = head[pos]; i != -1; i = ne[i]) {
            int j = e[i];
            if(dis[j] > dis[pos] + w[i]) {
                dis[j] = dis[pos] + w[i];
                if(!vis[j]) {
                    vis[j] = true;
                    q.push(j);
                }
            }
        }
    }
    if(dis[n] == 0x3f3f3f3f) cout << "impossible" ;
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
    spfa(1);
    return 0;
}
```

---

## 进阶问题

### spfa判断负环

<a href="https://www.acwing.com/activity/content/problem/content/921/">题目链接</a>

**题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你判断图中是否存在负权回路。

**输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

**输出格式**

如果图中存在负权回路，则输出 `Yes`，否则输出 `No`。

**数据范围**

$1≤n≤2000,$

$1≤m≤10000,$

图中涉及边长绝对值均不超过 10000。

**输入样例**

```
3 3
1 2 -1
2 3 4
3 1 -4
```

**输出样例**

```
Yes
```

### 题解

如果不存在负环的话，每个节点可以通过其他节点作为中间节点然后再到达该节点来进行松弛，所以每个节点最多松弛n次，因此只要某个节点进入队列了超过了n次，那必定存在负环，用cnt数组记录松弛次数，判断即可

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 2005, M = 1e4 + 5;
int head[N], e[M], ne[M], w[M], idx = 0;
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = head[a];
    head[a] = idx ++ ;
}
int n, m;
bool vis[N];
int dis[N];
queue<int> q;
int cnt[N];
void spfa() {
    for(int i = 1; i <= n; i ++ ) {
        q.push(i);
        vis[i] = true;
    }
    while(q.size()) {
        int pos = q.front();
        q.pop();
        vis[pos] = false;
        for(int i = head[pos]; i != -1; i = ne[i]) {
            int j = e[i];
            if(dis[j] > dis[pos] + w[i]) {
                dis[j] = dis[pos] + w[i];
                cnt[j] = cnt[pos] + 1;
                if(cnt[j] >= n) {
                    cout << "Yes" << '\n';
                    return ;
                }
                if(!vis[j]) {
                    vis[j] = true;
                    q.push(j);
                }
            }
        }
    }
    cout << "No";
}
int main() {
    cin >> n >> m;
    memset(head, -1, sizeof(head));
    while(m -- ) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();
    return 0;
}
```

---

### Link with Game Glitch

<a href="https://ac.nowcoder.com/acm/contest/33187/D">题目链接</a>

**题目大意**

Link有一个食谱，他可以通过若干个一种食材制作出若干个另一种食材，由于食谱有些错误，可能出现某些食材被无限制造的情况，先加入一个参数w，使每个被制作食材的产生数量变为$w \times 原来可被产生的数量$。

现问你最大的w为多少

**输入格式**

第一行输入两个整数n，m，代表食材种类以及食谱数量

接下来m行每行四个数$a_i,b_i,c_i,d_i$，表示可以用$a_i$个$b_i$型食材造出$c_i$个$d_i$型食材

**输出格式**

输出一个实数w，误差在$10^{-6}$内的答案被认为正确

**数据范围**

$2 \le n \le 1000,2 \le m \le 2000$

$1 \le a_i,b_i,c_i,d_i \le n, b_i \ne d_i, 1 \le a_i,c_i \le 10^3$

**输入样例**

```
3 3
1 1 2 2
1 2 2 1
1 3 1 1
```

**输出样例**

```
0.5000000000
```

### 题解

最大值考虑二分，很容易发现食材之间可以建图，若某种食材能被无限制造，则其必要条件为某种食材在环中，将产生数量的比例作为边权，我们发现若环中长度之积大于1，则会造成资源无限的情况，乘积可能很大，考虑取对数，接下来就变成了判正环问题，资源有限等价于不存在正环，代码与判负环稍有不同，只需要将最短路改成最长路即可

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 2005;
const double eps = 1e-8;
int head[N], e[N], ne[N], idx = 0;
int f[N];
double dis[N];
double w[N];
bool vis[N];
int n, m;
queue<int> q;
void add(int a, int b, double c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = head[a];
    head[a] = idx ++ ;
}
bool check(double x) {
    memset(vis, false, sizeof(vis));
    memset(f, 0, sizeof(f));
    while(q.size()) q.pop();
    for(int i = 1; i <= n ; i ++ ) {
        vis[i] = true;
        dis[i] = 0;
        q.push(i);
    }
    while(q.size()) {
        int pos = q.front();
        q.pop();
        vis[pos] = false;
        for(int i = head[pos]; i != -1; i = ne[i]) {
            int j = e[i];
            if(dis[j] < dis[pos] + w[i] + x) { // 最长路
                dis[j] = dis[pos] + w[i] + x;
                f[j] = f[pos] + 1;
                if(f[j] >= n) return false;
                if(!vis[j]) {
                    q.push(j);
                    vis[j] = true;
                }
            }    
        }
    }
    return true;
}
int main() {
    cin >> n >> m;
    memset(head, -1, sizeof(head));
    for(int i = 1; i <= m; i ++ ) {
        int a, b;
        double x, y;
        cin >> x >> a >> y >> b;
        add(a, b, log(y * 1.0 / x));
    }
    
    double l = 0, r = 1.0;
    while(l < r - eps) { // 这里使用eps可以通过，但有时候需要限制循环次数
        double mid = (l + r) / 2.0;
        if(check(log(mid)))l = mid;
        else r = mid;
    }
    printf("%.8lf", l);
    return 0;
}
```