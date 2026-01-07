# 随想录 Day 74  Bellman_ford 


## Bellman_ford  队列优化

[94. 城市间货物运输 I](https://kamacoder.com/problempage.php?pid=1152)
    
时间限制：1.000S  空间限制：256MB
题目描述
某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。


网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。


请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。如果最低运输成本是一个负数，它表示在遵循最优路径的情况下，运输过程中反而能够实现盈利。


城市 1 到城市 n 之间可能会出现没有路径的情况，同时保证道路网络中不存在任何负权回路。

输入描述
第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。 

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v （单向图）。

输出描述
如果能够从城市 1 到连通到城市 n， 请输出一个整数，表示运输成本。如果该整数是负数，则表示实现了盈利。如果从城市 1 没有路径可达城市 n，请输出 "unconnected"。
输入示例
6 7
5 6 -2
1 2 1
5 3 1
2 5 2
2 4 -3
4 6 4
1 3 5
输出示例
1
### 提交
**Bellman_ford算法的核心思想是 对所有边进行松弛n-1次操作（n为节点数量），从而求得目标最短路。**
```cpp
# include <iostream>
# include <vector>
# include <climits>
# include <queue>
using namespace std;

class Edge {
    public:
    Edge(int e, int w) {
        end = e;
        weight = w;
    }
    int end, weight;
};
int n, m;
int main() {
    cin>> n >> m;
    vector<vector<Edge> > edges(n+1);
    vector<int> minDir(n+1, INT_MAX);
    for (int i = 0; i < m; i++) {
        int start, end, weight;
        cin>> start >>end >>weight;
        edges[start].push_back(Edge(end, weight));
    }
    queue<int> que;
    minDir[1] = 0;
    que.push(1);
    while(!que.empty()) {
        int now = que.front();
        que.pop();
        for (Edge edge : edges[now]) {
            if (minDir[now] + edge.weight < minDir[edge.end]) {
                minDir[edge.end] =  minDir[now] + edge.weight;
                que.push(edge.end);
            }
        } 
    }
    // for (int relex = 0 ; relex < n; relex ++) {
    //     for (Edge edge: edges) {
    //         if (minDir[edge.start] != INT_MAX) {
    //             minDir[edge.end] = min(minDir[edge.end], minDir[edge.start] + edge.weight);
    //         }
    //     }
    // }
    if (minDir[n] != INT_MAX) {
        cout<< minDir[n];
    } else {
        cout<< "unconnected";
    }
}
```

## 判断是否有负回路

[95. 城市间货物运输 II](https://kamacoder.com/problempage.php?pid=1153)
    
时间限制：1.000S  空间限制：256MB
题目描述
某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。


网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。


然而，在评估从城市 1 到城市 n 的所有可能路径中综合政府补贴后的最低运输成本时，存在一种情况：图中可能出现负权回路。负权回路是指一系列道路的总权值为负，这样的回路使得通过反复经过回路中的道路，理论上可以无限地减少总成本或无限地增加总收益。为了避免货物运输商采用负权回路这种情况无限的赚取政府补贴，算法还需检测这种特殊情况。


请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。同时能够检测并适当处理负权回路的存在。


城市 1 到城市 n 之间可能会出现没有路径的情况

输入描述
第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。 

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

输出描述
如果没有发现负权回路，则输出一个整数，表示从城市 1 到城市 n 的最低运输成本（包括政府补贴）。如果该整数是负数，则表示实现了盈利。如果发现了负权回路的存在，则输出 "circle"。如果从城市 1 无法到达城市 n，则输出 "unconnected"。
输入示例
4 4
1 2 -1
2 3 1
3 1 -1 
3 4 1
输出示例
circle
提示信息
路径中存在负权回路，从 1 -> 2 -> 3 -> 1，总权值为 -1，理论上货物运输商可以在该回路无限循环赚取政府补贴，所以输出 "circle" 表示已经检测出了该种情况。


数据范围：

1 <= n <= 1000；
1 <= m <= 10000;

-100 <= v <= 100;
```cpp
# include <iostream>
# include <vector>
# include <climits>
using namespace std;

class Edge {
    public: 
    int start, end, weight;
};
int n , m;

int main() {
    cin>> n >>m;
    vector<Edge> edges(m);
    for (int i = 0; i < m; i++) {
        cin>> edges[i].start >> edges[i].end >> edges[i].weight;
    }
    vector<int> minDis(n + 1, INT_MAX);
    minDis[1] = 0;
    int minDisN;
    int minDisN1;
    for (int relax =0 ; relax <= n; relax++) {
        for (Edge edge: edges) {
            if (minDis[edge.start] != INT_MAX && minDis[edge.start] +edge.weight < minDis[edge.end]) {
                minDis[edge.end] = minDis[edge.start] +edge.weight;
            } 
        }
        if (relax == n-1) minDisN = minDis[n];
        if (relax == n) minDisN1 = minDis[n];
    }
    if (minDisN == INT_MAX) {
        cout<< "unconnected";
        return 0;
    }
    if (minDisN1 != minDisN) {
        cout<< "circle";
        return 0;
    }
    cout<< minDisN1;
}
```
## 有限次最短路径

[96. 城市间货物运输 III](https://kamacoder.com/problempage.php?pid=1154)
    
时间限制：1.000S  空间限制：256MB
题目描述
某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。


网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。


请计算在最多经过 k 个城市的条件下，从城市 src 到城市 dst 的最低运输成本。

输入描述
第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

最后一行包含三个正整数，src、dst、和 k，src 和 dst 为城市编号，从 src 到 dst 经过的城市数量限制。

输出描述
输出一个整数，表示从城市 src 到城市 dst 的最低运输成本，如果无法在给定经过城市数量限制下找到从 src 到 dst 的路径，则输出 "unreachable"，表示不存在符合条件的运输方案。
输入示例
6 7
1 2 1
2 4 -3
2 5 2
1 3 5
3 5 1
4 6 4
5 6 -2
2 6 1
输出示例
0
提示信息
从 2 -> 5 -> 6 中转一站，运输成本为 0。 

1 <= n <= 1000； 

1 <= m <= 10000; 

-100 <= v <= 100;

### 注意负回路
```cpp
# include <iostream>
# include <vector>
# include <climits>
using namespace std;

class Edge {
    public: 
    int start, end, weight;
};
int n , m, src, dst, k;

int main() {
    cin>> n >>m;
    vector<Edge> edges(m);
    for (int i = 0; i < m; i++) {
        cin>> edges[i].start >> edges[i].end >> edges[i].weight;
    }
    cin >>src >> dst>> k;
    vector<int> minDis(n + 1, INT_MAX);
    vector<int> minDisTemp(n+1);
    minDis[src] = 0;
    int minDisN;
    int minDisN1;
    for (int relax =0 ; relax <= k; relax++) {
        minDisTemp = minDis;
        for (Edge edge: edges) {
            if (minDisTemp[edge.start] != INT_MAX && minDisTemp[edge.start] +edge.weight < minDis[edge.end]) {
                minDis[edge.end] = minDisTemp[edge.start] +edge.weight;
            } 
        }
        // if (relax == n-1) minDisN = minDis[n];
        // if (relax == n) minDisN1 = minDis[n];
    }
    if (minDis[dst] == INT_MAX) {
        cout<< "unreachable";
        return 0;
    }
    cout<< minDis[dst];
}
```