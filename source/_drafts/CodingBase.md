---
title: Algorithm Programming
date: 2024-06-24 17:02:55
toc: true
tags:
    - Professional Knowledge
categories: knowledge
excerpt: This is a post about the data structure and algorithms aimed to improve the coding ability.
---
# 2024年机试复习——数据结构与算法

## 一、数据结构

### 栈
#### 数组模拟
```c++
#include<cstdlib>
#include<iostream>
using namespace std;
#define MAX_SIZE 100

int st[MAX_SIZE];
int main()
{
    int num, obj;
    cin >> num >> obj;
    while(num){
        st[++*st] = num % obj;
        num = num / obj;
    }
    while(*st){
        cout << st[(*st)--];
    }
    return 0;
}
```
#### STD标准库

![image-20240623142204592](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240623142204592.png)

### 单调栈

+ 典型代码结构

```c++
//insert x
while(!sta.empty() && sta.top()<x)
    sta.pop();
sta.push(x);
```

+ 给定 $n$ 个数据：$a_{1...n}$，定义函数 $f(i)$ 表示第 $i$ 个元素之后第一个大于 $a_i$ 的元素的下标，即 $f(i)=\min_{i<j\leq n,a_j>a_i}\{j\}$，若不存在则 $f(i)=0$
+ 单调栈中不一定存储单调的元素值（毕竟不是排序），但是一定是与单调元素有关的数据，比如这里维护一个单调递增栈，但是栈中存储对应元素的下标.

```c++
#include<iostream>
#include<stack>
using namespace std;
const int MAX_SIZE = 3000002;
int p[MAX_SIZE], f[MAX_SIZE];

int main()
{
    int m;
    cin >> m;
    for(int i=1;i<=m;i++){
        cin >> p[i];
    }
    stack<int> st;
    for(int i=m;i>=1;i--){
        while(!st.empty() && p[i] >= p[st.top()]) st.pop();
        f[i] = st.empty() ? 0:st.top();
        st.push(i);
    }
    for(int i=1;i<=m;i++){
        cout << f[i] << " ";
    }
    return 0;
}
```

### 队列

#### 数组模拟

![image-20240623142623355](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240623142623355.png)

#### STD标准库

![image-20240623142656589](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240623142656589.png)

STD标准库——双端队列

![image-20240623142736280](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240623142736280.png)

### 单调队列

+ 单调：队列中元素的规律，保持递增/递减

+ 队列：元素只能从队头和队尾进行操作

##### 应用背景

给出一个长度为 $n$ 的数组，编程输出每 $k$ 个连续的数中的最大值和最小值。

+ 单调队列：维护一个递减的单调队列，当前元素大于之前已经进队的元素，则**从队尾弹出**队中元素，再将当前元素从队尾入队；
+ 窗口控制：当前元素进队将使得队列元素数大于 $k$，则**从队头弹出**队中元素（其最先进入队列，最先离开）；

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#define maxn 1000100
using namespace std;
int q[maxn], a[maxn];
int n, k;

// 递增单调队列
void getmin() {  // 得到这个队列里的最小值，直接找到最后的就行了
  int head = 0, tail = -1;
  for (int i = 1; i < k; i++) {
    while (head <= tail && a[q[tail]] >= a[i]) tail--;
    q[++tail] = i;
  }
  for (int i = k; i <= n; i++) {
    while (head <= tail && a[q[tail]] >= a[i]) tail--;
    q[++tail] = i;
    while (q[head] <= i - k) head++;   
    // q[head] 存储最先进队列的元素下标，即窗口的最左端，i 为窗口最右端
    printf("%d ", a[q[head]]);
  }
}

// 递减单调队列
void getmax() {  // 和上面同理
  int head = 0, tail = -1;
  for (int i = 1; i < k; i++) {
    while (head <= tail && a[q[tail]] <= a[i]) tail--;
    q[++tail] = i;
  }
  for (int i = k; i <= n; i++) {
    while (head <= tail && a[q[tail]] <= a[i]) tail--;
    q[++tail] = i;
    while (q[head] <= i - k) head++;
    printf("%d ", a[q[head]]);
  }
}

