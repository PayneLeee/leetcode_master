# 2024/5/28 Day43 Dynamic Programming  343. 整数拆分  96.不同的二叉搜索树 

## 343. 整数拆分
[题目链接 343](https://leetcode.cn/problems/integer-break/)
给定一个正整数 n ，将其拆分为 k 个 正整数 的和（ k >= 2 ），并使这些整数的乘积最大化。

返回 你可以获得的最大乘积 。

### 提交代码
#### dp
```cpp
class Solution {
public:
    int integerBreak(int n) {
        int dp[n+1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;
        if(n <= 2) return dp[n];
        for (int i = 3; i < n + 1; i++) {
           //cout<< i << endl;
            dp[i] = dp[i-1];
            for (int j = 1; j < i; j++) {
                dp[i] = max(max(dp[i], j* dp[i-j]), j* (i-j));
            }
            //cout<<dp[i] << "  " << i << endl;
        }
        return dp[n];
    }
};
```
#### greedy
```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n == 2) return 1;
        if (n == 3) return 2;
        if (n == 4) return 4;
        int result = 1;
        while (n > 4) {
            result *= 3;
            n -= 3;
        }
        result *= n;
        return result;
    }
};
```

## 96.不同的二叉搜索树 

[题目链接 96](https://leetcode.cn/problems/unique-binary-search-trees/)
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。

### dp

dp[3]，就是 元素1为头结点搜索树的数量 + 元素2为头结点搜索树的数量 + 元素3为头结点搜索树的数量

元素1为头结点搜索树的数量 = 右子树有2个元素的搜索树数量 * 左子树有0个元素的搜索树数量

元素2为头结点搜索树的数量 = 右子树有1个元素的搜索树数量 * 左子树有1个元素的搜索树数量

元素3为头结点搜索树的数量 = 右子树有0个元素的搜索树数量 * 左子树有2个元素的搜索树数量

有2个元素的搜索树数量就是dp[2]。

有1个元素的搜索树数量就是dp[1]。

有0个元素的搜索树数量就是dp[0]。

所以dp[3] = dp[2] * dp[0] + dp[1] * dp[1] + dp[0] * dp[2]

按照头节点第几位去递归

```cpp
class Solution {
public:
    int numTrees(int n) {
        int dp[n+1];
        memset(dp, 0, (n+1) * sizeof(dp[0]));
        dp[0] = 1;
        dp[1] = 1;

        for(int i = 2; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j-1] * dp[i-j];
            }
        }
        return dp[n];
    }
};
```
### 数学
```cpp
class Solution {
public:
    int numTrees(int n) {
        long long C = 1;
        for (int i = 0; i < n; ++i) {
            C = C * 2 * (2 * i + 1) / (i + 2);
        }
        return (int)C;
    }
};
```