## 概述

本章讲解Floyd算法

Floyd算法又称为插点法，是一种利用动态规划的思想寻找给定的加权图中多源点之间最短路径的算法

相比于Digkstra算法思路更加简单，更容易理解，但是效率会明显低很多，可以作为初步学习的一种方法。

---

## 操作步骤

1. 在主函数中创建一个矩阵，存储输入的两点间的距离。
2. 在Floyd函数中，初始化记录最短距离的矩阵和记录中介点的矩阵。初始化之后将主函数的矩阵复制给记录最短距离的矩阵。
3. 用三层循环不断更新最短距离。

---

## 经典问题

### Floyd求最短路

<a href="https://www.acwing.com/problem/content/856/">题目链接</a>

**题目描述**

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，边权可能为负数。

再给定 k 个询问，每个询问包含两个整数 x 和 y，表示查询从点 x 到点 y 的最短距离，如果路径不存在，则输出 `impossible`。

数据保证图中不存在负权回路。

**输入格式**

第一行包含三个整数 n,m,k。

接下来 m 行，每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

接下来 k 行，每行包含两个整数 x,y，表示询问点 x 到点 y 的最短距离。

**输出格式**

共 k 行，每行输出一个整数，表示询问的结果，若询问两点间不存在路径，则输出 `impossible`。

**数据范围**

$1≤n≤200,$

$1≤k≤n^2$

$1≤m≤20000,$

图中涉及边长绝对值均不超过 10000。

**输入样例**

```
3 3 2
1 2 1
2 3 2
1 3 1
2 1
1 3
```

**输出样例**

```
impossible
1
```

### 题解

Floyd模板提

### 代码

```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int N = 205;
int dis[N][N];
int n, m;
void floyd() {
    for(int i = 1; i <= n; i ++ ) {
        for(int j = 1; j <= n; j ++ ) {
            for(int k = 1; k <= n; k ++ ) {
                dis[j][k] = min(dis[j][k], dis[j][i] + dis[i][k]);
            }
        }
    }
}
signed main() {
    cin >> n >> m;
    int k;
    cin >> k;
    for(int i = 1; i <= n; i ++ ) {
        for(int j = 1; j <= n; j ++ ) {
            if(i == j) dis[i][j] = 0;
            else dis[i][j] = 0x3f3f3f3f;
        }
    }
    while(m -- ) {
        int a, b, c;
        cin >> a >> b >> c;
        dis[a][b] = min(dis[a][b], c);
    }
    floyd();
    while(k -- ) {
        int a, b;
        cin >> a >> b;
        if(dis[a][b] >= 0x3f3f3f3f / 2) {
            cout << "impossible" << '\n';
        } else {
            cout << dis[a][b] << '\n';
        }
    }
    return 0;
}
```