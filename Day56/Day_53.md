# 随想录Day56 |  300.最长递增子序列 674. 最长连续递增序列 718. 最长重复子数组

重复子串专题

##  300.最长递增子序列
[题目链接 300](https://leetcode.cn/problems/longest-increasing-subsequence/)

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的
子序列


### 第一次提交
以第i 个数为结尾的最长递增子序列长度来dp
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int>dp(nums.size(), 1);
        int res = 1;
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i ; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            res = max(res, dp[i]);
        }
        return res;
    }
    
};
```
## 674. 最长连续递增序列
[题目链接 674](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

### 第一次提交
递归公式简单很多

```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);
        int res = 1;
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] > nums[i-1]) dp[i] = dp[i-1] + 1;
            else dp[i] = 1;
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

## 718. 最长重复子数组
[题目链接 718](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/submissions/538634294/) 给两个整数数组 nums1 和 nums2 ，返回 两个数组中 公共的 、长度最长的子数组的长度 。

 

示例 1：

输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
示例 2：

输入：nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
输出：5

### 第一次提交
```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp( nums1.size(),vector<int>(nums2.size(), 0));
        int res = 0;
        //if (nums1[0] == nums2[0]) dp[0][0] = 1;
        for (int i = 0; i < nums2.size(); i++) {
            if (nums1[0] == nums2[i]) {
                dp[0][i] = 1;
                res = 1;
            }
        }
        for (int i = 0; i < nums1.size(); i++) {
            if (nums2[0] == nums1[i]) {
                dp[i][0] = 1;
                res = 1;
            }
        }
        for ( int i = 1; i < nums1.size(); i++) {
            for (int j = 1; j < nums2.size(); j ++) {
                if (nums1[i] == nums2[j]) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = 0;
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }
};
```

### 学习题解

dp数组定义：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为dp[i][j]。

[随想录](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
#### 空间压缩，滚动数组
```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<int> dp(nums2.size(), 0) ;
        int res = 0;
        for (int j = nums2.size() -1 ; j >= 0; j--) {
            dp[j] = nums1[0] == nums2[j] ? 1 : 0;
            res = max(res, dp[j]);
        }
        for (int i = 1; i < nums1.size(); i++) {
            for (int j = nums2.size() -1 ; j >= 0; j--) {
                if(j == 0) {
                    dp[j] = nums1[i] == nums2[0] ? 1 : 0;
                } else {
                    dp[j] = nums1[i] == nums2[j] ? dp[j-1] + 1 : 0;
                }
                res = max(res, dp[j]);
            }
        }  
        return res;
    }
        
};
```