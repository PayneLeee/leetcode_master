## 随想录 Day44 01背包问题，你该了解这些！  01背包问题，你该了解这些！ 滚动数组   416. 分割等和子集 

## 46. 携带研究材料
[题目链接](https://kamacoder.com/problempage.php?pid=1046)

题目描述
小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的空间，并且具有不同的价值。 

小明的行李空间为 N，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料只能选择一次，并且只有选与不选两种选择，不能进行切割。

输入描述
第一行包含两个正整数，第一个整数 M 代表研究材料的种类，第二个正整数 N，代表小明的行李空间。

第二行包含 M 个正整数，代表每种研究材料的所占空间。 

第三行包含 M 个正整数，代表每种研究材料的价值。

输出描述
输出一个整数，代表小明能够携带的研究材料的最大价值。
输入示例
6 1
2 2 3 1 5 2
2 3 1 5 4 3
输出示例
5
提示信息
小明能够携带 6 种研究材料，但是行李空间只有 1，而占用空间为 1 的研究材料价值为 5，所以最终答案输出 5。 

数据范围：
1 <= N <= 5000
1 <= M <= 5000
研究材料占用空间和价值都小于等于 1000

### 第一次提交
分类讨论初始边界非常不优雅

```cpp
# include<iostream>
# include<vector>
using namespace std;

int main() {
    int types, space;
    cin>> types >> space;
    vector<int> occu(types, 0);
    vector<int> values(types, 0);
    for (int i = 0; i < types; i++) {
        cin>> occu[i];
    }
    for (int i = 0; i < types; i++) {
        cin>> values[i];
    }
    // for(auto i : occu) {
    //     cout << i << " ";
    // }
    // cout << endl;
    // for(auto i : values) {
    //     cout << i << " ";
    // }
    // cout << endl;
    //dp[i][j]: materials from 0-i, space limit j
    vector<vector<int> > dp(types, vector<int>(space+1, 0));
    //cout << dp [types -1][space];
    for (int j = 1; j < space + 1; j++) {
        for(int i = 0; i < types; i++) {
            if (occu[i] <= j) {
                //take i
                if (i == 0) {
                    dp[i][j] = values[i];
                } else {
                    int take = dp[i-1][j - occu[i]] + values[i];
                    // do not take i
                    int leave = dp[i-1][j];
                    //cout << take <<" take " << leave << " leave";
                    dp[i][j] = max(take, leave);
                }
                
            }
            else {
                if (i == 0) dp[i][j] = 0;
                else dp[i][j] = dp[i-1][j];
            };
           // cout << i << " "<<j<<" "<< dp[i][j]<< endl;
        }
    }
    cout<< dp[types - 1][space];
}
```
### 进化
优雅的初始化
```cpp
# include<iostream>
# include<vector>
using namespace std;

int main() {
    int types, space;
    cin>> types >> space;
    vector<int> occu(types, 0);
    vector<int> values(types, 0);
    for (int i = 0; i < types; i++) {
        cin>> occu[i];
    }
    for (int i = 0; i < types; i++) {
        cin>> values[i];
    }
    vector<vector<int> > dp(types + 1, vector<int>(space+1, 0));
    for (int j = 1; j < space + 1; j++) {
        for(int i = 1; i < types + 1; i++) {
            if (occu[i-1] <= j) {
                int take = dp[i-1][j - occu[i-1]] + values[i-1];
                int leave = dp[i-1][j];
                dp[i][j] = max(take, leave);
            }
            else {
                dp[i][j] = dp[i-1][j];
            };
        }
    }
    cout<< dp[types][space];
}
```
空间坍缩
```cpp
# include<iostream>
# include<vector>
using namespace std;

int main() {
    int types, space;
    cin>> types >> space;
    vector<int> occu(types, 0);
    vector<int> values(types, 0);
    for (int i = 0; i < types; i++) {
        cin>> occu[i];
    }
    for (int i = 0; i < types; i++) {
        cin>> values[i];
    }
    vector<int> dp(space + 1, 0);
    
    for (int i = 0; i < types; i++) {
        for (int j = space; j >= 0; j--) {
            if (occu[i] <= j) {
                dp[j] = max(dp[j], dp[j - occu[i]] + values[i]);
            } else {
                continue;
            }
        }
    }
    cout<< dp[space];
}
```
## 416. 分割等和子集 
[题目链接](https://leetcode.cn/problems/partition-equal-subset-sum/description/)给你一个 只包含正整数 的 非空 数组 nums 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
### 视为价值和占用空间一样的背包问题
注意这里nums[i] <= j 而不是 小于j
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum %2 != 0) return false;
        int target = sum/2;
        vector<int> dp(target + 1, 0);
        for (int i = 0; i < nums.size(); i++) {
            for(int j = target; j >= nums[i] ; j--) {
                if (nums[i] <= j) {
                    dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
                }
            }
        }
        return dp[target] == target;
    }
};
```
### 其实第一直觉是回溯
超出时间限制
```cpp
class Solution {
public:
    bool backtrack(vector<int>& nums, int cur, int target) {
        if (target == 0) return true;
        if (cur == nums.size()) return false;
        return backtrack(nums, cur + 1, target) || backtrack(nums, cur+1, target - nums[cur]);
    }
    bool canPartition(vector<int>& nums) {
        int total = accumulate(nums.begin(), nums.end(), 0);
        if (total%2 != 0) return false;
        int target = total/2;
        return backtrack(nums,0, target);
    }
};
```
#### 使用数组记忆化搜索
注意这里用map会超时
```cpp
class Solution {
public:
    
    //重复部分为 cur target
    int history[201][20001];
    bool backtrack(vector<int>& nums, int cur, int target) {
        if (target < 0) {
            return false;
        }
        if (target == 0) {
            history[cur][target] = 1;
            return true;
        }
        if (cur == nums.size()) {
            history[cur][target] = 0;
            return false;
        }
        if (history[cur][target] != -1) return history[cur][target];
        bool res = backtrack(nums, cur + 1, target) || backtrack(nums, cur+1, target - nums[cur]);
        history[cur][target] = int(res);
        return res;
    }
    bool canPartition(vector<int>& nums) {
        int total = accumulate(nums.begin(), nums.end(), 0);
        if (total%2 != 0) return false;
        int target = total/2;
        memset(history, -1, sizeof(history));
        return backtrack(nums,0, target);
    }
};
```