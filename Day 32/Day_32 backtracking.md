# 2024/5/19 Day32 backtracking 332.重新安排行程 51. N皇后 37. 解数独 
## 332.重新安排行程
[题目链接 332](https://leetcode.cn/problems/reconstruct-itinerary/description/)
给你一份航线列表 tickets ，其中 tickets[i] = [fromi, toi] 表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。

所有这些机票都属于一个从 JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前。
假定所有机票至少存在一种合理的行程。且所有的机票 必须都用一次 且 只能用一次。

### Container MAP
```cpp
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class map;
```
默认排序存储

访问修改
```cpp
pair<const string, int>& target : targets[result[result.size() - 1]]

for (map<string, int>::iterator it =  targets[result[result.size() - 1]].begin(); it !=  targets[result[result.size() - 1]].end(); it ++)
```
这里iterator 可以直接访问和修改map的第二个参数 it->second

想要拿出来pair<string, int>, 需要*it，如果不加 *
```cpp
no known conversion from 'map<string, int>::iterator' (aka '_Rb_tree_iterator<std::pair<const std::basic_string<char, std::char_traits<char>, std::allocator<char>>, int>>') to 'value_type' (aka 'std::pair<std::basic_string<char>, int>') for 1st argument
```
### 使用map的解法
map 第一种遍历写法
```cpp
class Solution {
public:
    unordered_map<string, map<string, int> > flights;
    int ticketCnt;
    bool backtrack(vector<string>& res) {
        if (res.size() == ticketCnt +1) return true;
        for(pair<const string, int> &flight : flights[res[res.size() - 1]]) {
            if (flight.second > 0) {
                res.push_back(flight.first);
                flight.second --;
                if (backtrack(res)) return true;
                res.pop_back();
                flight.second ++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        ticketCnt = tickets.size();
        vector<string> res;
        res.push_back("JFK");
        for (vector<string> ticket : tickets) {
            flights[ticket[0]][ticket[1]] ++;
        }
        backtrack(res);
        return res;
    }
};
```
map第二种遍历写法
```cpp
for(map<string, int> :: iterator it = flights[res[res.size()-1]].begin(); it != flights[res[res.size()-1]].end(); it++) {
    if (it -> second > 0) {
        res.push_back(it -> first);
        it -> second --;
        if (backtrack(res)) return true;
        res.pop_back();
        it -> second ++;
    }
}        
```
改变参数传递
```cpp
class Solution {
public:
    unordered_map<string, map<string, int> > flights;
    int ticketCnt;
    vector<string> res;
    bool backtrack(string airport) {
        if (res.size() == ticketCnt +1) return true;
        for(map<string, int> :: iterator it = flights[airport].begin(); it != flights[airport].end(); it++) {
            if (it -> second > 0) {
                res.push_back(it -> first);
                it -> second --;
                if (backtrack(it->first)) return true;
                res.pop_back();
                it -> second ++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        ticketCnt = tickets.size();
        res.push_back("JFK");
        for (vector<string> ticket : tickets) {
            flights[ticket[0]][ticket[1]] ++;
        }
        backtrack("JFK");
        return res;
    }
};
```
### Greedy的解法
思路及算法

Hierholzer 算法用于在连通图中寻找欧拉路径，其流程如下：

1. 从起点出发，进行深度优先搜索。

2. 每次沿着某条边从某个顶点移动到另外一个顶点的时候，都需要删除这条边。

3. 如果没有可移动的路径，则将所在节点加入到栈中，并返回。



这样就能保证我们可以「一笔画」地走完所有边，最终的栈中逆序地保存了「一笔画」的结果。我们只要将栈中的内容反转，即可得到答案。

作者：力扣官方题解
链接：https://leetcode.cn/problems/reconstruct-itinerary/solutions/389885/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/

回顾一下priority_Queue的用法
```cpp
class Solution {
public:
    class cmp {
        public:
        cmp() {}
        bool operator() (const std::string& a, const std::string& b) {
            return a > b;
            //小顶堆
        }
    };
    unordered_map<string, priority_queue<string, vector<string>, cmp> > flights;

    vector<string> res;
    void greedy (string airport){
        while(! flights[airport].empty()) {
            string temp = flights[airport].top();
            flights[airport].pop();
            greedy(temp);
        }
        res.push_back(airport);       
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for (vector<string> ticket : tickets) {
            flights[ticket[0]].push(ticket[1]);
        }
        greedy("JFK");
        reverse(res.begin(), res.end());
        return res;
    }
};
```

## 51. N皇后

[题目链接 51](https://leetcode.cn/problems/n-queens/)
按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

### 第一次提交-回溯

这里 i-row确定了唯一一个左斜线
i+row 确定了唯一一个右斜线
```cpp
class Solution {
public:
    vector<vector<string>> result;
    vector<string> map;
    unordered_set<int> col;
    unordered_set<int> left;
    unordered_set<int> right;
    int N;
    void dfs(int row) {
        if (row > N) {
            result.push_back(map);
            return;
        }
        string temp = "";
        for (int j = 1; j <= N; j++) {
            if (col.count(j))
                continue;
            if (left.count(j + row))
                continue;
            if (right.count(j - row))
                continue;
            col.insert(j);
            left.insert(j + row);
            right.insert(j - row);
            string temp = "";
            for (int i = 1; i <= N; i++) {
                if (i == j) {
                    temp += 'Q';
                    continue;
                }
                temp += '.';
            }
            map.push_back(temp);
            dfs(row + 1);
            map.pop_back();
            col.erase(j);
            left.erase(j + row);
            right.erase(j - row);
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        N = n;
        dfs(1);
        return result;
    }
};
```
### 整理代码结构清晰
哈希表用数组，超级提速

**注意数组如何初始化，如何传递参数**
```cpp
class Solution {
public:
    int N;
    vector<vector<string>> res;
    vector<string> path;
    void backtrack(int row, bool* left, bool* right, bool*col) {
        if (row == N) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < N ; i++) {
            if (left[i-row + N] || col[i] || right[i - N + row + N]) continue;
            string level = "";
            for (int j = 0; j < N; j++) {
                if (j == i) level.push_back('Q');
                else level.push_back('.');
            }
            path.push_back(level);
            left[i-row + N] = true;
            col[i] = true;
            right[i - N + row + N] = true;
            backtrack(row + 1, left, right, col);
            path.pop_back();
            left[i-row + N] = false;
            col[i] = false;
            right[i - N + row + N] = false;
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        N = n;
        bool left[2*n] ;
        bool right[2*n];
        bool col[n];
        memset( left, false, sizeof(left));
        memset( right, false, sizeof(right));
        memset( col, false, sizeof(col));
        backtrack(0, left, right, col);
        return res;
    }
};
```
[随想录](https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html#%E6%80%9D%E8%B7%AF) 思路相近，技巧上没有我的方法好

## 37. 解数独 



### 第一次提交
```cpp
class Solution {
public:
    unordered_set <char> row[9];
    unordered_set <char> col[9];
    unordered_set <char> block[9];
    vector <int> empty;
    bool stop = false;
    void backtrack( vector< vector <char> >& board, int now) {
        if(now == empty.size()) {
            stop = true;
            return;
        }
        int i = empty[now]/9, j = empty[now] % 9, index = ((int)i/3) * 3 + (int)j/3;
        for (char ch = '1'; ch <= '9'; ch++) {
            if (row[i].count(ch) || col[j].count(ch) || block[index].count(ch)) continue;
            row[i].insert(ch);
            col[j].insert(ch);
            block[index].insert(ch);
            board[i][j] = ch;
            backtrack(board, now+1);
            if (stop) return;
            row[i].erase(ch);
            col[j].erase(ch);
            block[index].erase(ch);
            board[i][j] = '.';
        }        

    }
    void solveSudoku(vector<vector<char>>& board) {
        //init
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') {
                    empty.push_back(9*i + j);
                    continue;
                } 
                int index = ((int)i/3) * 3 + (int)j/3;
                row[i].insert(board[i][j]);
                col[j].insert(board[i][j]);
                block[index].insert(board[i][j]);
            }
        }
        backtrack(board, 0);
    }
};
```
理解了index之后就很平庸了


## 回溯总结

[随想录](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html#%E6%A3%8B%E7%9B%98%E9%97%AE%E9%A2%98)
