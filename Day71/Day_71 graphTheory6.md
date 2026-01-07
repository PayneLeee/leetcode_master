# 随想录 Day 71 最小生成树 prim算法 kruskal算法




[53. 寻宝（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1053)

时间限制：1.000S  空间限制：128MB
题目描述
在世界的某个区域，有一些分散的神秘岛屿，每个岛屿上都有一种珍稀的资源或者宝藏。国王打算在这些岛屿上建公路，方便运输。

不同岛屿之间，路途距离不同，国王希望你可以规划建公路的方案，如何可以以最短的总公路距离将 所有岛屿联通起来（注意：这是一个无向图）。 

给定一张地图，其中包括了所有的岛屿，以及它们之间的距离。以最小化公路建设长度，确保可以链接到所有岛屿。

输入描述
第一行包含两个整数V 和 E，V代表顶点数，E代表边数 。顶点编号是从1到V。例如：V=2，一个有两个顶点，分别是1和2。

接下来共有 E 行，每行三个整数 v1，v2 和 val，v1 和 v2 为边的起点和终点，val代表边的权值。

输出描述
输出联通所有岛屿的最小路径总距离

## prim算法精讲 

prim算法核心就是三步，我称为prim三部曲，大家一定要熟悉这三步，代码相对会好些很多：

第一步，选距离生成树最近节点

第二步，最近节点加入生成树

第三步，更新非生成树节点到生成树的距离（即更新minDist数组）

```cpp
# include <iostream>
# include <vector>
using namespace std;
int v, e;
int main() {
    cin >> v >> e;
    vector<vector<int> > map(v +1, vector<int>(v+1, 100001));
    for(int i = 0; i < e; i++ ) {
        int start, weight, end;
        cin>> start>> end>> weight;
        map[start][end] = weight;
        map[end][start] = weight;
    }
    // for(int i = 0; i < v; i++) {
    //     for (int j = 0; j < v; j++) {
    //         cout<< map[i][j]<< " ";
    //     }
    //     cout << endl;
    // }
    vector<bool> inTree(v+1, false);
    vector<int> minDis(v+1, 100001);
    inTree[1] = true;
    int cur = 1;
    int res = 0;
    int inIndex = 1;
    
    for (int t = 1 ; t < v; t++) {
        int minD = 100001;
        for (int i = 1; i <= v; i++) {
            //cout << i << "original "<< minDis[i] << endl;
            //cout<< cur << " "<< map[cur][i] <<" "<< i << endl;
            if (inTree[i] != true) {
                minDis[i] = min(minDis[i], map[cur][i]);
                if (minDis[i] < minD) {
                    inIndex = i;
                    minD = minDis[i];
                }
                //cout << i << "changed "<< minDis[i] << endl;
            }
           
        }
        //cout<< inIndex << " " << minD << endl;
        
        res += minD;
        inTree[inIndex] = true;
        cur = inIndex;
    }
    
    cout<< res;
}
```

## kruskal算法精讲
kruscal的思路：

边的权值排序，因为要优先选最小的边加入到生成树里

遍历排序后的边

如果边首尾的两个节点在同一个集合，说明如果连上这条边图中会出现环

如果边首尾的两个节点不在同一个集合，加入到最小生成树，并把两个节点加入同一个集合
```cpp
# include <iostream>
# include <vector>
# include <algorithm>
using namespace std;
int v, e;
class Edge{
    public:
    int v1,v2, weight;
    
};
class cmp {
    public:
    bool operator()(const Edge &a, const Edge &b ) {
        return a.weight < b.weight;
    }
};
class DisjointSet {
    public:
    vector<int> father;
    DisjointSet(int n) {
        father.resize(n+1, 0);
        for (int i = 0; i < n+1; i++) {
            father[i] = i;
        }
    }
    int find(int u) {
        return father[u] == u ? u : father[u] = find(father[u]);
    }
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
   cin>> v >> e;
   vector<Edge> edges(e);
   for (int i = 0; i < e; i++) {
       cin>> edges[i].v1 >> edges[i].v2>>edges[i].weight;
   }
   sort(edges.begin(), edges.end(), cmp());
   
//   for (int i = 0; i < e; i++) {
//       cout<< edges[i].v1 <<" "<< edges[i].v2<<" "<< edges[i].weight<< endl;;
//   }
    DisjointSet sets(v);
    int res = 0;
    for (Edge edge : edges) {
        if (sets.isSame(edge.v1, edge.v2)) continue;
        else {
            res += edge.weight;
            sets.join(edge.v1, edge.v2);
        }
    }
    cout << res;
}
```