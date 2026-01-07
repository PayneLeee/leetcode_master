# 随想录Day60 |  回文  647. 回文子串 516.最长回文子序列


##  647. 回文子串
[题目链接 647](https://leetcode.cn/problems/palindromic-substrings/description/)
给你一个字符串 s ，请你统计并返回这个字符串中 回文子串 的数目。

回文字符串 是正着读和倒过来读一样的字符串。

子字符串 是字符串中的由连续字符组成的一个序列。

 
### 第一次提交
注意这里遍历方向
```cpp
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool> > dp(s.size(), vector<bool>(s.size(), false));
        int res = 0;
        for (int j = 0; j < s.size(); j ++) {
            for (int i = j; i >= 0 ; i --) {
                if (s[i] == s[j]) {
                    if (j-i <= 1)  {
                        res ++;
                        dp[i][j] = true;
                    } else if (dp[i+1][j-1] == true) {
                        res ++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        return res;
    }
};
```
### 学习题解
双指针方法 遍历回文中心
```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        s.push_back('#');
        int cnt = 0;
        for (int i = 0; i < n; i ++) {
            //i is the center
            for (int j= 0; j <= i && j+i <= n; j++) {
                if (s[i+j] == s[i-j]) cnt ++;
                else break;
            }
            for (int j = 0; j <= i && j + i + 1 <n +1; j++) {
                    if (s[i-j] == s[i+j+1]) cnt++;
                    else break;
             }           
        }
        return cnt;
    }
};
```
## 516.最长回文子序列
[题目链接 516](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)
给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。
### 提交
```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int> > dp(s.size(), vector<int>(s.size(), 0));
        int res = 1;
        for (int j = 0; j < s.size(); j++) {
            for(int i = j; i >= 0; i--) {
                if (s[i] == s[j]) {
                    if (j - i < 1) {
                        dp[i][j] = 1;
                    } else if (j - i == 1) {
                        dp[i][j] = 2;
                        res = max(res, dp[i][j]);
                    } else if (dp[i+1][j-1] != 0) {
                        dp[i][j] = dp[i+1][j-1] + 2;
                        res = max(res, dp[i][j]);
                    }
                } else dp[i][j] = max(dp[i][j-1], dp[i+1][j]);
            }
        }
        return res;
    }
};
```
### 学习题解优化
[随想录](https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html)
更巧妙的初始化
用到了i >j 的时候都是0， 直接0 + 2 把j-i = 1 的情形涵盖了

是一个技巧不是思想的关键
```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for (int i = 0; i < s.size(); i++) dp[i][i] = 1;
        for (int i = s.size() - 1; i >= 0; i--) {
            for (int j = i + 1; j < s.size(); j++) {
                if (s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size() - 1];
    }
};
```