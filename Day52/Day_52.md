# 随想录Day52 |  123.买卖股票的最佳时机III 188.买卖股票的最佳时机IV

今天的题比较经典，见到的次数也比较多了，跳过二维dp，直接压缩空间写比较精简的题解。
##  123.买卖股票的最佳时机III

[题目链接 123](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/submissions/537819745/)

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。。

### 第一次提交
hold /sold 最高次数dp
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        //hold0, sold 0, hold1, sold1;
        int sold[2] = {0};
        int hold[2] = {0};
        hold[0] = -prices[0];
        hold[1] = -prices[0];
        for (int price: prices) {
            int *tempHold = hold;
            int *tempSold = sold;
            hold[0]  = max(tempHold[0], -price);
            sold[0] = max(tempSold[0], tempHold[0] + price);
            hold[1] = max(tempHold[1], tempSold[0] - price);
            sold[1] = max(tempSold[1], tempHold[1] + price);
        }
        return sold[1];
    }
};
```
## 188.买卖股票的最佳时机IV

[题目链接 188](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/submissions/537828962/)

给你一个整数数组 prices 和一个整数 k ，其中 prices[i] 是某支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。也就是说，你最多可以买 k 次，卖 k 次。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 第一次提交

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int hold[k], sold[k];
        memset(hold, 0, sizeof(hold));
        memset(sold, 0, sizeof(sold));
        for (int p = 0 ; p< prices.size(); p++) {
            for ( int i = 0; i < k ; i++) {
                if (p == 0) {
                    hold[i] = -prices[0];
                } else {
                    if (i == 0) {
                        int temp = hold[0];
                        hold[0] = max(hold[0], -prices[p]);
                        sold[0] = max(sold[0], hold[0] + prices[p]);
                    } else {
                        int temp = hold[i];
                        hold[i] = max(hold[i], sold[i-1]-prices[p]);
                        sold[i] = max(sold[i], hold[i] + prices[p]);
                    }
                }
            }
        }
        return sold[k-1];
    }
};
```