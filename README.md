# Contents  
- [快速幂](#快速幂)
- [树状数组+差分： 区间修改+单点查询](#树状数组差分-区间修改单点查询)
- [树状数组+差分](#树状数组差分)
- [树状数组：单点修改，区间和查询](#树状数组单点修改区间和查询)

## 快速幂
```cpp
int quickpow(int x,int y,int p)
{
    int n=1;
    while(y!=0)
    {
        if (y&1) n=(n*x)%p;
        x=(x*x)%p;
        y=y>>1;
	}
    return n;
}
```

## 树状数组+差分： 区间修改+单点查询
```cpp

const int MAXN = 5e5 + 10;

int a[MAXN];
int c[MAXN];

int n,m;

void modify(int idx,ll x)
{
    // 从叶子结点一路向上更新
    for (int i = idx;i <= n;i += lowbit(i))
    {
        c[i] += x;
    }
}

ll sum(int idx)
{
    // 查询：由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和
    ll ans = 0;
    for (int i = idx;i > 0;i -= lowbit(i))
    {
        ans += c[i];
    }
    return ans;
}

void pls(int l,int r,int k)
{
    modify(l, k);
    modify(r + 1, -k);
}

void init()
{
    rep(i,1,n) {
        pls(i,i,a[i]);
    }
}

int main()
{
    scii(n,m);
    rep(i,1,n) sci(a[i]);
    init();

    int t;
    int x,y,k;

    while (m --) {
        sci(t);
        if (t == 1) {
            sciii(x,y,k);
            // modify
            pls(x,y,k);
        } else if (t == 2) {
            sci(x);
            // get_ans: sum_up
            printf("%lld\n",sum(x));
        }
    }
    return 0;
}

```


## 树状数组+差分

```cpp
int p[1000] = {0};
int a[1000];
int n;

void pls(int l,int r,int k)
{
    p[l] += k;
    p[r + 1] -= k;
}

void init()
{
    REP(i,0,n) {
        pls(i,i,a[i]);
    }
}

int main()
{
    scanf("%d",&n);

    READ(a,0,n);
    init();

    Tprint(p,0,n);
    printf("\n");

    int q;
    scanf("%d",&q);
    int l,r;
    while (q --) {
        scanf("%d%d",&l,&r);
        pls(l,r,1);
    }

    int s[1000];

    s[0] = p[0];
    rep(i,1,n - 1) {
        s[i] = s[i - 1] + p[i];
    }

    rep(i,0,n - 1) printf("%d ",s[i]);
    return 0;
}

```


## 树状数组：单点修改，区间和查询

```cpp
const int MAXN = 5e5 + 10;

ll a[MAXN];
ll c[MAXN];

int n,m;

void modify(int i,ll x)
{
    // 从叶子结点一路向上更新
    for (;i <= n;i += lowbit(i)) {
        c[i] += x;
    }
}

ll sum(int i)
{
    // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和
    ll ans = 0;
    for (;i > 0;i -= lowbit(i))
    {
        ans += c[i];
    }
    return ans;
}

int main()
{
    scii(n,m);
    rep(i,1,n) scl(a[i]);
    rep(i,1,n) modify(i, a[i]);
    int t,x,y;
    while (m --) {
        sciii(t,x,y);
        if (t == 1) {
            // modify
            modify(x, y);
        } else if (t == 2) {
            // query
            printf("%lld\n",sum(y) - sum(x - 1));
        }
    }
    re0;
}

```

## connected undirected simple graph（无向连通图）
对于任意u、v，都有路径使得u、v连通

# 图论
## 判环



# STL
## rope模板（可持久化平衡树）
```cpp
#include<ext/rope>
using namespace __gnu_cxx;

rope list;
list.insert(p,str)      // 在p的位置插入str
list.erase(p,c)         // 删除list的从p开始的c个节点
list.substr(p,c)        // 提取list的p位置开始的c个节点
list.copy(p,c,str)      // 将list的p位置开始的c个节点复制给str
list.replace(pos,x)     // 从pos开始替换成x
```




# 回文串
## Manacher（马拉车：线性找回文串个数或者最大回文串）

```cpp
#include<cstdio>
#include<cstring>
#include<string>
#include<vector>
#include<iostream>
typedef long long ll;
using namespace std;

void Manacher(string s) {//计算最长回文串 
    // Insert '#'
    string t = "$#";
    for (int i = 0; i < s.size(); ++i) {//这里有时候会卡时间（s.size要提前算出来） 
        t += s[i];
        t += "#";
    }
    // Process t
    vector<int> p(t.size(), 0);
    int mx = 0, id = 0, resLen = 0, resCenter = 0;
    int num = 0;//统计回文串数量
    for (int i = 1; i < t.size(); ++i) {
        p[i] = mx > i ? min(p[2 * id - i], mx - i) : 1;
        while (t[i + p[i]] == t[i - p[i]]) ++p[i];
        if (mx < i + p[i]) {
            mx = i + p[i];
            id = i;
        }
        if (resLen < p[i]) {
            resLen = p[i];
            resCenter = i;
        }
        num += p[i]/2;//统计回文串数量 
    }
    cout<<"统计有多少回文串:\n"<<num<<endl;
    cout<<"最长回文串（之一）\n"<<s.substr((resCenter - resLen) / 2, resLen - 1)<<endl;
}

int main(){
	string x;
	cin>>x;
	Manacher(x);
	
	return 0;
} 
```


