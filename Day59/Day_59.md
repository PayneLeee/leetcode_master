# 随想录Day59 |  编辑距离  115.不同的子序列 583. 两个字符串的删除操作 72. 编辑距离 

## 总结
**按最后一次（操作最后一个元素的动作）分类讨论**
不同子序列： 是否选取最后一个元素

删除操作：最后删除哪个词的最后一个元素

编辑距离：三种操作的哪种来解决最后一个元素不匹配的情况

##  115.不同的子序列
[题目链接 115](https://leetcode.cn/problems/distinct-subsequences/description/)
给你两个字符串 s 和 t ，统计并返回在 s 的 子序列 中 t 出现的个数，结果需要对 109 + 7 取模。


### 第一次提交
注意初始化
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<unsigned long long int> > dp (s.size() + 1, vector<unsigned long long int>(t.size() + 1, 0));
        for (int i = 0; i <s.size() + 1; i ++ ) dp[i][0] = 1;

        for (int i = 1; i <s.size() + 1; i ++ ) {
            for ( int j = 1; j < t.size() + 1; j++) {
                if (s[i-1] != t[j-1]) dp[i][j] = dp[i-1][j];//注意这里和匹配字符串不同需要
                else {
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                    // 选取最后一个元素// 不选取最后一个元素
                }
                cout<< dp[i][j] <<" ";
            }
            cout<< endl;
        }
        return dp[s.size()][t.size()];
    }
};
```
## 583. 两个字符串的删除操作 
[题目链接 583](https://leetcode.cn/problems/delete-operation-for-two-strings/description/)给定两个单词 word1 和 word2 ，返回使得 word1 和  word2 相同所需的最小步数。

每步 可以删除任意一个字符串中的一个字符。

### 提交
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int> > dp(word1.size() + 1, vector<int>(word2.size() +1, 0));
        for (int i = 0; i <= word1.size(); i++) dp[i][0] = i;
        for (int j = 0; j <= word2.size(); j++) dp[0][j] = j;
        for (int i = 1; i <= word1.size(); i++) {
            for (int j = 1; j <= word2.size(); j++) {
                if (word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = min(dp[i-1][j], dp[i][j-1])+ 1;
                //cout<< dp[i][j]<<" ";
            }
            //cout<<endl;
        }
    
        return dp[word1.size()][word2.size()];
    }
};
```

## 72 编辑距离
[题目链接 72](https://leetcode.cn/problems/edit-distance/description/) 给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数  。

太经典了
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int> > dp(word1.size() + 1, vector<int>(word2.size() + 1,0));
        for (int i = 0; i < word1.size() + 1; i++) dp[i][0] =  i;
        for (int j = 0; j < word2.size() + 1; j++) dp[0][j] = j;
        for (int i = 1; i <= word1.size(); i++) {
            for (int j = 1; j <= word2.size(); j++) {
                if(word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1])) + 1;
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

