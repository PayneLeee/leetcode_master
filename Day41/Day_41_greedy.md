# 2024/5/25 Day39 Dynamic Programming   理论基础  509. 斐波那契数  70. 爬楼梯  746. 使用最小花费爬楼梯 

## 理论基础
1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组（打印）

## 509. 斐波那契数
[题目链接](https://leetcode.cn/problems/fibonacci-number/description/)
斐波那契数 （通常用 F(n) 表示）形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给定 n ，请计算 F(n) 。

### 提交代码
```cpp
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        int prePre = 0;
        int pre = 1;
        int res = 0;
        for (int i = 1; i< n ; i++) {
            res = pre + prePre;
            prePre = pre;
            pre = res;
        }
        return res;
    }
};
```

##  70. 爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
### 提交代码
```cpp
class Solution {
public:
    int climbStairs(int n) {
        int prepre = 1, pre = 2, now = 3;
        if (n == 1) return 1;
        if (n == 2) return 2;
        for (int i = 2; i < n; i++) {
            now = pre + prepre;
            prepre = pre;
            pre = now;
        }
        return now;
    }
};
```

## 746. 使用最小花费爬楼梯 
给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        int dp[n + 1]; //meaning: the min cost reach step i
        memset(dp,  0, (n + 1)* sizeof(dp[0]));
        dp[0] = 0;
        if (cost.size() <= 1) return 0;
        dp[1] = 0;
        for (int i = 2; i < cost.size() + 1; i++) {
            dp[i] = min(dp[i-1] + cost[i-1], dp[i-2]+cost[i-2]);
        }
        return dp[cost.size()];
    }
};
```
### 压缩空间
```cpp
// 版本二
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int dp0 = 0;
        int dp1 = 0;
        for (int i = 2; i <= cost.size(); i++) {
            int dpi = min(dp1 + cost[i - 1], dp0 + cost[i - 2]);
            dp0 = dp1; // 记录一下前两位
            dp1 = dpi;
        }
        return dp1;
    }
};
```