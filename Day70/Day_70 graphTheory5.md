# 随想录 Day 69 并查集 108. 冗余连接

## 108. 冗余连接

[108. 冗余连接](https://kamacoder.com/problempage.php?pid=1181)

时间限制：1.000S  空间限制：256MB
题目描述
树可以看成是一个图（拥有 n 个节点和 n - 1 条边的连通无环无向图）。 


现给定一个拥有 n 个节点（节点标号是从 1 到 n）和 n 条边的连通无向图，请找出一条可以删除的边，删除后图可以变成一棵树。

输入描述
第一行包含一个整数 N，表示图的节点个数和边的个数。

后续 N 行，每行包含两个整数 s 和 t，表示图中 s 和 t 之间有一条边。

输出描述
输出一条可以删除的边。如果有多个答案，请删除标准输入中最后出现的那条边。
输入示例
3
1 2
2 3
1 3
输出示例
1 3
### 提交
认清规律，去掉成环的
```cpp
# include<iostream>
# include<vector>
using namespace std;
class DisjointSet {
    public:
    vector<int> father;
    DisjointSet(int n) {
        father.resize(n+1, 0);
        for (int i = 0; i < n+1; i++) {
            father[i] = i;
        }
    };
    int find(int u) {
        return father[u] == u ? u : father[u] = find(father[u]);
    };
    bool isSame(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return;
        father[u] = v;
    }
};

int main() {
    int n, u, v;
    cin>> n;
    //cout<< n;
    DisjointSet set(n);
    
    for (int i = 0; i < n; i++) {
        cin>> u>> v;
        //cout <<u << v<<endl;
        if (set.isSame(u, v)) {
            cout<< u <<" "<<v;
        } else {
            set.join(u,v);
        }
    }
}
```
## 109. 冗余连接II

[109. 冗余连接II](https://kamacoder.com/problempage.php?pid=1182)
时间限制：1.000S  空间限制：256MB
题目描述
有向树指满足以下条件的有向图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。有根树拥有 n 个节点和 n - 1 条边。 


输入一个有向图，该图由一个有着 n 个节点(节点编号 从 1 到 n)，n 条边，请返回一条可以删除的边，使得删除该条边之后该有向图可以被当作一颗有向树。

输入描述
第一行输入一个整数 N，表示有向图中节点和边的个数。 

后续 N 行，每行输入两个整数 s 和 t，代表 s 节点有一条连接 t 节点的单向边

输出描述
输出一条可以删除的边，若有多条边可以删除，请输出标准输入中最后出现的一条边。
输入示例
3
1 2
1 3
2 3
输出示例
2 3
### 提交
分清三种情况
1：入度为2 任意删除一个
2：入度为2，有环
3，没有入度为2，但是有环

```cpp
# include<iostream>
# include<vector>
# include<unordered_set>
using namespace std;
class DisjointSet {
    public:
    vector<int> father;
    DisjointSet(int n) {
        father.resize(n+1, 0);
        for (int i = 0; i < n+1; i++) {
            father[i] = i;
        }
    };
    int find(int u) {
        return father[u] == u ? u : father[u] = find(father[u]);
    };
    bool isSame(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return;
        father[v] = u;
    }
};
int n;
vector<int> u;
vector<int> v;
bool isTree(int removeIndex, bool flag = false) {
    DisjointSet sets(n);
    for (int i = 0; i < n; i++) {
        //cout <<u << v<<endl;
        if (i == removeIndex) continue;
        
        if (sets.isSame(u[i], v[i])) {
            if (flag) cout<< u[i] <<" "<<v[i];
            return false;
        } else {
            sets.join(u[i],v[i]);
        }
    }
    return true;
}
int main() {
    cin>> n;
    //cout<< n;
    u.resize(n);
    v.resize(n);
    vector<int> count(n + 1, 0);
    bool flag = false;
    unordered_set <int> indexs;
    DisjointSet set(n);
    for (int i = 0; i < n; i++) {
        //cout <<u << v<<endl;
        cin>> u[i]>>v[i];
        count[v[i]]++;
    }
    for (int i = n-1; i >= 0; i--) {
        //cout <<u << v<<endl;
        if(count[i] > 1) {
            flag = true;
            indexs.insert(i);
        }
    }
   // cout << flag << endl;
    
    if (flag) {
        for (int i = n-1; i >= 0; i--) {
        //cout <<u << v<<endl;
            if(indexs.count(v[i]) && isTree(i) == true) {
               // cout<<"!!!!!"<<endl;
                cout<< u[i] <<" "<<v[i];
                break;
            }
        }
    } else {//有环
        bool _ = isTree(-1, true);
    }
}
```