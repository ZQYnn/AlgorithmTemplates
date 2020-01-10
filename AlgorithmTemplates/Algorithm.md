---
title: 算法基础模板题
date: 2019/12/19 24:00:00
tags: Algorithm
categories: Algorithm
---

在将近期学的模板放到到这里，日后整理出文章
  <!-- more -->

## 基础模板

给出基础算法的模板题，以及算法的模板。

[toc]







### 快速排序

```c++
/*
    1.快速排序中首先找到 分界点
    2.通过分界点 x 实现划分区间 排序 让 x左边的值 小于x 游标的值大于x
    3.递归此过程实现排序  
    选择的分节点是任意的
	注意边界的问题如果是 使用的是   quick_sort(a, l, i - 1), quick_sort(a, i , r); 这个边界
那么使用的 temp 的位置就是( l + r + 1) / 2 注意上取整

	如果使用的是quick_sort(a, l, j), quick_sort(a, j + 1 , r); 这个边界
 	确定使用的 边界指的职位就是(l + r ) / 2
 	
 当给定的序列有序时，如果每次选择区间左端点进行划分，每次会将区间[L, R]划分成[L, L]和[L + 1, R]，那么相当于每次递归右半部分的区间长度只会减少1，所以就需要递归 n−1次了，时间复杂度会达到 n^2。但每次选择区间中点或者随机值时，划分的两个子区间长度会比较均匀，那么期望只会递归 logn层。
*/

void quicksort(int q[], int l, int r){
    if (l >= r) return ;
    int temp = q[(l + r) / 2], i = l - 1, j = r + 1;
    while (i < j){
        do i ++; while (q[i] < temp);
        do j --; while (q[j] > temp);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```

### 归并排序

```c++
void merge_sort(int q[], int l, int r){
    if (l >= r) return;
    int mid = (l + r) / 2;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r){
        if (q[i] < q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    }
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

### 二分

**二分的模板**

```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```



例题

给定一个按照升序排列的长度为n的整数数组，以及 q 个查询。

对于每个查询，返回一个元素k的起始位置和终止位置（位置从0开始计数）。

如果数组中不存在该元素，则返回“-1 -1”。

**输入格式**

第一行包含整数n和q，表示数组长度和询问个数。

第二行包含n个整数（均在1~10000范围内），表示完整数组。

接下来q行，每行包含一个整数k，表示一个询问元素。

输出格式

共q行，每行包含两个整数，表示所求元素的起始位置和终止位置。

如果数组中不存在该元素，则返回“-1 -1”。

**数据范围**

1≤n≤1000001≤n≤100000
1≤q≤100001≤q≤10000
1≤k≤100001≤k≤10000

**输入样例**：

```
6 3
1 2 2 3 3 4
3
4
5
```

输出样例：

```
3 4
5 5
-1 -1
```

解题代码

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

    while (m -- )
    {
        int x;
        scanf("%d", &x);

        int l = 0, r = n - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (q[mid] >= x) r = mid;
            else l = mid + 1;
        }

        if (q[l] != x) cout << "-1 -1" << endl;
        else
        {
            cout << l << ' ';

            int l = 0, r = n - 1;
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (q[mid] <= x) l = mid;
                else r = mid - 1;
            }

            cout << l << endl;
        }
    }

    return 0;
}

```

例题2 数开三次方计算值（浮点数二分）

```c++
#include <iostream>
#include <cstdio>
using namespace std;

int main(){
    double x;
    scanf("%lf", &x);
    double l = -100, r = 100;
    while (r - l > 1e-8){
        double mid = (l + r) / 2;
        if (mid * mid * mid >= x) r = mid;
        else l = mid;
    }
    printf("%.6lf\n", l);
    return 0;
}
```



### 高精度计算

