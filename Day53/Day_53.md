# 随想录Day53 |  309. 买卖股票的最佳时机含冷冻期 714. 买卖股票的最佳时机含手续费

今天的题也是比较经典，见到的次数也比较多了，跳过二维dp，直接压缩空间写比较精简的题解。

##  309. 买卖股票的最佳时机含冷冻期
[题目链接 309](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)
给定一个整数数组prices，其中第  prices[i] 表示第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 第一次提交
三个状态，本次卖出，之前卖出，正在持有
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int hold = -prices[0], soldPrev = 0, sellNoW = 0;
        for (int i = 1; i < prices.size(); i++) {
            int prevHold = hold, prevSoldprev = soldPrev, prevSellNow = sellNoW;//, prevFreeze = freeze;
            hold = max(prevHold, prevSoldprev - prices[i]);
            soldPrev = max(prevSoldprev, prevSellNow);
            sellNoW = prevHold + prices[i];
        }
        return max(soldPrev, sellNoW);
    }
};
```
## 714. 买卖股票的最佳时机含手续费
[题目链接 714](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

给定一个整数数组 prices，其中 prices[i]表示第 i 天的股票价格 ；整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

### 第一次提交
很简单的小变化

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int hold = -prices[0], sold = 0;
        for (int i = 1; i < prices.size(); i++) {
            int tempHold = hold, tempSold = sold;
            hold = max(hold, sold - prices[i]);
            sold = max(sold, hold + prices[i] - fee);
        }
        return sold;
    }
};
```