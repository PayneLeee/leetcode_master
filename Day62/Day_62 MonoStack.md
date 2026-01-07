# 随想录Day62 |  单调栈 739. 每日温度


##  739. 每日温度
[题目链接 739](https://leetcode.cn/problems/daily-temperatures/)

给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 answer[i] 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。

 
### 第一次提交
逆向遍历
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> mono;
        vector<int> res;
        mono.push(-1);
        for (int i = temperatures.size() - 1; i >= 0 ; i--) {
            while(mono.top() != -1 && temperatures[i] >= temperatures[mono.top()]) {
                mono.pop();
            }
            if (mono.top() == -1) res.push_back(0);
            else res.push_back(mono.top() - i );
            mono.push(i);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
### 学习题解
正向遍历
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> mono;
        vector<int> res(temperatures.size(), 0);
        mono.push(-1);
        for (int i = 0; i <temperatures.size() ; i++) {
           if (mono.top() == -1) {
                mono.push(i);
           } else if (temperatures[mono.top()] >= temperatures[i]) {
                mono.push(i);
        //    } else if (temperatures[mono.top()] == temperatures[i]) {
        //         mono.push(i);
           } else if (temperatures[mono.top()] < temperatures[i]) {
                while(mono.top() != -1 && temperatures[mono.top()] < temperatures[i]) {
                    res[mono.top()] = i - mono.top();
                    mono.pop();
                }
                mono.push(i);
           }
        }
        return res;
    }
};
```

## 496.下一个更大元素 I
[题目链接 496](https://leetcode.cn/problems/next-greater-element-i/description/)

nums1 中数字 x 的 下一个更大元素 是指 x 在 nums2 中对应位置 右侧 的 第一个 比 x 大的元素。

给你两个 没有重复元素 的数组 nums1 和 nums2 ，下标从 0 开始计数，其中nums1 是 nums2 的子集。

对于每个 0 <= i < nums1.length ，找出满足 nums1[i] == nums2[j] 的下标 j ，并且在 nums2 确定 nums2[j] 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 -1 。

返回一个长度为 nums1.length 的数组 ans 作为答案，满足 ans[i] 是如上所述的 下一个更大元素 。

### 第一次提交
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        vector<int> nextBig(nums2.size(), -1);
        unordered_map<int, int> valueToLoc;
        for (int i = 0; i < nums2.size(); i++) valueToLoc[nums2[i]] = i;
        stack<int> mono;
        mono.push(-1);
        for (int i = 0; i < nums2.size(); i++) {
            if (mono.top() == -1) {
                mono.push(i);
            } else if (nums2[mono.top()] > nums2[i]){
                mono.push(i);
            } else if (nums2[mono.top()] == nums2[i]){
                mono.push(i);
            } else if (nums2[mono.top()] < nums2[i]) {
                while(mono.top() != -1 && nums2[mono.top()] < nums2[i]) {
                    nextBig[mono.top()] = nums2[i];
                    mono.pop();
                }
                mono.push(i);
            }

        }
        for (int i : nums1) {
            res.push_back(nextBig[valueToLoc[i]]);
        }
        return res;
    }
};
```

## 503. 下一个更大元素 II
[题目链接 503](https://leetcode.cn/problems/next-greater-element-ii/description/)
给定一个循环数组 nums （ nums[nums.length - 1] 的下一个元素是 nums[0] ），返回 nums 中每个元素的 下一个更大元素 。

数字 x 的 下一个更大的元素 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1 。

### 第一次提交
```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> doubleNums;
        doubleNums.insert(doubleNums.end(), nums.begin(), nums.end());
        doubleNums.insert(doubleNums.end(), nums.begin(), nums.end());
        vector<int> res(nums.size() * 2, -1);
        stack<int> mono;
        mono.push(-1);
        for (int i = 0; i< nums.size()*2; i++) {
            if (mono.top() == -1) {
                mono.push(i);
            } else if (doubleNums[mono.top()] > doubleNums[i]) {
                mono.push(i);
            } else if (doubleNums[mono.top()] == doubleNums[i]) {
                mono.push(i);   
            } else if (doubleNums[mono.top()] < doubleNums[i]) {
                while (mono.top() != -1 && doubleNums[mono.top()] < doubleNums[i]) {
                    res[mono.top()] = doubleNums[i];
                    mono.pop();
                }
                mono.push(i);
            }
        }
        return vector<int>(res.begin(), res.begin() + nums.size());
    }
};
```

### 学习题解，模拟走两边
```cpp
 for (int i = 1; i < nums.size() * 2; i++) { 
    // 模拟遍历两边nums，注意一下都是用i % nums.size()来操作
    if (nums[i % nums.size()] < nums[st.top()]) st.push(i % nums.size());
    else if (nums[i % nums.size()] == nums[st.top()]) st.push(i % nums.size()); 
    else {
        while (!st.empty() && nums[i % nums.size()] > nums[st.top()]) {
            result[st.top()] = nums[i % nums.size()];
            st.pop();
        }
        st.push(i % nums.size());
    }
}
```