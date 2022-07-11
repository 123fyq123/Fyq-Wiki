## 概述
用于求解模数互质的线性同余方程组

---

## 解题方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/b6de6db0cc1f45a7888810861add8747.png)

---

## 代码模板

```cpp
int exgcd(int a,int b,int &x,int &y)  
{  
    if(b==0)  
    {  
        x=1;  
        y=0;  
        return a;  
    }  
    int ans=exgcd(b,a%b,x,y);  
    int tmp=x;  
    x=y;  
    y=tmp-(a/b)*y;  
    return ans;  
}  
int crt(int n)  
{  
    int ans=0;  
    for(int i=1;i<=n;i++)  
    {  
        int x,y;  
        exgcd(MOD/mod[i],mod[i],x,y);  
        ans=(ans+MOD/mod[i]*x*yu[i])%MOD;  
    }  
    return (ans%MOD+MOD)%MOD;  
}  
```

---

## 扩展中国剩余定理
### 概述
用于求解模数不互质的线性同余方程组
### 解题方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/454150fae07f483b9df0d00521f4cdfc.png)
### 代码模板

```cpp
#include<bits/stdc++.h>  
#define int long long  
using namespace std;  
const int N=1e5+5;  
int x,y;  
int n;  
int mod[N],yu[N];  
int qmul(int a,int n,int mod)  
{  
    int res=0;  
    while(n)  
    {  
        if(n&1) res=(res+a)%mod;  
        a=(a+a)%mod;  
        n>>=1;  
    }  
    return res;  
}  
int exgcd(int a,int b,int &x,int &y)  
{  
    if(b==0)  
    {  
        x=1;  
        y=0;  
        return a;  
    }  
    int ans=exgcd(b,a%b,x,y);  
    int tmp=x;  
    x=y;  
    y=tmp-(a/b)*y;  
    return ans;  
}  
int ex_crt(int* mod,int *yu)  
{  
    int ans=yu[1];  
    int m=mod[1];  
    for(int i=2;i<=n;i++)  
    {  
        int tmp=(yu[i]%mod[i]-ans%mod[i]+mod[i])%mod[i];//yu[i]-x=t*m(mod mod[i]) 求t
        int gcd=exgcd(m,mod[i],x,y);  
        if(tmp%gcd) return -1;  
        int times=tmp/gcd;  
        int bg=mod[i]/gcd;  
        x=qmul(x,times,bg);//这里乘法可能会溢出，所以写个快速加法  
        ans=ans+m*x;  
        m=m/gcd*mod[i];  
        ans=(ans%m+m)%m;  
    }  
    return (ans%m+m)%m;  
}  
signed main()  
{  
    cin>>n;  
    for(int i=1;i<=n;i++) cin>>mod[i]>>yu[i];  
    cout<<ex_crt(mod,yu)<<endl;  
    return 0;  
}  
```

---

## 经典例题
### 例1.洛谷P1495 中国剩余定理
**题目描述**

自从曹冲搞定了大象以后，曹操就开始捉摸让儿子干些事业，于是派他到中原养猪场养猪，可是曹冲满不高兴，于是在工作中马马虎虎，有一次曹操想知道母猪的数量，于是曹冲想狠狠耍曹操一把。举个例子，假如有 1616 头母猪，如果建了 33 个猪圈，剩下 11 头猪就没有地方安家了。如果建造了 55 个猪圈，但是仍然有 11 头猪没有地方去，然后如果建造了 77 个猪圈，还有 22 头没有地方去。你作为曹总的私人秘书理所当然要将准确的猪数报给曹总，你该怎么办？

**输入格式**

第一行包含一个整数n为建立猪圈的次数，接下来n行，每行两个整数ai，bi表示建立了ai个猪圈，有bi头猪没有去处，规定ai，aj互质

**输出格式**

输出包含一个正整数，即为曹冲至少养母猪的数目。

**输入样例**
```
3
3 1
5 1
7 2
```
**输出样例**
```
16
```
**代码：**

