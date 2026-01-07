# 随想录 day 46  完全背包 518. 零钱兑换 II 377. 组合总和 Ⅳ
[题目链接](https://kamacoder.com/problempage.php?pid=1052)
题目描述
小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的重量，并且具有不同的价值。

小明的行李箱所能承担的总重量为 N，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料可以选择无数次，并且可以重复选择。

输入描述
第一行包含两个整数，N，V，分别表示研究材料的种类和行李空间 

接下来包含 N 行，每行两个整数 wi 和 vi，代表第 i 种研究材料的重量和价值
### 第一次提交
```cpp
# include<iostream>
# include<vector>
using namespace std;

int main() {
    int type, maxBag;
    cin >> type >> maxBag;
    vector<int> value;
    vector<int> mass;
    for (int i = 0; i < type; i++) {
        int v, m;
        cin>> m>> v;
        value.push_back(v);
        mass.push_back(m);
    }
    vector<int> dp(maxBag +1, 0);
    for(int i = 0; i < type; i++) {
        for (int j = 1; j <= maxBag; j++) {
            if(mass[i] <= j) {
                dp[j] = max(dp[j], dp[j- mass[i]] + value[i]);
            }
        }
        //for(auto x: dp) cout<< x << " ";
       // cout<< endl;
    }
    cout<< dp[maxBag]<<endl;
}
```
### 探讨
遍历顺序可以交换
## 518. 零钱兑换 II
[题目链接 518](https://leetcode.cn/problems/coin-change-ii/description/)

给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

### 和完全背包问题区别不大
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0) ;
        dp[0] = 1;
        for (int coin : coins) {
            for (int i = 0; i < amount+1; i++) {
                if (i >= coin) {
                    dp[i] += dp[i-coin];
                }
            }
        }
        return dp[amount];
    }
};
```
### note
本题中我们是外层for循环遍历物品（钱币），内层for遍历背包（金钱总额），还是外层for遍历背包（金钱总额），内层for循环遍历物品（钱币）呢？

我在动态规划：关于完全背包，你该了解这些！ (opens new window)中讲解了完全背包的两个for循环的先后顺序都是可以的。

但本题就不行了！

因为纯完全背包求得装满背包的最大价值是多少，和凑成总和的元素有没有顺序没关系，即：有顺序也行，没有顺序也行！

而本题要求凑成总和的组合数，元素之间明确要求没有顺序。

所以纯完全背包是能凑成总和就行，不用管怎么凑的。

本题是求凑出来的方案个数，且每个方案个数是为组合数。

那么本题，两个for循环的先后顺序可就有说法了。

我们先来看 外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。

代码如下：
```cpp
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```
假设：coins[0] = 1，coins[1] = 5。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。

所以这种遍历顺序中dp[j]里计算的是组合数！

如果把两个for交换顺序，代码如下：
```cpp
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```
背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。

此时dp[j]里算出来的就是排列数！

可能这里很多同学还不是很理解，建议动手把这两种方案的dp数组数值变化打印出来，对比看一看！（实践出真知）
## 377. 组合总和 Ⅳ
[题目链接](https://leetcode.cn/problems/combination-sum-iv/description/) 

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < target+1; i++) {
            for (int num: nums) {            
                if (num <= i && dp[i] < INT_MAX - dp[i - num]) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
};
```
### note
如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

如果把遍历nums（物品）放在外循环，遍历target的作为内循环的话，举一个例子：计算dp[4]的时候，结果集只有 {1,3} 这样的集合，不会有{3,1}这样的集合，因为nums遍历放在外层，3只能出现在1后面！

所以本题遍历顺序最终遍历顺序：target（背包）放在外循环，将nums（物品）放在内循环，内循环从前到后遍历。