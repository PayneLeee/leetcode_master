# 随想录Day48 57.爬楼梯进阶 322.零钱兑换 279.完全平方数
## 57.爬楼梯进阶
[题目链接](https://kamacoder.com/problempage.php?pid=1067)
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。 

每次你可以爬至多m (1 <= m < n)个台阶。你有多少种不同的方法可以爬到楼顶呢？ 

注意：给定 n 是一个正整数。

### 完全背包
```cpp
# include <iostream>
# include <vector>
using namespace std;
int main() {
    int n, m;
    cin>> n>>m;
    vector<int> dp(n+1, 0);
    dp[0] = 1;
    for( int i = 0; i < n+1; i++) {
        for (int j = 1; j <= m; j++) {
            if (i >= j) {
                dp[i] += dp[i-j];
            }
        }
    }
    cout<< dp[n];
} 
```
## 322.零钱兑换
[题目链接 322](https://leetcode.cn/problems/coin-change/)

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

### 最少的数量-背包
注意溢出，以及-1的判别条件
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int>dp(amount+1, INT_MAX-1);
        dp[0] = 0;
        for(int i = 0; i < amount+1; i++) {
            for(int coin : coins) {
                if (i>= coin) {
                    dp[i] = min(dp[i], dp[i-coin]+1);
                }
            }
        }
        if (dp[amount] == INT_MAX-1) return -1;
        return dp[amount];
    }
};
```
## 278 完全平方数

[题目链接](https://leetcode.cn/problems/perfect-squares/description/)
给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

### 很朴素的完全背包
```CPP
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, n);
        dp[0] = 0;
        for (int i = 0; i < n+1; i++) {
            for (int j = 0; j*j <= i; j++) {
                    dp[i] = min(dp[i], dp[i - j*j] + 1);
            }
        }
        return dp[n];
    }
};
```