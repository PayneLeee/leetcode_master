# 代码随想录Day29 | 回溯   39. 组合总和  40.组合总和II  131.分割回文串

## 39. 组合总和
[题目链接 39](https://leetcode.cn/problems/combination-sum/description/)

给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 target 的不同组合数少于 150 个。
### 第一次提交
```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backTrack(vector<int>& candidates, int target, int cur) {
        if (target == 0) {
            res.push_back(path);
            return;
        }
        if (cur >= candidates.size()) return;
        if (target < 0) return;
        int cnt = target / candidates[cur] + 1;
        for (int i = 0; i < cnt; i++) {
            for (int j = 0 ; j < i; j ++) {
                path.push_back(candidates[cur]);
            }            
            backTrack(candidates, target - candidates[cur] * i, cur + 1);
            for (int j = 0 ; j < i; j ++) {
                path.pop_back();
            } 
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backTrack(candidates, target, 0);
        return res;
    }
};
```
[随想录](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html#%E6%80%9D%E8%B7%AF) 不需要考虑每个元素选取几个，总体思路不变

代码思路更简单但是剪枝效果没有我的方法好
```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backTrack(vector<int>& candidates, int target, int cur) {
        if (target == 0) {
            res.push_back(path);
            return;
        }
        if (cur >= candidates.size()) return;
        if (target < 0) return;
        for (; cur < candidates.size(); cur++) {
            if (target - candidates[cur] >= 0) {
                path.push_back(candidates[cur]);
                //这里不到下一层去
                backTrack(candidates, target - candidates[cur], cur);
                path.pop_back();
            }            
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backTrack(candidates, target, 0);
        return res;
    }
};
```
## 40.组合总和II
[题目链接 40](https://leetcode.cn/problems/combination-sum-ii/description/) 

给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用 一次 。

注意：解集不能包含重复的组合。 

### 第一次提交
这里和三数之和，四数之和里面使用的去重手段是一样的
```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backTrack(vector<int> candidates, int target, int start) {
        if (target == 0) {
            res.push_back(path);
            return;
        }
        if (start >= candidates.size()) return;
        for (int cur = start;cur < candidates.size(); cur++) {
            //去重
            if (cur > start && candidates[cur] == candidates[cur - 1]) continue;
            if (target - candidates[cur] >= 0) {
                path.push_back(candidates[cur]);
                backTrack(candidates, target - candidates[cur], cur + 1);
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backTrack(candidates, target, 0);
        return res;
    }
};
```

[随想录](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#%E6%80%9D%E8%B7%AF)
这里通过visited来判断是否需要去重

去重回顾：

fast slow： 整个数组去重

cur = cur-1: 指针从前往后

cur = cur+1: 指针从后向前
## 131.分割回文串
[题目链接 131](https://leetcode.cn/problems/palindrome-partitioning/description/)
给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 
回文串
 。返回 s 所有可能的分割方案。
### 第一次提交
```cpp
class Solution {
public:
    bool check_reverse(string s) {
        if (s.size() == 0 ) return false;
        int check = s.size()/2;
        for (int i = 0; i < check; i++) {
            if (s[i] != s[s.size() - i - 1]) return false;
        }
        return true;
    }
    vector<vector<string>> res;
    vector<string> path;
    //string temp; temp 不能作为全局变量，因为每次回溯之间不能累计操作
    void backTrack(string s, int cur) {
        string temp = "";
        if (cur == s.size()) {
            res.push_back(path);
            return;
        }
        for (; cur < s.size(); cur++) {
            temp = temp + s[cur];
            if (!check_reverse(temp)) continue;
            if (check_reverse(temp)) {
                path.push_back(temp);
                backTrack(s, cur+1);
                path.pop_back();
            } 
        }
    }
    vector<vector<string>> partition(string s) {
        backTrack(s, 0);
        return res;
    }
};
```
### 复用
[随想录](https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)：
动态规划 + 哈希表（使用数组）来把所有子串是否回文算出来

#### 用一个umap记录复用信息
```cpp
class Solution {
public:
    bool check_reverse(string s) {
        if (s.size() == 0 ) return false;
        int check = s.size()/2;
        for (int i = 0; i < check; i++) {
            if (s[i] != s[s.size() - i - 1]) return false;
        }
        return true;
    }
    vector<vector<string>> res;
    vector<string> path;
    unordered_map<string, bool> map;
    //string temp; temp 不能作为全局变量，因为每次回溯之间不能累计操作
    void backTrack(string s, int cur) {
        string temp = "";
        if (cur == s.size()) {
            res.push_back(path);
            return;
        }
        for (; cur < s.size(); cur++) {
            temp = temp + s[cur];
            if (!map.count(temp)){
                map[temp] = check_reverse(temp);
            }
            if(map[temp]) {
                path.push_back(temp);
                backTrack(s, cur+1);
                path.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        backTrack(s, 0);
        return res;
    }
};
```
#### 动态规划记录回文串

```cpp
void computePalindrome(const string& s) {
        // isPalindrome[i][j] 代表 s[i:j](双边包括)是否是回文字串 
        isPalindrome.resize(s.size(), vector<bool>(s.size(), false)); // 根据字符串s, 刷新布尔矩阵的大小
        for (int i = s.size() - 1; i >= 0; i--) { 
            // 需要倒序计算, 保证在i行时, i+1行已经计算好了
            for (int j = i; j < s.size(); j++) {
                if (j == i) {isPalindrome[i][j] = true;}
                else if (j - i == 1) {isPalindrome[i][j] = (s[i] == s[j]);}
                else {isPalindrome[i][j] = (s[i] == s[j] && isPalindrome[i+1][j-1]);}
            }
        }
    }
```