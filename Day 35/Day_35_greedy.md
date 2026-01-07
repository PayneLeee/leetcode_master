# 2024/5/20 Day35 greedy  122.买卖股票的最佳时机II  55. 跳跃游戏  45.跳跃游戏II 



## 122.买卖股票的最佳时机II
[题目链接 122](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)
给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。

### 第一次提交
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n <= 1) return 0;
        int res = 0;
        for (int i = 1; i < n; i++) {
            if (prices[i] > prices[i-1]) res += prices[i] - prices[i-1];
        }
        return res;
    }
};
```
这是一组很有启发性的动态规划题组

## 55. 跳跃游戏

[题目链接 55](https://leetcode.cn/problems/jump-game/description/) 给你一个非负整数数组 nums ，你最初位于数组的 第一个下标 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 true ；否则，返回 false 。

从后到前greedy
### 第一次提交
 ```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int size = nums.size();
        int reachable = size - 1;
        for (int i = size -1; i >= 0; i--) {
            if (i + nums[i] >= reachable) reachable = i;
            if (reachable == 0) return true;
        }
        return false;
    }
};
 ```

## 45.跳跃游戏II 

[题目链接 45](https://leetcode.cn/problems/jump-game-ii/description/) 给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。

每个元素 nums[i] 表示从索引 i 向前跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:

0 <= j <= nums[i] 
i + j < n
返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        int start = 0;
        int end  = 0;
        int res = 0;
        while(end < n-1 && start <= end) {
            res ++;
            int nextEnd = end;
            for(int i = start; i <= end; i++) {
                if (nums[i] + i > nextEnd) {
                    nextEnd = nums[i] + i;
                }
            }
            start = end +1;
            end = nextEnd;
        }
        return res;
    }
};
```