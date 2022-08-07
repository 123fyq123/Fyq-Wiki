## 概述

NTT用来加速多项式乘法，最大的优点是可以取模，是FFT取模升级版

---

## 前置知识 

### 阶

若$(a, m) == 1$, 使得$a^d \equiv 1(mod\ m)$的最小正整数d，称为a对模m的阶，记作$\delta_m^a$

### 原根

**概念:**

设$m$为正整数，a为整数，若$\delta_m^a=\phi(m)$，则称a为模m的一个原根，其中$\phi(m)$为m的欧拉函数

**性质:**

若$g$对于$P$是原根，则$g^i\ mod\ p \ne g^j \ mod\ (p为质数,且i\ne j,1\le i,j\le p - 1)$

**求解方法**

原根一般比较小，求原根的方法一般是从2开始枚举，然后暴力判断$g^{P-1} \equiv 1\ (mod\ p)(p 为素数)$

值得一提的是，常见模数998244353 , 1004535809 , 469762049的原根都是3

---

## 例1.挑选队友

<a href="https://ac.nowcoder.com/acm/contest/26013/C">题目链接</a>

![](../../../image/7.png)


**输入样例**

```
5 3 4
1 2 2
```

**输出样例**

```
4
```

### 题解

对于第i个群，对应的生成函数是$F_i(x) = \sum_{j=1}^{s[i]}C_{s_i}^jx^j$

则令$G(x)=\prod_{i=1}^{n} F(x)$

套NTT模板后$[x^k]G(x)$即为答案

### 代码

```c++
#include<bits/stdc++.h>
#define int long long
#define poly vector<int> 
using namespace std;
const int N = 3e5 + 5, p = 998244353, G = 3;
int v;
int lim = 1, RR[N], L;
poly f[N];

int fac[N], infac[N];
int qpow(int a, int n, int mod) {
    int res = 1;
    while(n) {
        if(n & 1) res = res * a % mod;
        a = a * a % mod;
        n >>= 1;
    }
    return res;
}

int invf(int x) {return qpow(x, p - 2, p);}
void init(int n, int mod) {
    infac[0] = fac[0] = 1;
    for(int i = 1; i <= n; i ++ ) fac[i] = fac[i - 1] * i % mod;
    infac[n] = qpow(fac[n], mod - 2, mod);
    for(int j = n; j >= 1; j -- ) {
        infac[j - 1] = infac[j] * j % mod;
    }
}

int C(int a, int b, int mod) {
    if(a < b) return 0;
    return fac[a] * infac[b] % mod * infac[a - b] % mod;
}

void NTT(poly &A,int type)
{
    for(int i=0;i<lim;++i)
    if(i<RR[i])swap(A[i],A[RR[i]]);
    for(int mid=1;mid<lim;mid<<=1)
    {
        int wn=qpow(G,(p-1)/(mid<<1), p);
        if(type==-1)wn=qpow(wn,p-2, p);
        for(int len=mid<<1,pos=0;pos<lim;pos+=len)
        {
            int w=1;
            for(int k=0;k<mid;++k,w=w*wn%p)
            {
                int x=A[pos+k],y=w*A[pos+mid+k]%p;
                A[pos+k]=(x+y)%p;
                A[pos+k+mid]=(x-y-p)%p;
            }
        }
    }
    if(type==-1)
    {
        int inv_n=invf(lim);
        for(int i=0;i<lim;++i)
        A[i]=A[i]*inv_n%p;
    }
}
poly operator *(poly f,poly g)
{
    int deg=f.size()+g.size()-1;
    for(lim=1,L=0;lim<=deg;lim<<=1)L++;
    for(int i=0;i<lim;i++)RR[i]=(RR[i>>1]>>1)|((i&1)<<L-1);
    f.resize(lim);g.resize(lim);
    NTT(f,1);NTT(g,1);
    for(int i=0;i<lim;i++)f[i]=f[i]*g[i]%p;
    NTT(f,-1);f.resize(deg);
    return f;
}
poly dfs(int l,int r)
{
    if(l==r)return f[l];
    int mid=l+r>>1;
    return dfs(l,mid)*dfs(mid+1,r);
}

signed main() {
    int n, m, k;
    scanf("%lld%lld%lld", &n, &m, &k);
    init(1e5 + 5, p);
    for(int i = 1; i <= m; i ++ ) {
        scanf("%lld", &v);
        f[i].resize(v + 1);
        for(int j = 1; j <= v; j ++ ) {
            f[i][j] = C(v, j, p);
        }
    }
    
    poly res = dfs(1, m);
    cout << (res[k] % p + p) % p << '\n';
    return 0;
}
```

