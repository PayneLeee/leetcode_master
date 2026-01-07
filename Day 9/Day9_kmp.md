# 2024/4/25 代码随想录Day 9：28. 实现 strStr() 459.重复的子字符串 字符串总结  双指针回顾 


## 28. 实现 strStr()

[题目链接 28](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/) 给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串的第一个匹配项的下标（下标从 0 开始）。如果 needle 不是 haystack 的一部分，则返回  -1 。

### 第一次提交

常看常新
[随想录](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)

[SolutionDemo](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/575568/shua-chuan-lc-shuang-bai-po-su-jie-fa-km-tb86/)

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size();
        int n = needle.size();
        if (n == 0) return 0;
        vector<int> next(n);
        for (int front = 0, back = 1; back < n; back++) {
            while(front > 0 && needle[front] != needle[back]) front = next[front-1];
            if (needle[front] == needle[back]) front++;
            next[back] = front;
        }
        for (int pat = 0,str = 0; str < m; str++) {
            while(pat > 0 && haystack[str] != needle[pat]) pat = next[pat-1];
            if (haystack[str] == needle[pat]) pat++;
            if (pat == n) return str - n + 1;
        }
        return -1;
    }
};
```
整理一下
```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size();
        int n = needle.size();
        vector<int> next(n);
        for (int i = 1, j = 0; i < n; i++) {
            while (j > 0 && needle[i] != needle[j])
                j = next[j - 1];
            if (needle[i] == needle[j])
                j++;
            next[i] = j;
        }
        for (int i = 0, j = 0; i < m; i++) {
            while (j > 0 && haystack[i] != needle[j])
                j = next[j - 1];
            if (haystack[i] == needle[j])
                j++;
            if (j == n)
                return i - n + 1;
        }
        return -1;
    }
};
```
本质上findNext是在match的时候记录了一些中间结果
```cpp
class Solution {
public:
    int matchMessagePattern(string message, string pattern, vector<int> &next, bool getNext) {
        int m = message.size();
        int n = pattern.size();
        if (getNext) next = vector<int>(m);
        for (int i = 0, j = 0; i < m; i++) {
            if (getNext && i == 0) continue;
            while(j > 0 && message[i] != pattern[j]) j = next[j - 1];
            if (message[i] == pattern[j]) j++;
            if (getNext) next[i] = j;
            if (j == n) return i - n + 1;
        }
        return -1;
    }
    int strStr(string haystack, string needle) {
        int m = haystack.size();
        int n = needle.size();
        vector<int> next(n);
        int _ = matchMessagePattern(needle, needle,next, true);        
        return matchMessagePattern(haystack, needle, next, false);
    }
};
```
#### 学习一下python 函数

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        try:
            return haystack.index(needle)
        except ValueError:
            return -1

class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```
## 459.重复的子字符串
[题目链接 459](https://leetcode.cn/problems/repeated-substring-pattern/description/)

### 第一次提交
太暴力了
```cpp
class Solution {
public:
    bool check(string s, int i) {//abac
        int n = s.size();
        int times = n/i;
        cout <<times<<endl;
        for (int r = 0 ; r < i; r++) {
            for (int t = 0; t < times - 1 ; t++) {
                //cout <<t* i + r<< " "<< t*i + t + r<<endl;
                if(s[t* i + r] != s[t*i + i + r]) return false;
            }
        }
        return true;
    }
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        int rep = n/2;
        for ( int i = 1; i <= rep; i++) {
            if (n % i == 0){
                cout<< s << i<< endl;
                if (check(s, i)) return true;
            }
        }
        return false;
    }
};
```
### 学习优秀题解
[随想录](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

[视频](https://www.bilibili.com/video/BV1cg41127fw/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

#### 优雅的数学方法
移动匹配方法
```cpp
class Solution {
public:
    bool kmp(string quary, string pattern) {
        int m = quary.size();
        int n = pattern.size();
        vector<int>next(n);
        for (int i = 1 , j = 0; i < n; i++) {
            while(j > 0 && pattern[i] != pattern[j]) j = next[j-1];
            if (pattern[i] == pattern[j]) j++;
            next[i] = j;
        }
        for (int i = 0 , j = 0; i < m; i++) {
            while(j > 0 && quary[i] != pattern[j]) j = next[j-1];
            if (quary[i] == pattern[j]) j++;
            if (j == n) return true;
        }
        return false;
    }
    bool repeatedSubstringPattern(string s) {
        string doubles = s+s;
        doubles.pop_back();
        doubles.erase(0, 1);//这里注意erase的用法，起始位置+个数
        cout<< doubles<< endl;
        return kmp(doubles, s);
    }
};
```
学习一下库函数
```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) != s.size();
    }
};
```
[find 函数](https://cplusplus.com/reference/string/string/find/)

string (1)	
size_t find (const string& str, size_t pos = 0) const;

c-string (2)	
size_t find (const char* s, size_t pos = 0) const;

buffer (3)	
size_t find (const char* s, size_t pos, size_t n) const;

character (4)	
size_t find (char c, size_t pos = 0) const;

#### 更优雅的前后字串方法

类KMP方法，构造next数组

比较惊艳的是两个点，**一个是前后缀的解决思路**，还有一个是**判断条件是整除为0**
```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        //find next(n-1)
        int n = s.size();
        vector<int> next(n);
        for (int i = 1, j = 0; i < n; i++) {
            while(j > 0 && s[i] != s[j]) j = next[j -1];
            if (s[i] == s[j]) j++;
            next[i] = j;
        }
        return next[n - 1] != 0 && n % (n - next[n - 1]) == 0;
    }
};
```
## 字符串&双指针总结
几道经典题重新写一下
### 双指针法
[151.反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

分解成去除和反转两个功能，注意最后判断边界
1. 去除多余0
2. 反转
3. 反转单个单词
   
```cpp
class Solution {
public:
    void removeExtraBlank(string& s) {//slow fast index
        int slow = 0, fast = 0;
        int n = s.size();
        while(s[fast] ==' ') fast++;
        for (; fast < n; fast++) {
            if (s[fast] != ' ') {
                s[slow] = s[fast];
                slow++;
            } else if (s[fast] == ' ' && fast+1 < s.size()&& s[fast+1] != ' ') {
                s[slow] = s[fast];
                slow++;
            }
        }
        s.resize(slow);
    }
    void reverse(string& s, int start, int end) {
        int half = (end - start) / 2;
        for (int i = 0; i < half; i++) {
            swap(s[start + i], s[end - 1 - i]);
        }

    }
    string reverseWords(string s) {
        removeExtraBlank(s);
        cout<< s << endl;
        reverse(s, 0, s.size());
        int start = 0;
        for (int i = 0; i <= s.size(); i++) {
            if (i == s.size()) {
                reverse(s, start, i);
                break;
            }
            if (s[i] == ' ') {
                reverse(s, start, i);
                start = i + 1;
            }            
        }
        return s;
    }
};
```
[142.环形列表2](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

快慢指针，暴力用哈希表，优雅的数学方法

这次写需要注意while大循环终止条件的判定，如果只判定fast不为空， fast->next->next 有可能会访问到空指针

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == nullptr) return nullptr;
        if (head->next == nullptr) return nullptr;
        ListNode * slow = head->next, * fast = head->next->next;
        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                fast = head;
                while(slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }
                return fast;
            }
        }
        return nullptr;
    }
};
```


[15.三数之和](https://leetcode.cn/problems/3sum/description/)

细节重点在去重，这一遍第一次尝试出现的问题是去重之前应该先left，right各内缩。
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) { 
        vector<vector<int>> res; 
        int n = nums.size();
        sort(nums.begin(), nums.end());
        for(int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;
            if (nums[i] > 0) return res;
            int right = nums.size() - 1;
            int left = i+1;
            while(left < right) {
                if (nums[i] + nums[left] + nums[right] == 0) {
                    res.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    left ++;//这里要注意
                    right --;
                    while(left > i+1 && nums[left] == nums[left-1] && left < right) left++;
                    while(right < nums.size()-1  && nums[right] == nums[right + 1] && left < right) right--;
                } else if (nums[i] + nums[left] + nums[right] > 0) {
                    right --;
                } else {
                    left ++;
                }
            }
        }
        return res;
    }
};
```

[18.四数之和](https://leetcode.cn/problems/4sum/description/)

同上，多的只是工程量，还有剪枝的思考，今天不重新写了



### KMP
今天的内容，再优雅ac一次

[KMP 28](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

一把过
```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size();
        int n = needle.size();
        vector<int> next(n);
        for (int i = 1, j = 0; i< n; i++) {
            while(j > 0 && needle[i] != needle[j]) j = next[j-1];
            if (needle[i] == needle[j]) j++;
            next[i] = j;
        }
        for (int i = 0, j = 0; i < m; i++) {
            while(j > 0 && haystack[i] != needle[j]) j = next[j-1];
            if (haystack[i] == needle[j]) j++;
            if (j == n) return i - n + 1;
        }
        return -1;
    }
};
```
[重复子字符串 459](https://leetcode.cn/problems/repeated-substring-pattern/description/)
```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        vector<int> next(n);
        for(int i = 1, j = 0; i < n; i++) {
            while (j > 0 && s[i] != s[j]) j = next[j-1];
            if (s[i] == s[j]) j++;
            next[i] = j;
        }
        if (next[n-1] == 0) return false;
        return n % (n - next[n-1]) == 0 ? true : false;
    }
};
```