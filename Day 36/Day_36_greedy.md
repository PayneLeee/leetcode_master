# 2024/5/22 Day36 greedy   1005.K次取反后最大化的数组和  134. 加油站 135. 分发糖果  


## 1005.K次取反后最大化的数组和
[题目链接 1005](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/submissions/533789165/) 给你一个整数数组 nums 和一个整数 k ，按以下方法修改该数组：

选择某个下标 i 并将 nums[i] 替换为 -nums[i] 。
重复这个过程恰好 k 次。可以多次选择同一个下标 i 。

以这种方式修改数组后，返回数组 可能的最大和 。

### 提交
按绝对值排序
```cpp
class Solution {
public:
    class cmp{
        public:
        cmp(){}
        bool operator()(const int a, const int b) {
            return abs(a) > abs(b);
        }
    };
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp());
        int minus = 0;
        for(int i = 0; i < nums.size(); i++) {
            if (nums[i] < 0 && k>0) {
                nums[i] *= -1;
               k--;
            }
        }
        if (k %2 == 1) nums[nums.size()-1] *= -1;
        for (int i : nums) cout << i << " ";
        int res = 0;
        for (int i : nums) res += i;
        return res;        
    }
};
```

## 134. 加油站
[题目链接 134](https://leetcode.cn/problems/gas-station/description/) 在一条环路上有 n 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 gas 和 cost ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1 。如果存在解，则 保证 它是 唯一 的。

### 第一次提交
greedy 策略： 找到gas 比cost大且前一个gas比cost 小的第一个站
时间效率很差
```cpp
class Solution {
public:
    bool check(vector<int>& gas, vector<int> &cost, int cur) {
        int remain = 0;
        int n = gas.size();
        for (int i = 0; i < n; i++) {
            remain += gas[(i + cur) % n];
            remain -= cost[(i + cur) % n];
            if (remain < 0) return false;
        }
        return true;
    }
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        if (check(gas, cost, 0)) return 0;
        for (int i = 1; i < n; i++) {
            if (cost[i] - gas[i] <= 0 && cost[(i + n - 1) % n] - gas[(i + n - 1) % n] > 0) {
                if (check(gas, cost, i)) return i;
            }
        }
        return -1;
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html#%E6%80%9D%E8%B7%AF)

#### 思路一
**本质**： 一圈差值都是确定的，在哪个点开始，实际上就是开始的时候加了多少油再走一圈的插值（走一圈都会亏这么多）

**情况一**：如果gas的总和小于cost总和，那么无论从哪里出发，一定是跑不了一圈的

**情况二**：rest[i] = gas[i]-cost[i]为一天剩下的油，i从0开始计算累加到最后一站，如果累加没有出现负数，说明从0出发，油就没有断过，那么0就是起点。

**情况三**：如果累加的最小值是负数，汽车就要从非0节点出发，从后向前，看哪个节点能把这个负数填平，能把这个负数填平的节点就是出发节点。

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int minMinus = 0;
        int sum = 0;
        int n = gas.size();
        for (int i = 0; i < n;i++) {
            sum += gas[i];
            sum -= cost[i];
            minMinus = min(minMinus, sum);
        }
        cout<< minMinus;
        if (sum < 0) return -1;
        if (minMinus>= 0) return 0;
        
        for (int i = gas.size() - 1; i >= 0; i--) {
            int rest = gas[i] - cost[i];
            minMinus += rest;
            if (minMinus >= 0) {
                return i;
            }
        }
        return -1;
    }
};
```
#### 思路二
只要油比cost多，总能有一个点走通

**那么局部最优：当前累加rest[i]的和curSum一旦小于0，起始位置至少要是i+1，因为从i之前开始一定不行。全局最优：找到可以跑一圈的起始位置。**


特别注意这里会出现局部和大于0 然而总和小于0， 仍然没有解的情况出现
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int sum = 0;
        int res = 0;
        int total = 0;
        for (int i = 0; i < gas.size(); i++) {
            sum += gas[i];
            sum -= cost[i];
            total += gas[i];
            total -= cost[i];
            if (sum < 0) {
                res = i + 1;
                sum = 0;
            }
            
        }
        if (total < 0) return -1;
        return res;
    }
};
```

## 135. 分发糖果  
[题目链接 135](https://leetcode.cn/problems/candy/) n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。

 ### 提交
 一次考虑单边，难度在于要够贪，而且这个题基本很难用其他方法做出来
 ```cpp
 class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        int candies[n];
        memset(candies, 0, sizeof(candies));
        // for(int i: candies) cout << i << " ";
        // cout<< endl;
        for (int i = 1; i < n; i++){
            if (ratings[i] > ratings[i-1]) candies[i] = candies[i-1] +1;
        }
        // for(int i: candies) cout << i << " ";
        // cout<< endl;
        for (int i = n-2; i >= 0; i--){
            if (ratings[i] > ratings[i+1] && candies[i] <= candies[i+1]) candies[i] = candies[i+1] +1;
        }
        // for(int i: candies) cout << i << " ";
        // cout<< endl;
        int res = 0;
        for (int i: candies){
            res += i;
        }
        return res + n;
    }
};
```
### 整理一下
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);
        // 从前向后
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) candyVec[i] = candyVec[i - 1] + 1;
        }
        // 从后向前
        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1] ) {
                candyVec[i] = max(candyVec[i], candyVec[i + 1] + 1);
            }
        }
        // 统计结果
        int result = 0;
        for (int i = 0; i < candyVec.size(); i++) result += candyVec[i];
        return result;
    }
};
```