# 随想录 Day 74  Floyd / A* 


## Bellman_ford  队列优化

[97. 小明逛公园](https://kamacoder.com/problempage.php?pid=1155)

时间限制：1.000S  空间限制：256MB
题目描述
小明喜欢去公园散步，公园内布置了许多的景点，相互之间通过小路连接，小明希望在观看景点的同时，能够节省体力，走最短的路径。 


给定一个公园景点图，图中有 N 个景点（编号为 1 到 N），以及 M 条双向道路连接着这些景点。每条道路上行走的距离都是已知的。


小明有 Q 个观景计划，每个计划都有一个起点 start 和一个终点 end，表示他想从景点 start 前往景点 end。由于小明希望节省体力，他想知道每个观景计划中从起点到终点的最短路径长度。 请你帮助小明计算出每个观景计划的最短路径长度。

输入描述
第一行包含两个整数 N, M, 分别表示景点的数量和道路的数量。 

接下来的 M 行，每行包含三个整数 u, v, w，表示景点 u 和景点 v 之间有一条长度为 w 的双向道路。 

接下里的一行包含一个整数 Q，表示观景计划的数量。 

接下来的 Q 行，每行包含两个整数 start, end，表示一个观景计划的起点和终点。

输出描述
对于每个观景计划，输出一行表示从起点到终点的最短路径长度。如果两个景点之间不存在路径，则输出 -1。
输入示例
7 3
2 3 4
3 6 6
4 7 8
2
2 3
3 4
输出示例
4
-1
### 提交
1、确定dp数组（dp table）以及下标的含义

这里我们用 grid数组来存图，那就把dp数组命名为 grid。

grid[i][j][k] = m，表示 节点i 到 节点j 以[1...k] 集合为中间节点的最短距离为m。

```cpp
# include <iostream>
# include <vector>
using namespace std;
int n, m;
int cnt;
int main() {
    cin>> n >>m;
    vector<vector<vector<int> > >  grid(n + 1, vector<vector<int>>(n + 1, vector<int>(n + 1, 10005))); 
    for (int c = 0; c < m; c++) {
        int i, j , weight;
        cin>> i >>j >> weight;
        //cout << i << j << weight<<endl;
        grid[i][j][0] = weight;
        //注意这里是双向图
        grid[j][i][0] = weight;
    }
    for (int k = 1 ; k < n+1; k++) {
        for (int i = 1; i < n+1; i++) {
            for (int j = 1; j < n+1; j++){
                grid[i][j][k] = min(grid[i][j][k-1], grid[i][k][k-1] + grid[k][j][k-1]);
            }
        }
    }
    cin>> cnt;
    for (int i = 0; i < cnt; i++) {
        int start, end;
        cin>> start >> end;
        if (grid[start][end][n] > 10000){
            cout<< -1<< endl;
        } else {
            cout<< grid[start][end][n]<<endl;
        }
    }
}
```

## A* method

[原理介绍](https://www.redblobgames.com/pathfinding/a-star/introduction.html)

题目：
[126. 骑士的攻击](https://kamacoder.com/problempage.php?pid=1203)

时间限制：1.000S  空间限制：256MB
题目描述
在象棋中，马和象的移动规则分别是“马走日”和“象走田”。现给定骑士的起始坐标和目标坐标，要求根据骑士的移动规则，计算从起点到达目标点所需的最短步数。


棋盘大小 1000 x 1000（棋盘的 x 和 y 坐标均在 [1, 1000] 区间内，包含边界）

输入描述
第一行包含一个整数 n，表示测试用例的数量，1 <= n <= 100。

接下来的 n 行，每行包含四个整数 a1, a2, b1, b2，分别表示骑士的起始位置 (a1, a2) 和目标位置 (b1, b2)。

输出描述
输出共 n 行，每行输出一个整数，表示骑士从起点到目标点的最短路径长度。
输入示例
6
5 2 5 4
1 1 2 2
1 1 8 8
1 1 8 7
2 1 3 3
4 6 4 6
输出示例
2
4
6
5
1
0
### 提交
注意几个细节

memst 再string.h包中

注意huristicbic 必须带const 因为会被operator调用

A* 算法中没走一步要* 5  (1^2 + 2^2)


pq.push(vector<int>{x, y}); 注意这里的语法，容易写成小括号，还不好查错误。
```cpp
# include <iostream>
# include <vector>
# include <queue>
# include<string.h>
using namespace std;
int n;
int moves[1001][1001];
int dir[8][2]={-2,-1,-2,1,-1,2,1,2,2,1,2,-1,1,-2,-1,-2};
vector<int> start(2);
vector<int> target(2);
int Heuristic(const vector<int>& a, const vector<int>&b)  { // 欧拉距离
    return (a[1] - b[1]) * (a[1] - b[1]) + (a[0] - b[0]) * (a[0] - b[0]); // 统一不开根号，这样可以提高精度
};
class cmp {
    public:
    bool operator() (const vector<int>& a, const vector<int>&b) {
        return moves[a[0]][a[1]] * 5 + Heuristic(a, target) > moves[b[0]][b[1]] * 5 + Heuristic(b, target);
    }
};
int shortestPath() {
    if (start == target) return 0;
    priority_queue<vector<int>, vector<vector<int> >, cmp > pq;
    //<vector<int> > q;
    //q.push(start);
    pq.push(start);
    //cout<< start[0] << "  " <<start[1] <<  endl;
    while(pq.size() != 0) {
        vector<int> now = pq.top();
        pq.pop();
        //cout << "now[0] " << now[0] << "  now[1] " <<now[1] <<"  move[now[0]][now[1]]  "<<moves[now[0]][now[1]] <<endl;
        for (int idx = 0; idx < 8; idx++) {
            int x = now[0] + dir[idx][0];
            int y = now[1] + dir[idx][1];
            //cout << "now[0] " << now[0] << "now[1] " <<now[1] <<"move[now[0]][now[1]]  "<<moves[now[0]][now[1]] <<endl;
            if (x == target[0] && y == target[1]) {
                return moves[now[0]][now[1]] + 1;
            }
            if (x >= 1 && x <= 1000 && y >= 1 && y <= 1000) {
                if (moves[x][y] == 0) {
                    
                    moves[x][y] = moves[now[0]][now[1]] + 1;
                    pq.push(vector<int>{x, y});
                    //cout << " x "<<x << " y "<< y <<" moves[x][y]" << moves[x][y]<<  endl;
                }
            }
            
        }
        
    }
    return -1;
};
int main() {
    cin>> n;
    for(int t = 0; t < n; t ++) {
        memset(moves,0,sizeof(moves));
        cin>> start[0] >> start[1]>> target[0] >> target[1];
        //cout << start[0] << " " << start[1]<< " " << target[0]<< " "<< target[1] << endl;
        cout << shortestPath()<<endl;
    }
}
```
## A* 补充题
https://leetcode.cn/problems/shortest-path-in-binary-matrix/
### 1091. 二进制矩阵中的最短路径
提示
中等
给你一个 n x n 的二进制矩阵 grid 中，返回矩阵中最短 畅通路径 的长度。如果不存在这样的路径，返回 -1 。
二进制矩阵中的 畅通路径 是一条从 左上角 单元格（即，(0, 0)）到 右下角 单元格（即，(n - 1, n - 1)）的路径，该路径同时满足下述要求：
路径途经的所有单元格的值都是 0 。
路径中所有相邻的单元格应当在 8 个方向之一 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。
畅通路径的长度 是该路径途经的单元格总数。

 #### Odinary bfs method 
比较难看并不推荐
```CPP
class Solution {
public:
    int dirs[8][2] = {
        {-1, 1}, {0, 1}, {1, 1},
        {-1, 0},         {1, 0},
        {-1, -1},{0, -1}, {1, -1}
    };
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        queue<pair<int,int> > que;
        int n = grid.size(); 
        if (grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;
        if ( n == 1 ) return 1;
        que.emplace(0, 0);
        grid[0][0] = 1;
        int res = 1;
        while(!que.empty()) {
            res ++;
            int size = que.size();
            while (size--) {
                pair<int, int> temp = que.front();
                que.pop();
                for (int i = 0; i < 8; i++) {
                    int x = temp.first + dirs[i][0];
                    int y = temp.second + dirs[i][1];
                    if (x == n-1 && y == n-1) return res;
                    if (x >= 0 && x < n && y >= 0 && y < n) {
                        if (grid[x][y] == 0) {
                            que.emplace(x, y);
                            grid[x][y] = 1;
                        }
                    }
                }
            }
        }
        return -1;
                }                                                                                                                                      
};
```
#### bfs with class method 
```CPP
class Solution {
public:
    int dirs[8][2] = {
        {-1, 1}, {0, 1}, {1, 1},
        {-1, 0},         {1, 0},
        {-1, -1}, {0, -1}, {1, -1}
    };
    class Node{
        public:
        int x, y, dis;
        Node(int a, int b, int c = 0) {
            x = a;
            y = b;
            dis = c;
        }
    };
    

    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        queue<Node> que;
        int n = grid.size();

        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) return -1;
        if (n == 1) return 1;

        Node start(0, 0, 1);
        que.push(start);
        grid[0][0] = 1;
        while (!que.empty()) {
            Node nod = que.front();
            que.pop();
            for (int i = 0; i < 8; i++) {
                int dx = nod.x + dirs[i][0];
                int dy = nod.y + dirs[i][1];
                int disten = nod.dis + 1;
                if (dx == n-1 && dy == n-1) {
                    return disten;
                }
                if (dx < n && dx >= 0 && dy < n && dy >= 0 && grid[dx][dy] == 0) {
                    grid[dx][dy] = 1;
                    Node temp(dx, dy, disten);
                    que.push(temp);
                }
            }
        }
        return -1;
    }
};
```

#### A* method
```CPP
class Solution {
public:
    int dirs[8][2] = {
        {-1, 1}, {0, 1}, {1, 1},
        {-1, 0},         {1, 0},
        {-1, -1}, {0, -1}, {1, -1}
    };

    class Node{
        public:
        int x, y, dis, h;
        Node(int a, int b, int c) {
            x = a;
            y = b;
            dis = c;
            h = c + max(-a, -b);//针对切比雪夫距离的优化
        }
        friend bool operator <(Node f1, Node f2) {
            return f1.h > f2.h;
        }
    };   


    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        priority_queue<Node> que;
        vector<vector<int> > minmap(n, vector<int>(n, 10000));
//记录当前最小
        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) return -1;
        if (n == 1) return 1;

        Node start(0, 0, 1);
        que.push(start);
        //grid[0][0] = 1;
        minmap[0][0] = start.h;

        while (!que.empty()) {
            Node nod = que.top();
            que.pop();
            for (int i = 0; i < 8; i++) {
                int dx = nod.x + dirs[i][0];
                int dy = nod.y + dirs[i][1];
                int disten = nod.dis + 1;
                if (dx == n-1 && dy == n-1) {
                    return disten;
                }
                if (dx < n && dx >= 0 && dy < n && dy >= 0 && grid[dx][dy] == 0) {
                    Node temp(dx, dy, disten); 
                    if( minmap[dx][dy] > temp.h){
                        minmap[dx][dy] = temp.h;
                        que.push(temp);
                    }
                }
            }
        }
        return -1;
    }
};
```
### sliding-puzzle
https://leetcode.cn/problems/sliding-puzzle/

在一个 2 x 3 的板上（board）有 5 块砖瓦，用数字 1~5 来表示, 以及一块空缺用 0 来表示。一次 移动 定义为选择 0 与一个相邻的数字（上下左右）进行交换.
最终当板 board 的结果是 [[1,2,3],[4,5,0]] 谜板被解开。
给出一个谜板的初始状态 board ，返回最少可以通过多少次移动解开谜板，如果不能解开谜板，则返回 -1 。
#### BFS
```CPP
class Solution {
public:
    string boardToString (vector<vector<int>> board) {
        string ret = "";
        for(int j = 0 ; j < 2; j++) {
            for (int i = 0; i < 3; i++) {
                ret += char(board[j][i] + '0');
            }
        }
        return ret;
    };

    vector<vector<int>> trans = {{1, 3}, {0, 2, 4}, {1, 5}, {0, 4}, {1, 3, 5}, {2, 4}};

    vector<string> nextStates(string now) {
        vector<string> ret;
        int loc = now.find('0');
        for (int exchangeLoc : trans[loc]) {
            swap(now[loc], now[exchangeLoc]);
            ret.push_back(now);
            swap(now[loc], now[exchangeLoc]);
        }
        return ret;
    };

    int slidingPuzzle(vector<vector<int>>& board) {
        string start = boardToString(board);
        if (start == "123450") return 0;
        unordered_set<string> reached;
        reached.insert(start);

        queue<pair<string, int> >q;
        q.emplace(start, 0);

        while(!q.empty()) {
            string now = q.front().first;
            int step = q.front().second;
            q.pop();
            step ++;
            for (string nxt : nextStates(now)) {
                if (nxt == "123450") return step;
                if (!reached.count(nxt)) {
                    q.emplace(nxt, step);
                    reached.insert(nxt);
                }
            }
        }
        
        return -1;
    }
};
```
#### A* methods

```CPP
class Solution {
public:
    string boardToString (vector<vector<int>> board) {
        string ret = "";
        for(int j = 0 ; j < 2; j++) {
            for (int i = 0; i < 3; i++) {
                ret += char(board[j][i] + '0');
            }
        }
        return ret;
    };

    vector<vector<int> > trans = {{1, 3}, {0, 2, 4}, {1, 5}, {0, 4}, {1, 3, 5}, {2, 4}};
    class States {
        public:
        vector<vector<int> > Manhatten = {
            {0, 1, 2, 1, 2, 3},
            {1, 0, 1, 2, 1, 2},
            {2, 1, 0, 3, 2, 1},
            {1, 2, 3, 0, 1, 2},
            {2, 1, 2, 1, 0, 1},
            {3, 2, 1, 2, 1, 0}
        };
        string state;
        int step;
        int h;
        int f;        
        

        States(string st, int sp) {
            state = st;
            step = sp;
            h = get_h(st);
            f = h + sp;
        };

        int get_h(string st) {
            int ret = 0;
            for (int i = 0; i <6; i++) {
                if (st[i] != '0') {
                    ret += Manhatten[i][st[i] - '1'];
                }
            }
            return ret;
        };

        friend bool operator < (const States &a, const States &b) {
            return a.f > b.f;
        };

    };


    vector<string> nextStates(string now) {
        vector<string> ret;
        int loc = now.find('0');
        for (int exchangeLoc : trans[loc]) {
            swap(now[loc], now[exchangeLoc]);
            ret.push_back(now);
            swap(now[loc], now[exchangeLoc]);
        }
        return ret;
    };

    int slidingPuzzle(vector<vector<int>>& board) {
        string start = boardToString(board);
        if (start == "123450") return 0;
        States init(start, 0);
        unordered_map<string, int> min_reached;
        min_reached[init.state] = init.f;

        priority_queue<States>q;
        q.push(init);

        while(!q.empty()) {
            States current = q.top();
            q.pop();
            string now = current.state;
            int step = current.step;
            step ++;
            for (string nxt : nextStates(now)) {
                if (nxt == "123450") return step;
                States next(nxt, step);
                if (!min_reached.count(nxt) || min_reached[nxt] > next.f ) {
                    min_reached[nxt] = next.f;
                    q.push(next);
                }
            }
        }

        return -1;
    }
};
```

