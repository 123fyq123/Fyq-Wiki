## 概述

主要用于解决$a^n=b(mod \ p)$ 问题，求解最小非负整数n，使其满足左式，普通BSGS求解$a$与$p$互质的情况，扩展BSGS可求解不互质情况

---

## 普通BSGS

### 解题步骤

1.取$m=ceil(sqrt(p))$(向上取整)，若解存在，可令$n=i*m-j$，即数对$(i,j)$存在

2.$a^n = b ( mod \ p ) => a^{i*m-j} = b( mod \ p ) -> a_{i*m} = b*aj ( mod \ p )$

3.在$0-m$范围内枚举$b*aj$的值，并用hash表存储$j$

4.在$1-m$范围内枚举$a_i*m$的值，在hash表中查找，若找到，则输出$i*m-j$，反之无答案

### 代码模板

```cpp
#include<bits/stdc++.h>  
#define int long long  
using namespace std;  
int a,p,b;  
unordered_map<int,int> mp;  
int qpow(int a,int n,int mod)  
{  
    int ans=1;  
    while(n)  
	    {  
	        if(n&1) ans=ans*a%mod;  
	        a=a*a%mod;  
	        n>>=1;  
	    }  
	    return ans;  
	}  
int bsgs(int a,int b,int p)  
{  
    a%=p;  
    b%=p;  
    if(1==b%p) return 0;  //特判结果为0的情况
    int m=ceil(sqrt(p));  
    int baj=1;  
    for(int i=0;i<=m;i++)  
    {  
        if(i==0)  
        {  
            baj=b%p;  
            mp[baj]=i;  
            continue;  
        }  
        baj*=a;  
        baj%=p;  
        mp[baj]=i;  
    }  
    int am=qpow(a,m,p);  
    int tmp=1;  
    for(int i=1;i<=m;i++)//由于假设n为i*m-j，若i从0开始枚举则有可能输数  
    {  
        tmp*=am;  
        tmp%=p;  
        if(mp.count(tmp)) return i*m-mp[tmp];  
    }  
    return -1;  
}  
signed main()  
{  
    cin.tie(0);  
    cout.tie(0);  
    ios::sync_with_stdio(0);  
    while(cin>>a>>p>>b)  
    {  
        mp.clear();  
        if(a==0&&p==0&&b==0) break;  
        int ans=bsgs(a,b,p);  
        if(ans==-1) cout<<"No Solution"<<endl;  
        else cout<<ans<<endl;  
    }  
    return 0;  
}  
```

---

## 扩展BSGS

### 解题步骤

1.当$a$与$p$互质时，套用普通BSGS即可

2.当$a$与$p$不互质时，设$gcd$为$a$与$p$的最大公约数，对于方程$a^n=b ( mod \ p ) $，一定有 $a/gcd * a^{n-1} = b/gcd ( mod\  p/gcd )$存在，因此若$b$不为$gcd$的倍数，则方程无解

3.对于新方程 令 $b/ gcd /( a / gcd) = b’$, $p/gcd = p’$ ,  用逆元处理$b’$ , 递归求解即可

### 代码模板

