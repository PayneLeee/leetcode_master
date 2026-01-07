# 随想录 Day 65 99. 岛屿数量

## 99. 岛屿数量 

[卡码网链接](https://kamacoder.com/problempage.php?pid=1171)

题目描述
给定一个由 1（陆地）和 0（水）组成的矩阵，你需要计算岛屿的数量。岛屿由水平方向或垂直方向上相邻的陆地连接而成，并且四周都是水域。你可以假设矩阵外均被水包围。

输入描述
第一行包含两个整数 N, M，表示矩阵的行数和列数。

后续 N 行，每行包含 M 个数字，数字为 1 或者 0。

输出描述
输出一个整数，表示岛屿的数量。如果不存在岛屿，则输出 0。
输入示例
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
输出示例
3
### 提交 bfs/dfs版本
```cpp
# include<iostream>
# include<vector>
# include<queue>
using namespace std;

int res;
int dirs[4][2] = { {-1, 0}, {1, 0}, {0, -1}, {0, 1}};
void dfs(vector<vector<int> >& map, int i, int j, int maxi, int maxj) {
    if (map[i][j] == 0) return;
    map[i][j] = 0;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >= 0 && x< maxi &&y >= 0 && y < maxj) {
            dfs(map, x, y, maxi, maxj);
        }
    }
}

void bfs(vector<vector<int> >& map, int i, int j, int maxi, int maxj) {
    if (map[i][j] == 0) return;
    map[i][j] = 0;
    vector<int> now;
    now.push_back(i);
    now.push_back(j);
    queue<vector<int> > que;
    que.push(now);
    while(que.size() != 0) {
        i = que.front()[0];
        j = que.front()[1];
        que.pop();
        for (int k = 0; k < 4; k++) {
            int x = i + dirs[k][0];
            int y = j + dirs[k][1];
            if (x >= 0 && x< maxi &&y >= 0 && y < maxj) {
                if(map[x][y] == 1) {
                    map[x][y] = 0;
                    now[0] = x;
                    now[1] = y;
                    que.push(now);
                }
            }
        }
    }
}

int main() {
    int n, m;
    cin>> n>> m;
    res = 0;
    vector<vector<int> > map(n,vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++){
            cin >> map[i][j];
        }
    }
    //check input
    // for (int i = 0; i < n; i++) {
    //     for (int j = 0; j < m; j++){
    //         cout << map[i][j] << " ";
    //     }
    //     cout << endl;
    // }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++){
            if (map[i][j] == 1) {
                res++;
                dfs(map, i, j, n, m);
            }
        }
    }
    cout<< res;
}
```

## 100. 岛屿的最大面积

[题目描述](https://kamacoder.com/problempage.php?pid=1172)

给定一个由 1（陆地）和 0（水）组成的矩阵，计算岛屿的最大面积。岛屿面积的计算方式为组成岛屿的陆地的总数。岛屿由水平方向或垂直方向上相邻的陆地连接而成，并且四周都是水域。你可以假设矩阵外均被水包围。
输入描述
第一行包含两个整数 N, M，表示矩阵的行数和列数。后续 N 行，每行包含 M 个数字，数字为 1 或者 0，表示岛屿的单元格。
输出描述
输出一个整数，表示岛屿的最大面积。如果不存在岛屿，则输出 0。
输入示例
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
输出示例
4

### 提交 bfs/dfs版本
和上一道题只有少量区别
```cpp
# include<iostream>
# include<vector>
# include<queue>
using namespace std;

int count;
int res;
int dirs[4][2] = { {-1, 0}, {1, 0}, {0, -1}, {0, 1}};
void dfs(vector<vector<int> >& map, int i, int j, int maxi, int maxj) {
    if (map[i][j] == 0) return;
    count ++;
    map[i][j] = 0;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >= 0 && x< maxi &&y >= 0 && y < maxj) {
            dfs(map, x, y, maxi, maxj);
        }
    }
}

void bfs(vector<vector<int> >& map, int i, int j, int maxi, int maxj) {
    if (map[i][j] == 0) return;
    count ++;
    map[i][j] = 0;
    vector<int> now;
    now.push_back(i);
    now.push_back(j);
    queue<vector<int> > que;
    que.push(now);
    while(que.size() != 0) {
        i = que.front()[0];
        j = que.front()[1];
        que.pop();
        for (int k = 0; k < 4; k++) {
            int x = i + dirs[k][0];
            int y = j + dirs[k][1];
            if (x >= 0 && x< maxi &&y >= 0 && y < maxj) {
                if(map[x][y] == 1) {
                    count ++;
                    map[x][y] = 0;
                    now[0] = x;
                    now[1] = y;
                    que.push(now);
                }
            }
        }
    }
}

int main() {
    int n, m;
    cin>> n>> m;
    count = 0;
    res = 0;
    vector<vector<int> > map(n,vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++){
            cin >> map[i][j];
        }
    }
    //check input
    // for (int i = 0; i < n; i++) {
    //     for (int j = 0; j < m; j++){
    //         cout << map[i][j] << " ";
    //     }
    //     cout << endl;
    // }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++){
            if (map[i][j] == 1) {
                bfs(map, i, j, n, m);
                res = max(res, count);
                count = 0;
            }
        }
    }
    cout<< res;
}
```