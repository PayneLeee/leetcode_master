# 代码随想录Day31 | 回溯   491.递增子序列 46.全排列 47.全排列 II

## 491.递增子序列
[题目链接 491](https://leetcode.cn/problems/non-decreasing-subsequences/description/)

给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

### 第一次提交
同层不重复的问题如何去重

```cpp
class Solution {
public:
    vector<vector<int> > result;
    vector<int> path;
    void traceback(vector<int> & nums, int cur) {
        int last = path.size()-1;
        if(path.size() > 2) {
                    result.push_back(vector<int> (path.begin() + 1, path.end()));
                }  
        if (cur >= nums.size()) return;
        unordered_set<int> level_visited;
        level_visited.clear();
        for (int i = cur; i < nums.size(); i++) {
            if (nums[i] >= path[last]) {
                if (i > cur && level_visited.count(nums[i])) continue;
                level_visited.insert(nums[i]);
                path.push_back(nums[i]);
                traceback(nums, i+1);
                path.pop_back();                
            }
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        path.push_back(-101);
        traceback(nums, 0);
        return result;
    }
};


```
## 46.全排列
[题目链接 46](https://leetcode.cn/problems/permutations/description/) 给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

### 第一次提交

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtrack(vector<int> &nums, vector<int>& visited, int cnt) {
        if (cnt == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i] == 0) {
                visited[i] = 1;
                path.push_back(nums[i]);
                backtrack(nums, visited, cnt + 1);
                path.pop_back();
                visited[i] = 0;
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> visited(nums.size(), 0);
        backtrack(nums, visited, 0);
        return result;
    }
};
```

## 47.全排列 II
[题目链接 47](https://leetcode.cn/problems/permutations-ii/) 给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

 

### 第一次提交
时间效率很差

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int>path;
    void backtrack(vector<int> nums, vector<int>& visited, int cnt) {
        if (cnt == nums.size()) {
            res.push_back(path);
            return;
        }
        unordered_set<int> vis;
        vis.clear();
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i] == 0 &&!  vis.count(nums[i])) {
                vis.insert(nums[i]);
                visited[i] = 1;
                path.push_back(nums[i]);
                backtrack(nums, visited, cnt + 1);
                visited[i] = 0;
                path.pop_back();
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        //sort(nums.begin(), nums.end());//简化去重过程
        vector<int> visited(nums.size(), 0);
        backtrack(nums, visited, 0);
        return res;        
    }
};
```
这里通过排列可以简化去重过程，只用跟前一个相比


时间效率变好一些
```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int>path;
    void backtrack(vector<int> nums, vector<int>& visited, int cnt) {
        if (cnt == nums.size()) {
            res.push_back(path);
            return;
        }
        int prev = -11;
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i] == 0) {
                if (nums[i] != prev) prev = nums[i];
                else if (nums[i] == prev) continue;
                visited[i] = 1;
                path.push_back(nums[i]);
                backtrack(nums, visited, cnt + 1);
                visited[i] = 0;
                path.pop_back();
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());//简化去重过程
        vector<int> visited(nums.size(), 0);
        backtrack(nums, visited, 0);
        return res;        
    }
};
```