# 随想录 Day 69 并查集 107. 寻找存在的路径
## 理论基础

```cpp
int n = 1005; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, 0); // C++里的一种数组结构

// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}
// 并查集里寻根的过程
int find(int u) {
    return u == father[u] ? u : father[u] = find(father[u]); // 路径压缩
}

// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

// 将v->u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根
    if (u == v) return ; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    father[v] = u;
}
```

## 107. 寻找存在的路径

[107. 寻找存在的路径](https://kamacoder.com/problempage.php?pid=1179)

时间限制：1.000S  空间限制：256MB
题目描述
给定一个包含 n 个节点的无向图中，节点编号从 1 到 n （含 1 和 n ）。


你的任务是判断是否有一条从节点 source 出发到节点 destination 的路径存在。

输入描述
第一行包含两个正整数 N 和 M，N 代表节点的个数，M 代表边的个数。 

后续 M 行，每行两个正整数 s 和 t，代表从节点 s 与节点 t 之间有一条边。 

最后一行包含两个正整数，代表起始节点 source 和目标节点 destination。

输出描述
输出一个整数，代表是否存在从节点 source 到节点 destination 的路径。如果存在，输出 1；否则，输出 0。
输入示例
5 4
1 2
1 3
2 4
3 4
1 4
输出示例
1
### 并查集的直接应用
写成class感觉更舒服
```cpp
# include <iostream>
# include <vector>
using namespace std;

int n, m;

class DisjointSet {
    public:
    vector<int> father;
    DisjointSet(int n) {
        father.resize(n+1, 0);
        for (int i = 0; i < n + 1; i++) {
            father[i] = i;
        }
    }
    int find(int a) {
        if (a == father[a]) return a;
        return father[a] = find(father[a]);
    }
    bool isSame(int a, int b) {
        a = find(a);
        b = find(b);
        return a == b;
    }
    void join(int a, int b) {
        a = find(a);
        b = find(b);
        if (a == b) return;
        father[a] = b;
    }
};
int main() {
    cin>> n>> m;
    DisjointSet sets(n);
    for (int i = 0; i < m; i++) {
        int a, b;
        cin>>a>>b;
        //cout << a <<b<< endl;
        sets.join(a, b);
    }
    int source, destination;
    cin>>source>> destination;
    cout<< int(sets.isSame(source, destination));
}
```
### resize
```cpp
// resizing vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some initial content:
  for (int i=1;i<10;i++) myvector.push_back(i);

  myvector.resize(5);
  myvector.resize(8,100);
  myvector.resize(12);

  std::cout << "myvector contains:";
  for (int i=0;i<myvector.size();i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```
 Edit & run on cpp.sh

Output:
myvector contains: 1 2 3 4 5 100 100 100 0 0 0 0