---

## 例2.tokitsukaze and Another Protoss and Zerg

<a href="https://ac.nowcoder.com/acm/contest/26013/D">题目链接</a>


![](../../../image/8.png)

**输入输出描述**

![](../../../image/9.png)

### 题解

对于第i场:

+   不选星灵:有$2^{b[i]}-1$种选法
+   不选虫族:则对于选j个星灵有$C_{a[i]}^j$种选法

则第i场的生成函数为

$F_i(x)=2^{b[i]}-1+\sum_{j=1}{a[i]}C_{a[i],j}x^j$

令$G(x)=\prod_{i=1}^{n}F_i(x)$

套NTT板子即可求出每一项

### 代码

```c++
#include<bits/stdc++.h>
#define int long long
#define poly vector<int> 
using namespace std;
const int N = 2e5 + 5, p = 998244353, G = 3;
poly f[N];
int lim = 1, L;
int RR[N];

int a[N], b[N];
int fac[N], infac[N];

int qpow(int a, int n, int p) {
    int res = 1;
    while(n) {
        if(n & 1) res = res * a % p;
        a = a * a % p;
        n >>= 1;
    }
    return res;
}

int invf(int x) {
    return qpow(x, p - 2, p);
}

void init(int n) {
    fac[0] = infac[0] = 1;
    for(int i = 1; i <= n; i ++ ) fac[i] = fac[i - 1] * i % p;
    infac[n] = qpow(fac[n], p - 2, p);
    for(int i = n; i >= 1; i -- ) infac[i - 1] = infac[i] * i % p;
}

int C(int a, int b) {
    if(a < b) return 0;
    return fac[a] * infac[a - b] % p * infac[b] % p;
}

void NTT(poly &A,int type)
{
    for(int i=0;i<lim;++i)
    if(i<RR[i])swap(A[i],A[RR[i]]);
    for(int mid=1;mid<lim;mid<<=1)
    {
        int wn=qpow(G,(p-1)/(mid<<1), p);
        if(type==-1)wn=qpow(wn,p-2, p);
        for(int len=mid<<1,pos=0;pos<lim;pos+=len)
        {
            int w=1;
            for(int k=0;k<mid;++k,w=w*wn%p)
            {
                int x=A[pos+k],y=w*A[pos+mid+k]%p;
                A[pos+k]=(x+y)%p;
                A[pos+k+mid]=(x-y-p)%p;
            }
        }
    }
    if(type==-1)
    {
        int inv_n=invf(lim);
        for(int i=0;i<lim;++i)
        A[i]=A[i]*inv_n%p;
    }
}

poly operator *(poly f,poly g)
{
    int deg=f.size()+g.size()-1;
    for(lim=1,L=0;lim<=deg;lim<<=1)L++;
    for(int i=0;i<lim;i++)RR[i]=(RR[i>>1]>>1)|((i&1)<<L-1);
    f.resize(lim);g.resize(lim);
    NTT(f,1);NTT(g,1);
    for(int i=0;i<lim;i++)f[i]=f[i]*g[i]%p;
    NTT(f,-1);f.resize(deg);
    return f;
}

poly dfs(int l,int r)
{
    if(l==r)return f[l];
    int mid=l+r>>1;
    return dfs(l,mid)*dfs(mid+1,r);
}

signed main() {
    int n;
    cin >> n;
    int sum = 0;
    init(2e5 + 1);
    for(int i = 1; i <= n; i ++ ) { 
        scanf("%lld", &a[i]);
        sum += a[i];
    }
    sum ++ ;
    for(int i = 1; i <= n; i ++ ) scanf("%lld", &b[i]);
    for(int i = 1; i <= n; i ++ ) {
        f[i].resize(a[i] + 2);
        for(int j = 0; j <= a[i]; j ++ ) {
            if(j == 0) f[i][j] = ((qpow(2, b[i], p) - 1) % p + p) % p;
            else f[i][j] = C(a[i], j);
        }
    }
    
    poly res = dfs(1, n);
    for(int i = 0; i < sum; i ++ ) {
        if(i != sum) printf("%lld ", (res[i] % p + p) % p);
        else printf("%lld", (res[i] % p + p) % p);
    }
    return 0;
}
```

