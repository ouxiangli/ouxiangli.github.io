## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/ouxiangli/ouxiangli.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ouxiangli/ouxiangli.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.


# 一级标题  
## 二级标题  
### 三级标题  
#### 四级标题  
##### 五级标题  
###### 六级标题  



## 快速幂
```cpp
int pow3(int x,int n){
    if(n==0) return 1;
    else {
        while((n&1)==0){
        n>>=1;
        x*=x;
        }
    }
    int result=x;
    n>>=1;
    while(n!=0){
        x*=x;
        if(n&1) result*=x;
        n>>=1;
    }
    return result;
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
    // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和 // 查询： 由于每个c结点相当于一小段前缀和，因此全+起来，最后求得的便是总共的前缀和
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