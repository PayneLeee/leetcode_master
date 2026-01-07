# 代码随想录Day28 | 回溯  216.组合总和III 17.电话号码的字母组合


## 216.组合总和III
[题目链接](https://leetcode.cn/problems/combination-sum-iii/description/)

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：

只使用数字1到9

每个数字 最多使用一次 

返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

### 经典回溯方法

注意 k 和 n 的意义

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtrack(int k, int n, int cur) {
        if (k == 0 && n == 0) {
            res.push_back(path);
            return;
        }
        if (cur > 9) return;
        if (n <= 0) return;
        if (k <= 0) return;
        for (; cur <= 9; cur++) {
            if (n - cur >= 0) {
                path.push_back(cur);
                backtrack(k - 1, n - cur, cur + 1);
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        backtrack(k, n, 1);
        return res;
    }
};
```

### 剪枝

[随想录](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html)

## 17.电话号码的字母组合

[题目链接](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/) 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
### 第一次提交

```cpp
class Solution {
public:
    unordered_map<char, string> digToAl;
    
    vector<string> res;
    string path;
    void backTrack(string digits, int cur) {
        if(cur == digits.size()) {
            res.push_back(path);
            return;
        }
        char curDigit = digits[cur];
        for (char c : digToAl[curDigit]) {
            path.push_back(c);
            backTrack(digits, cur+1);
            path.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if (digits == "") return res;
        digToAl['2'] = "abc";
        digToAl['3'] = "def";
        digToAl['4'] = "ghi";
        digToAl['5'] = "jkl";
        digToAl['6'] = "mno";
        digToAl['7'] = "pqrs";
        digToAl['8'] = "tuv";
        digToAl['9'] = "wxyz";
        backTrack(digits, 0);
        return res;
    }
};
```
[随想录](https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html) 思路基本一致，我的方法用map也好访问一点