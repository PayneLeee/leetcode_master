# 随想录 Day 72 拓扑排序、最短路径 朴素dijkstra


## 117. 软件构建

[117. 软件构建](https://kamacoder.com/problempage.php?pid=1191)

时间限制：1.000S  空间限制：256MB
题目描述
某个大型软件项目的构建系统拥有 N 个文件，文件编号从 0 到 N - 1，在这些文件中，某些文件依赖于其他文件的内容，这意味着如果文件 A 依赖于文件 B，则必须在处理文件 A 之前处理文件 B （0 <= A, B <= N - 1）。请编写一个算法，用于确定文件处理的顺序。
输入描述
第一行输入两个正整数 N, M。表示 N 个文件之间拥有 M 条依赖关系。

后续 M 行，每行两个正整数 S 和 T，表示 T 文件依赖于 S 文件。

输出描述
输出共一行，如果能处理成功，则输出文件顺序，用空格隔开。 

如果不能成功处理（相互依赖），则输出 -1。

输入示例
5 4
0 1
0 2
1 3
2 4
输出示例
0 1 2 3 4
### 提交：这里没必要像题解一样用umap
接下来我给出 拓扑排序的过程，其实就两步：

找到入度为0 的节点，加入结果集

将该节点从图中移除
```cpp
# include <iostream>
# include <vector>
# include <queue>
using namespace std;
int n , m;
int main() {
    cin>> n >> m;
    vector<int> inCnt(n , 0);
    vector<vector<int> > edges(n);
    for (int i = 0; i < m; i++) {
        int start, end;
        cin >> start >> end;
        edges[start].push_back(end);
        inCnt[end]++;
    }
    // for (int i = 0; i < n; i++) {
    //     cout << inCnt[i]<<" ";
    // }
    //cout << endl;
    //int cnt = 0;
    queue<int> out;
    for (int i = 0; i < n; i++) {
        if (inCnt[i] == 0) {
            out.push(i);
        }
    }
    vector<int> res;
    while(out.size() != 0 ) {
       int now = out.front();
       out.pop();
       res.push_back(now);
       for (int j : edges[now]) {
           inCnt[j]--;
           if (inCnt[j] == 0) out.push(j);
       }
    }
    ///注意这里输出格式
    if (res.size() == n) {
        for (int i = 0; i < n; i++) {
            if (i == n-1) cout << res[i];
            else cout<< res[i] << " ";
        }
    } else cout << -1;
}
```
## 47. 参加科学大会

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

```cpp
# include <iostream>
# include <vector>
# include <climits>
using namespace std;
int n, m;

int main() {
    cin>> n >> m;
    vector<vector<int> > map(n + 1, vector<int>(n + 1, INT_MAX));
    for (int i = 0; i < m ; i++) {
        int start, end, weight;
        cin>> start>> end >> weight;
        //cout << start << end << weight << endl;
        map[start][end] = weight;
    }
    vector <bool> visited(n + 1, false);
    vector <int> minDis(n+1, INT_MAX);
    visited[0] = true;
    minDis[0] = 0;
    minDis[1] = 0;
    visited[1] = true;
    int now = 1;
    int res = 0;
    for (int t = 0; t < n-1; t++) {
        //update minDis
        int minD = INT_MAX, inIndex = -1;
        for (int i = 1; i <= n; i++) {
            if (visited[i] == false) {
                //cout << i << "original " << minDis[i] << endl;
                if (map[now][i] != INT_MAX) {
                    minDis[i] = min(minDis[i], minDis[now] + map[now][i]);
                }
                if (minDis[i] < minD) {
                    minD = minDis[i];
                    inIndex = i;
                }
                //cout << i << "changed " << minDis[i] << endl;
            }
        }
        //find target point and visited = true
        if (minD == INT_MAX) break;
        //cout << inIndex << "chosen one " << minDis[inIndex] << endl;
        visited[inIndex] = true;
        now = inIndex;
        //res = minD;
    }
    
    if(visited[n] == true){
        cout << minDis[n];
    }else {
        cout << -1;
    }
    //cout << res;
}
```