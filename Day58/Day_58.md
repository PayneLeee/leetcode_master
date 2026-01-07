# 随想录Day58 |  392.判断子序列
重复子串专题
## 392.判断子序列
[题目链接 392](https://leetcode.cn/problems/is-subsequence/description/)
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。
### 第一次提交

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.size() < 1) return true;
        if(s.size() > t.size()) return false;
        vector<bool> dp(s.size(), false);
        int now = 0;
        if (s[0] == t[0]) {
            dp[0] = true;
            now = 1;
        }
        for (int i = 0; i < t.size(); i++) {
            if (t[i] == s[now]) {
                dp[now] = true;
                now ++;
            }
        }
        return dp[s.size() - 1];
    }
};
```
### 自我优化和看题解
不需要维护一个动态数组
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.size() < 1) return true;
        if(s.size() > t.size()) return false;
       // vector<bool> dp(s.size(), false);
        int now = 0;
        if (s[0] == t[0]) {
            //dp[0] = true;
            now = 1;
        }
        for (int i = 0; i < t.size(); i++) {
            if (t[i] == s[now]) {
                //dp[now] = true;
                now ++;
            }
        }
        return now == s.size();
    }
};
```
[随想录](https://programmercarl.com/0392.%E5%88%A4%E6%96%AD%E5%AD%90%E5%BA%8F%E5%88%97.html#%E6%80%BB%E7%BB%93) 维护一个表格dp有点太浪费了