---

## 例3.Count Set

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=7191">题目链接</a>

**题目描述**

给定一个排列$p\{1,2,3,..,n\}$和一个非负整数k，要求计算$p$的子集$T$的数量满足以下性质:

+   $|T|=k$
+   $|P(T) \cap T|=0$

注:$P(T)$表示$P(T)=\{y|y=p_x,x\in T\}$


**输入格式**

$T$组输入

每组输入第一行包含两个正整数$n,k$

第二行包含n个整数$P1,P2...Pn$表示给定的排列

**输出格式**

输出一个正整数表示答案对$998244353$取余的结果

**数据范围**

$1\le T \le 15$

$1 \le n \le 5\times 10^5,0\le k \le n$

所有的样例保证$\sum n \le 5 \times 10^6$


**输入样例**

```
3
5 1
5 3 2 1 4
5 2
2 5 1 3 4
10 3
10 9 3 8 6 4 5 7 2 1
```

**输出样例**

```
5
5
40
```

### 题解

我们发现第$i$个数与第$p[i]$个数显然不能同时选择，若将所有的限制条件建图，我们会发现这样形成了一个一个的环(单个点为自环)，上述限制条件也被转化成每个环中不能选择相邻的两个点

要求在所有的环中选$k$个点的方案数，我们可以先考虑他的子问题

如何求在一个大小为m的环种选$k$个点的方案数？

我们发现限制条件是相邻，若将选出的点占用两个位置，就可以保证选出的这些点两两不相邻，注意到是环，破环成链后我们对断开处分两种情况讨论

+   若断开处不是选中的点:此时相当于在$m-k$个相同物体中选$k$个,答案为$C_{m-k}^k$
+   若断开处是选中的点:此时相当于在$m-2-(k-1)$个相同物体中选$k-1$个,答案为$C_{m-k-1}^{k-1}$

因此可以构造出每个环对应的生成函数

$F_i(x)=\sum_{j=1}^{size[i]}(C_{size[i]-j-1}^{j-1}+C_{size[i]-j}^j)x^j$

累乘后得到$G(x)=\prod_{i=1}^{cnt}F_i(x)$

则$[x^k]G(x)$即为答案

分治NTT加速即可

### 代码

