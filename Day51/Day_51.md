# 随想录day 51 | 121. 买卖股票的最佳时机 122.买卖股票的最佳时机II 

## 121. 买卖股票的最佳时机
[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

### 两个方法
#### greedy
主要的卡点在于如何让大值永远在最小值的右边

保持最大值在最右侧
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minimum = INT_MAX;
        int result = 0;
        for (int price : prices) {
            minimum = min(minimum, price);
            result = max(result, price - minimum);
        }
        return result;
    }
};
```
#### DP

sold: 已经卖出
hold：持有

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int sold = 0;
        int hold = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            sold = max(sold, hold + prices[i]);
            hold = max(hold, -prices[i]);
        }
        return sold;
    }
};
```
## 122.买卖股票的最佳时机II 
给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。
### greedy
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
### dp
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int hold = -prices[0];
        int sold = 0;
        for ( int price: prices) {
            int prehold = hold;
            int presold = sold;
            hold = max(hold, sold - price);
            sold = max(sold, hold + price);
        }
        return sold;
    }
};
```