int main() {
  scanf("%d%d", &n, &k);
  for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
  getmin();
  printf("\n");
  getmax();
  printf("\n");
  return 0;
}
```

### 异或链表

![image-20240623143636009](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240623143636009.png)

### 哈希表/散列表

#### 拉链法/开散列法

拉链法是在每个存放数据的地方开一个链表，如果有多个键值索引到同一个地方，只用把他们都放到那个位置的链表里就行了。

```c++
const int SIZE = 1000000;
const int M = 999997;

struct HashTable {
  struct Node {
    int next, value, key;
  } data[SIZE];

  int head[M], size;

  int f(int key) { return (key % M + M) % M; }

  int get(int key) {
    for (int p = head[f(key)]; p; p = data[p].next)
      if (data[p].key == key) return data[p].value;
    return -1;
  }

  int modify(int key, int value) {
    for (int p = head[f(key)]; p; p = data[p].next)
      if (data[p].key == key) return data[p].value = value;
  }

  int add(int key, int value) {
    if (get(key) != -1) return -1;
    data[++size] = (Node){head[f(key)], value, key};  // 新节点指向 f(key) 对应链表首节点
    head[f(key)] = size;  // 更新 f(key) 链表首节点
    return value;
  }
};
```

#### 闭散列法

闭散列方法把所有记录直接存储在散列表中，如果发生冲突则根据某种方式继续进行探查，比如线性探查法。

```c++
const int N = 360007;  // N 是最大可以存储的元素数量

class Hash {
 private:
  int keys[N];
  int values[N];

 public:
  Hash() { memset(values, 0, sizeof(values)); }

  int& operator[](int n) {
    // 返回一个指向对应 Hash[Key] 的引用
    // 修改成不为 0 的值 0 时候视为空
    int idx = (n % N + N) % N, cnt = 1;
    while (keys[idx] != n && values[idx] != 0) {
      idx = (idx + cnt * cnt) % N;
      cnt += 1;
    }
    keys[idx] = n;
    return values[idx];
  }
};
```

### 并查集
+ 合并（Union）：合并两个元素所属集合（合并对应的树）
+ 查询（Find）：查询某个元素所属集合（查询对应的树的根节点），这可以用于判断两个元素是否属于同一集合
+ 用途：擅长动态维护许多具有传递性的关系，A 与 B 有某种关系，B 与 C 有某种关系，则 A 与 C 也有某种确定关系

```c++
int fa[SIZE];  // 存储并查集
for(int i=1;i<=n;i++) fa[i] = i; // 初始化为 n 个节点构成的单独的树

// 查询操作
int get(int x)
{
    if(x == fa[x]) return x;    // 获取树根
    return fa[x] = get(fa[x]);   // 路径压缩，fa 赋值为代表元素
}

// Merge 操作
void merge(int x, int y)
{
    fa[get(x)] = get(y);  // x 的树根作为 y 的树根的子节点
}
```

##### 边带权并查集

+ 在树中的每条边上记录一个权值，即维护一个数组 $d$，用 $d[x]$ 保存节点 $x$ 到父节点 $fa[x]$ 之间的边权
+ 在每次路径压缩后，每个访问过的节点都会直接指向树根，同时更新这些节点的 $d$ 值，就可以利用路径压缩过程来统计每个节点到树根之间的信息


![image-20240624162355090](https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/image-20240624162355090.png)

```c++
#include<iostream>
#include<cstdlib>
using namespace std;
#define MAX 30002
#define abs(a,b) (a > b ? a-b: b-a)

int fa[MAX];
int sz[MAX];
int d[MAX];

int get(int x)
{
    if(x == fa[x]) return x;
    int root = get(fa[x]);    // 递归
    d[x] += d[fa[x]];        // 递归重叠计算，节点到树根的边权和
    return fa[x] = root;
}

