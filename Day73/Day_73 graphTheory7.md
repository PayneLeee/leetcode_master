# 随想录 Day 73 堆排序dijkstra Bellman_ford 



## 117. 堆排序dijkstra


[7. 参加科学大会（第六期模拟笔试）](https://kamacoder.com/problempage.php?pid=1047)

时间限制：1.500S  空间限制：128MB
题目描述
小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。

小明的起点是第一个车站，终点是最后一个车站。然而，途中的各个车站之间的道路状况、交通拥堵程度以及可能的自然因素（如天气变化）等不同，这些因素都会影响每条路径的通行时间。

小明希望能选择一条花费时间最少的路线，以确保他能够尽快到达目的地。

输入描述
第一行包含两个正整数，第一个正整数 N 表示一共有 N 个公共汽车站，第二个正整数 M 表示有 M 条公路。 

接下来为 M 行，每行包括三个整数，S、E 和 V，代表了从 S 车站可以单向直达 E 车站，并且需要花费 V 单位的时间。

输出描述
输出一个整数，代表小明从起点到终点所花费的最小时间。
输入示例
7 9
1 2 1
1 3 4
2 3 2
2 4 5
3 4 2
4 5 3
2 6 4
5 7 4
6 7 9
输出示例
12
### 提交
注意这里的n+1 序号不要溢出
```cpp
# include <iostream>
# include <vector>
# include <climits>
# include <queue>
//# include <prioirty_queue>
using namespace std;
int n, m;
class cmp {
    public:
    bool operator() (const pair<int, int>& a, const pair<int, int> &b) {
        return a.second > b.second;
    }
};
int main() {
    cin>> n >> m;
    vector<vector<pair<int, int> > > vertexAndWeight(n + 1);
    for (int i = 0; i < m; i++) {
        pair<int, int> temp;
        int now;
        cin>> now>> temp.first >> temp.second;
       //cout << now <<  temp.first << temp.second << endl;
        vertexAndWeight[now].push_back(temp);
    }
    
    
    vector <bool> visited(n + 1, false);
    vector <int> minDis(n+1, INT_MAX);
    priority_queue<pair<int, int> , vector<pair<int, int>>, cmp>vertexes;
    //visited[0] = true;
    minDis[1] = 0;
    vertexes.push(pair<int, int>(1, 0));
    //vertexes.push(pair<int, int>(7, 12));
    
    while (!vertexes.empty()) {
        pair<int,int> inVertex = vertexes.top();
        vertexes.pop();
        
        //cout<<" current   "<< inVertex.first <<" "<<inVertex.second<<endl;
        
        if (visited[inVertex.first] == true) continue;
        visited[inVertex.first] = true;
        //minDis[inVertex.first] = inVertex.second;
        for (pair<int, int> edge : vertexAndWeight[inVertex.first]) {
            
            if (visited[edge.first] == false) {
                if (edge.second + minDis[inVertex.first] < minDis[edge.first])
                    //cout<< inVertex.first <<" "<<edge.first << " "<< edge.second<< endl;
                    minDis[edge.first] = edge.second + minDis[inVertex.first];
                    //cout<< inVertex.first <<" "<<edge.first << " "<< minDis[edge.first]<< endl;
                    vertexes.push(pair<int, int>(edge.first, minDis[edge.first]));
                    //vertexes.push(pair<int, int>(7,12));
                    //vertexes.push()
            }
        }
    }
    
    if(visited[n] == true){
        cout << minDis[n];
    }else {
        cout << -1;
    }
}
```
## Bellman_ford 

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
using namespace std;

class Edge {
    public:
    int start, end, weight;
};
int n, m;
int main() {
    cin>> n >> m;
    vector<Edge> edges(m);
    vector<int> minDir(n+1, INT_MAX);
    for (int i = 0; i < m; i++) {
        cin>> edges[i].start >>edges[i].end >> edges[i].weight;
    }
    minDir[1] = 0;
    for (int relex = 0 ; relex < n; relex ++) {
        for (Edge edge: edges) {
            if (minDir[edge.start] != INT_MAX) {
                minDir[edge.end] = min(minDir[edge.end], minDir[edge.start] + edge.weight);
            }
        }
    }
    if (minDir[n] != INT_MAX) {
        cout<< minDir[n];
    } else {
        cout<< "unconnected";
    }
}
```