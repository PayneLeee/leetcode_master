# 2024/4/23 代码随想录Day 6： 202. 快乐数 1. 两数之和  


##  202. 快乐数

set的应用


[题目链接 202](https://leetcode.cn/problems/happy-number/description/) 编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

 
### 第一次提交
[随想录](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)

[leetcode good solution](https://leetcode.cn/problems/happy-number/solutions/224894/kuai-le-shu-by-leetcode-solution/)

三种有趣的做法
1. Hash
2. Fast-slow 检测环
3. 数学推到寻找环
#### c++ version 
1.Hash
``` cpp
class Solution {
public:
    int happy(int n) {
        int res = 0;
        while (n > 0) {
            // cout<< n<< endl;
            res += (n % 10) * (n % 10);
            n /= 10;
        }
        return res;
    }
    bool isHappy(int n) {
        // cout<< happy(n)<<endl;
        // return true;
        unordered_set<int> visited;
        while (n != 1) {
            if (visited.count(n)) return false;
            visited.insert(n);
            n = happy(n);
        }
        return true;
    }
};
```
2. Fast-slow index
``` cpp
class Solution {
public:
    int happy(int n) {
        int res = 0;
        while (n > 0) {
            // cout<< n<< endl;
            res += (n % 10) * (n % 10);
            n /= 10;
        }
        return res;
    }
    bool isHappy(int n) {
        // cout<< happy(n)<<endl;
        // return true;
        unordered_set<int> visited;
        while (n != 1) {
            if (visited.count(n)) return false;
            visited.insert(n);
            n = happy(n);
        }
        return true;
    }
};
```
#### Python Version
1.Hash
divmod 函数
```python
class Solution: 
    def happy(n):
        res = 0
        while n > 0:
            n, digit = divmod(n, 10)
            res += digit**2
        return res
    def isHappy(self, n: int) -> bool:
        seen = set()
        while n != 1 and n not in seen:
            seen.add(n)
            n = happy(n)
        return n == 1
```

1.Fast-Slow
```python
class Solution: 
    def happy(self, n):
        res = 0
        while n > 0:
            n, digit = divmod(n, 10)
            res += digit**2
        return res
    def isHappy(self, n: int) -> bool:
        slow_runner = n
        fast_runner = self.happy(n)
        while fast_runner!= 1 and slow_runner != fast_runner:
            slow_runner = self.happy(slow_runner)
            fast_runner = self.happy(self.happy(fast_runner))
        return fast_runner == 1
```
## 1. 两数之和

[题目链接 1](https://leetcode.cn/problems/two-sum/) 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

[随想录]
### 第一次提交
#### C++ Version
使用hash map
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> map;
        vector<int> res= {};
        for (int i = 0; i < nums.size(); i++) {
            if (map.count(target - nums[i])) {
                res.push_back(map[target-nums[i]]);
                res.push_back(i);
                break;
            } else {
                map[nums[i]] = i;
            }
        }
        return res;
    }
};
```
hash map的其他语法
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map <int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            // 遍历当前元素，并在map中寻找是否有匹配的key
            auto iter = map.find(target - nums[i]); 
            if(iter != map.end()) {
                return {iter->second, i};
            }
            // 如果没找到匹配对，就把访问过的元素和下标加入到map中
            map.insert(pair<int, int>(nums[i], i)); 
        }
        return {};
```