```cpp
#include<bits/stdc++.h>  
using namespace std;  
#define int long long  
const int inf=1e8;  
int a,p,b,x,y;  
int qpow(int a,int n,int mod)  
{  
    int ans=1;  
    while(n)  
    {  
        if(n&1) ans=ans*a%mod;  
        a=a*a%mod;  
        n>>=1;  
    }  
    return ans;  
}  
int exgcd(int a,int b,int &x,int &y)  
{  
    if(!b)  
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
unordered_map<int,int> mp;  
int bsgs(int a,int b,int p)  
{  
    mp.clear();  
    a=a%p;  
    //b=(b%p+p)%p;  
    if(1%p==b%p) return 0;//特判结果为0的情况  
    int m=ceil(sqrt(p));  
    int baj=1;  
    for(int i=0;i<=m;i++)  
    {  
        if(i==0)  
        {  
            baj=b%p;  
            mp[baj]=i;  
            continue;  
        }  
        baj*=a;  
        baj%=p;  
        mp[baj]=i;  
    }  
    int am=qpow(a,m,p);  
    int tmp=1;  
    for(int i=1;i<=m;i++)  
    {  
        tmp*=am;  
        tmp%=p;  
        if(mp.count(tmp)) return i*m-mp[tmp];  
    }  
    return -1;  
}  
int exbsgs(int a,int b,int p)  
{  
    b=(b%p+p)%p;  //令b为正
    if(1%p==b%p) return 0;  
    int gcd=exgcd(a,p,x,y);  
    if(gcd>1)  
    {  
        if(b%gcd) return -inf;  
        exgcd(a/gcd,p/gcd,x,y);  
        int a2=a;  
        int b2=b/gcd*x%(p/gcd);  
        int p2=p/gcd;  
        return exbsgs(a2,b2,p2)+1;  
    }  
    return bsgs(a,b,p);  
}  
signed main()  
{  
    ios::sync_with_stdio(0);  
    while(cin>>a>>p>>b)  
    {  
        if(!a&&!b&&!p) break;  
        int ans=exbsgs(a,b,p);  
        if(ans<0) cout<<"No Solution"<<endl;  
        else cout<<ans<<endl;  
    }  
    return 0;  
}  
```

---

## 例1.洛谷P4861 按钮

[题目入口](https://www.luogu.com.cn/problem/P4861)

**题目描述**

Ada被关在了一个房间里。房间的铁门上有一个按钮，还有一个显示屏显示着“1”。
旁边还有一行小字：“这是一个高精度M进制计算器，每按一次按钮，屏幕上的数便会乘以K。当个位数再次变为1时，门就开了。”
由于Ada急于出去，所以你要在1s之内求出她的最小按键次数。

**输入格式**

一行，两个整数m和k
输出格式
一行一个数字，表示最小按键次数。
如果无论Ada按多少次都无法让门打开，输出"Let's go Blue Jays!"（不含引号）。

**输入样例1**
```
11 2
```
**输出样例1**
```
10
```
**输入样例2**
```
6 26
```
**输出样例2**
```
Let's go Blue Jays!
```

### 题解

若最后答案存在，则有两种表达方式，$k^n$和$m*x+1$，于是可以列出方程，$k^n = 1 ( mod\ m )$ ,套用由于答案不能为$0$，于是特判一下原模板中输出$0$的情况即可

### 代码

```cpp
#include<bits/stdc++.h>  
using namespace std;  
#define int long long  
int m,k;  
unordered_map<int,int> mp;  
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
int qpow(int a,int n,int mod)  
{  
    int ans=1;  
    while(n)  
    {  
        if(n&1) ans=ans%mod*a%mod;  
        a=a%mod*a%mod;  
        n>>=1;  
    }  
    return ans;  
}  
int bsgs(int a,int b,int p)  
{  
    a%=p;  
    b%=p;  
    int m=ceil(sqrt(p));  
    int tmp;  
    for(int i=0;i<=m;i++)  
    {  
        if(i==0)  
        {  
            tmp=b%p;  
            mp[tmp]=i;  
            continue;  
        }  
        tmp*=a;  
        tmp%=p;  
        mp[tmp]=i;  
    }  
    int am=qpow(a,m,p);  
    int now=1;  
    for(int i=1;i<=m;i++)  
    {  
        now*=am;  
        now%=p;  
        //cout<<"now="<<now<<endl;  
        if(mp.count(now)&&i*m-mp[now]==0) continue;  //特判结果为0的
        else if(mp.count(now)) return i*m-mp[now];  
    }  
    return -1;  
}  
int x,y;  
signed main()  
{  
    cin>>m>>k;  
    int gcd=exgcd(m,k,x,y);  
    if(gcd!=1)  
    {  
        cout<<"Let's go Blue Jays!"<<endl;  
        return 0;  
    }  
    int ans=bsgs(k,1,m);  
    if(ans==-1) cout<<"Let's go Blue Jays!"<<endl;  
    else cout<<ans<<endl;  
    return 0;  
}  
```