```c++
#include <bits/stdc++.h>

#define fp(i, a, b) for (int i = (a), i##_ = (b) + 1; i < i##_; ++i)
#define fd(i, a, b) for (int i = (a), i##_ = (b) - 1; i > i##_; --i)
#define file(s) freopen(s".in","r",stdin),freopen(s".out","w",stdout)
using namespace std;
const int N = 5e5 + 5, P = 998244353;
using arr = int[N];
using ll = int64_t;
/*---------------------------------------------------------------------------*/
class Cipolla {
    int P, I2{};
    using pll = pair<ll, ll>;
#define X first
#define Y second
    ll mul(ll a, ll b) const { return a * b % P; }
    pll mul(pll a, pll b) const { return {(a.X * b.X + I2 * a.Y % P * b.Y) % P, (a.X * b.Y + a.Y * b.X) % P}; }
    template<class T> T POW(T a, int b, T x) { for (; b; b >>= 1, a = mul(a, a)) if (b & 1) x = mul(x, a); return x; }
public:
    Cipolla(int p = 0) : P(p) {}
    pair<int, int> sqrt(int n) {
        int a = rand(), x;
        if (!(n %= P))return {0, 0};
        if (POW(n, (P - 1) >> 1, 1) == P - 1) return {-1, -1};
        while (POW(I2 = ((ll) a * a - n + P) % P, (P - 1) >> 1, 1) == 1) a = rand();
        x = (int) POW(pll{a, 1}, (P + 1) >> 1, {1, 0}).X;
        if (2 * x > P) x = P - x;
        return {x, P - x};
    }
#undef X
#undef Y
};
/*---------------------------------------------------------------------------*/
#define ADD(a, b) (((a) += (b)) >= P ? (a) -=P : 0) // (a += b) %= P
#define SUB(a, b) (((a) -= (b)) < 0 ? (a) += P: 0)  // ((a -= b) += P) %= P
#define MUL(a, b) ((ll) (a) * (b) % P)
//vector<int> getInv(int L) {
//    vector<int> inv(L); inv[1] = 1;
//    fp(i, 1, L - 1) inv[i] = MUL((P - P / i), inv[P % i]);
//    return inv;
//}
//auto inv = getInv(N); // NOLINT
int POW(ll a, int b = P - 2, ll x = 1) { for (; b; b >>= 1, a = a * a % P) if (b & 1) x = x * a % P; return x; }
//int INV(int a) { return a < N ? inv[a] : POW(a); }
namespace NTT {
    const int g = 3;
    vector<int> Omega(int L) {
        int wn = POW(g, P / L);
        vector<int> w(L); w[L >> 1] = 1;
        fp(i, L / 2 + 1, L - 1) w[i] = MUL(w[i - 1], wn);
        fd(i, L / 2 - 1, 1) w[i] = w[i << 1];
        return w;
    }
    auto W = Omega(1 << 21); // NOLINT
    void DIF(int *a, int n) {
        for (int k = n >> 1; k; k >>= 1)
            for (int i = 0, y; i < n; i += k << 1)
                fp(j, 0, k - 1)
                    y = a[i + j + k], a[i + j + k] = MUL(a[i + j] - y + P, W[k + j]), ADD(a[i + j], y);
    }
    void IDIT(int *a, int n) {
        for (int k = 1; k < n; k <<= 1)
            for (int i = 0, x, y; i < n; i += k << 1)
                fp(j, 0, k - 1)
                    x = a[i + j], y = MUL(a[i + j + k], W[k + j]),
                    a[i + j + k] = x - y < 0 ? x - y + P : x - y, ADD(a[i + j], y);
        int Inv = P - (P - 1) / n;
        fp(i, 0, n - 1) a[i] = MUL(a[i], Inv);
        reverse(a + 1, a + n);
    }
}
namespace Polynomial {
    using Poly = std::vector<int>;

    // mul/div int
    Poly &operator*=(Poly &a, int b) { for (auto &x : a) x = MUL(x, b); return a; }
    Poly operator*(Poly a, int b) { return a *= b; }
    Poly operator*(int a, Poly b) { return b * a; }
    Poly &operator/=(Poly &a, int b) { return a *= POW(b); }
    Poly operator/(Poly a, int b) { return a /= b; }

    // Poly add/sub
    Poly &operator+=(Poly &a, Poly b) {
        a.resize(max(a.size(), b.size()));
        fp(i, 0, b.size() - 1) ADD(a[i], b[i]);
        return a;
    }
    Poly operator+(Poly a, Poly b) { return a += b; }
    Poly &operator-=(Poly &a, Poly b) {
        a.resize(max(a.size(), b.size()));
        fp(i, 0, b.size() - 1) SUB(a[i], b[i]);
        return a;
    }
    Poly operator-(Poly a, Poly b) { return a -= b; }

    // Poly mul
    void DFT(Poly &a) { NTT::DIF(a.data(), a.size()); }
    void IDFT(Poly &a) { NTT::IDIT(a.data(), a.size()); }
    int norm(int n) { return 1 << (32 - __builtin_clz(n - 1)); }
    void norm(Poly &a) { if (!a.empty()) a.resize(norm(a.size()), 0); }
    Poly &dot(Poly &a, Poly &b) {
        fp(i, 0, a.size() - 1) a[i] = MUL(a[i], b[i]);
        return a;
    }
    Poly operator*(Poly a, Poly b) {
        int n = a.size() + b.size() - 1, L = norm(n);
        if (a.size() <= 8 || b.size() <= 8) {
            Poly c(n);
            fp(i, 0, a.size() - 1) fp(j, 0, b.size() - 1)
                c[i + j] = (c[i + j] + (ll) a[i] * b[j]) % P;
            return c;
        }
        a.resize(L), b.resize(L);
        DFT(a), DFT(b), dot(a, b), IDFT(a);
        return a.resize(n), a;
    }

    // Poly inv
    Poly Inv2k(Poly a) { // a.size() = 2^k
        int n = a.size(), m = n >> 1;
        if (n == 1) return {POW(a[0])};
        Poly b = Inv2k(Poly(a.begin(), a.begin() + m)), c = b;
        b.resize(n), DFT(a), DFT(b), dot(a, b), IDFT(a);
        fp(i, 0, n - 1) a[i] = i < m ? 0 : P - a[i];
        DFT(a), dot(a, b), IDFT(a);
        return move(c.begin(), c.end(), a.begin()), a;
    }
    Poly Inv(Poly a) {
        int n = a.size();
        norm(a), a = Inv2k(a);
        return a.resize(n), a;
    }

    // Poly div/mod
    Poly operator/(Poly a,Poly b){
        int k = a.size() - b.size() + 1;
        if (k < 0) return {0};
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());
        b.resize(k), a = a * Inv(b);
        a.resize(k), reverse(a.begin(), a.end());
        return a;
    }

    pair<Poly, Poly> operator%(Poly a, const Poly& b) {
        Poly c = a / b;
        a -= b * c, a.resize(b.size() - 1);
        return {c, a};
    }

    // Poly sqrt
    Poly Sqrt(Poly a) {
        int n = a.size(), k = norm(n);
        Poly b = {(new Cipolla(P))->sqrt(a[0]).first}, c;
        a.resize(k * 2, 0);
        for (int L = 2; L <= k; L <<= 1) {
            b.resize(2 * L, 0), c = Poly(a.begin(), a.begin() + L) * Inv(b);
            fp(i, 0, 2 * L - 1) b[i] = MUL(b[i] + c[i], (P + 1) / 2);
        }
        return b.resize(n), b;
    }

    // Poly calculus
    void Derivative(Poly &a) {
        fp(i, 1, a.size() - 1) a[i - 1] = MUL(i, a[i]);
        a.pop_back();
    }
}
using namespace Polynomial;
long long fac[N + 5], invfac[N + 5];
long long th[N + 5];
long long C(int n,int m)
{
    if(n<0 || n<m)
        return 0;
    else
        return fac[n] * invfac[m] % P * invfac[n - m] % P;
}
long long a[N + 5], b[N + 5];
Poly p[N + 5];
bool vis[N + 5];
int num[N + 5];
int cnt = 0;
int main()
{
    th[0] = 1;
    for (int i = 1; i <= N;i++)
        th[i] = th[i - 1] * 2 % P;
    fac[0] = fac[1] = 1;
    for (int i = 2; i <= N;i++)
        fac[i] = fac[i - 1] * i % P;
    invfac[N] = POW(fac[N]);
    for (int i = N - 1; i >= 1;i--)
        invfac[i] = invfac[i + 1] * (i + 1) % P;
    invfac[0] = invfac[1] = 1;
    int n, m = 0;
    int ttt;
    scanf("%d", &ttt);
    while(ttt -- ) {
        int k;
        scanf("%d%d", &n, &k);
        for(int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

        memset(vis, false, sizeof(vis));
        cnt = 0;
        for(int i = 1; i <= n; i ++ ) {
            if(!vis[i]) {
                vis[i] = true;
                num[cnt] = 1;
                long long t = a[i];
                while(!vis[t]) {
                    vis[t] = true;
                    num[cnt] ++ ;
                    t = a[t];

                }
                if(num[cnt] > 1) cnt ++ ;
            }
        }

        for(int i = 0; i < cnt; i ++ ) {
            long long sz = num[i];
            p[i].resize(sz + 1);
            for(long long j = 0; j <= sz; j ++ ) {
                p[i][j] = (C(sz - j - 1LL, j - 1LL) + C(sz - j, j)) % P; // p[i]表示i的生成函数
            }
        }

        function<Poly(int, int)> calc = [&](int L, int R)
        {
            if (L == R)
                return p[L];
            int mid = (L + R) >> 1;
            return calc(L, mid) * calc(mid + 1, R);
        };
        auto now = calc(0, cnt - 1);
        printf("%d\n", now[k]);
    }
    return 0;
}
```