void merge(int x, int y)
{
    x = get(x), y = get(y);
    fa[x] = y;     // 合并两个子树
    d[x] = sz[y];
    sz[y] += sz[x];
}
int main()
{
    for(int i=1;i<=30000;i++){
        fa[i] = i;
        sz[i] = 1;
    }
    int T=0, x, y;
    char ch;
    cin >> T;
    while(T--){
        cin >> ch >> x >> y;
        if(ch == 'C'){
            if(get(x) == get(y)) cout << abs(d[x], d[y]) - 1 << endl;
            else cout << "-1\n";
        }else merge(x, y);
    }
    return 0;
}
```

### 二叉堆

+ 大根堆：父亲的权值不小于儿子的权值，小根堆：父亲的权值不大于儿子的权值
+ 操作：

  + 插入操作：插入最右下节点，然后向上调整
  + 删除操作：将根节点与最后一个节点交换，然后删除最后一个节点，并向下调整根节点

#### 数组模拟

```c++
#include<iostream>
using namespace std;

#define MAX_SIZE 100
int h[MAX_SIZE];

void up(int x)
{
    while(x > 1 && h[x] > h[x / 2]){
        swap(h[x], h[x/2]);
        x /= 2;
    }
}

void down(int x)
{
    while(x * 2 <= h[0])
    {
        int high = x*2;
        if(high+1 <= h[0] && h[high+1] > h[high]) high++;
        if(h[high] <= h[x]) break;
        swap(h[high], h[x]);
        x = high;
    }
}

int main()
{
    cin >> h[0];
    for(int i=1;i<=h[0];i++){
        cin >> h[i];
    }

    for(int i=h[0]/2;i>=1;i--) down(i);

    for(int i=1;i<=h[0];i++)
        cout << h[i] << " ";

    return 0;
}
```

#### STD 标准库

```c++
#include<queue>
//降序队列(默认大根堆)
priority_queue <int,vector<int>,less<int> >q;
//升序队列(小根堆)
priority_queue <int,vector<int>,greater<int> > q;
```

### 对顶堆

​	对顶堆由一个大根堆与一个小根堆组成，小根堆维护大值即前 $k$ 大的值（包含第 $k$ 个），大根堆维护小值即比 第 $k$ 大数小的其他数。

+ 维护：当小根堆的大小小于 $k$ 时，不断将大根堆堆顶元素取出并插入小根堆，直到小根堆的大小等于 $k$；当小根堆的大小大于 $k$ 时，不断将小根堆堆顶元素取出并插入大根堆，直到小根堆的大小等于 $k$；
+ 插入元素：若插入的元素大于等于小根堆堆顶元素，则将其插入小根堆，否则将其插入大根堆，然后维护对顶堆；
+ 查询第 $k$ 大元素：小根堆堆顶元素即为所求；
+ 删除第 $k$ 大元素：删除小根堆堆顶元素，然后维护对顶堆；
+ $k$ 值 $+-1$：根据新的 $k$ 值直接维护对顶堆；

#### 查询序列中位数

1. 向序列中插入一个元素
2. 输出并删除当前序列的中位数（若序列长度为偶数，则输出较小的中位数）
3. 输入输出：$t$ 表示测试组数，接下来每组若干数据 $x = -1$ 表示查询并删除中位数，$x=0$ 表示该组测试数据结束

```c++
#include <cstdio>
#include <iostream>
#include <queue>
using namespace std;

int main() {
  int t, x;
  scanf("%d", &t);
  while (t--) {
    // 大根堆，维护前一半元素（存小值）
    priority_queue<int, vector<int>, less<int> > a;
    // 小根堆，维护后一半元素（存大值）
    priority_queue<int, vector<int>, greater<int> > b;
    while (scanf("%d", &x) && x) {
      // 若为查询并删除操作，输出并删除大根堆堆顶元素
      // 因为这题要求输出中位数中较小者（偶数个数字会存在两个中位数候选）
      // 这个和上面的第k大讲解有稍许出入，但如果理解了上面的，这个稍微变通下便可理清
      if (x == -1) {
        printf("%d\n", a.top());
        a.pop();
      }
      // 若为插入操作，根据大根堆堆顶的元素值，选择合适的堆进行插入
      else {
        if (a.empty() || x <= a.top())
          a.push(x);
        else
          b.push(x);
      }
      // 对对顶堆进行调整（调整策略便是根据前后元素个数对半）
      if (a.size() > (a.size() + b.size() + 1) / 2) {
        b.push(a.top());
        a.pop();
      } else if (a.size() < (a.size() + b.size() + 1) / 2) {
        a.push(b.top());
        b.pop();
      }
    }
  }
  return 0;
}
```