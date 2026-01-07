# 2024/5/28 Day42 Dynamic Programming   理论基础   62.不同路径  63. 不同路径 II

## 62.不同路径
[题目链接 62](https://leetcode.cn/problems/unique-paths/)
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

### 提交代码
#### 回溯+复用
```cpp
class Solution {
public:
    int traversal(int x, int y,  int history[101][101]) {
        if (x == 1|| y == 1) {
            history[x][y] = 1;
            return 1;
        }
        if (history[x-1][y] == 0 ) {
            history[x-1][y] = traversal(x-1, y, history);
        }
        if (history[x][y-1] == 0 ) {
            history[x][y-1] = traversal(x, y-1, history);
        }
        return history[x-1][y] + history[x][y-1];        
    }
    int uniquePaths(int m, int n) {
        int history[101][101] = {0};
        return traversal(m, n, history);
    }
};
```
#### dp
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        int dp[101][101] = {0};
        dp[0][1] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <=n ; j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m][n];
    }
};
```

## 63. 不同路径 II

[题目链接 63](https://leetcode.cn/problems/unique-paths-ii/description/)
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。
### 提交代码
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int dp[101][101] = {0};
        dp[0][1] = 1;
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j<= n; j++) {
                if (obstacleGrid[i-1][j-1] == 1) dp[i][j] = 0;
                else dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m][n];
    }
};
```