# 随想录Day57 |  1143.最长公共子序列 1143.最长公共子序列 53. 最大子序和

重复子串专题

##  53. 最大子序和
[题目链接 1143](https://leetcode.cn/problems/longest-common-subsequence/description/)

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。


### 第一次提交
太经典了
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size() + 1;
        int n = text2.size() + 1;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if(text1[i-1] == text2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[m-1][n-1];
    }
};
```
## 1035. 不相交的线
[题目链接 1035](https://leetcode.cn/problems/uncrossed-lines/description/)

在两条独立的水平线上按给定的顺序写下 nums1 和 nums2 中的整数。

现在，可以绘制一些连接两个数字 nums1[i] 和 nums2[j] 的直线，这些直线需要同时满足：

 nums1[i] == nums2[j]
且绘制的直线不与任何其他连线（非水平线）相交。
请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

### 第一次提交
和上一道题完全一样

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size() + 1;
        int n = nums2.size() + 1;
        vector<vector<int> > dp(m, vector<int> (n, 0));
        for ( int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (nums1[i - 1] == nums2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```

## 53. 最大子数组和
[题目链接 53](https://leetcode.cn/problems/maximum-subarray/description/)
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

子数组
是数组中的一个连续部分。
 

### 第一次提交
递归方法
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int>dp(nums.size() + 1, 0);
        int res = INT_MIN;
        for (int i = 1; i < nums.size() + 1; i++) {
            dp[i] = max(dp[i-1] + nums[i - 1], nums[i - 1]);
            res = max(res, dp[i]);
        }
        return res;
    }
};
```
回忆一下greedy
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int cnt = 0;
        int res = nums[0];
        for (int i : nums) {
            if (cnt < 0) cnt = i;
            else cnt += i;
            res = max(res, cnt);
        }
        return res;
    }
};
```