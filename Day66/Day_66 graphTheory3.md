# 随想录 Day 66 110. 字符串接龙 105. 有向图的完全可达性 106. 岛屿的周长
## 110. 字符串接龙

[110. 字符串接龙](https://kamacoder.com/problempage.php?pid=1183)

时间限制：1.000S  空间限制：256MB
题目描述
字典 strList 中从字符串 beginStr 和 endStr 的转换序列是一个按下述规格形成的序列： 


1. 序列中第一个字符串是 beginStr。

2. 序列中最后一个字符串是 endStr。 

3. 每次转换只能改变一个字符。 

4. 转换过程中的中间字符串必须是字典 strList 中的字符串，且strList里的每个字符串只用使用一次。 


给你两个字符串 beginStr 和 endStr 和一个字典 strList，找到从 beginStr 到 endStr 的最短转换序列中的字符串数目。如果不存在这样的转换序列，返回 0。

输入描述
第一行包含一个整数 N，表示字典 strList 中的字符串数量。 第二行包含两个字符串，用空格隔开，分别代表 beginStr 和 endStr。 后续 N 行，每行一个字符串，代表 strList 中的字符串。
输出描述
输出一个整数，代表从 beginStr 转换到 endStr 需要的最短转换序列中的字符串数量。如果不存在这样的转换序列，则输出 0。
输入示例
6
abc def
efc
dbc
ebc
dec
dfc
yhn
输出示例
4
### 提交

和题解的区别在于，题解是每个位置换26个字母，我这里是直接判断一遍词典里面给的词。

**注意string的size得转换成int形式**

```cpp
# include <iostream>
# include <vector>
# include <unordered_set>
# include <string>
# include <queue>
using namespace std;


bool distance(string a, string b) {
    int count = 0;
    if (a.size() != b.size()) return false;
    //cout << a.size();
    for (int i = 0; i< int(a.size()); i++) {
        if (a[i] != b[i]) count++;
        if (count > 1) return false;
    }
    if (count == 1) return true;
    return false;
}
int main() {
    int cnt;
    cin>> cnt;
    string begin, end;
    cin>> begin >> end;
    vector<string> strList(cnt + 1);
    vector<bool> visited(cnt + 1, false);
    for (int i = 0; i < cnt; i++) {
        cin>> strList[i];
    }
    strList[cnt] = end;
    
    queue<string> que;
    que.push(begin);
    int length = 1;
    while(que.size() != 0) {
        int size = que.size();
        length ++;
        for (int i = 0; i < size; i++) {
            string temp = que.front();
            que.pop();
            for (int j = 0; j < cnt + 1; j++) {
                if (visited[j] ==false && distance(temp, strList[j])) {
                    if (j == cnt){
                        cout<< length;
                        return 0;
                    }
                    visited[j] = true;
                    que.push(strList[j]);
                }
            }
        }
    }
    cout<< 0;
    return 0 ;
}
```
## 105. 有向图的完全可达性

[105. 有向图的完全可达性](https://kamacoder.com/problempage.php?pid=1177)

时间限制：1.000S  空间限制：256MB
题目描述
给定一个有向图，包含 N 个节点，节点编号分别为 1，2，...，N。现从 1 号节点开始，如果可以从 1 号节点的边可以到达任何节点，则输出 1，否则输出 -1。
输入描述
第一行包含两个正整数，表示节点数量 N 和边的数量 K。 后续 K 行，每行两个正整数 s 和 t，表示从 s 节点有一条边单向连接到 t 节点。
输出描述
如果可以从 1 号节点的边可以到达任何节点，则输出 1，否则输出 -1。
输入示例
4 4
1 2
2 1
1 3
3 4
输出示例
1
### 直接提交
```cpp
# include<iostream>
# include<vector>
# include<queue>
using namespace std;
int n, m;
int main() {
    cin>> n>> m;
    vector<vector<int> > map(n+1);
    for (int i = 0; i < m; i++) {
        int vertex, to;
        cin>> vertex>> to;
        map[vertex].push_back(to);
    }
    vector<bool> visited(n+1, false);
    visited[0] = true;
    queue<int> que;
    que.push(1);
    visited[1] = true;
    while(que.size() != 0) {
        int now = que.front();
        que.pop();
        for (int i : map[now]) {
            if (visited[i] == false) {
                visited[i] = true;
                que.push(i);
            }
        }
    }
    for (bool b : visited) {
        //cout <<b << " ";
        if (!b) {
            
            cout << -1;
            return 0;
        }
    }
    cout<< 1;
}
```
## 106. 岛屿的周长

[106. 岛屿的周长](https://kamacoder.com/problempage.php?pid=1178)
时间限制：1.000S  空间限制：256MB
题目描述
给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿是被水包围，并且通过水平方向或垂直方向上相邻的陆地连接而成的。


你可以假设矩阵外均被水包围。在矩阵中恰好拥有一个岛屿，假设组成岛屿的陆地边长都为 1，请计算岛屿的周长。岛屿内部没有水域。

输入描述
第一行包含两个整数 N, M，表示矩阵的行数和列数。之后 N 行，每行包含 M 个数字，数字为 1 或者 0，表示岛屿的单元格。
输出描述
输出一个整数，表示岛屿的周长。
输入示例
5 5
0 0 0 0 0 
0 1 0 1 0
0 1 1 1 0
0 1 1 1 0
0 0 0 0 0
输出示例
14
## 直接提交
注意这在边缘的也是岛边
```cpp
# include<iostream>
# include<vector>
using namespace std;
int dirs[4][2] = {
    {0, -1},{0, 1},{1, 0},{-1, 0}
};
int n, m;
int main() {
    cin>> n>> m;
    vector<vector<int> > map(n, vector<int>(m));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin>>map[i][j];
        }
    }
    int res = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (map[i][j] == 1) {
                for (int k = 0; k < 4; k++) {
                    int x = i + dirs[k][0];
                    int y = j + dirs[k][1];
                    if (x >=0 && x < n && y >=0 && y < m) {
                       if (map[x][y] == 0) res++; 
                    } else {
                        res++;
                    }
                }
            }
        }
    }
    cout << res;
}
```