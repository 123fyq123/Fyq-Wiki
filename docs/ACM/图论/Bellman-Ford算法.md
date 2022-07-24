## 概述

 Dilijkstra算法，这个算法解决单源最短路径问题，但是它不能解决带有负权边（边的权值为负数）的图，原因见👉<a href="https://blog.csdn.net/Kprogram/article/details/81220702">Korpse的博客</a>

 而Bellman-Ford算法解决了负环这个问题，同时Bellman-Ford算法擅长解决有边数限制的最短路问题

---

## 操作步骤

1. 初始化所有d[s],源点d[s]=0,其他d[s]=INF

2. 进行n-1次循环，在循环体中遍历所有的边，进行松弛计算（if(d[v]>d[u]+w[u][v]) d[v]=d[u]+w[u][v]）  

3. 遍历图中所有的边，检验是否出现这种情况：d[v]>d[u]+w[u][v],若出现则返回false,没有最短路


---

## 经典问题

### 有边数限制的最短路

 <a href="https://www.acwing.com/problem/content/855/">题目链接</a>

 **题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出从 1 号点到 n 号点的最多经过 k 条边的最短距离，如果无法从 1 号点走到 n 号点，输出 `impossible`。

注意：图中可能 存在负权回路 。

**输入格式**

第一行包含三个整数 n,m,k。

接下来 m 行，每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

点的编号为 1∼n。

**输出格式**

输出一个整数，表示从 1 号点到 n 号点的最多经过 k 条边的最短距离。

如果不存在满足条件的路径，则输出 `impossible`。

**数据范围**

$1≤n,k≤500,$

$1≤m≤10000,$

$1≤x,y≤n，$

任意边长的绝对值不超过 10000。

**输入样例**

```
3 3 1
1 2 1
2 3 1
1 3 3
```

**输出样例**

```
3
```

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;
const int N=1e4+5;

struct node_
{
    int from,to,dis;
}node[N];
int dis[N],last[N];
int n,m,k;

void bm_ford(int s,int t)
{
    memset(dis,0x3f,sizeof(dis));
    dis[s]=0;
    for(int i=1;i<=k;i++)
    {
        memcpy(last,dis,sizeof(dis));
        for(int j=1;j<=m;j++)
        {
            int a=node[j].from;
            int b=node[j].to;
            int c=node[j].dis;
            dis[b]=min(dis[b],last[a]+c);
        }
    }
    if(dis[t]>0x3f3f3f3f/2) cout<<"impossible"<<endl;
    else cout<<dis[t];
}
int main()
{
    cin>>n>>m>>k;
    for(int i=1;i<=m;i++) cin>>node[i].from>>node[i].to>>node[i].dis;
    bm_ford(1,n);
    return 0;
}
```