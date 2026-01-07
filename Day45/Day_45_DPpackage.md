## 随想录 Day45  1049. 最后一块石头的重量 II  494. 目标和  474.一和零  


## 1049. 最后一块石头的重量 II
[题目链接](https://leetcode.cn/problems/last-stone-weight-ii/)

有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

### 第一次提交
转化为已经熟悉的背包问题

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int all = accumulate(stones.begin(), stones.end(), 0);
        int half = all / 2;
        vector<int> dp(half + 1, 0);
        for (int i = 0; i < stones.size(); i++) {
            for (int j = half; j >= 0; j --) {
                if (stones[i] <= j) {
                    dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
                }
            }
        }
        return all - 2* dp[half];
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html#%E6%80%9D%E8%B7%AF) 思路相似

## 494. 目标和

[题目链接 494](https://leetcode.cn/problems/target-sum/description/) 给你一个非负整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

提示：

1 <= nums.length <= 20

0 <= nums[i] <= 1000

0 <= sum(nums[i]) <= 1000

-1000 <= target <= 1000

### 第一次提交
直觉直接上回溯+记忆化搜索

```cpp
class Solution {
public:
    int res;
    int his[21][40001];
    int backtrack(vector<int>& nums, int cur, int target) {
        //cout << cur <<" cur " <<target<< " target "<<endl;
        if (target == 0 && cur == nums.size()) {
            his[cur][target + 20000] = 1;
            return 1;
        }
        if (cur == nums.size()) {
            return 0;
        }
        if (his[cur][target + 20000] == 0) {
            his[cur][target + 20000] = backtrack(nums, cur+1, target + nums[cur]) + backtrack(nums, cur+1, target - nums[cur]);
        }
        return his[cur][target + 20000];
    }
    int findTargetSumWays(vector<int>& nums, int target) {
        res = 0;
        memset(his, 0, sizeof(his));
        return backtrack(nums, 0, target);        
    }
};
```

### 学习题解，转化为背包问题
如何转化为01背包问题呢。

假设加法的总和为x，那么减法对应的总和就是sum - x。

所以我们要求的是 x - (sum - x) = target

x = (target + sum) / 2

此时问题就转化为，装满容量为x的背包，有几种方法。

这里的x，就是bagSize，也就是我们后面要求的背包容量。
```cpp

class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int total = accumulate(nums.begin(), nums.end(), 0);
        if (abs(target) > total) return false;
        if ((target + total) % 2 == 1) return 0;
        int bagSize = (target + total) / 2;
        int dp[bagSize + 1];
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        for(int i = 0; i < nums.size(); i++) {
            for (int j = bagSize; j >= 0; j--) {
                if (nums[i] <= j) {
                    dp[j] = dp[j] + dp[j - nums[i]];
                }
            }
        }
        return dp[bagSize];
    }
};
```

## 474.一和零  

[题目链接 474](https://leetcode.cn/problems/ones-and-zeroes/description/) 给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

 ### 第一次提交
延用背包问题解决思路

 ```cpp
 class Solution {
public:
    vector<int> transform(string str) {
        vector<int> res(2, 0);
        for(char c: str) {
            if (c == '0') res[0]++;
            if (c == '1') res[1]++;
        }
        return res;
    }
    int findMaxForm(vector<string>& strs, int m, int n) {
        int dp[m+1][n+1];
        memset(dp, 0, sizeof(dp));
        vector<vector<int>> cnt;
        for(int i = 0; i < strs.size(); i++) {
            cnt.push_back(transform(strs[i]));
        }
        for (int i = 0; i < cnt.size(); i++) {
            for ( int j = m; j >= 0; j--) {
                for(int k  = n; k >= 0; k--) {
                    if (cnt[i][0] <= j && cnt[i][1] <= k) {
                        dp[j][k] = max(dp[j][k], dp[j -cnt[i][0]][k - cnt[i][1]] + 1);
                    }
                }
            }
        }
        return dp[m][n];
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html#%E6%80%9D%E8%B7%AF) 思路上没区别
