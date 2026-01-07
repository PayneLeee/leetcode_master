# 2024/4/24 代码随想录Day 8：344.反转字符串、541. 反转字符串II、剑指Offer 05.替换空格、151.翻转字符串里的单词、剑指Offer58-II.左旋转字符串

## 344.反转字符串

[题目链接 344](https://leetcode.cn/problems/reverse-string/description/) 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

### 第一次提交
非常直观的直接交换
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int half = s.size() * 0.5;
        for (int i = 0; i < half; i++) {
            swap(s[i],s[s.size() - i - 1]);
        }
    }
};
```
### 学习优秀题解
[LeetcodeMaster](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

[vedio](https://www.bilibili.com/video/BV1fV4y17748/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

字符串的特殊：操作方法的区别，有一些特定的操作
#### pythonVersion
使用reversed
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s[:] = reversed(s)
```
使用切片
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s[:] = s[: : -1]
```
## 541. 反转字符串II
[Description 541](https://leetcode.cn/problems/reverse-string-ii/description/) 给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。
### 第一次提交
自然想到用前文结果，注意反转的开闭集合
```cpp
class Solution {
public:
    string reverse(int i, int j, string s) {//[i, j)
        int n = (j - i) * 0.5;
        for (int k = 0; k < n; k++) swap(s[k+i], s[j - k - 1]);
        return s;
    }
    string reverseStr(string s, int k) {
        int cnt = s.size();
        int i = 0;
        while(cnt > 0) {
            s = reverse(2* k * i, min(2* k*i + k, (int) s.size()), s);
            cnt -= 2*k;
            i++;
        }
        return s;
    }
};
```
### 学习优秀题解
[LeetcodeMaster](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)

[vedio](https://www.bilibili.com/video/BV1dT411j7NN/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

可以用for循环直接控制前k个，使用reverse的库函数
#### cpp Version
```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += (2 * k)) {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k );
            } else {
                // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
};
```
#### Python Version
非常优雅的python写法
```python
class Solution(object):
    def reverseStr(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: str
        """
        p = 0
        while p < len(s):
            p2 = p + k
            s = s[:p] + s[p:p2][:: -1] + s[p2:]
            p = p+ 2*k
        return s
```
## 替换数字

[Discription](https://kamacoder.com/problempage.php?pid=1064) 
给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。

例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。

对于输入字符串 "a5b"，函数应该将其转换为 "anumberb"

输入：一个字符串 s,s 仅包含小写字母和数字字符。

输出：打印一个新的字符串，其中每个数字字符都被替换为了number

样例输入：a1b2c3

样例输出：anumberbnumbercnumber

数据范围：1 <= s.length < 10000。

### 第一次提交
很直接的想遇到啥往res里面添加啥
```cpp
# include <iostream>
using namespace std;

bool is_number(char c) {
    if (c >= '0' && c <= '9') return true;
    return false;
}
int main() {
    string s;
    while(cin >> s) {
        string res;
        for (char c : s) {
            if (is_number(c)) {
                res += "number";
            } else {
                res.push_back(c);
            }
        }
        cout<< res;
    }
    
}
```
[随想录](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html) 不用辅助空间的双指针方法

注意这里resize之后的变化
```cpp
# include <iostream>
using namespace std;

bool is_number(char c) {
    if (c >= '0' && c <= '9') return true;
    return false;
}
int main() {
    string s;
    while(cin >> s) {
        int countNumber = 0;
        //int size = s.size();
        for (char c : s) {
            if (is_number(c)) {
                countNumber++;
            }
        }
        int sOldIndex = s.size() - 1;
        s.resize(s.size() + countNumber * 5);
        int sNewIndex = s.size() - 1;
        while(sOldIndex >= 0) {
            if (is_number(s[sOldIndex])){
                s[sNewIndex--] = 'r';
                s[sNewIndex--] = 'e';
                s[sNewIndex--] = 'b';
                s[sNewIndex--] = 'm';
                s[sNewIndex--] = 'u';
                s[sNewIndex--] = 'n';
            }else {
                s[sNewIndex--] = s[sOldIndex];
            }
            sOldIndex --;
        }
        cout << s<< endl;
    }
}
```
## 151.翻转字符串里的单词
[题目链接151](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

[随想录](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)

[视频](https://www.bilibili.com/video/BV1uT41177fX/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

分为三个步骤：

去0： fast slow

反转全部

反转单词
### 第一次提交
``` cpp
class Solution {
public:
    string moveExtraBlank(string s) {
        int fast = 0;
        int slow = 0;
        while(s[fast] == ' ') fast++;
        for (; fast < s.size(); fast++) {
            if (s[fast] != ' ') {
                s[slow ++ ] = s[fast];
            } else if (s[fast] == ' ' && fast + 1 < s.size() && s[fast +1] != ' ') {
                s[slow ++ ] = s[fast];
            }
        }
        s.resize(slow);
        return s;
    };
    string reverseSingleWords (string s) {
        int start = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                reverse(s.begin() + start, s.begin() + i);
                start = i + 1;
            } 
            if (i == s.size() - 1) {
                reverse(s.begin() + start, s.begin() + s.size());// =s.end()
            }
        }
        return s;
    }
    string reverseWords(string s) {
        // remove blank
        s = moveExtraBlank(s);
        //total reverse
        reverse(s.begin(), s.end());
        //reverse all words
        return reverseSingleWords(s);
    }
};
```
## 右旋字符串
[题目链接](https://kamacoder.com/problempage.php?pid=1065)
字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。

例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。

输入：输入共包含两行，第一行为一个正整数 k，代表右旋转的位数。第二行为字符串 s，代表需要旋转的字符串。

输出：输出共一行，为进行了右旋转操作后的字符串。
[随想录](https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

### 第一次提交
注意include algorithm
```cpp
// abcdefg
// gfedcba
// fgabcde
#include <iostream>
#include <algorithm>
using namespace std;
int main() {
    int rev = 0;
    string s;
    cin>> rev;
    cin>> s;
    int n = s.size();
    reverse(s.begin(), s.end());
    reverse(s.begin(), s.begin()+ rev);
    reverse(s.begin()+rev, s.end());
    cout<< s<< endl;
}
```