[之前写过](https://www.zqynn.cn/2019/10/21/Algorithm/AlgorithmofHighPrecision)，这里在列举出来

- 大整数的加法计算

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <vector>
  using namespace std;
  
  //模拟大数相加的过程
  vector<int> add(vector<int> &a, vector<int> &b){
      vector<int> ans;
      int t = 0;
      for (int i = 0; i < a.size() || i < b.size(); i++){
          if (i < a.size()) t += a[i];
          if (i < b.size()) t += b[i];
          ans.push_back(t % 10);
          t /= 10;
      }
      if (t) ans.push_back(1);
      return ans;
  }
  
  int main(){
      string s1, s2;
      cin >> s1 >> s2;
      vector<int> a, b;
      for (int i = s1.size() - 1; i >= 0; i--) a.push_back(s1[i] - '0');
      for (int i = s2.size() - 1; i >= 0; i--) b.push_back(s2[i] - '0');
      auto c = add(a ,b);
      for (int i = c.size() - 1; i >= 0; i--) cout << c[i];
     return 0;
  }
  ```

- 大整数减法

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <vector>
  using namespace std;
  bool cmp(vector<int> &a, vector<int> b){
      if (a.size() != b.size()) return a.size() > b.size();
      for (int i = a.size() - 1; i >= 0; i--){
          if (a[i] != b[i]) return a[i] > b[i];
      }
      return true;
  }
  vector<int> sub(vector<int> &a, vector<int> &b){
      vector<int> ans;
      for (int i = 0, t = 0; i < a.size(); i++){
          t = a[i] - t;
          if (i < b.size()) t -= b[i];
          ans.push_back((t + 10) % 10);
          if (t < 0) t = 1;
          else t = 0;
      }
      while(ans.size() > 1 && ans.back() == 0) ans.pop_back();
      return ans;
  }
  int main(){
      string s1, s2;
      cin >> s1 >> s2;
      vector<int> a, b, c;
      for (int i = s1.size() - 1; i >= 0; i--) a.push_back(s1[i] - '0');
      for (int i = s2.size() - 1; i >= 0; i--) b.push_back(s2[i] - '0');
      if (cmp(a, b))
          c = sub(a ,b);
      else
          c = sub(b, a), cout << "-";
      for (int i = c.size() - 1; i >= 0; i--) cout << c[i];
     return 0;
  }
  ```

- 大整数的乘法

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <vector>
  using namespace std;
  
  vector<int> mul(vector<int> &a, int b){
      vector<int> ans;
      int t  = 0;
      for (int i = 0; i < a.size() || t; i++){
          if (i < a.size()) t +=  a[i] * b;
          ans.push_back(t % 10);
          t /= 10;
      }
      return ans;
  }
  int main(){
      string s1;
      int b;
      vector<int> a;
      cin >> s1 >> b;
      for (int i = s1.size() - 1; i >= 0; i--) a.push_back(s1[i] - '0');
      auto c = mul(a, b);
      for (int i = c.size() - 1; i >= 0; i--) cout << c[i];
     return 0;
  }
  
  ```

- 大整数除法

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <vector>
  #include <algorithm>
  using namespace std;
  // r代表余数
  vector<int> div(vector<int> &a, int b, int &r){
      vector<int> ans;
      for (int i = a.size() - 1; i >= 0; i--){
          r = r * 10 + a[i];
          ans.push_back(r / b);
          r %= b;
      }
      reverse(ans.begin(), ans.end());
      while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
      return ans;
  }
  int main(){
      string s1;
      int b, r = 0;
      vector<int> a;
      cin >> s1 >> b;
      for (int i = s1.size() - 1; i >= 0; i--) a.push_back(s1[i] - '0');
      auto c = div(a, b, r);
      for (int i = c.size() - 1; i >= 0; i--) cout << c[i];
      cout << endl << r << endl;
     return 0;
  }
  ```

  



### 前缀和差分

### 双指针

题目：

给定一个长度为n的整数序列，请找出最长的不包含重复数字的连续区间，输出它的长度。

**输入格式**

第一行包含整数n。

第二行包含n个整数（均在0~100000范围内），表示整数序列。

**输出格式**

共一行，包含一个整数，表示最长的不包含重复数字的连续子序列的长度。

**数据范围**

1≤n≤1000001≤n≤100000

**输入样例**：

```
5
1 2 2 3 5
```

**输出样例**：

```
3
```



思路：i指针假设是在左边的的不断枚举的，此时j指针检查不断从右向左枚举检查是否满足性质 j向后移动时删除掉对应不在区间内的数，其中通过s[i] 表示每个数出现的次数  时间复杂度0(n)

```c++
#include <iostream>

using namespace std;
const int N = 100010;
int n;
int q[N], s[N];

int main(){
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);
    int res = 0;
    for (int i = 0, j = 0; i < n; i ++ ){
        s[q[i]] ++ ;
        while (j < i && s[q[i]] > 1) s[q[j ++ ]] -- ;
        res = max(res, i - j + 1);
    }
    cout << res << endl;
    return 0;
}
```

### 位运算

位运算的操作

- n的二进制整数中第K位是多少？ 

   1. 第k位 移动到最后一位 （个位）n >> k
2. 查看个位是多少 与 1
   
   代码实现

   ```c++
#include <cstdio>
   #include <cmath>
   using namespace std;
   int main(){
       const int n = 32;
       for (int k = 5; k >= 0; k --) printf("%d", n >> k & 1);
       return 0;
   }
   ```

- lowbit运算（树状数组中基本操作）

  返回x的最后一位1所代表的的值是多少？ 

  例如 x 为 101000  则返回  1000 

  **方式**： x & -x  ， -x 表示 补码  就是~x + 1 则 x  & （~x + 1 ） 

  x =   	  1010 ... 10...0

  ~x =   	0101...  01...1

  ~x + 1 = 0101... 10...0

  与运算  最后就是 代表最后一位1所代表的的值

  补码的原理  x + （ -x） == 0；

  -x ==  0 - x 

  0（int类型） 32个0 -x 需要接为1 后32个0   含义就是 ~x + 1

  **应用** 求x（binary）中1的个数

  **代码实现**：

  ```c++
  #include <cstdio>
  #include <iostream>
  
  using namespace std;
  
  int lowbit(int x){
      return x & -x;
  }
  int main(){
      int n;
      cin >> n;
      while (n --){
          int x, ans = 0;
          cin >> x;
          while (x) x -= lowbit(x), ans++;
          printf("%d ", ans);
      }
      return 0;
  }
  ```

  

### 离散化

基础 1 课程中：1.07

### 区间合并

## 数据结构部分

### 模拟单链表（数组）

```c++
#include <iostream>
#include <vector>
#include <cstdio>
using namespace std;

const int N = 100010;


// head 表示头结点的下标
// e[i] 表示节点i的值
// ne[i] 表示节点i的next指针是多少
// idx 存储当前已经用到了哪个点
int head, idx, e[N], ne[N];
// 初始化
void init(){
    head = -1, idx = 0;
}
// 将x插到头结点
void add_to_head(int x){
    e[idx] = x, ne[idx] = head, head = idx ++ ;
}
// 将x插到下标是k的点后面
void add(int k, int x){
    e[idx] = x, ne[idx] = ne[k], ne[k] = idx ++ ;
}
// 将下标是k的点后面的点删掉
void remove(int k){
    ne[k] = ne[ne[k]];
}
void add_to_tail(int x){
    int i = head;
    do i = ne[i];  while(ne[i] != -1);
    e[idx] = x ,ne[idx] = -1, ne[i] = idx++;
}

int main(){
    int m;
    cin >> m;
    init();
    while (m -- ) {
        int k, x;
        char op;
        cin >> op;
        if (op == 'T'){
            cin >> x;
            add_to_tail(x);
        } else if (op == 'H') {
            cin >> x;
            add_to_head(x);
        } else if (op == 'D') {
            cin >> k;
           // 如果k == 0 那么就删除头结点
            if (!k) head = ne[head];
           // 第二种操作就是删除 k - 1的点
            else remove(k - 1);
        } else{
            cin >> k >> x;
            add(k - 1, x);
        }
    }
    for (int i = head; i != -1; i = ne[i])  printf("%d ", e[i]);
    return 0;
}
```

### 模拟双链表

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int m;
int e[N], l[N], r[N], idx;
// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}

int main()
{
    cin >> m;
    // 0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
    while (m -- )
    {
        string op;
        cin >> op;
        int k, x;
        if (op == "L")
        {
            cin >> x;
            insert(0, x);
        }
        else if (op == "R")
        {
            cin >> x;
            insert(l[1], x);
        }
        else if (op == "D")
        {
            cin >> k;
            remove(k + 1);
        }
        else if (op == "IL")
        {
            cin >> k >> x;
            insert(l[k + 1], x);
        }
        else
        {
            cin >> k >> x;
            insert(k + 1, x);
        }
    }
    for (int i = r[0]; i != 1; i = r[i]) cout << e[i] << ' ';
    cout << endl;
    return 0;
}
```

### 模拟栈

```c++
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 1e6 + 10;
//实现stack的模拟
// 入栈的时候需要像将 栈顶指针加 1之后添加元素
int n;
int stk[N], t;

int main(){
    cin.tie(0);
    scanf("%d", &n);
    string str;
    while (n --){
        int temp;
        cin >> str;
        if (str == "push") cin >> temp, stk[++t] = temp;
        else if (str == "pop") t--;
        else if (str == "empty") cout << (t? "NO" : "YES") << endl;
        else cout << stk[t] << endl;
    }
    return 0;
}
```

### 模拟队列

```c++
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 1e6 + 10;
int n;
// hh表示队头指针
int que[N], hh, tt = -1;
void queueinArray(){
    cin.tie(0);
    scanf("%d", &n);
    string str;
    while (n --){
        int temp;
        cin >> str;
        if (str == "push") cin >> temp, que[++tt] = temp;
        else if (str == "pop") hh++;
        else if (str == "empty") cout << (tt >= hh ? "NO" : "YES") << endl;
        else cout << que[hh] << endl;
    }
}
int main(){
   queueinArray();
    return 0;
}
```

### 单调栈的使用

删除逆序的情况， 存放在队列中,存在给定的数列 输出左边的第一个比本身小的数，如果不存在的话 那么就返回 -1

**使用暴力的做法**：通过使用两重`for`,对每个数进行枚举，向左边查找如果找到的话就输出 并break， 如果没有的话那么就返回 -1；时间 复杂度为${0(n^2)}$



**思路**：对于以上的那种方法暴力的做法改进， 如果存在${a_i > a_j}$，并且i > j那么 说明在向左边查找的时候一定是不会 使用到${a_i}$的那么此时的 ai就需要删除，如果我们将这些元素存放在，stack中那去不符合以上的条件的数据，那么久删除的话，每一次查找的数字就是栈顶的元素，并且此时的时间 复杂度为${0(n)}$

```c++
#include <iostream>
#include <cstdio>
using  namespace std;
const int N = 1e5 + 10;
int stk[N], tt;
int n;
int main() {
    scanf("%d", &n);
    while (n --){
        int temp;
        scanf("%d", &temp);
        while (tt && stk[tt] >= temp) tt --;
        if (!tt) printf("-1 ");
        else printf("%d ", stk[tt]);
        stk[++ tt] = temp;
    }
    return 0;
}
```

### 单调队列

思想类似于滑动窗口，给定一定长度的数字序列，窗口的长度为一确定的值，通过让窗口的每次向前移动一个位置找到当前的窗口中最大， 小的值。 

**数组的保存**： 通过是`a[i]`保存数组序列 ，`q[i]`为队列的下标

滑动窗口的实现：

```c++
#include <iostream>
#include <cstdio>
using  namespace std;
const int N = 1e6 + 10;
int a[N], q[N], tt = -1, hh = 0;
int main() {
    int n, k ;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    for (int i = 0; i < n; i++){
        //判断队头是否已经滑出窗口 i - k + 1 表示就是起点的位置
        if (hh <= tt && i - k + 1 > q[hh] ) hh++;
        //如果队尾的值比当前需要插入的大 那么就删除队尾的点 此时就是单调递增的队列
        while (hh <= tt && a[q[tt]] >= a[i]) tt--;
        q[++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    tt = -1, hh = 0;
    for (int i = 0; i < n; i++){
        //判断队头是否已经滑出窗口 i - k + 1 表示就是起点的位置
        if (hh <= tt && i - k + 1 > q[hh] ) hh++;
        //单调递减队列
        while (hh <= tt && a[q[tt]] <= a[i]) tt--;
        q[++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    return 0;
}
```

### KMP的实现

主要思想：找到最长的匹配前缀 后缀 的最大值

```c++
#include <iostream>

using namespace std;

const int N = 10010, M = 100010;

int n, m;
int ne[N];
char s[M], p[N];

int main()
{
  	 cin.tie(0);
    cin >> n >> p + 1 >> m >> s + 1;
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++ ;
        ne[i] = j;
    }
    for (int i = 1, j = 0; i <= m; i ++ )
    {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++ ;
        if (j == n)
        {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    return 0;
}
```

### trie树

```c++
#include <iostream>
#include <cstdio>
using namespace std;
const int N = 1e6 + 10;
int son[N][26],  cnt[N], idx;
char str[N];

void inserts(char str[]){
    int p = 0;
    for (int i = 0; str[i]; i++){
         //如果插入的字符串不存在的话
         int u =  str[i] - 'a';
         if (!son[p][u]) son[p][u] = ++idx;
         p = son[p][u];
    }
    cnt[p]++;
}
int query(char str[]){

    int p = 0;
    for (int i = 0; str[i]; i++){
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main(){
    int n;
    scanf("%d", &n);
    while(n --){
        char op[2];
        scanf("%s%s", op, str);
        if (*op == 'I') inserts(str);
        else printf("%d\n",query(str));
    }
    return 0;
}

```

### 并查集

并查集（Union—Find Set）是一种维护集合的一种数据结构，

支持的操作

1. 集合的 合并
2. 查找两个数，是否在同一个集合中

核心代码：find的理解， find[3] =  1 表示 3的父节点为 1

通过递归的方式实现路径压缩 核心代码：

```c++
int find(int x){
   if (p[x] != x) p[x] = find(p[x]);
   return p[x];
}
```

例题如下：

- 并查集的基本操作

  ```c++
  /*
  1.并查集  通过树的形式 将数组结合起来实现 合并 与查询 
  */
  #include <iostream>
  #include <cstdio>
  using namespace std;
  int const N = 1e5 + 10;
  int p[N];
  
  
  
  int main(){
      int n, m;
      scanf("%d%d",&n, &m);
      for (int i = 0; i < n; i++) p[i] = i;
      while (m --){
          char op[2];
          int a, b;
          scanf("%s%d%d", op, &a, &b);
          //此时区间合并
          if (*op == 'M') p[find(a)] = find(b);
          else{
              if (find(a) == find(b)) puts("YES");
              else puts("NO");
          }
      }
      return 0;
  }
  ```

- 连通块的个数

  ```c++
  #include <iostream>
  #include <cstdio>
  
  using namespace std;
  
  const int N = 100010;
  
  int n, m;
  int p[N], size[N];
  
  int find(int x){
      if (p[x] != x) p[x] = find(p[x]);
      return p[x];
  }
  
  int main(){
      scanf("%d %d", &n, &m);
      for (int i = 1; i <= n; i ++ ){
          p[i] = i;
          size[i] = 1;
      }
      while (m -- ){
          string op;
          int a, b;
          cin >> op;
          if (op == "C"){
              scanf("%d %d", &a, &b);
              a = find(a), b = find(b);
              if (a != b){
                  p[a] = b;
                  size[b] += size[a];
              }
          }
          else if (op == "Q1"){
              scanf("%d %d", &a, &b);
              if (find(a) == find(b)) puts("Yes");
              else puts("No");
          }
          else{
              scanf("%d",&a);
              cout << size[find(a)] << endl;
          }
      }
      return 0;
  }
  ```

  

### 堆和堆排序

```c++
#include <iostream>
#include <algorithm>
#include <cstdio>

using namespace std;

const int N = 100010;
int n, m, len;
int h[N];

void down(int u){
    int t = u;
    if( u * 2 <= len && h [2 * u] < h[t]) t = 2 * u;
    if (u * 2 + 1 <= len && h[2 * u + 1] < h[t]) t = 2 * u + 1;
    if (t != u){
        swap(h[t], h[u]);
        down(t);
    }
}
void up(int u){
   while (u / 2 && h[u / 2] > h[u]){
      swap(h[u], h[u / 2]);
      u /= 2;
   }
}
int main(){
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &h[i]);
    len = n;
    for (int i = n / 2; i; i -- ) down(i);
    while (m -- ){
        printf("%d ", h[1]);
        h[1] = h[len -- ];
        down(1);
    }
    puts("");
    return 0;
}
```

**模拟堆**（构建最小堆）

堆的两个基本操作 down 操作 将数值较小的点向下移动， up操作将数值较大的带你向上移动。

那么手写堆实现的基本操作为：

- 插入一个数  heap[++size] = k,  up(size)
- 输出集合中 当前最小的值 输出h[1]
- 删除集合中最小的数   heap[1] = heap[size--],  down(1);
- 删除第k个插入的数 删除 K heap[k] = heap[size], up(k), down(k)
- 修改第k个插入的数，更改为x    heap[k] = x, up(k)

维护一个集合，初始时集合为空，支持如下几种操作：

1. “I x”，插入一个数x；
2. “PM”，输出当前集合中的最小值；
3. “DM”，删除当前集合中的最小值（数据保证此时的最小值唯一）；
4. “D k”，删除第k个插入的数；
5. “C k x”，修改第k个插入的数，将其变为x；

现在要进行N次操作，对于所有第2个操作，输出当前集合的最小值。

**输入格式**

第一行包含整数N。

接下来N行，每行包含一个操作指令，操作指令为”I x”，”PM”，”DM”，”D k”或”C k x”中的一种。

**输出格式**

对于每个输出指令“PM”，输出一个结果，表示当前集合中的最小值。

每个结果占一行。

**输入样例**：

```
8
I -10
PM
I -10
D 1
C 2 8
I 6
PM
DM
```

**输出样例**：

```
-10
6
```

**解题代码**

```c++
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 100010;
int h[N], ph[N], hp[N], size, n, m;

void heap_swap(int a, int b){
    //交换由堆到指针
    swap(ph[hp[a]],ph[hp[b]]);
   //交换方向的指针，指向的堆
    swap(hp[a], hp[b]);
   //交换对应指向的值
    swap(h[a], h[b]);
}
void down(int u){
    int t = u;
    if( u * 2 <= size && h [2 * u] < h[t]) t = 2 * u;
    if (u * 2 + 1 <= size && h[2 * u + 1] < h[t]) t = 2 * u + 1;
    if (t != u){
        heap_swap(t, u);
        down(t);
    }
}
void up(int u){
   while (u / 2 && h[u / 2] > h[u]){
      heap_swap(u, u / 2);
      u /= 2;
   }
}
int main(){
    scanf("%d", &n);
    while(n --){
        char op[5];
        int x, k;
        scanf("%s", op);
        if (!strcmp(op, "I")){
            scanf("%d", &x);
            size ++, m++;
            ph[m] = size, hp[size] = m;
            h[size] = x;
            up(size);
        } else if (!strcmp(op, "PM")) printf("%d\n", h[1]);
          else if (!strcmp(op, "DM")){
            heap_swap(1, size --);
            down(1);
        } else if ( !strcmp(op, "D")){
            scanf("%d", &k);
            k = ph[k];
            heap_swap(k, size--);
            up(k), down(k);
        } else {
            scanf("%d%d", &k, &x);
            k = ph[k];
            h[k] = x;
            up(k), down(k);
        }
    }
    return 0;
}
```

### 哈希表

维护一个集合，支持如下几种操作：

1. “I x”，插入一个数x；
2. “Q x”，询问数x是否在集合中出现过；

现在要进行N次操作，对于每个询问操作输出对应的结果。

**输入格式**

第一行包含整数N，表示操作数量。

接下来N行，每行包含一个操作指令，操作指令为”I x”，”Q x”中的一种。

**输出格式**

对于每个询问指令“Q x”，输出一个询问结果，如果x在集合中出现过，则输出“Yes”，否则输出“No”。

每个结果占一行。

**数据范围**

1≤N≤1051≤N≤105
−109≤x≤109−109≤x≤109

**输入样例**：

```
5
I 1
I 2
I 3
Q 2
Q 5
```

**输出样例**：

```
Yes
No
```

**解题**：

- 拉链法的使用

  ```c++
  #include <iostream>
  #include <cstring>
  #include <cstdio>
  
  using namespace std;
  
  const int N = 100003;
  int h[N], e[N], ne[N], idx;
  void insert(int x){
      //hash
      int k = (x % N + N) % N;
      e[idx] = x, ne[idx] = h[k], h[k] = idx ++;
  }
  bool find(int x){
      int k = (x % N + N) % N;
      for(int i = h[k]; i != -1; i = ne[i])
          if (e[i] == x)  return true;
      return false;
  }
  int main(){
      int n;
      scanf("%d", &n);
      memset(h, -1, sizeof h);
      while(n --){
          char op[2];
          int x;
          scanf("%s %d", op, &x);
          if (*op =='I') insert(x);
          else{
              if(find(x)) puts("Yes");
              else puts("No");
          }
      }
      return 0;
  }
  ```

- 开放寻址法（ ）

  ```c++
  #include <cstring>
  #include <iostream>
  
  using namespace std;
  
  const int N = 200003, null = 0x3f3f3f3f;
  
  int h[N];
  
  int find(int x){
      int t = (x % N + N) % N;
      while (h[t] != null && h[t] != x){
          t ++ ;
          if (t == N) t = 0;
      }
      return t;
  }
  int main(){
      memset(h, 0x3f, sizeof h);
      int n;
      scanf("%d", &n);
      while (n -- ){
          char op[2];
          int x;
          scanf("%s%d", op, &x);
          if (*op == 'I') h[find(x)] = x;
          else{
              if (h[find(x)] == null) puts("No");
              else puts("Yes");
          }
      }
      return 0;
  }
  
  
  ```

  



### STL的使用

  之前写过STL的[使用方式](https://blog.csdn.net/weixin_42265429/article/details/95773275)以及[补充](https://blog.csdn.net/weixin_42265429/article/details/97024652)，看一下基本使用：

  - vector 变长数组，倍增的思想
  
    ```C++
    vector, 
        size()  返回元素个数
        empty()  返回是否为空
        clear()  清空
        front()/back()
        push_back()/pop_back()
        begin()/end()
        []
        支持比较运算，按字典序
    
    //二维的初始化
    vector<vector<int>> dp(n + 1, vector<int>(m + 1));
    //结合string
    vector<string> t(n, string(n, '.'));
    // 使用字典序的方式
    vector<int> a(4, 3), b(3, 4);
    if (a < b) puts("yes");
    ```
  
  - pair
  
    ```c++
    pair<int, int>
        first, 第一个元素
        second, 第二个元素
        支持比较运算，以first为第一关键字，以second为第二关键字（字典序）
    ```
  
  - string
  
    ```c++
    string，字符串
        size()/length()  返回字符串长度
        empty()
        clear()
        substr(起始下标，(子串长度))  返回子串
        c_str()  返回字符串所在字符数组的起始地址
    ```
  
  - queue 
  
    ```c++
    queue, 队列
        size()
        empty()
        push()  向队尾插入一个元素
        front()  返回队头元素
        back()  返回队尾元素
        pop()  弹出队头元素
    ```
  
  - priority_queue
  
    ```c++
    
    priority_queue, 优先队列，默认是大根堆
        push()  插入一个元素
        top()  返回堆顶元素
        pop()  弹出堆顶元素
        定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;
    ```
  
  - stack, 栈
  
    ```c++
        size()
        empty()
        push()  向栈顶插入一个元素
        top()  返回栈顶元素
        pop()  弹出栈顶元素
    ```
  
  - deque
  
    ```c++
    deque, 双端队列
        size()
        empty()
        clear()
        front()/back()
        push_back()/pop_back()
        push_front()/pop_front()
        begin()/end()
        []
    ```
  
  - set, map, multiset, multimap
  
     基于平衡二叉树（红黑树），动态维护有序序列
  
    ```c++
        size()
        empty()
        clear()
        begin()/end()
        ++, -- 返回前驱和后继，时间复杂度 O(logn)
           set/multiset
         insert()  插入一个数
           find()  查找一个数
           count()  返回某一个数的个数
           erase()
           (1) 输入是一个数x，删除所有x   O(k + logn)
           (2) 输入一个迭代器，删除这个迭代器
           lower_bound()/upper_bound()
           lower_bound(x)  返回大于等于x的最小的数的迭代器
           upper_bound(x)  返回大于x的最小的数的迭代器
    ```
    
  - map
  
    ```c++
       map/multimap
            insert()  插入的数是一个pair
            erase()  输入的参数是pair或者迭代器
            find()
            []  注意multimap不支持此操作。 时间复杂度是 O(logn)
            lower_bound()/upper_bound()
    
    unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表
        和上面类似，增删改查的时间复杂度是 O(1)
        不支持 lower_bound()/upper_bound()， 迭代器的++，--
    ```
  
  - bitset
  
    ```c++
    bitset, 圧位
        bitset<10000> s;
        ~, &, |, ^
        >>, <<
        ==, !=
        []
        count()  返回有多少个1
        any()  判断是否至少有一个1
        none()  判断是否全为0
        set()  把所有位置成1
        set(k, v)  将第k位变成v
        reset()  把所有位变成0
        flip()  等价于~
        flip(k) 把第k位取反
    ```

## 图论和搜索

### dfs

- 全排列问题：

  ```c++
  #include <iostream>
  
  using namespace std;
  
  const int N = 10;
  int a[N], n;
  bool visit[N];
  void dfs(int cur){
      if (cur == n){
          for (int i = 0; i < n; i++) printf("%d ", a[i]);
          cout << endl;
          return;
      }
      for (int i = 1; i <= n; i++){
          if (!visit[i]){
              a[cur] = i;
              visit[i] = true;
              dfs(cur + 1);
              visit[i] = false;
          }
      }
  }
  int main(){
      scanf("%d", &n);
      dfs(0);
      return 0;
  }
  ```

- N—Queens问题

  ```c++
  #include <iostream>
  #include <cstdio>
  using namespace std;
  
  const int N = 20;
  bool col[N], dg[N], udg[N];
  char q[N][N];
  int n;
  void dfs(int u){
      if (u == n){
          for (int i = 0; i < n; i ++ ) puts(q[i]);
          puts("");
          return;
      }
      for (int i = 0; i < n; i ++ )
          if (!col[i] && !dg[u + i] && !udg[n - u + i]){
              q[u][i] = 'Q';
              col[i] = dg[u + i] = udg[n - u + i] = true;
              dfs(u + 1);
              col[i] = dg[u + i] = udg[n - u + i] = false;
              q[u][i] = '.';
          }
  }
  
  int main(){
      cin >> n;
      for (int i= 0; i < n; i++)
          for (int  j = 0; j < n; j++)
              q[i][j] = '.';
      dfs(0);
      return 0;
  }
  ```

### bfs

**迷宫问题**：

- 模拟queue 

  ```c++
  #include <iostream>
  #include <cstring>
  #include <cstdio>
  using namespace std;
  
  const int N = 110;
  int g[N][N], d[N][N];
  int m, n;
  pair<int, int> q[N * N], pre[N][N];
  
  
  // 不使用STL队列方式 同数组的方式模拟 队列
  int bfs(){
      int hh = 0, tt = 0;
      //当前的对头保存的是起始的位置
      q[0] = {0, 0};
      /**默认设置 -1,表示第一次修改设置为 -1， 而一般是从0的位置添加过去的，其他的位置存在且不为-1的话
       * 那么认为不是第一次添过去的，此时就不是最短的路径 
       **/
      memset(d, -1, sizeof d);
      // 起始的位置设置为 0
      d[0][0] = 0;
      // 四个方向向量的 移动位置 上 下 左 右
      int dx[4] = {-1, 1, 0, 0}, dy[4] = {0, 0, -1, 1};
      // 如果队列中的元素不为空的话
      while (hh <= tt){
          //取出的对头的 元素 先后移动一位++ 即将对头元素出队列
          auto t = q[hh++];
          for (int i = 0; i < 4; i++){
              int x = t.first + dx[i], y = t.second + dy[i];
              // 如果没有超出边界的话， 并且不是障碍位置  并且是第一次访问的话
              if (x < n &&  x >= 0 && y < m && y >= 0  && g[x][y] == 0 && d[x][y] == -1){
                  //那么我们就将刚刚从上一个点移动过来的位置 + 1
                  d[x][y] = d[t.first][t.second] + 1;
                  // 将当前的位置送进队列
                  q[++ tt] = {x, y};
                  //
                  pre[x][y] = t;
                 
              }
          }
      }
      int x = n - 1, y = m - 1;
      // 如果当前的位置存在返回的路径
      while (x || y){
          printf("%d %d\n", x, y);
          auto t = pre[x][y];
          x = t.first, y = t.second;
      }
      return d[n - 1][m - 1];
  }
  int main(){
      cin >> n >> m;
      for (int i = 0; i < n; i++)
          for (int j = 0; j < m; j++)
              cin >> g[i][j];
      cout << bfs() << endl;
      return 0;
      
  }
  ```

  

- STL实现

  ```c++
  #include <cstring>
  #include <iostream>
  #include <algorithm>
  #include <queue>
  
  using namespace std;
  
  typedef pair<int, int> PII;
  
  const int N = 110;
  int n, m;
  int g[N][N], d[N][N];
  int bfs()
  {
      queue<PII> q;
  
      memset(d, -1, sizeof d);
      d[0][0] = 0;
      q.push({0, 0});
  
      int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
  
      while (q.size())
      {
          auto t = q.front();
          q.pop();
  
          for (int i = 0; i < 4; i ++ )
          {
              int x = t.first + dx[i], y = t.second + dy[i];
  
              if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] == 0 && d[x][y] == -1)
              {
                  d[x][y] = d[t.first][t.second] + 1;
                  q.push({x, y});
              }
          }
      }
      return d[n - 1][m - 1];
  }
  int main()
  {
      cin >> n >> m;
      for (int i = 0; i < n; i ++ )
          for (int j = 0; j < m; j ++ )
              cin >> g[i][j];
  
      cout << bfs() << endl;
  
      return 0;
  }
  ```


### 树的深度优先遍历

  


```c++
  #include <cstdio>
#include <cstring>
  #include <iostream>
#include <algorithm>
  
  using namespace std;
  
  const int N = 100010, M = N * 2;
  
  int n;
  int h[N], e[M], ne[M], idx;
  int ans = N;
  bool st[N];
  
  void add(int a, int b)
  {
      e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  int dfs(int u)
  {
      st[u] = true;
  
      int size = 0, sum = 0;
      for (int i = h[u]; i != -1; i = ne[i])
      {
          int j = e[i];
          if (st[j]) continue;
  
          int s = dfs(j);
          size = max(size, s);
          sum += s;
      }
  
      size = max(size, n - sum - 1);
      ans = min(ans, size);
  
      return sum + 1;
  }
  
  int main()
  {
      scanf("%d", &n);
  
      memset(h, -1, sizeof h);
  
      for (int i = 0; i < n - 1; i ++ )
      {
          int a, b;
          scanf("%d%d", &a, &b);
          add(a, b), add(b, a);
      }
  
      dfs(1);
  
      printf("%d\n", ans);
  
      return 0;
  }
```

  

### 树的广度优先遍历

**树中顶点的层次**：

- STL实现

  ```c++
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    #include <algorithm>
    #include <queue>
    
    using namespace std;
    
    const int N = 100010;
    
    int n, m;
    int h[N], e[N], ne[N], idx;
    int d[N];
    
    void add(int a, int b)
    {
        e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
    }
    
    int bfs()
    {
        memset(d, -1, sizeof d);
    
        queue<int> q;
        d[1] = 0;
        q.push(1);
    
        while (q.size())
        {
            int t = q.front();
            q.pop();
    
            for (int i = h[t]; i != -1; i = ne[i])
            {
                int j = e[i];
                if (d[j] == -1)
                {
                    d[j] = d[t] + 1;
                    q.push(j);
                }
            }
        }
    
        return d[n];
    }
    
    int main()
    {
        scanf("%d%d", &n, &m);
        memset(h, -1, sizeof h);
    
        for (int i = 0; i < m; i ++ )
        {
            int a, b;
            scanf("%d%d", &a, &b);
            add(a, b);
        }
    
        cout << bfs() << endl;
    
        return 0;
    }
  ```

- 模拟队列

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <cstring>
  
  using namespace std;
  
  const int N = 100010;
  int e[N], ne[N], h[N], idx;
  int q[N], d[N];
  int n, m;
  
  void add(int a, int b){
      e[idx] = b,  ne[idx] = h[a], h[a] = idx++;
  }
  int bfs(){
      q[0] = 1;
      memset(d, -1, sizeof d);
      d[1] = 0;
      int hh = 0, tt = 0;
      while(hh <= tt){
          int t = q[hh++];
          for (int i = h[t]; i != -1; i = ne[i]){
              // 表示当前可以到达的点
              int j = e[i];
              if (d[j] == -1){
                  d[j] = d[t] + 1;
                  q[++ tt] = j;
              }
          }
      }
      return d[n];
  }
  int main(){
      scanf("%d %d", &n, &m);
      memset(h, -1, sizeof h);
      for (int i = 0; i < m; i++){
          int a, b;
          scanf("%d %d", &a, &b);
          add(a ,b);
      }
      
      printf("%d", bfs());
      return 0;
  }
  ```

  



  

  

### 拓扑排序

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n, m;
int h[N], e[N], ne[N], idx;
int d[N];
int q[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

bool topsort()
{
    int hh = 0, tt = -1;

    for (int i = 1; i <= n; i ++ )
        if (!d[i])
            q[ ++ tt] = i;

    while (hh <= tt)
    {
        int t = q[hh ++ ];

        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (-- d[j] == 0)
                q[ ++ tt] = j;
        }
    }

    return tt == n - 1;
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);

        d[b] ++ ;
    }

    if (!topsort()) puts("-1");
    else
    {
        for (int i = 0; i < n; i ++ ) printf("%d ", q[i]);
        puts("");
    }

    return 0;
}

```



### dijkstra求最短路

给定一个n个点m条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出-1。

**输入格式**

第一行包含整数n和m。

接下来m行每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

**输出格式**

输出一个整数，表示1号点到n号点的最短距离。

如果路径不存在，则输出-1。

**数据范围**

1≤n≤5001≤n≤500,
1≤m≤1051≤m≤105,
图中涉及边长均不超过10000。

输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

输出样例：

```
3
```

- 朴素版

  ```c++
  #include <iostream>
  #include <cstdio>
  #include <algorithm>
  #include <cstring>
  using namespace std;
  
  const int N = 510;
  int g[N][N], dist[N];
  bool vis[N];
  //n表示点的个数， m是边的个数
  int m, n;
  int dijkstra(){
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
      for (int i = 0; i < n - 1; i++){
           int t = -1;
          for (int j = 1; j <= n; j++)
              if (!vis[j] && (t == -1 || dist[t] > dist[j]))
                  t = j;
          if (t == -1) break;
          for (int j = 1; j <= n; j++)
              dist[j] = min(dist[j], dist[t] + g[t][j]);
          vis[t] = true;
      }
      if (dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
  }
  int main(){
      cin >> n >> m;
      memset(g, 0x3f, sizeof g);
      while (m --){
          int x, y, z;
          cin >> x >> y >> z;
          g[x][y] = min(g[x][y], z);
      }
      printf("%d",dijkstra());
      return 0;
  }
  
  ```
  
- 堆优化版（如果点和边都在1e5 以上的话需要 堆优化 稀疏图使用邻接表）

  ```c++
  
  
  #include <cstring>
  #include <algorithm>
  #include <cstdio>
  #include <queue>
  
  using namespace std;
  // first 表示最短的距离， sec表示当前的下标
  typedef pair<int, int> PII;
  
  const int N = 1e5 + 10;
  
  int n, m;
  int h[N], w[N], e[N], ne[N], idx;
  int dist[N];
  bool vis[N];
  void add(int a, int b, int c){
      e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++;
  }
  int dijkstra(){
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
      priority_queue<PII, vector<PII>, greater<PII>> heap;
      heap.push({0, 1});
      while(heap.size()){
          auto t = heap.top();
          heap.pop();
          int ver = t.second, dis = t.first;
          for  (int i = h[ver]; i != -1; i = ne[i]){
              int j = e[i];
              if (dist[j] > dis + w[i]){
                  dist[j] = dis + w[i];
                  heap.push({dist[j], j});
              }
          }
      }
      if (dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
  }
  int main(){
      scanf("%d%d", &n, &m);
      memset(h, -1, sizeof h);
      while (m--){
          int a, b, c;
          scanf("%d%d%d", &a, &b, &c);
          add(a, b, c);
      }
      printf("%d", dijkstra());
      return 0;
  }
  ```
  
  

### Bellman_ford算法

边数限制的最短路：

给定一个n个点m条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你求出从1号点到n号点的最多经过k条边的最短距离，如果无法从1号点走到n号点，输出impossible。

注意：图中可能 **存在负权回路** 。

**输入格式**

第一行包含三个整数n，m，k。

接下来m行，每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

**输出格式**

输出一个整数，表示从1号点到n号点的最多经过k条边的最短距离。

如果不存在满足条件的路径，则输出“impossible”。

**数据范围**

1≤n,k≤5001≤n,k≤500,
1≤m≤100001≤m≤10000,
任意边长的绝对值不超过10000。

**输入样例**：

```
3 3 1
1 2 1
2 3 1
1 3 3
```

**输出样例**：

```
3
```

**解题代码**：

```C++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, M = 10010;

int n, m, k;
int dist[N], last[N];

struct Edge{
    int a, b, c;
}edges[M];

void bellman_ford(){
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < k; i++){
        memcpy(last, dist, sizeof dist);
        for (int j = 0; j < m; j++){
            auto e = edges[j];
            dist[e.b] = min(dist[e.b], last[e.a] + e.c);
        }
    }
}

int main(){
    scanf("%d %d %d", &n, &m, &k);
    
    for (int i = 0; i < m; i++){
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        edges[i] = {a, b, c};
    }
    bellman_ford();
    if (dist[n] > 0x3f3f3f3f / 2)  puts("impossible");
    else printf("%d\n", dist[n]);
    return 0;
}
```

模板

```C++
int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}
```

### SPFA

- SPFA求最短路问题，同样地， 可以解决dijskstra类的问题，[例题代码](https://www.acwing.com/problem/content/853/)：

  ```c++
  #include <iostream>
  #include <cstdio> 
  #include <cstring>
  #include <queue>
  
  using namespace std;
  
  const int N = 1e6 + 10;
  int h[N], e[N], ne[N], w[N], idx;
  int dist[N];
  bool vis[N];
  int n, m;
  //链式存储的方式相当于正向的邻接表，以a位起点的一种拉链法相当于 而e[idx] 表示可以走到的点 
  void add(int a, int b, int c){
      e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  int spfa(){
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
      queue<int> q;
      q.push(1);
      vis[1] = true;
      
      while(q.size()){
          int t = q.front();
          q.pop();
         // 从队列中出去的时候设置位false 
          vis[t] = false;
          for (int i = h[t]; i != -1; i = ne[i]){
              //表示可以到达的点的下标
              int j = e[i]; 
              if (dist[j] > dist[t] + w[i]){
                   dist[j] = dist[t] + w[i];
                   if (!vis[j]){
                       q.push(j);
                       vis[j] = true;
                   }
              }
          }
      }
      return dist[n];
  }
  
  int main(){
      scanf("%d%d", &n, &m);
      memset(h, -1, sizeof h);
      while (m -- ){	
          int a, b, c;
          scanf("%d%d%d", &a, &b, &c);
          add(a, b, c);
      }
      int t = spfa();
      if (t == 0x3f3f3f3f) puts("impossible");
      else printf("%d\n", t);
      return 0;
  }
  ```
  
- SPFA判断负环的问题：[例题](https://www.acwing.com/problem/content/854/)

  假设图中n个顶点，添加`cnt`数组保存上一个状态过来的边的个数，此依迭代如果出现`cnt[i] >= n`的话，表示的就是`1 - i` 走存在 至少存在n条边，那么至少走过了 `n + 1`个点，有抽屉原理得到那么至少有一次是走过两次的，一个点走了两次说明存在自环，而在`SPFA`中求的就是最短路，那么这个环就是负环，所以负环的条件 就是 `cnt[i] >= n`

  ```c++
  #include <cstring>
  #include <iostream>
  #include <algorithm>
  #include <queue>
  
  using namespace std;
  
  const int N = 2010, M = 10010;
  
  int n, m;
  int h[N], w[M], e[M], ne[M], idx;
  int dist[N], cnt[N];
  bool vis[N];
  
  void add(int a, int b, int c){
      e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
  }
  
  int spfa(){
      queue<int> q;
      // 求整个图中是否存在负环 而不是某一个 起点存在负环
      for (int i = 1; i <= n; i ++ ){
          vis[i] = true;
          q.push(i);
      }
      while (q.size()){
          int t = q.front();
          q.pop();
          vis[t] = false;
          for (int i = h[t]; i != -1; i = ne[i]){
              int j = e[i];
              if (dist[j] > dist[t] + w[i]){
                  dist[j] = dist[t] + w[i];
                  cnt[j] = cnt[t] + 1;
                  if (cnt[j] >= n) return true;
                  if (!vis[j]){
                      q.push(j);
                      vis[j] = true;
                  }
              }
          }
      }
  
      return false;
  }
  
  int main()
  {
      scanf("%d%d", &n, &m);
  
      memset(h, -1, sizeof h);
  
      while (m -- ){
          int a, b, c;
          scanf("%d%d%d", &a, &b, &c);
          add(a, b, c);
      }
  
      if (spfa()) puts("Yes");
      else puts("No");
  
      return 0;
  }
  ```



### Floyd算法

floyd是一种求全局最短的方法 [例题](https://www.acwing.com/problem/content/856/)

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, INF = 1e9;

int g[N][N];
int n, m, q;

void floyd(){
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                g[i][j] = min(g[i][k] + g[k][j] , g[i][j]);
}


int main(){
    scanf("%d %d %d", &n, &m, &q);
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (i == j) g[i][j] = 0;
            else g[i][j] = INF;
    while (m --){
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }
    floyd();
    while(q --){
        int a, b;
        scanf("%d %d", &a, &b);
        int t = g[a][b];
        if (t > INF / 2)  puts("impossible");
        else printf("%d\n", t);
    }
    return 0;
}
```



### Prim算法

解决最小生成树的问题 注意区分`dijkstra`,例题：

Prim算法步骤：

- 设置一个集合默认为空，不断添加已经构成的点，假设所有点到此集合的距离为`INF`
- 找到距离集合最近的点 纳入集合中 设置为 访问过。
- 通过找到的点，更新其他点集合的距离。循环此过程

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>

using namespace std;

const int  N = 510, INF = 0x3f3f3f3f;
int dist[N], g[N][N];
bool vis[N];
int n, m, res;
int prim(){
    memset(dist, 0x3f, sizeof dist);
    for (int i = 0; i < n; i++){
        int t = -1;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        if (i && dist[t] == INF) return INF;
        if (i) res +=  dist[t];
        vis[t] = true;
        for (int j = 1; j <= n; j++)
            dist[j] = min(dist[j], g[t][j]);
    }
    return res;
}
int main() {
   scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof g);
    while (m -- ){
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = g[b][a] = min(g[a][b], c);
    }
    int t = prim();
    if (t == INF) puts("impossible");
    else printf("%d\n", t);
    return 0;
}


```

### Kruskal算法

回顾并查集的[路径压缩方式](#并查集)，[例题](https://www.acwing.com/problem/content/861/)

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010, INF = 0x3f3f3f3f;

int n, m;
int p[N];

struct Edge{
    int a, b, w;
    bool operator< (const Edge &W) const{
        return w < W.w;
    }
}edges[M];
/*
bool cmp(Edge a, Edge b){
    return a.w < b.w;
}
*/
int find(int x){
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal(){
    
    for (int i = 1; i <=  n; i++) p[i] = i;
    sort(edges, edges + m);
    //sort(edges, edges + m, cmp);
    int res = 0, cnt = 0;
    for (int i = 0; i < m; i++){
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;
        a = find(a), b = find(b);
        if (a != b){
            cnt ++;
            res += w;
            p[a] = b;
        }
    }
    if (cnt < n - 1) return INF;
    return res;
}

int main(){
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i ++ ){
        int a, b, w;
        scanf("%d%d%d", &a, &b, &w);
        edges[i] = {a, b, w};
    }
    int t = kruskal();
    if (t == INF) puts("impossible");
    else printf("%d\n", t);
    return 0;
}
```

## 数论

### 质数

- 试除法求质数

  ```
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  bool is_prime(int x)
  {
      if (x < 2) return false;
      for (int i = 2; i <= x / i; i ++ )
          if (x % i == 0)
              return false;
      return true;
  }
  
  int main()
  {
      int n;
      cin >> n;
  
      while (n -- )
      {
          int x;
          cin >> x;
          if (is_prime(x)) puts("Yes");
          else puts("No");
      }
  
      return 0;
  }
  ```



- 因式分解法求质数

  ```c++
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  void divide(int x)
  {
      for (int i = 2; i <= x / i; i ++ )
          if (x % i == 0)
          {
              int s = 0;
              while (x % i == 0) x /= i, s ++ ;
              cout << i << ' ' << s << endl;
          }
      if (x > 1) cout << x << ' ' << 1 << endl;
      cout << endl;
  }
  
  int main()
  {
      int n;
      cin >> n;
      while (n -- )
      {
          int x;
          cin >> x;
          divide(x);
      }
  
      return 0;
  }
  ```

- 筛法求质数（朴素）

  ```c++
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  const int N= 1000010;
  
  int primes[N], cnt;
  bool st[N];
  
  void get_primes(int n)
  {
      for (int i = 2; i <= n; i ++ )
      {
          if (st[i]) continue;
          primes[cnt ++ ] = i;
          for (int j = i; j <= n; j += i)
              st[j] = true;
      }
  }
  
  int main(){
      int n;
      cin >> n;
      get_primes(n);
      cout << cnt << endl;
      return 0;
  }
  ```
  
- 线性筛法

  ```c++
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  const int N= 1000010;
  
  int primes[N], cnt;
  bool st[N];
  
  void get_primes(int n)
  {
      for (int i = 2; i <= n; i ++ )
      {
          if (!st[i]) primes[cnt ++ ] = i;
          for (int j = 0; primes[j] <= n / i; j ++ )
          {
              st[primes[j] * i] = true;
              if (i % primes[j] == 0) break;
          }
      }
  }
  
  int main()
  {
      int n;
      cin >> n;
  
      get_primes(n);
  
      cout << cnt << endl;
  
      return 0;
  }
  ```

  

### 约数

- 最大公约数

  ```c++
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  
  int gcd(int a, int b)
  {
      return b ? gcd(b, a % b) : a;
  }
  
  
  int main()
  {
      int n;
      cin >> n;
      while (n -- )
      {
          int a, b;
          scanf("%d%d", &a, &b);
          printf("%d\n", gcd(a, b));
      }
  
      return 0;
  }
  ```

  

## 动态规划

背包问题是动态规划问题中的典型问题，具体如下几种解析：

### 01背包问题

- 动态规划的二维写法：

  ```c++
  #include <iostream>
  #include <cstdio>
  
  using namespace std; 
  
  const int N = 1010;
  // m 表示物品的数量， N表示背包的容量
  int m, n;
  int dp[N][N], w[N], v[N];
  
  int main(){
      cin >> m >> n;
      
      for (int i = 1; i <= m; i++) cin >> v[i] >> w[i];
      
      for (int i = 1; i <= m; i++){
          for (int j = 1; j <= n; j++){
              dp[i][j] = dp[i - 1][j];
              if (j >= v[i])
              dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
          }
      }
      cout << dp[m][n];
      return 0;
  }
   
  ```

- 优化为一维的写法

  对于滚动数组的理解：由二维的数组可知，我们每一层中的dp[i] 是由dp[i - 1]计算得到的，dp[i]又是从左半边的 到的，那么我们就可以从末尾计算过程，右边不满足条件的不计算过程将 dp[i - 1]左边和 dp[i]的右边拼接起来，这样的方式就是滚动数组 

  ```c++
  #include <iostream>
  
  using namespace std; 
  
  const int N = 1010;
  // m 表示物品的数量， N表示背包的容量
  int m, n;
  int dp[N], w[N], v[N];
  
  int main(){
      cin >> m >> n;
      
      for (int i = 1; i <= m; i++) cin >> v[i] >> w[i];
      
      for (int i = 1; i <= m; i++)
          for (int j = n; j >= v[i]; j--)
              dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
              
      cout << dp[n];
      return 0;
  }
  
  ```

  

### 完全背包问题

完全背包问题：同一种物品可以选择多次，那么我们就枚举可以K次选择，类似于01背包问题

- 朴素版本

  ```c++
  #include <iostream>
  
  using namespace std; 
  
  const int N = 1010;
  // m 表示物品的数量， N表示背包的容量
  int m, n;
  int dp[N][N], w[N], v[N];
  
  int main(){
      cin >> m >> n;
      
      for (int i = 1; i <= m; i++) cin >> v[i] >> w[i];
  
      for (int i = 1; i <= m; i++)
          for (int j = 0; j <= n; j++)
              for (int k = 0; k * v[i] <= j; k++)
                  dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i] * k] + k * w[i]);
                  
      cout << dp[m][n];
      return 0;
  }
  ```

  

- 优化版本

  不同于滚动数组 这里是正向 枚举

  ```c++
  #include <iostream>
  
  using namespace std; 
  
  const int N = 1010;
  // m 表示物品的数量， N表示背包的容量
  int m, n;
  int dp[N], w[N], v[N];
  
  int main(){
      cin >> m >> n;
      
      for (int i = 1; i <= m; i++) cin >> v[i] >> w[i];
      
      for (int i = 1; i <= m; i++)
          for (int j = v[i]; j <= n; j++)
                  dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
                  
      cout << dp[n];
      return 0;
  }
  ```

  

### 多重背包问题

- 多重背包的朴素版本

  通过完全背包的思路的启发对于每种的物品最多只能选择s[i]次，我们将刚刚的k改为s[i]即可

  ```c++
  #include <iostream>
  
  using namespace std; 
  
  const int N = 1010;
  // m 表示物品的数量， N表示背包的容量
  int m, n;
  int dp[N][N], w[N], v[N], s[N];
  
  int main(){
      cin >> m >> n;
      
      for (int i = 1; i <= m; i++) cin >> v[i] >> w[i] >> s[i];
      
      for (int i = 1; i <= m; i++)
          for (int j = 0; j <= n; j++)
              for (int k = 0; k * v[i] <= j && k <= s[i]; k++)
                  dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i] * k] + k * w[i]);
                  
      cout << dp[m][n];
      return 0;
  }
  ```

- 多重背包的优化

  对于每种物品存在有多种选择时：思考一个问题 是否需要一一枚举每种可能？ 如果选择的物品的种类 为1k次， 容量为1k， 每种选择为次数最大为1k 此时 时间复杂为${O(nmk)}$ 此时最高的计算次数就是1E，在C++中此时一般给定的用时为1s，此时就会出现TLE, 对此需要对其进行优化
  
  **优化方法**：在此之前我们考虑  IPv4地址 表示方式使用的是32为二进制表示，每八位使用一个点一分割， 这样的方法称为点分十进制方法表示，那么其中每八位通过十进制表示的数据范围是0-255即（${2^8-1}$）,对于这里我们并没有一一枚举，而是通过每位不同位上的二进制表示，通过八个01分别代表十进制数 128 64 32 16 8 4 2 1其中 选或者不选的方法组合表示 0 - 255其中任何一个数字，那么回归到我们的多重背包问题将给定的是s[i]可以选择的数量用同样的方法分解，假设 对于s[i]的最大值位255，那么我们就可以分八组128 64 32 16 8 4 2 1 计算， 但是对于s[i]的值不可能凑巧是${2^k - 1}$， 那么多余的部分就 用  ${s[i]- 2^k - 1}$, 假设s[i]位 280，那么添加多的一个分组的数字就是 25 此时组成的数据就是128 64 32 16 8 4 2 1  25  此时就可有组成 0- 280之间的任何一个数字， 分组之后最每个组做一次 01背包问题即可 。
  
  **证明此过程**
  
  代码实现
  
  ```c++
  #include <iostream>
  #include <algorithm>
  
  using namespace std;
  
  const int N = 12010, M = 2010;
  
  int n, m;
  int v[N], w[N];
  int f[M];
  
  int main()
  {
      cin >> n >> m;
  
      int cnt = 0;
      for (int i = 1; i <= n; i ++ )
      {
          int a, b, s;
          cin >> a >> b >> s;
          int k = 1;
          while (k <= s)
          {
              cnt ++ ;
              v[cnt] = a * k;
              w[cnt] = b * k;
              s -= k;
              k *= 2;
          }
          if (s > 0)
          {
              cnt ++ ;
              v[cnt] = a * s;
              w[cnt] = b * s;
          }
      }
  
      n = cnt;
  
      for (int i = 1; i <= n; i ++ )
          for (int j = m; j >= v[i]; j -- )
              f[j] = max(f[j], f[j - v[i]] + w[i]);
  
      cout << f[m] << endl;
  
      return 0;
  }
  ```
  



### 分组背包问题

将多个物品分租，每组中只能选择一个物品： 存在不理解的地方 需要回顾

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int v[N][N], w[N][N], s[N];
int dp[N];

int main(){
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ ){
        cin >> s[i];
        for (int j = 0; j < s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }

    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= 0; j -- )
            for (int k = 0; k < s[i]; k ++ )
                if (v[i][k] <= j)
                    dp[j] = max(dp[j], dp[j - v[i][k]] + w[i][k]);

    cout << dp[m] << endl;

    return 0;
}
```



### 线性dp

### 区间dp

## 贪心

## 时间复杂度分析