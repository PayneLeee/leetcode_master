# 2024/5/20 Day34 greedy 455.分发饼干  376. 摆动序列  53. 最大子序和 


## 455.分发饼干
[题目链接 455](https://leetcode.cn/problems/assign-cookies/submissions/533285619/)
假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

### 第一次提交
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int child = 0, biscuit = 0, res = 0;
        for (; child < g.size() && biscuit < s.size();) {
            if (g[child] <= s[biscuit]) {
                res ++;
                child ++;
                biscuit ++;
            } else {
                biscuit ++;
            }
        }
        return res;
    }
};
```
## 376.摆动数列
[题目链接 376](https://leetcode.cn/problems/wiggle-subsequence/description/) 如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。

相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

### 第一次提交
第一反应是理解成一个动态规划问题，对每个元素i维护两个属性：

1. 结尾为该元素，且倒数第二个数小于该元素的最长摆动序列个数 up
2. 结尾为该元素，且倒数第二个数大于该元素的最长摆动序列个数 down
   
转化成了一个动态规划问题,时间空间效率都很好
```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        vector<pair<int, int>> upAndDown(nums.size());
        int res = 1;
        upAndDown[0].first = 1;
        upAndDown[0].second = 1;
        res = 1;
        for (int i = 0; i < nums.size(); i++) {
            int up = 1, down = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) up = max(up, upAndDown[j].second + 1);
                else if (nums[i] < nums[j]) down = max(down, upAndDown[j].first + 1);
            }
            upAndDown[i].first = up;
            upAndDown[i].second = down;
            res = max(res, max(up, down));
        }
        return res;
    }
};
```

### 学习一下greedy
不好想也不好证明，不是很喜欢这个办法
```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        int curDiff = 0; // 当前一对差值
        int preDiff = 0; // 前一对差值
        int result = 1;  // 记录峰值个数，序列默认序列最右边有一个峰值
        for (int i = 0; i < nums.size() - 1; i++) {
            curDiff = nums[i + 1] - nums[i];
            // 出现峰值
            if ((preDiff <= 0 && curDiff > 0) || (preDiff >= 0 && curDiff < 0)) {
                result++;
                preDiff = curDiff; // 注意这里，只在摆动变化的时候更新prediff
            }
        }
        return result;
    }
};
```
## 53. 最大子序和 
[题目链接 53](https://leetcode.cn/problems/maximum-subarray/description/) 给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

子数组
是数组中的一个连续部分。

### 第一次提交
动态规划

以每个元素结尾的最大子序列和
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre = 0;
        int res = nums[0];
        for (int i :nums) {
            pre = max(i, i+pre);
            res = max(res, pre);
        }
        return res;
    }
};
```

### greedy 思路

局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。

全局最优：选取最大“连续和”
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