```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
ll mod[15];  
ll yu[15];  
ll MOD=1;  
ll exgcd(ll a,ll b,ll &x,ll &y)  
{  
    if(b==0)  
    {  
        x=1;  
        y=0;  
        return a;  
    }  
    ll ans=exgcd(b,a%b,x,y);  
    ll tmp=x;  
    x=y;  
    y=tmp-(a/b)*y;  
    return ans;  
}  
ll crt(int n)  
{  
    ll ans=0;  
    for(int i=1;i<=n;i++)  
    {  
        ll x,y;  
        exgcd(MOD/mod[i],mod[i],x,y);  
        ans=(ans+MOD/mod[i]*x*yu[i])%MOD;  
    }  
    return ans;  
}  
int main()  
{  
    int n;  
    cin>>n;  
    for(int i=1;i<=n;i++)  
    {  
        cin>>mod[i]>>yu[i];  
    }  
    for(int i=1;i<=n;i++)  MOD*=mod[i];  
    cout<<(crt(n)+MOD)%MOD;  
    return 0;  
}  
```

---

### 例2.HDU3579 Hello Kiki
**题目背景**

One day I was shopping in the supermarket. There was a cashier counting coins seriously when a little kid running and singing "门前大桥下游过一群鸭，快来快来 数一数，二四六七八". And then the cashier put the counted coins back morosely and count again...
Hello Kiki is such a lovely girl that she loves doing counting in a different way. For example, when she is counting X coins, she count them N times. Each time she divide the coins into several same sized groups and write down the group size Mi and the number of the remaining coins Ai on her note.
One day Kiki's father found her note and he wanted to know how much coins Kiki was counting.

**输入格式**

The first line is T indicating the number of test cases.
Each case contains N on the first line, Mi(1 <= i <= N) on the second line, and corresponding Ai(1 <= i <= N) on the third line.
All numbers in the input and output are integers.
1 <= T <= 100, 1 <= N <= 6, 1 <= Mi <= 50, 0 <= Ai < Mi

**输出格式**

For each case output the least positive integer X which Kiki was counting in the sample output format. If there is no solution then output -1.

**输入样例**
```
2
2
14 57
5 56
5
19 54 40 24 80
11 2 36 20 76
```
**输出样例**
```
Case 1: 341
Case 2: 5996
```
**代码：**

```cpp
#include<bits/stdc++.h>  
typedef long long ll;  
using namespace std;  
ll M[10];  
ll A[10];  
ll ex_gcd(ll a,ll b,ll &x,ll &y)  
{  
    if(b==0)  
    {  
        x=1;  
        y=0;  
        return a;  
    }  
    ll ans=ex_gcd(b,a%b,x,y);  
    ll tmp=x;  
    x=y;  
    y=tmp-(a/b)*y;  
    return ans;  
}  
ll ex_crt(ll* yu,ll* mod,ll n)  
{  
    ll ans=yu[1];  
    ll m=mod[1];  
    ll x,y;  
    ll now;  
    for(ll i=2;i<=n;i++)  
    {  
        now=(yu[i]%mod[i]-ans%mod[i]+mod[i])%mod[i];//t*m=ak-x(mod m)  
                                                    //now=ak-x  满足 now<m  
        ll gcd=ex_gcd(m,mod[i],x,y);//扩欧求方程的解  
        if(now%gcd) return -1;//判断t是否存在  
        ll k=now/gcd;  
        x*=k;//求的t'为t'*m=gcd (mod m)的解，需要乘以倍数  
        int s=mod[i]/gcd;  
        x=(x%s+mod[i]/gcd)%(mod[i]/gcd);//x=x+t*m更新x  
        ans=ans+x*m;//  
        m=m*mod[i]/gcd;  
    }  
    return (ans+m)%m?(ans+m)%m:m;  
}  
int main()  
{  
    ll t;  
    cin>>t;  
    for(ll i=1;i<=t;i++)  
    {  
        memset(A,0,sizeof(A));  
        memset(M,0,sizeof(M));  
        ll n;  
        cin>>n;  
        for(ll j=1;j<=n;j++) cin>>M[j];  
        for(ll j=1;j<=n;j++) cin>>A[j];  
        cout<<"Case "<<i<<": "<<ex_crt(A,M,n)<<endl;  
    }  
    return 0;  
}  
```
