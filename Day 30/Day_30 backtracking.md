# 代码随想录Day30 | 回溯  93.复原IP地址  78.子集 90.子集II
## 93.复原IP地址
[题目链接 93](https://leetcode.cn/problems/restore-ip-addresses/)

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。
### 第一次提交
```cpp
class Solution {
public:
    vector<string> res;
    string path;
    bool isValid(string s) {
        if (s.size() == 0) return false;
        if (s.size() == 1) return true;
        if (s.size() == 2) return !(s[0] == '0');
        if (s.size() == 3) {
            if (s[0] == '0') return false;
            return stoi(s) < 256;
        }
        return false;
    }
    void backtrack(string s, int cur, int cnt) {
        string temp = "";
        if (cnt == 3) {
            for (int i = cur; i < s.size(); i++) temp += s[i];
            if (isValid(temp)){
                path += temp;
                res.push_back(path);
            } 
            return;            
        } else {
            for (; cur < s.size() && cur < cur + 3; cur++) {
                temp += s[cur];
                if (isValid(temp)){
                    //cout <<"append "<< cur << " cur "<< path << " path "<<endl;
                    string past = path;
                    path += temp;
                    path += '.';
                    backtrack(s, cur + 1, cnt + 1);
                    path = past;
                    //cout <<"backtrack " << cur << " cur "<< path << " path "<<endl;
                } 
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        backtrack(s, 0, 0);
        cout<<isValid("255");
        return res;
    }
};
```
## 78.子集
[题目链接 78](https://leetcode.cn/problems/subsets/)
给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的
子集
（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

### 第一次提交

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtrack(vector<int>&nums, int cur) {
        res.push_back(path);
        for(; cur < nums.size(); cur ++) {
            path.push_back(nums[cur]);
           // res.push_back(path);
            backtrack(nums, cur + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
       // res.push_back(path);
        backtrack(nums, 0);
        return res;
    }
};
```

## 90.子集II
[题目链接 90](https://leetcode.cn/problems/subsets-ii/description/)
给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的 
子集
（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。
  

### 第一次提交

```cpp
class Solution {
public: 
    vector<vector<int>> res;
    vector<int> path;
    void backtrack(vector<int>& nums, int cur) {
        res.push_back(path);
        for(int i = cur; i< nums.size(); i++) {
            if (i > cur && nums[i] == nums[i-1]) continue;
            path.push_back(nums[i]);
            backtrack(nums, i+1);            
            path.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        backtrack(nums, 0);
        return res;
    }
};
```
