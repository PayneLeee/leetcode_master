# 随想录 Day 49 139.单词拆分 多重背包

## 139 单词拆分
[题目链接 139](https://leetcode.cn/problems/word-break/description/)给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

### 第一次提交
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<int> length;
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for ( int i = 0; i < s.size()+ 1; i++) {
            for (string word: wordDict) {
                if (word.size() <= i) {
                    string temp(s.begin()+i- word.size(), s.begin() + i);
                    if (temp == word) {
                        dp[i] = dp[i] || dp[i- word.size()];
                    }
                }
            }
        }
        return dp[s.size()];
    }
   
};
```

## 多重背包
[随想录](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html#%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85) 多重化为单重

[题目链接](https://kamacoder.com/problempage.php?pid=1066)
你是一名宇航员，即将前往一个遥远的行星。在这个行星上，有许多不同类型的矿石资源，每种矿石都有不同的重要性和价值。你需要选择哪些矿石带回地球，但你的宇航舱有一定的容量限制。 

给定一个宇航舱，最大容量为 C。现在有 N 种不同类型的矿石，每种矿石有一个重量 w[i]，一个价值 v[i]，以及最多 k[i] 个可用。不同类型的矿石在地球上的市场价值不同。你需要计算如何在不超过宇航舱容量的情况下，最大化你所能获取的总价值。
输入描述
输入共包括四行，第一行包含两个整数 C 和 N，分别表示宇航舱的容量和矿石的种类数量。 

接下来的三行，每行包含 N 个正整数。具体如下： 

第二行包含 N 个整数，表示 N 种矿石的重量。 

第三行包含 N 个整数，表示 N 种矿石的价格。 

第四行包含 N 个整数，表示 N 种矿石的可用数量上限。

输出描述
输出一个整数，代表获取的最大价值。
输入示例
10 3
1 3 4
15 20 30
2 3 2
输出示例
90
提示信息
数据范围：
1 <= C <= 10000;
1 <= N <= 10000;
1 <= w[i], v[i], k[i] <= 10000;

### 提交
```cpp
# include<iostream>
# include<vector>
using namespace std;
int main() {
    int c, n;
    cin>> c >> n;
    vector<int> weight;
    vector<int> value;
    vector<int> cnt;
    for (int i =0 ; i < n; i++) {
        int temp;
        cin>> temp;
        weight.push_back(temp);
    }
    for (int i =0 ; i < n; i++) {
    int temp;
    cin>> temp;
    value.push_back(temp);
    }
    for (int i =0 ; i < n; i++) {
    int temp;
    cin>> temp;
    cnt.push_back(temp);
    }
    vector<int> fweight;
    vector<int> fval;
    for (int i = 0; i < n; i++) {
        while(cnt[i] --) {
            fweight.push_back(weight[i]);
            fval.push_back(value[i]);
        }
    }
    vector<int> dp(c+1, 0);
    for(int i = 0; i < fweight.size(); i++) {
        for (int j = c; j>=0; j--) {
            if (fweight[i] <= j) {
                dp[j] = max(dp[j], dp[j - fweight[i]] + fval[i]);
            }
        }
    }
    cout<< dp[c];
}
```
### 重构
```cpp
#include<iostream>
#include<vector>
using namespace std;
int main() {
    int bagWeight,n;
    cin >> bagWeight >> n;
    vector<int> weight(n, 0);
    vector<int> value(n, 0);
    vector<int> nums(n, 0);
    for (int i = 0; i < n; i++) cin >> weight[i];
    for (int i = 0; i < n; i++) cin >> value[i];
    for (int i = 0; i < n; i++) cin >> nums[i];

    vector<int> dp(bagWeight + 1, 0);

    for(int i = 0; i < n; i++) { // 遍历物品
        for(int j = bagWeight; j >= 0; j--) { // 遍历背包容量
            // 以上为01背包，然后加一个遍历个数
            for (int k = 1; k <= nums[i]; k++) { // 遍历个数
                if(j - k * weight[i] >= 0) {
                    dp[j] = max(dp[j], dp[j - k * weight[i]] + k * value[i]);
                }
            }
        }
    }

    cout << dp[bagWeight] << endl;
}
```