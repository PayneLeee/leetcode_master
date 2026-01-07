# 2024/5/24 Day38 greedy   435. 无重叠区间  763.划分字母区间  56. 合并区间 


**遇到两个维度权衡的时候，一定要先确定一个维度，再确定另一个维度。如果两个维度一起考虑一定会顾此失彼。**

重叠区间问题

## 435. 无重叠区间 
[题目链接 435](https://leetcode.cn/problems/non-overlapping-intervals/description/)
给定一个区间的集合 intervals ，其中 intervals[i] = [starti, endi] 。返回 需要移除区间的最小数量，使剩余区间互不重叠 。
### 提交
注意这里是两边都开的括号不重叠
```cpp
class Solution {
public:
    class cmp {
        public:
        cmp(){}
        bool operator()(const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        }
    };
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp());
        int arrow = intervals[0][1];
        int cnt = 1;
        for (vector<int> interval : intervals) {
            if (interval[0] >= arrow) {
                cnt ++;
                arrow = interval[1];
            } else {
                arrow = min (arrow, interval[1]);
            }
        }
        return intervals.size() - cnt;
    }
};
```

## 763.划分字母区间
[题目链接 763](https://leetcode.cn/problems/partition-labels/)
给你一个字符串 s 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 s 。

返回一个表示每个字符串片段的长度的列表。
### 第一次提交

未知化为已知， 遍历一遍字符串可以得到一个字母的起始位置和终止位置，之后可以转化成类似区间去重

时间效率意外还很不错。
```cpp
class Solution {
public:
    static bool cmp (const pair<int, int>& a, const pair<int, int>& b) {
        return a.first < b.first;
    }
    vector<int> partitionLabels(string s) {
        unordered_map<char, pair<int, int> > map;
        for (int i = 0; i < s.size(); i++) {
            char c = s[i];
            if (map.count(c)) {
                map[c].second = i;
            } else {
                pair<int, int> temp;
                temp.first = i;
                temp.second = i;
                map[c] = temp;
            }
        }
        vector<pair<int, int> > container;
        for (unordered_map<char, pair<int, int> > :: iterator  it = map.begin(); it != map.end(); it++) {
            container.push_back(it->second);
        }
        sort(container.begin(), container.end(), cmp);
        int arrow = container[0].second;
        int start = 0;
        vector<int> res;
        container.push_back(make_pair(s.size(), s.size()));
        for (pair<int, int> p : container) {
            if (p.first > arrow) {
                if (res.size() == 0) {
                    res.push_back(p.first);
                    start = p.first;
                }
                else {
                    res.push_back(p.first - start);
                    start = p.first;
                }
                arrow = p.second;
            } else {
                arrow = max (arrow, p.second);
            }
        }
        return res;
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

并不需要记录起始位置
可以分为如下两步：

1. 统计每一个字符最后出现的位置

2. 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点

用数组要比用unordered_map快
```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            hash[s[i] - 'a'] = i;
        }
        int right = 0;
        int left = 0;
        vector<int> res;
        for (int i = 0; i < s.size(); i++) {
            right = max(right, hash[s[i] - 'a']);
            if (right == i) {
                res.push_back(right + 1 - left);
                left = right + 1;
            }
        }
        return res;
    }
};
```
## 56. 合并区间 
[题目链接 56](https://leetcode.cn/problems/merge-intervals/) 以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

### 第一次提交

和划分字母区间中我的第一次做法很像
```cpp
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        int start = intervals[0][0];
        int end = intervals[0][1];
        vector<vector<int>> res;
        for (vector<int> interval : intervals) {
            if (interval[0] > end) {
                vector<int> temp;
                temp.push_back(start);
                temp.push_back(end);
                res.push_back(temp);
                start = interval[0];
                end = interval[1];
            } else {
                end = max(end, interval[1]);
            }
        }
        vector<int> temp;
        temp.push_back(start);
        temp.push_back(end);
        res.push_back(temp);
        return res;
    }
};
```

并没有很难