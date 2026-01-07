# 随想录 Day 66 101. 孤岛的总面积
## 101. 孤岛的总面积


[101. 孤岛的总面积](https://kamacoder.com/problempage.php?pid=1173)

时间限制：1.000S  空间限制：256MB
题目描述
给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿指的是由水平或垂直方向上相邻的陆地单元格组成的区域，且完全被水域单元格包围。孤岛是那些位于矩阵内部、所有单元格都不接触边缘的岛屿。


现在你需要计算所有孤岛的总面积，岛屿面积的计算方式为组成岛屿的陆地的总数。

输入描述
第一行包含两个整数 N, M，表示矩阵的行数和列数。之后 N 行，每行包含 M 个数字，数字为 1 或者 0。
输出描述
输出一个整数，表示所有孤岛的总面积，如果不存在孤岛，则输出 0。
输入示例
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
输出示例
1
### 提交 bfs/dfs版本
混用了，这里main函数调用bfs和dfs同效果
```cpp
# include<iostream>
# include<vector>
# include<queue>
using namespace std;
int count;
int dirs[4][2] = {
    {1, 0}, {-1, 0}, {0, 1}, {0, -1}
};
int n, m;
void dfs(vector<vector<int> >& map, int i , int j) {
    if (map[i][j] == 0) return;
    count ++;
    map[i][j] = 0;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >= 0 && x < n && y>= 0 &&y < m) {
            dfs(map, x, y);
        }
    }
}
void bfs(vector<vector<int> >& map, int i, int j) {
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
            if (x >= 0 && x< n &&y >= 0 && y < m) {
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
    cin>> n >> m;
    vector<vector<int> >map(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin>>map[i][j];
        }
    }
    count = 0;
    for (int j = 0; j < m; j++) {
        bfs(map, 0, j);
        bfs(map, n-1, j);
    }
    for (int i = 0; i < n; i++) {
        bfs(map, i, 0);
        bfs(map, i, m-1);
    }
    count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            dfs(map, i, j);
        }
    }
    cout << count;
}
```

## 102. 沉没孤岛

[102. 沉没孤岛](https://kamacoder.com/problempage.php?pid=1174)

时间限制：1.000S  空间限制：256MB
题目描述
给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿指的是由水平或垂直方向上相邻的陆地单元格组成的区域，且完全被水域单元格包围。孤岛是那些位于矩阵内部、所有单元格都不接触边缘的岛屿。


现在你需要将所有孤岛“沉没”，即将孤岛中的所有陆地单元格（1）转变为水域单元格（0）。

输入描述
第一行包含两个整数 N, M，表示矩阵的行数和列数。

之后 N 行，每行包含 M 个数字，数字为 1 或者 0，表示岛屿的单元格。

输出描述
输出将孤岛“沉没”之后的岛屿矩阵。 注意：每个元素后面都有一个空格
输入示例
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
输出示例
1 1 0 0 0
1 1 0 0 0
0 0 0 0 0
0 0 0 1 1
提示信息

### 提交 bfs/dfs版本
先遍历所有和边缘相链接的，变成2，从空间上省去了用visitedmap的情况
```cpp
# include<iostream>
# include<vector>
# include<queue>
using namespace std;
int count;
int dirs[4][2] = {
    {1, 0}, {-1, 0}, {0, 1}, {0, -1}
};
int n, m;
void dfs(vector<vector<int> >& map, int i , int j, int changeTo) {
    if (map[i][j] != 1) return;
    count ++;
    map[i][j] = changeTo;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >= 0 && x < n && y>= 0 &&y < m) {
            dfs(map, x, y, changeTo);
        }
    }
}
int main() {
    cin>> n >> m;
    vector<vector<int> >map(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin>>map[i][j];
        }
    }
    count = 0;
    for (int j = 0; j < m; j++) {
        dfs(map, 0, j, 2);
        dfs(map, n-1, j, 2);
    }
    for (int i = 0; i < n; i++) {
        dfs(map, i, 0, 2);
        dfs(map, i, m-1, 2);
    }
    count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            dfs(map, i, j, 0);
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (map[i][j] == 2) {
                map[i][j] = 1;
            }
            cout << map[i][j]<< " ";
        }
        cout << endl;
    }
}
```
## 103. 水流问题
[题目描述](https://kamacoder.com/problempage.php?pid=1175)

现有一个 N × M 的矩阵，每个单元格包含一个数值，这个数值代表该位置的相对高度。矩阵的左边界和上边界被认为是第一组边界，而矩阵的右边界和下边界被视为第二组边界。


矩阵模拟了一个地形，当雨水落在上面时，水会根据地形的倾斜向低处流动，但只能从较高或等高的地点流向较低或等高并且相邻（上下左右方向）的地点。我们的目标是确定那些单元格，从这些单元格出发的水可以达到第一组边界和第二组边界。

输入描述
第一行包含两个整数 N 和 M，分别表示矩阵的行数和列数。 

后续 N 行，每行包含 M 个整数，表示矩阵中的每个单元格的高度。

输出描述
输出共有多行，每行输出两个整数，用一个空格隔开，表示可达第一组边界和第二组边界的单元格的坐标，输出顺序任意。
输入示例
5 5
1 3 1 2 4
1 2 1 3 2
2 4 7 2 1
4 5 6 1 1
1 4 1 2 1
输出示例
0 4
1 3
2 2
3 0
3 1
3 2
4 0
4 1

## 提交
第一次就直接写出了逆流而上方法,整体逻辑和题解基本没区别
```cpp
# include <iostream>
# include <vector>
using namespace std;
int dirs[4][2] = {
    {1, 0},{-1, 0},{0, 1},{0, -1}
};
int n, m;
void dfs(vector<vector<int> > map, vector<vector<int> >& from, int i , int j) {
    if (from[i][j] == 0) return;
    from[i][j] = 0;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >=0 && x < n && y >=0 && y < m) {
            if (map[x][y] >= map[i][j]) {
                dfs(map, from, x, y);
            }
        }
    }
}
int main() {
    cin>> n >> m;
    vector<vector<int> > map(n, vector<int>(m));
    vector<vector<int> > formBolderOne(n, vector<int>(m, 1));
    vector<vector<int> > formBolderTwo(n, vector<int>(m, 1));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin>>map[i][j];
        }
    }
    for (int i = 0; i < n; i++) {
        dfs(map, formBolderOne, i , 0);
        dfs(map, formBolderTwo, i , m-1);
    }
    for (int j = 0; j < m; j++) {
        dfs(map, formBolderOne, 0 , j);
        dfs(map, formBolderTwo, n-1 , j);
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (formBolderOne[i][j] == 0 && formBolderTwo[i][j] == 0) {
                cout << i << " "<<j << endl;
            }
        }
    }
}
```

## 104.建造最大岛屿

[103. 水流问题](https://kamacoder.com/problempage.php?pid=1175)
     
现有一个 N × M 的矩阵，每个单元格包含一个数值，这个数值代表该位置的相对高度。矩阵的左边界和上边界被认为是第一组边界，而矩阵的右边界和下边界被视为第二组边界。


矩阵模拟了一个地形，当雨水落在上面时，水会根据地形的倾斜向低处流动，但只能从较高或等高的地点流向较低或等高并且相邻（上下左右方向）的地点。我们的目标是确定那些单元格，从这些单元格出发的水可以达到第一组边界和第二组边界。

输入描述
第一行包含两个整数 N 和 M，分别表示矩阵的行数和列数。 

后续 N 行，每行包含 M 个整数，表示矩阵中的每个单元格的高度。

输出描述
输出共有多行，每行输出两个整数，用一个空格隔开，表示可达第一组边界和第二组边界的单元格的坐标，输出顺序任意。
输入示例
5 5
1 3 1 2 4
1 2 1 3 2
2 4 7 2 1
4 5 6 1 1
1 4 1 2 1
输出示例
0 4
1 3
2 2
3 0
3 1
3 2
4 0
4 1
### 提交
耗费了较多时间，
1：注意输入，要检测
2：与一块儿地多次接壤，会重复计算
```cpp
# include<iostream>
# include<vector>
# include<unordered_map>
# include<unordered_set>
using namespace std;
int n, m;
int count;
int res;
int dirs[4][2] = {
    {1, 0}, {-1, 0}, {0, 1}, {0, -1}
};
void dfs(vector<vector<int> >& map, int i , int j, int changeTo) {
    if (map[i][j] != 1) return;
    map[i][j] = changeTo;
    count ++;
    for (int k = 0; k < 4; k++) {
        int x = i + dirs[k][0];
        int y = j + dirs[k][1];
        if (x >=0 && x < n && y >=0 && y < m) {
            dfs(map, x, y, changeTo);
        }
    }
}
int main() {
    cin >>n >> m;
    vector<vector<int> > map(n, vector<int>(m, 0));
    unordered_map<int, int> match;
    match[0] = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >>map[i][j];
        }
    }
    count= 0;
    res = 0;
    int type = 1;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (map[i][j] == 1) {
                type ++;
                dfs(map, i, j , type);
                match[type] = count;
                res = max(count, res);
                count = 0;
            }
        }
    }
    unordered_set<int> set;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (map[i][j] == 0) {
                int temp = 0;
                for (int k = 0; k < 4; k++) {
                    int x = i + dirs[k][0];
                    int y = j + dirs[k][1];
                    if (x >=0 && x < n && y >=0 && y < m) {
                        if (!set.count(map[x][y])) {
                            set.insert(map[x][y]);
                            temp += match[map[x][y]];
                        }
                    }
                }
                set.clear();
                res = max(res, temp + 1);
            }
        }
        
    }
    cout<< res;
}
```